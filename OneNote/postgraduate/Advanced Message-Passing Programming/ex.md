Ex1  
**主题一句话**  
用 MPI RMA（One-sided）+ MPI_Win_fence（active target，同步 epoch）实现 3 个进程的“左边自增，中间搬运，右边打印”的 10 次迭代流水传递。
 
**这段代码实现的算法是什么**  
对应你课件里的 Algorithm 1（3 个进程，10 次迭代）：

- rank 0（LEFT）：有一个 window，里面 2 个 int：buffer[0] 初始 0，buffer[1] 初始 20；每次迭代按奇偶对其中一个自增
- rank 1（MIDDLE）：自己 window 为空；每次迭代从 rank0 的某个 int 读出来，再写到 rank2 的对应位置
- rank 2（RIGHT）：有一个 window，2 个 int 初始都 0；每次迭代按奇偶打印其中一个

关键点：读写都用 RMA（MPI_Get/MPI_Put），同步用 fence，把一次迭代切成一个 epoch。
 
**正确的流程长什么样（****Fence** **模型）**  
Fence 模型最重要的规则：

- MPI_Win_fence 是 collective：同一个 communicator 里的所有进程必须以同样的次数、同样的顺序调用。
- 两次 fence 之间是一个 epoch：MPI_Put/MPI_Get/MPI_Accumulate 必须发生在 epoch 内。
- 第二个 fence 返回时：保证该 epoch 内的 RMA 操作完成并对目标可见（完成语义）。

所以每次迭代 i 的“标准形状”应是：

1. MPI_Win_fence(..., win) 开始 epoch
2. LEFT 做本地自增；MIDDLE 做 Get+Put；RIGHT 不做 RMA（只等）
3. MPI_Win_fence(..., win) 结束 epoch并完成 RMA
4. RIGHT 在 fence 之后打印（保证读到的是更新后的值）
 
**你这份代码里最关键的错误点（一定要改）**  
**1) MPI_Win_create** **的** **size** **在所有进程上写死成了** **2** **个** **int**  
你现在写的是：
 
MPI_Win_create(buffer, sizeof(int) * 2, 1, MPI_INFO_NULL, MPI_COMM_WORLD, &win);  
这表示“每个 rank 都暴露 8 bytes 的 window”。  
但 Algorithm 1 里 rank1 window 是空的；而你 rank1 还 malloc(0)，却告诉 MPI “我有 8 bytes 可被远程访问”。这属于 **不一致的** **window** **描述**，轻则逻辑错，重则运行时崩。  
正确做法：每个进程用自己的 my_count 决定 size：

- LEFT/RIGHT：size = 2*sizeof(int)
- MIDDLE：size = 0

**2)** **你把** **MPI_Put** **放在了第二个** **fence** **之后（在** **epoch** **外）**  
你目前 MIDDLE 的结构是（简化后）：
 
MPI_Win_fence(...); // fence A 开始 epoch  
MPI_Get(...); // OK, 在 epoch 内  
MPI_Win_fence(...); // fence B 结束 epoch  
MPI_Put(...); // 错, 已经不在 epoch 内了  
这会直接导致 **未定义行为**：Put 可能不生效、可能死锁、可能随机错。  
正确结构必须是：
 
MPI_Win_fence(...); // 开始 epoch  
MPI_Get(...);  
MPI_Put(...);  
MPI_Win_fence(...); // 结束 epoch，保证完成  
**3) displacement** **的单位：你现在是** **“bytes”****，但写法很容易让人误以为是** **“int** **下标****”**  
你在 MPI_Win_create(..., disp_unit=1, ...)，所以 displacement 以字节为单位。  
这意味着 target_disp 应传 “字节偏移”。  
你现在传的是 sizeof(int) * k，在 disp_unit=1 的情况下是对的，但对初学者非常绕。  
更直观的写法是：把 disp_unit 设成 sizeof(int)，然后 displacement 直接用 0/1（按 int 下标）。
 
**带注释的****“****正确版本****”****（最贴近你原结构，只修关键错误）**  
我保留你的整体框架，只把 window 大小、epoch 内 Put/Get 顺序、disp_unit 写得更清楚。
 
#include \<stdio.h\>  
#include \<stdlib.h\>  
#include \<mpi.h\>  
#include "../include/util.h"
 
int main(int argc, char* argv[])  
{  
MPI_Init(&argc, &argv);
 
int comm_size;  
MPI_Comm_size(MPI_COMM_WORLD, &comm_size);  
if (comm_size != 3) {  
EPCC_printf("This application is meant to be run with 3 MPI processes, not %d.\n", comm_size);  
MPI_Abort(MPI_COMM_WORLD, 1); // 初学者建议：直接终止，避免继续跑出奇怪结果  
}
 
int my_rank;  
MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);
 
const int LEFT = 0;  
const int MIDDLE = 1;  
const int RIGHT = 2;
 
int my_count = 0; // 本进程 window 里有几个 int  
int* buffer = NULL; // 本进程暴露给别人的内存（window 的 base）  
MPI_Win win;
 
// 1) 按题意分配并初始化本地 window buffer  
if (my_rank == LEFT) {  
my_count = 2;  
buffer = (int*)malloc(sizeof(int) * my_count);  
buffer[0] = 0;  
buffer[1] = 20;  
} else if (my_rank == MIDDLE) {  
my_count = 0;  
buffer = NULL; // 空 window：base 可以是 NULL  
} else { // RIGHT  
my_count = 2;  
buffer = (int*)malloc(sizeof(int) * my_count);  
buffer[0] = 0;  
buffer[1] = 0;  
}
 
// 2) 创建 window  
// 推荐：disp_unit = sizeof(int)，这样 displacement 直接用 “int 下标” 表达  
MPI_Aint win_size_bytes = (MPI_Aint)my_count * (MPI_Aint)sizeof(int);
 
==MPI_Win_create(==  
==buffer, // base==  
==win_size_bytes, // size in bytes (====每个进程可以不同，====rank1== ==为== ==0)==  
==sizeof(int), // disp_unit====：位移单位== === 1== ==个== ==int==  
==MPI_INFO_NULL,==  
==MPI_COMM_WORLD,==  
==&win==  
==);==
 
// 3) 迭代 10 次：每次迭代用一对 fence 包住一个 epoch  
for (int i = 0; i \< 10; i++) {
 
// fence-1：开始 epoch（所有进程必须都调用）  
MPI_Win_fence(0, win);
 
// 这次迭代要操作哪一个槽位：  
// i=0 (even) -\> 操作槽 0（first int）  
// i=1 (odd) -\> 操作槽 1（second int）  
int idx = i % 2;
 
if (my_rank == LEFT) {  
// LEFT：本地自增（这是普通内存写，不是 RMA）  
buffer[idx]++;
 
} else if (my_rank == MIDDLE) {  
// MIDDLE：把 LEFT 的 idx 槽读出来，再写到 RIGHT 的 idx 槽
 
int temp = 0;
 
// 注意：displacement 现在是 “以 sizeof(int) 为单位”的下标  
// 所以 target_disp 直接传 idx  
MPI_Get(  
&temp, 1, MPI_INT, // origin buffer  
LEFT, idx, 1, MPI_INT,// target  
win  
);
 
MPI_Put(  
&temp, 1, MPI_INT, // origin buffer  
RIGHT, idx, 1, MPI_INT,// target  
win  
);
 
} else { // RIGHT  
// RIGHT：epoch 内不需要做 RMA，只等待 fence 完成语义  
}
 
// fence-2：结束 epoch，并保证本轮 Get/Put 完成且对目标可见  
MPI_Win_fence(0, win);
 
// fence 之后打印才安全：RIGHT 的 buffer 已经被更新  
if (my_rank == RIGHT) {  
EPCC_printf("iter %d: buffer[%d] = %d\n", i, idx, buffer[idx]);  
}  
}
 
// 4) 释放资源（MPI_Win_free 也是 collective）  
MPI_Win_free(&win);
 
free(buffer); // rank1 buffer=NULL 也安全  
MPI_Finalize();  
return EXIT_SUCCESS;  
}
 
**这段程序每次迭代到底发生了什么（按时间线讲）**  
以 i=1（odd）为例：

1. 三个进程一起进入 MPI_Win_fence：epoch 开始
2. LEFT：buffer[1]++，把 second int 从 20 加到 21
3. MIDDLE：
    - MPI_Get 从 LEFT 的 buffer[1] 读到 temp
    - MPI_Put 把 temp 写到 RIGHT 的 buffer[1]
4. 三个进程一起进入第二个 MPI_Win_fence：epoch 结束
    - fence 返回时：保证 MIDDLE 的 Put 已经“落到”RIGHT 的 window 里
5. RIGHT：在 fence 之后打印 buffer[1]，一定看到更新后的值 21
 
Ex2￼￼  
**你已经完成的部分：****disp_unit** **改动点**  
你现在：
 
MPI_Win_create(buffer, sizeof(int) * 2, sizeof(int), ...);  
含义：

- window 位移单位 = sizeof(int)
- 所以 target_displacement 传 0 表示 buffer[0]
- 传 1 表示 buffer[1]

因此你把 MPI_Get(..., disp=(i+1)%2, ...) 这种写法从“按字节偏移”变成“按 int 下标偏移”，这是 Exercise 2 要求的核心。
 
**但现在仍然有** **2** **个必须修的错误**  
**错误** **1****：****MPI_Put** **放在了** **epoch** **外（未定义行为）**  
你 MIDDLE 的顺序是：
 
MPI_Win_fence(0, win); // 开始 epoch  
MPI_Get(...); // 在 epoch 内 OK  
MPI_Win_fence(0, win); // 结束 epoch  
MPI_Put(...); // 错：已经不在 epoch 内  
MPI_Put 必须发生在两次 fence 之间。否则未定义行为。  
正确写法：把 MPI_Put 移到第二个 fence 之前，并且 MIDDLE 也要像其他 rank 一样在本轮迭代里调用两次 fence。
 
**错误** **2****：****MPI_Win_create** **的** **size** **对所有** **rank** **都写死成** **2** **个** **int****（****rank1** **实际是空** **window****）**  
你 MIDDLE（rank1）是 malloc(0)，但仍然：
 
MPI_Win_create(buffer, sizeof(int) * 2, ...);  
这表示 rank1 暴露 8 bytes 的 window。可它根本没有有效内存，这会导致错误。  
正确做法：每个进程用自己的 my_count 决定 window size：

- LEFT/RIGHT：my_count=2 -\> size = 2*sizeof(int)
- MIDDLE：my_count=0 -\> size = 0，base 可以是 NULL
 
**给你一份****“****最小改动版****”****，同时满足** **Exercise 2****（****disp_unit = sizeof(int)****）**  
我尽量不重写结构，只改必要行，让它正确。
 
#include \<stdio.h\>  
#include \<stdlib.h\>  
#include \<mpi.h\>  
#include "../include/util.h"
 
int main(int argc, char* argv[])  
{  
MPI_Init(&argc, &argv);
 
int comm_size;  
MPI_Comm_size(MPI_COMM_WORLD, &comm_size);  
if (comm_size != 3) {  
EPCC_printf("This application is meant to be run with 3 MPI processes, not %d.\n", comm_size);  
MPI_Abort(MPI_COMM_WORLD, 1);  
}
 
int my_rank;  
MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);
 
const int LEFT = 0;  
const int MIDDLE = 1;  
const int RIGHT = 2;
 
int my_count = 0;  
int* buffer = NULL;  
MPI_Win win;
 
switch (my_rank)  
{  
case LEFT:  
my_count = 2;  
buffer = malloc(sizeof(int) * my_count);  
buffer[0] = 0;  
buffer[1] = 20;  
break;
 
case MIDDLE:  
my_count = 0;  
buffer = NULL; // 空 window：base 允许 NULL  
break;
 
case RIGHT:  
my_count = 2;  
buffer = malloc(sizeof(int) * my_count);  
buffer[0] = 0;  
buffer[1] = 0;  
break;  
}
 
// size 必须按本进程 my_count 来  
MPI_Aint win_size = (MPI_Aint)my_count * (MPI_Aint)sizeof(int);
 
// Exercise 2: disp_unit = sizeof(int)，displacement 用 int 下标  
MPI_Win_create(buffer, win_size, sizeof(int), MPI_INFO_NULL, MPI_COMM_WORLD, &win);
 
for (int i = 0; i \< 10; i++)  
{  
int idx = i % 2; // 本轮操作/打印的槽位：0 或 1
 
// fence 1：开始 epoch  
MPI_Win_fence(0, win);
 
switch (my_rank)  
{  
case LEFT:  
buffer[idx]++; // 本地自增（非 RMA）  
break;
 
case MIDDLE: {  
int temp_buffer = 0;
 
// 题意：odd 读 LEFT[0] 写 RIGHT[0]；even 读 LEFT[1] 写 RIGHT[1]  
// 也就是读/写的槽位是 (i+1)%2  
int srcdst = (i + 1) % 2;
 
MPI_Get(&temp_buffer, 1, MPI_INT, LEFT, srcdst, 1, MPI_INT, win);  
MPI_Put(&temp_buffer, 1, MPI_INT, RIGHT, srcdst, 1, MPI_INT, win);  
break;  
}
 
case RIGHT:  
// RIGHT 本轮不发 RMA  
break;  
}
 
// fence 2：结束 epoch，并保证 Get/Put 完成且对目标可见  
MPI_Win_fence(0, win);
 
if (my_rank == RIGHT) {  
EPCC_printf("iter %d: buffer[%d] = %d\n", i, idx, buffer[idx]);  
}  
}
 
MPI_Win_free(&win);
 
free(buffer); // NULL 也安全  
MPI_Finalize();  
return EXIT_SUCCESS;  
}
 
**你这题里** **displacement unit** **改动后，最容易犯的****“****单位错位****”**  
当 disp_unit = sizeof(int) 时：

- 想访问 target_buffer[k]，就传 target_displacement = k
- 千万别再写 sizeof(int) * k，否则会跳过 k*sizeof(int) 个 int，越界得很隐蔽。

你现在 MPI_Get(..., (i+1)%2, ...) 这种就属于“对 disp_unit=sizeof(int) 的正确写法”。

Ex3￼￼  
下面我会按“初学者能顺着读懂”的标准，把你这份 Exercise 3（GATS：post/start/complete/wait）代码做**非常全面的行内注释** **+** **流程解释** **+** **易错点清单**。同时我会明确指出：你现在这份代码的同步顺序里有一个“非常典型、会直接卡死/未定义”的坑，以及怎么改成正确的 GATS 配对。
 
**这段代码想实现什么（****Exercise 3** **目标）**  
把 Exercise 1/2 里用的 MPI_Win_fence（全体 collective，同步粗）替换成 **General Active Target Synchronisation (GATS)**：

- Target 侧：MPI_Win_post（开始 exposure epoch） -\> MPI_Win_wait（结束 exposure epoch）
- Origin 侧：MPI_Win_start（开始 access epoch） -\> 发起 RMA（Get/Put） -\> MPI_Win_complete（结束 access epoch）

核心变化：不再全体 barrier，而是**只有相关进程之间**通过 group 进行“局部握手”。
 
**先说结论：你这份代码里最关键的** **bug****（会卡死）**  
你在 LEFT 和 RIGHT（target）侧写的是：  
buffer_left_middle[i%2]++; // 先自增￼MPI_Win_post(middle_group, 0, win_left_middle);￼MPI_Win_wait(win_left_middle);  
但在 GATS 语义下：

- MPI_Win_post 必须与 origin 的 MPI_Win_start **匹配同一个** **exposure/access epoch**。
- 如果 target 先 post 再 wait，而 origin 还没来得及 start/complete，那 target 会在 wait 阻塞，没问题。
- 真正的问题是：你 LEFT 在 post/wait 之前就做了 buffer_left_middle[i%2]++，这只是普通本地 store，不是 RMA。它本身没问题，但更大的问题是：你的 MIDDLE 每轮做两段 access（先对 LEFT 的 Get，再对 RIGHT 的 Put），而 LEFT/RIGHT 每轮都只做自己的一段 post/wait，这在结构上可以，但必须保证 **每轮的****“****配对顺序****”****在三进程之间不会交叉错位**。

你现在的写法在很多 MPI 实现下会出现一个常见死锁形态：  
某轮里 RIGHT 已经 post/wait 等 MIDDLE 的 complete(win_middle_right)，但 MIDDLE 还卡在 start(win_left_middle) 等 LEFT 的 post（或相反）。因为你把两条 win 的同步“串行写在 MIDDLE 里”，而 LEFT/RIGHT 也串行 wait，一旦某个实现/调度导致顺序错位，就可能互相等待。  
最稳的做法是：**让每轮迭代的同步顺序固定成****“****两阶段流水****”**：

- 阶段 A：LEFT \<-\> MIDDLE 在 win_left_middle 上完成一次 Get
- 阶段 B：MIDDLE \<-\> RIGHT 在 win_middle_right 上完成一次 Put + RIGHT 打印

也就是说：LEFT 每轮只围绕 win_left_middle 做 post/wait；RIGHT 每轮只围绕 win_middle_right 做 post/wait；MIDDLE 每轮也先完成 win_left_middle 的 start/get/complete，再完成 win_middle_right 的 start/put/complete。  
你目前大体是这个顺序，但还缺少一些必要的“全局一致节拍”来避免错位（下面我给你更稳的版本）。
 
**你这份代码逐行****“****超详细注释版****”****（先按你当前写法注释）**  
我会保留你的代码结构，仅加注释，并在关键坑位标注 “这里可能死锁/建议改法”。  
#include \<stdio.h\>￼#include \<stdlib.h\>￼#include \<mpi.h\>￼#include "../include/util.h"  
int main(int argc, char* argv[])￼{￼ MPI_Init(&argc, &argv);  
// 题目规定必须 3 个进程：LEFT=0, MIDDLE=1, RIGHT=2￼ int comm_size;￼ MPI_Comm_size(MPI_COMM_WORLD, &comm_size);￼ if(comm_size != 3)￼ {￼ EPCC_printf("This application is meant to be run with 3 MPI processes, not %d.\n", comm_size);￼ // 建议：直接 Abort，避免继续跑导致未定义行为￼ // MPI_Abort(MPI_COMM_WORLD, 1);￼ }  
int my_rank;￼ MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);  
// 这里你定义了两套 window 的 buffer：￼ // win_left_middle: 只在 LEFT 暴露 2 个 int；MIDDLE 暴露 0￼ // win_middle_right: 只在 RIGHT 暴露 2 个 int；MIDDLE 暴露 0￼ //￼ // 注意：同一个 communicator 里每个 rank 的 window size 允许不同（可以为 0）￼ int my_count_left_middle = 0;￼ int* buffer_left_middle = NULL;￼ int my_count_middle_right = 0;￼ int* buffer_middle_right = NULL;  
const int LEFT = 0;￼ const int MIDDLE = 1;￼ const int RIGHT = 2;  
// GATS 需要 MPI_Group 来表示“允许访问的一组进程”￼ // 这里先拿到 MPI_COMM_WORLD 对应的 group（包含 0,1,2）￼ MPI_Group MPI_GROUP_WORLD;￼ MPI_Comm_group(MPI_COMM_WORLD, &MPI_GROUP_WORLD);￼ ￼ // 只在 LEFT 分配 buffer_left_middle；只在 RIGHT 分配 buffer_middle_right￼ // MIDDLE 不分配（空 window）￼ switch(my_rank)￼ {￼ case LEFT:￼ {￼ my_count_left_middle = 2;￼ buffer_left_middle = malloc(sizeof(int) * my_count_left_middle);￼ buffer_left_middle[0] = 0;￼ buffer_left_middle[1] = 20;￼ break;￼ }￼ case MIDDLE:￼ {￼ // 空 window：count=0，buffer=NULL￼ break;￼ }￼ case RIGHT:￼ {￼ my_count_middle_right = 2;￼ buffer_middle_right = malloc(sizeof(int) * my_count_middle_right);￼ buffer_middle_right[0] = 0;￼ buffer_middle_right[1] = 0;￼ break;￼ }￼ }  
// 下面你创建了三个只包含单个 rank 的 group：￼ // left_group = {0}, middle_group = {1}, right_group = {2}￼ //￼ // GATS 的关键：target 调 MPI_Win_post(group_of_origins)￼ // origin 调 MPI_Win_start(group_of_targets)￼ // group 的方向不要搞反  
int left_rank[1] = {LEFT};￼ MPI_Group left_group;￼ MPI_Group_incl(MPI_GROUP_WORLD, 1, left_rank, &left_group);  
int middle_rank[1] = {MIDDLE};￼ MPI_Group middle_group;￼ MPI_Group_incl(MPI_GROUP_WORLD, 1, middle_rank, &middle_group);  
int right_rank[1] = {RIGHT};￼ MPI_Group right_group;￼ MPI_Group_incl(MPI_GROUP_WORLD, 1, right_rank, &right_group);  
// 你这里还创建了 left_comm / middle_comm / right_comm￼ // 但后面完全没用到这些 communicator￼ // 在 Exercise 3 里其实不需要创建新 communicator，只用 group 就够了￼ MPI_Comm left_comm;￼ MPI_Comm_create_group(MPI_COMM_WORLD, left_group, 0, &left_comm);  
MPI_Comm middle_comm;￼ MPI_Comm_create_group(MPI_COMM_WORLD, middle_group, 0, &middle_comm);  
MPI_Comm right_comm;￼ MPI_Comm_create_group(MPI_COMM_WORLD, right_group, 0, &right_comm);  
// 创建两扇 window：￼ // win_left_middle：LEFT 暴露 2*int，MIDDLE/RIGHT size=0￼ // win_middle_right：RIGHT 暴露 2*int，MIDDLE/LEFT size=0￼ //￼ // 注意：下面 size 你写的是 sizeof(int) * my_count_xxx￼ // 这对未分配的 rank（count=0）没问题：size=0, base=NULL￼ // disp_unit = sizeof(int)，因此 displacement 用 int 下标（0/1）￼ MPI_Win win_left_middle;￼ MPI_Win win_middle_right;  
MPI_Win_create(buffer_left_middle,￼ sizeof(int) * my_count_left_middle,￼ sizeof(int),￼ MPI_INFO_NULL,￼ MPI_COMM_WORLD,￼ &win_left_middle);  
MPI_Win_create(buffer_middle_right,￼ sizeof(int) * my_count_middle_right,￼ sizeof(int),￼ MPI_INFO_NULL,￼ MPI_COMM_WORLD,￼ &win_middle_right);  
// 主循环 10 次迭代￼ // 题意：odd 轮（i=1,3,5...）：￼ // LEFT 增加 second int（idx=1）￼ // MIDDLE 读 LEFT[0] 写 RIGHT[0]￼ // RIGHT 打印 RIGHT[1]（second int）￼ //￼ // even 轮（i=0,2,4...）：￼ // LEFT 增加 first int（idx=0）￼ // MIDDLE 读 LEFT[1] 写 RIGHT[1]￼ // RIGHT 打印 RIGHT[0]（first int）￼ //￼ // 你的实现用 (i+1)%2 作为“搬运的槽位”，用 i%2 作为“打印/自增槽位”￼ // 这与题意是匹配的：自增 idx=i%2；搬运 srcdst=(i+1)%2；打印 idx=i%2（打印的是另一边）￼ for(int i = 0; i \< 10; i++)￼ {￼ switch(my_rank)￼ {￼ case LEFT:￼ {￼ // 本地自增（普通 store）￼ buffer_left_middle[i%2]++;  
// LEFT 是 target：暴露 win_left_middle 给 origin=MIDDLE￼ // post(group_of_origins = {MIDDLE})￼ MPI_Win_post(middle_group, 0, win_left_middle);  
// wait：阻塞直到所有匹配的 origin 调用了 complete￼ // 返回时：该 exposure epoch 内对本 window 的 RMA 在 target 上也完成￼ MPI_Win_wait(win_left_middle);  
break;￼ }￼ case MIDDLE:￼ {￼ int temp_buffer;  
// MIDDLE 作为 origin，对 LEFT 进行 access epoch￼ // start(group_of_targets = {LEFT})￼ MPI_Win_start(left_group, 0, win_left_middle);  
// 在 access epoch 内发起 RMA：从 LEFT 的 window 读一个 int￼ // displacement 单位为 sizeof(int)，所以 (i+1)%2 是 int 下标￼ MPI_Get(&temp_buffer, 1, MPI_INT,￼ 0, (i+1)%2, 1, MPI_INT,￼ win_left_middle);  
// complete：结束对 LEFT window 的 access epoch￼ // 返回时：保证该 access epoch 内的 RMA 在 origin 侧完成（发送缓冲区可复用等）￼ MPI_Win_complete(win_left_middle);  
// 接着对 RIGHT window 开始第二段 access epoch￼ MPI_Win_start(right_group, 0, win_middle_right);  
// 把刚才读到的值写到 RIGHT 的对应槽位￼ MPI_Put(&temp_buffer, 1, MPI_INT,￼ 2, (i+1)%2, 1, MPI_INT,￼ win_middle_right);  
// 结束对 RIGHT 的 access epoch￼ MPI_Win_complete(win_middle_right);  
break;￼ }￼ case RIGHT:￼ {￼ // RIGHT 是 target：暴露 win_middle_right 给 origin=MIDDLE￼ MPI_Win_post(middle_group, 0, win_middle_right);  
// 等待 MIDDLE 对该 window 的访问 complete￼ MPI_Win_wait(win_middle_right);  
// wait 返回后，MIDDLE 的 Put 已经在 RIGHT 的 window 上完成，可以安全打印￼ EPCC_printf("buffer[%d] = %d\n", i%2, buffer_middle_right[i%2]);￼ break;￼ }￼ }￼ }  
// 释放 window（collective on communicator MPI_COMM_WORLD）￼ MPI_Win_free(&win_left_middle);￼ MPI_Win_free(&win_middle_right);  
// 注意：你没有释放 group/comm，这会造成资源泄漏（小规模作业影响不大，但规范上要 free）￼ // MPI_Group_free(&left_group); MPI_Group_free(&middle_group); MPI_Group_free(&right_group);￼ // MPI_Group_free(&MPI_GROUP_WORLD);￼ // if (left_comm != MPI_COMM_NULL) MPI_Comm_free(&left_comm); 等  
MPI_Finalize();￼ return EXIT_SUCCESS;￼}
 
**这份代码****“****概念上对****”****，但我建议你做** **3** **个改动来保证稳**  
**改动** **1****：删掉** **MPI_Comm_create_group** **相关（你没用到）**  
只用 group 就足够。创建 comm 没意义还要 free，容易忘。  
**改动** **2****：显式分两阶段同步，避免错位死锁**  
最稳的 GATS 写法是：每轮让所有进程都按同一个“节拍”走：

- 阶段 A（LEFT 暴露，MIDDLE 读取）￼LEFT: post(win_left_middle) -\> wait(win_left_middle)￼MIDDLE: start(win_left_middle) -\> get -\> complete(win_left_middle)￼RIGHT: 这一阶段什么都不做（或做计算）
- 阶段 B（RIGHT 暴露，MIDDLE 写入，RIGHT 打印）￼RIGHT: post(win_middle_right) -\> wait(win_middle_right) -\> print￼MIDDLE: start(win_middle_right) -\> put -\> complete(win_middle_right)￼LEFT: 这一阶段什么都不做（或做计算）

你的代码把这两阶段都放在一个 switch 里是可以的，但为了避免实现差异导致错位，我建议把它写成“全局两段 if”，每个 rank 都经历两段（只是有的段为空操作）。  
**改动** **3****：资源释放完整（****groups / world group****）**  
考试/作业一般会看规范性：MPI_Group_free。
 
**给你一份****“****更稳、更像教科书答案****”****的** **GATS** **版本（带更全面注释）**  
==这份实现严格按两阶段节拍走，最不容易错，也最方便你写报告解释。==  
#include \<stdio.h\>￼#include \<stdlib.h\>￼#include \<mpi.h\>￼#include "../include/util.h"  
int main(int argc, char* argv[])￼{￼ MPI_Init(&argc, &argv);  
int comm_size, my_rank;￼ MPI_Comm_size(MPI_COMM_WORLD, &comm_size);￼ MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);  
const int LEFT = 0, MIDDLE = 1, RIGHT = 2;  
if (comm_size != 3) {￼ EPCC_printf("Need 3 MPI processes, got %d\n", comm_size);￼ MPI_Abort(MPI_COMM_WORLD, 1);￼ }  
// -----------------------------￼ // 1) 准备 window buffer￼ // -----------------------------￼ int left_count = 0;￼ int* left_buf = NULL; // 只在 LEFT 有效￼ int right_count = 0;￼ int* right_buf = NULL; // 只在 RIGHT 有效  
if (my_rank == LEFT) {￼ left_count = 2;￼ left_buf = (int*)malloc(sizeof(int) * left_count);￼ left_buf[0] = 0;￼ left_buf[1] = 20;￼ }  
if (my_rank == RIGHT) {￼ right_count = 2;￼ right_buf = (int*)malloc(sizeof(int) * right_count);￼ right_buf[0] = 0;￼ right_buf[1] = 0;￼ }  
// -----------------------------￼ // 2) 准备 groups（GATS 核心）￼ // -----------------------------￼ // GATS 的方向必须记住：￼ // - target 调 post(group_of_origins)￼ // - origin 调 start(group_of_targets)  
MPI_Group world_group;￼ MPI_Comm_group(MPI_COMM_WORLD, &world_group);  
int left_rank[1] = {LEFT};￼ int middle_rank[1] = {MIDDLE};￼ int right_rank[1] = {RIGHT};  
MPI_Group left_group, middle_group, right_group;￼ MPI_Group_incl(world_group, 1, left_rank, &left_group);￼ MPI_Group_incl(world_group, 1, middle_rank, &middle_group);￼ MPI_Group_incl(world_group, 1, right_rank, &right_group);  
// -----------------------------￼ // 3) 创建两个 windows￼ // -----------------------------￼ // win_left: LEFT 暴露 left_buf[2]；其他 rank size=0￼ // win_right: RIGHT 暴露 right_buf[2]；其他 rank size=0￼ //￼ // disp_unit = sizeof(int) -\> displacement 用 int 下标（0/1）￼ MPI_Win win_left, win_right;  
MPI_Aint left_size = (MPI_Aint)left_count * (MPI_Aint)sizeof(int);￼ MPI_Aint right_size = (MPI_Aint)right_count * (MPI_Aint)sizeof(int);  
MPI_Win_create(left_buf, left_size, sizeof(int), MPI_INFO_NULL, MPI_COMM_WORLD, &win_left);￼ MPI_Win_create(right_buf, right_size, sizeof(int), MPI_INFO_NULL, MPI_COMM_WORLD, &win_right);  
// -----------------------------￼ // 4) 10 iterations using GATS￼ // -----------------------------￼ for (int i = 0; i \< 10; i++) {  
int inc_print_idx = i % 2; // LEFT 自增 / RIGHT 打印的槽位￼ int move_idx = (i + 1) % 2; // MIDDLE 搬运的槽位（读 LEFT[move_idx] 写 RIGHT[move_idx]）  
// LEFT：本地先完成自增（这不是 RMA，只是普通内存写）￼ if (my_rank == LEFT) {￼ left_buf[inc_print_idx]++;￼ }  
// -------------------------￼ // Phase A: MIDDLE gets from LEFT (win_left)￼ // target = LEFT, origin = MIDDLE￼ // -------------------------￼ int temp = 0;  
if (my_rank == LEFT) {￼ // LEFT 是 target：允许 origin=MIDDLE 访问我的 win_left￼ MPI_Win_post(middle_group, 0, win_left);￼ MPI_Win_wait(win_left);￼ // wait 返回：本轮对 LEFT window 的 RMA（这里是 Get）在 target 上完成￼ }  
if (my_rank == MIDDLE) {￼ // MIDDLE 是 origin：我要访问 target=LEFT 的 win_left￼ MPI_Win_start(left_group, 0, win_left);￼ MPI_Get(&temp, 1, MPI_INT, LEFT, move_idx, 1, MPI_INT, win_left);￼ MPI_Win_complete(win_left);￼ // complete 返回：Get 在 origin 侧完成（temp 现在可用）￼ }  
// RIGHT 在 Phase A 无需做同步（可做计算），这里空着就行  
// -------------------------￼ // Phase B: MIDDLE puts to RIGHT (win_right), RIGHT prints￼ // target = RIGHT, origin = MIDDLE￼ // -------------------------￼ if (my_rank == RIGHT) {￼ MPI_Win_post(middle_group, 0, win_right);￼ MPI_Win_wait(win_right);￼ // wait 返回：Put 已经落到 RIGHT window，可以安全读并打印￼ EPCC_printf("iter %d: buffer[%d] = %d\n", i, inc_print_idx, right_buf[inc_print_idx]);￼ }  
if (my_rank == MIDDLE) {￼ MPI_Win_start(right_group, 0, win_right);￼ MPI_Put(&temp, 1, MPI_INT, RIGHT, move_idx, 1, MPI_INT, win_right);￼ MPI_Win_complete(win_right);￼ // complete 返回：Put 在 origin 侧完成；target 是否完成由 RIGHT 的 wait 保证￼ }  
// LEFT 在 Phase B 无需做同步（可做计算）￼ }  
// -----------------------------￼ // 5) cleanup￼ // -----------------------------￼ MPI_Win_free(&win_left);￼ MPI_Win_free(&win_right);  
MPI_Group_free(&left_group);￼ MPI_Group_free(&middle_group);￼ MPI_Group_free(&right_group);￼ MPI_Group_free(&world_group);  
free(left_buf);￼ free(right_buf);  
MPI_Finalize();￼ return EXIT_SUCCESS;￼}
 
**易错点清单（****Exercise 3** **高频扣分点）**

1. group 方向写反
2. post 传的是 “origins”
3. start 传的是 “targets”￼写反就匹配不上，直接卡死。
4. post/wait 与 start/complete 不匹配￼同一个 window 上：每个 exposure epoch 必须有匹配的 access epoch，否则 wait 永远等不到。
5. 以为 MPI_Win_complete 表示目标已更新￼complete 只保证 origin 侧完成；目标落地要看 target 的 MPI_Win_wait 返回。
6. window size 没按 rank 正确设置（空 window 要 size=0）￼否则会暴露不存在的内存。
7. 忘记释放 MPI_Group_free￼规范性扣分点。
 
如果你希望“注释更全面”到课堂作业/报告级别，我也可以按你们实验要求把每个 API 的语义用一句话贴在调用行上（比如 complete/wait 的 completion 语义、是否阻塞、对 buffer 可复用性等），并且给你一段可直接复制进 report 的 “Fence vs GATS 对比解释段落”。
 \> 来自 \<[https://chatgpt.com/g/g-p-697bedcd2a608191bd4f46274b4a3e7c-ampp/c/69a80c79-46b8-8386-9289-23c7e314acf3](https://chatgpt.com/g/g-p-697bedcd2a608191bd4f46274b4a3e7c-ampp/c/69a80c79-46b8-8386-9289-23c7e314acf3)\>
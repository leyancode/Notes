下面给你一份**“考前冲刺版用户手册”**。目标是：  
即使完全不懂 MPI One-sided，也能在 10–15 分钟内理解这段程序在干什么，并知道考试可能问什么。  
我会按 **直觉** **→** **结构** **→ API →** **执行流程** **→** **例子** **→** **常见错误** **→** **考试要点** 的顺序写。
 
**一、这段程序在做什么（最简单理解）**  
这段程序用 **MPI One-sided communication****（****RMA****）** 实现一个 **三进程流水线**。  
三个进程：

|   |   |   |
|---|---|---|
|**Rank**|**角色**|**作用**|
|0|LEFT|负责增加数值|
|1|MIDDLE|负责搬运数据|
|2|RIGHT|负责打印|

运行逻辑：  
每一轮循环：  
1️⃣ LEFT 更新一个数  
2️⃣ MIDDLE 从 LEFT 读取  
3️⃣ MIDDLE 把值写到 RIGHT  
4️⃣ RIGHT 打印  
因此：  
LEFT --\> MIDDLE --\> RIGHT￼增值 搬运 打印￼  
运行 10 次循环。
 
**二、为什么要用** **One-sided communication**  
传统 MPI：  
MPI_Send￼MPI_Recv￼  
必须两边同时调用。  
而 One-sided：  
MPI_Put￼MPI_Get￼  
只需要 **origin****（发起者）** 调用。  
目标进程 **target** 不需要调用发送/接收函数。  
但问题是：  
因为没有 send/recv 配对，  
必须额外提供 **同步机制**。
 
**三、****Three Core Concepts****（三个核心概念）**  
理解下面三个概念就理解了整个程序。
 
**1 Window****（窗口）**  
window = **允许别人访问的内存**  
例如：  
int buffer[2]￼  
暴露成 window：  
MPI_Win_create(buffer, ...)￼  
之后别的进程就可以：  
MPI_Get￼MPI_Put￼  
访问这块内存。
 
**2 Origin** **与** **Target**

|   |   |
|---|---|
|**角色**|**含义**|
|origin|发起 RMA 操作|
|target|被访问的进程|

例如：  
MPI_Get￼  
origin = MIDDLE￼target = LEFT￼  
MIDDLE 从 LEFT 的 window 读取。
 
**3 Epoch****（访问时间段）**  
RMA 访问必须发生在 **epoch** **内**。  
简单理解：  
start epoch￼ MPI_Get / MPI_Put￼end epoch￼
 
**四、****Fence vs GATS**  
Exercise 1 使用：  
MPI_Win_fence￼  
特点：

- 所有进程一起同步
- 类似 barrier

fence￼RMA￼fence￼
 
Exercise 3 使用：  
**GATS****（****General Active Target Synchronisation****）**  
更细粒度同步。  
角色分开：

|   |   |
|---|---|
|**角色**|**API**|
|Target|post / wait|
|Origin|start / complete|
 
**五、****GATS** **四个核心函数**  
必须记住。
 
**1 MPI_Win_post**  
Target 调用。  
含义：  
我允许这些 origin 访问我的 window￼  
MPI_Win_post(origin_group, ...)￼
 
**2 MPI_Win_start**  
Origin 调用。  
含义：  
我要访问这些 target￼  
MPI_Win_start(target_group, ...)￼
 
**3 MPI_Win_complete**  
Origin 调用。  
含义：  
我的 RMA 操作已经完成（origin 侧）￼
 
**4 MPI_Win_wait**  
Target 调用。  
含义：  
等待所有 origin 完成访问￼  
返回时：  
target window 已经更新￼
 
**六、完整同步模型**  
Fence：  
ALL processes￼ fence￼ RMA￼ fence￼
 
GATS：  
Target￼ post￼ wait￼  
Origin￼ start￼ RMA￼ complete￼
 
**七、本程序的结构**  
程序建立了 **两个** **window**  
原因：  
LEFT 和 RIGHT 不能直接通信。  
必须经过 MIDDLE。
 
**Window 1**  
win_left￼  
LEFT 暴露 window  
MIDDLE 读取  
LEFT \<-- MIDDLE￼
 
**Window 2**  
win_right￼  
RIGHT 暴露 window  
MIDDLE 写入  
MIDDLE --\> RIGHT￼
 
**八、程序执行流程（每一轮）**  
假设 i = 0
 
**Step 1**  
LEFT 更新数据  
buffer_left[i%2]++￼
 
**Step 2**  
MIDDLE 从 LEFT 读取  
同步：  
LEFT￼ MPI_Win_post￼ MPI_Win_wait￼  
MIDDLE￼ MPI_Win_start￼ MPI_Get￼ MPI_Win_complete￼
 
结果：  
temp = LEFT buffer￼
 
**Step 3**  
MIDDLE 写入 RIGHT  
同步：  
RIGHT￼ MPI_Win_post￼ MPI_Win_wait￼  
MIDDLE￼ MPI_Win_start￼ MPI_Put￼ MPI_Win_complete￼
 
结果：  
RIGHT buffer 更新￼
 
**Step 4**  
RIGHT 打印  
printf￼
 
**九、数据移动示例**  
假设：  
LEFT buffer = [0, 20]￼RIGHT buffer = [0, 0]￼
 
Iteration 0  
LEFT  
buffer[0]++￼  
变成  
[1, 20]￼
 
MIDDLE  
读取：  
LEFT[1]￼  
得到  
20￼  
写入：  
RIGHT[1]￼
 
RIGHT  
打印：  
buffer[0]￼
 
Iteration 1  
LEFT  
buffer[1]++￼  
变成  
[1, 21]￼
 
MIDDLE  
读取  
LEFT[0]￼  
写入  
RIGHT[0]￼
 
RIGHT  
打印  
buffer[1]￼
 
**十、为什么要用两个** **window**  
因为：  
LEFT 与 RIGHT 之间 **没有直接** **RMA**  
数据必须：  
LEFT -\> MIDDLE -\> RIGHT￼  
如果只用一个 window：  
会产生同步冲突。
 
**十一、程序中的** **Group**  
GATS 需要 group。  
原因：  
target 需要知道 **哪些** **origin** **可以访问它**。  
例如：  
LEFT￼ post(middle_group)￼  
表示：  
只有 MIDDLE 可以访问我￼
 
**十二、最容易犯的错误**  
**1 group** **写反**  
错误：  
post(target_group)￼  
正确：  
post(origin_group)￼
 
**2 start** **与** **post** **不匹配**  
必须匹配：  
post￼start￼complete￼wait￼  
否则：  
程序卡死￼
 
**3 displacement** **单位错误**  
window 创建：  
disp_unit = sizeof(int)￼  
因此：  
displacement = index￼  
不是字节数。
 
**4 window size** **不一致**  
如果某进程没有 window：  
size = 0￼base = NULL￼
 
**5** **忘记释放资源**  
MPI_Win_free￼MPI_Group_free￼
 
**十三、考试最可能问的** **5** **个问题**  
**Q1**  
什么是 window？  
答：  
MPI 允许远程访问的内存区域￼
 
**Q2**  
什么是 origin 和 target？  
origin = 发起 RMA 的进程￼target = 被访问的进程￼
 
**Q3**  
GATS 四个 API？  
MPI_Win_post￼MPI_Win_start￼MPI_Win_complete￼MPI_Win_wait￼
 
**Q4**  
complete 和 wait 的区别？  
complete -\> origin 完成￼wait -\> target 完成￼
 
**Q5**  
为什么需要 group？  
指定允许访问 window 的进程集合￼
 
**十四、最重要的一句话总结（考试用）**  
One-sided communication uses **windows and epochs** to allow remote memory access.  
In the GATS model, **targets control exposure epochs (post/wait)** and **origins control access epochs (start/complete)**, enabling fine-grained synchronisation without global barriers.
 
如果你愿意，我可以再给你一份 **“EPCC 45** **分钟闭卷测验版** **One-sided** **通信速记表****”**，基本把 **90%** **可能出现的题型**（包括陷阱题）全部压缩到 **两页笔记**。
 \> 来自 \<[https://chatgpt.com/g/g-p-697bedcd2a608191bd4f46274b4a3e7c-ampp/c/69a80c79-46b8-8386-9289-23c7e314acf3](https://chatgpt.com/g/g-p-697bedcd2a608191bd4f46274b4a3e7c-ampp/c/69a80c79-46b8-8386-9289-23c7e314acf3)\>
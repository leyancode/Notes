**Parallel Communication Primitives****￼**
 
**并行通信原语**  
**课程：****Design and Analysis of Parallel Algorithms**  
**主题：****Primitive Communication Operations****（通信基本操作）**
 
**1. Primitive Communication Operations**  
**基本通信操作****￼**

![Exported image](Exported%20image%2020260730180028-0.png)

许多并行算法都基于一组常见的**通信基本操作（****primitive collective communication operations****）**，例如 **broadcast****（广播）**、**scatter****（分发）** 和 **gather****（收集）**。  
这些操作构成了并行计算中最基础的通信模式。

- **“****先有鸡还是先有蛋？****”****（****Chicken and egg****）** —— 是这些通信原语先被设计出来，还是使用它们的算法先出现？
- ==我们可以将这些通信操作实现为一个==**通用库（****library****）**==，为不同体系结构（====architecture====）提供高效实现。==
- ==这样做可以让并行算法具有一定的==**机器独立性（****machine independence****）**==，避免重复造轮子（====reinventing the wheel====）==。
- 同时也提高了算法在不同平台上获得良好性能的可能性。
 
**2. Common Communication Patterns****￼**

![Exported image](Exported%20image%2020260730180029-1.png)

**常见的通信模式**

|   |   |   |
|---|---|---|
|**通信模式**|**英文说明**|**中文说明**|
|**Broadcast**|One-to-all|一个进程向所有其他进程发送相同的数据。|
|**Scatter**|One-to-all (personalised)|一个进程向所有其他进程发送不同的数据。|
|**Gather**|All-to-one|所有进程各自向一个进程发送不同的数据（与 scatter 相反）。|
|**All-to-all (all broadcast/all gather)**|All processes send data to all others (same data)|所有进程都向所有其他进程发送相同的数据。|
|**All-to-all (all scatter)**|All processes send data to all others (different data)|所有进程都向所有其他进程发送不同的数据。|
 
**3. Communication Primitives – Cost**  
**通信原语的代价分析****￼**

![Exported image](Exported%20image%2020260730180138-2.png)

我们关心以下几个问题：

- 在 **PRAM** **模型（****Parallel Random Access Machine****）** 下，这些通信原语的代价是多少？
- 在 **message passing** **模型（消息传递模型）** 下，它们的代价又是多少？
- 它们在什么条件下可以构成**代价最优（cost-optimal）**的算法？
 
**简单（****Naïve****）实现的代价——****one to all**

![Exported image](Exported%20image%2020260730180139-3.png)

一种直接的做法是让一个进程发送 ( p-1 ) 条消息，每条消息大小为 ( m )：

- 每个消息都是相同的数据，但发送到不同的进程。
- 网络拓扑（network topology）对性能影响极大。
 
**Case 1: Fully Connected Network**  
**完全连接网络**  
如果所有进程之间都直接相连：

![Exported image](Exported%20image%2020260730180141-4.png)

**Case 2: Ring Topology**  
**环形拓扑网络（****ring topology****）**

- 每条消息必须经过中间进程（可能这些进程已经拥有数据）。
- 最远的消息需要经过 ( l = \Theta(p) ) 条链路。
- 因此整个广播的耗时为：
- ![Exported image](Exported%20image%2020260730180142-5.png)

⚙️ 尽管如此，即使在环形拓扑下，我们仍然可以采用更高效的算法来改进广播性能。
   

**4. One-to-all Broadcast**  
**一对多广播（****One-to-all Broadcast****）****￼**

![Exported image](Exported%20image%2020260730180144-6.png)

在并行计算中，**广播（****broadcast****）** 是一种常见的通信操作。它的目标是让一个“根进程（root process）”将同一条消息高效地发送给所有其他进程。以下介绍了两种常见的实现方式：**Store-and-Forward****（存储转发）** 与 **Cut-Through****（直通传输）**。
 
**(a) Store-and-Forward (SF)****￼**

![Exported image](Exported%20image%2020260730180145-7.png)

**存储转发方式****￼****所以事实上。他****是圆环我们才能这么做，通过减半来操作，如果是一条线的话就不能通过****S&F**

- “==根====”====进程（====root process====）在环形网络（====ring topology====）中==**向两个方向同时发送消息**==。==
- 这种方式适用于 **torus****（环形网格）** 结构，但**不适用于 mesh（普通网格）**结构。
- 该方法可以将==最远距离==**缩短一半（****Halves the longest distance****）**==。==
- 每个接收进程在接收到消息后，**先复制（copy）消息，再将其转发（forward）**给下一个进程。
- 这种策略被称为 **Store-and-Forward****（****SF****）**，即**“接收-存储-再发送”**。

![Exported image](Exported%20image%2020260730180146-8.png)

其中：

![Exported image](Exported%20image%2020260730180204-9.png)

因此：

![Exported image](Exported%20image%2020260730180205-10.png)

✅ **要点总结：**  
Store-and-Forward 的通信代价与进程数 (p) 成线性关系，在大规模系统中效率较低。
 
**(b) Cut-Through Routing****￼**

![Exported image](Exported%20image%2020260730180206-11.png)

**直通路由方式**

- “根”进程通过 **Cut-Through Routing****（直通路由）** 同时向约 log p 个其他进程发送消息。
- 每个接收进程在接收的同时，**不必等待整个消息接收完成**即可**直接转发（forward）**下一段数据。
- 这使得数据流可以**连续传输（****pipeline-like****）**，大大减少整体延迟。
- 每个接收进程在转发的同时也会复制一份消息。
- 此方法可以将 **hypercube****（超立方体）** 的直接链路映射（map）到 **ring****（环形网络）** 的 cut-through 链路上，实现高效通信。

📘 **性能模型（****Cost Model****）：**

![Exported image](Exported%20image%2020260730180208-12.png)

✅ **要点总结：**  
Cut-Through Routing 显著降低了通信延迟，使广播的复杂度从线性 (O(p)) 降到对数级 (O(\log p))，在大规模并行系统中更高效。
 
**🔍** **对比总结**

|   |   |   |   |   |
|---|---|---|---|---|
|**实现方式**|**核心机制**|**网络结构要求**|**时间复杂度**|**特点**|
|**Store-and-Forward (SF)**|接收后再转发|适用于 torus，不适用于 mesh|( \Theta(m p) )|简单、易实现，但效率较低|
|**Cut-Through Routing**|边接收边转发|可映射到 hypercube 或 ring|( \Theta(m \log p) )|高效、延迟低，适合大规模系统|

非常好，这一段内容属于并行通信中 **broadcast****（广播）** 与 **all-to-all broadcast****（全互广播，也称** **all-gather****）** 的不同拓扑实现部分。下面我已为你整理成清晰的**中英双语课堂笔记格式**，保持了你之前笔记的风格（分节、表格、公式、要点总结），关键英文术语如 _mesh_、_torus_、_hypercube_、_all-gather_、_ring_ 等均保留英文原文。
 
**5.** **One-to-all Broadcast — Mesh / Torus****￼**

![Exported image](Exported%20image%2020260730180209-13.png)  
![Exported image](Exported%20image%2020260730180209-14.png)  
![Exported image](Exported%20image%2020260730180211-15.png)  
![Exported image](Exported%20image%2020260730180240-16.png)

**￼****￼****一对多广播：网格（****Mesh****）与环形网格（****Torus****）****￼****一、图里到底发生了什么？**  
右边 Figure 3.4 是 **4×4 mesh****（二维网格）**，节点编号如下：￼

![Exported image](Exported%20image%2020260730180241-17.png)

Root 在左下角节点 **0**。  
Mesh 只有四个方向 (up/down/left/right)，没有 wrap-around（行列不连成环）。  
Torus 则是每行每列都是环。  
**二、广播策略为何是****“****先行、后列****”**  
网格是一个二维结构，所以最自然的广播方式是：  
**第一步：在源节点所在的那一行进行广播**  
也就是 **最底下一行：****0 → 1 → 2 → 3**  
图中箭头标出的数字是“时间步”，表示消息依次沿着横向传播。  
到这步结束后：

![Exported image](Exported%20image%2020260730180242-18.png)

**第二步：每列同时向上传播广播**  
例如：

- 第 0 列：0 → 4 → 8 → 12
- 第 1 列：1 → 5 → 9 → 13
- 第 2 列：2 → 6 → 10 → 14
- 第 3 列：3 → 7 → 11 → 15

图中你能看到每列都有向上传播的箭头。  
这样所有节点都会收到消息。  
**三、重点：为什么这样做是最优的？**  
因为 mesh 是二维结构，它的最短路径必然由：

- 沿 **x** 方向走若干步（行方向）
- 然后沿 **y** 方向走若干步（列方向）

所以无论如何，总 hops ≈ 横向距离 + 纵向距离  
广播自然分成这两部分完成，而且是最高效的方式。

![Exported image](Exported%20image%2020260730180242-19.png)  
![Exported image](Exported%20image%2020260730180243-20.png)  
![Exported image](Exported%20image%2020260730180244-21.png)

￼￼  
在二维 **mesh/torus** 网络中，一对多广播可以分为两个阶段进行：

1. **第一阶段（****Row Broadcast****）**：￼沿源进程所在的行（source row）进行广播。
2. **第二阶段（****Column Broadcast****）**：￼在所有列上**并行（concurrently）**进行广播。
 
**参数设定**

- 每条边（edge）上有 ( d \sqrt{p} ) 个进程￼（假设是“正方形”形状的 mesh/torus 网络）
- ( d )：维度（dimension）
- ( p )：总进程数（number of processes）
   
![Exported image](Exported%20image%2020260730180245-22.png)

✅ **要点总结：**

- Mesh 与 Torus 的广播时间均随 (\sqrt{p}) 线性增长。
- Torus 因为双向环连结构，通信距离平均缩短一半。
- 该方法具有较好的可扩展性，适合在二维或更高维度拓扑上实现广播。
 
**6. One-to-all Broadcast — Hypercube****￼**

![Exported image](Exported%20image%2020260730180341-23.png)

**一对多广播：超立方体（****Hypercube****）**

- 一个具有 ( 2^d ) 个处理器的 **hypercube** 可以看作一个 **d** **维网格（****mesh****）**，￼每个维度上有 2 个处理器。
- 我们可以将二维 mesh 广播算法推广到 **d** **维** **hypercube**。
 ![Exported image](Exported%20image%2020260730180342-24.png)

**One-to-all Broadcast — Hypercube (Pseudo-code)****￼**

![Exported image](Exported%20image%2020260730180343-25.png)

下面我逐行把这段 **超立方体（****hypercube****）一对全广播算法** 的代码解释清楚。  
我会用 d=log₂p、节点编号的二进制表示、以及 AND/XOR 的含义，帮你完全理解。  
我会严格按照你的截图里的代码结构讲解。
 
**1.** **代码整体思路（先讲大方向）**  
在一个 d 维超立方体中：

- 共有 p = 2^d 个节点
- 每个节点用 d 位二进制编号（如 0101）
- 每一轮（iteration）只在一个维度上通信，通信对象是修改该维度的一位后得到的节点（也就是 XOR 一个 2^i）

算法运行 d = log₂p 轮，每一轮扩散一倍的节点数，因此最终完成 broadcast。
 
**2.** **程序结构（对应代码逐行解释）**  
**1–2** **行**  
mask := 2^d – 1 /* Set all bits of mask to 1 */￼for i := d – 1 downto 0 do /* Outer loop */  
mask 一开始是全 1（比如 d=4，mask=1111₂）。  
外层循环做 d 次，每次处理一个维度（从最高位到最低位）。  
也就是一次处理一个 bit。  
**3.** **关键：每轮的** **mask / XOR** **的用途**  
继续看代码：  
**4–5** **行**  
begin￼ mask := mask XOR 2^i /* Set bit i of mask to 0 */  
例如 d=4，mask=1111，i=2  
mask XOR 0100 = 1011  
把这一位变成 0。  
mask 控制“我是不是在这一轮已经是接收方”。  
**4.** **判断自己是发送者还是接收者**  
**6–7** **行**  
if (my_id AND mask) = 0 then  
意思是：  
如果我的 my_id 在 mask 为 1 的那些位里，全都是 0  
→ 我在这一轮属于消息已知的那一半  
→ 我要发送消息给别人  
如果不是 0，就属于“还没收到消息的那一半”，本轮将接收。  
因此：

- 低编号节点先收到消息，因此它们是发送者
- 高编号节点逐轮收到消息，因此它们是接收者

**5.** **确定通信的伙伴（****XOR** **找邻居）**  
**8–10** **行（****sender****）**  
if (my_id AND 2^i) = 0 then /* bit i is 0 */￼begin￼ msg_destination := my_id XOR 2^i￼ send X to msg_destination￼end  
如果我是发送者，并且第 i 位是 0，那么：

- 我将第 i 位翻转（XOR 2^i）得到邻居
- 将消息发给邻居

例子：my_id = 0001，i=2  
0001 XOR 0100 = 0101  
我送消息给 5 号节点。  
**6.** **接收者（****receiver****）**  
**12–14** **行**  
else begin￼ msg_source := my_id XOR 2^i￼ receive X from msg_source￼end  
接收方也是通过翻转第 i 位确定消息来源。  
例如：my_id = 0101（5号节点），i=2  
0101 XOR 0100 = 0001  
从 1 号节点收消息。  
**7.** **整体广播过程示例（非常关键）**  
假设 p=8, d=3：  
节点编号（3-bit）：  
000 001 010 011 100 101 110 111  
**第** **1** **轮（****i = 2****，对应** **bit2****）**  
000、001、010、011（低四个）在 mask=011 下满足 sender  
它们发送给 flip bit2（翻转最高位）后的邻居：  
000→100 ￼001→101 ￼010→110 ￼011→111  
现在所有节点都有消息。  
**第** **2** **轮（****i = 1****，对应** **bit1****）**  
最低两个（000、001）给 (010,011)  
以及 (100,101) 给 (110,111)  
**第** **3** **轮（****i = 0** **对应最低位）**  
000→001  
010→011  
100→101  
110→111  
经过 d=3 轮广播全部完成。  
**8.** **为什么时间复杂度是** **log₂p****？**  
每轮只有一条链路参与，每个节点只收发一次。  
运行 d 次：  
Tp = d (ts + tw m)
 ![Exported image](Exported%20image%2020260730180344-26.png)

**￼****￼****￼****￼****7. All-to-all Broadcast (All-Gather) — Ring**  
**全互广播（****All-Gather****）：环形拓扑**  
**定义：**

![Exported image](Exported%20image%2020260730180345-27.png)

每个进程都有一条长度为 (m) 的独立消息，需要广播给所有其他进程。  
等价地说，每个进程需要**收集所有进程的数据（****gather all values****）**。￼￼**问题定义：****All-to-All Broadcast**  
每个处理器都有一个大小为 **m** 的消息，要发送给所有其它处理器。  
等价地说：

- 每个处理器最终要收到 **p − 1** 条消息（来自其他所有节点）

如果你用最朴素方式：  
p 次单独的 one-to-all  
→ 时间 = p × one-to-all 的时间  
→ 太慢了￼  
**流水线示意（最重要）**  
以 p = 4 为例：

![Exported image](Exported%20image%2020260730180347-28.png)

**轮次** **0****：**

- 0 → 1 发送 M0
- 1 → 2 发送 M1
- 2 → 3 发送 M2
- 3 → 0 发送 M3

每个链路都被用上。  
**轮次** **1****：**

- 0 从 3 得到 M3、再把它发给 1
- 1 从 0 得到 M0、再发给 2
- 2 从 1 得到 M1、再发给 3
- 3 从 2 得到 M2、再发给 0

**轮次** **2****：**  
消息继续前进一格  
……  
这样一共需要 **p – 1** **轮**：

- 每个消息走完整个环一圈
- 每轮发送一次
- 因为 pipeline，p−1 轮就能完成所有消息的分发￼
- ![Exported image](Exported%20image%2020260730180348-29.png)
- ==为什么说====“====只是== ==one-to-all== ==的两倍==”￼￼
- ![Exported image](Exported%20image%2020260730180411-30.png) - ![Exported image](Exported%20image%2020260730180412-31.png)
-   
    
    
**关键挑战****￼**

![Exported image](Exported%20image%2020260730180413-32.png)

目标是找到比执行 (p) 次单独 one-to-all broadcast 更高效的算法。  
在 **ring****（环形拓扑）** 中，我们可以：

- 利用单向广播中**未使用的链路（****unused links****）**，
- 实现类似**流水线（pipelined）**的并行通信效果。
 
**性能模型（****Cost Model****）**

![Exported image](Exported%20image%2020260730180414-33.png)

✅ **要点总结：**

- 该算法需要 (p - 1) 步，每步都利用不同链路实现并行数据传输。
- 效率比执行 (p) 次独立广播高得多，仅约为单次广播时间的两倍。
 
**8. All-to-all Broadcast (All-Gather) — Mesh****￼**

![Exported image](Exported%20image%2020260730180415-34.png)

**全互广播：网格拓扑（****Mesh****）**

- ==假设一个二维== **mesh** ==网络（可扩展到更高维度）。==
- 整个通信过程分为两个阶段：
   

==注意这里是二维==，所以是根号下p,也就是一行的节点数n  
**阶段** **1****：行方向通信（****Row phase****）**

- 每一行独立地执行 ring-based all-to-all broadcast。
- 时间为：
- ![Exported image](Exported%20image%2020260730180416-35.png)

**阶段** **2****：列方向通信（****Column phase****）**

- 所有列并行执行与阶段 1 类似的广播。
- 但每个消息大小为 (m \sqrt{p})，因此时间为：
- ![Exported image](Exported%20image%2020260730180417-36.png)

**总体通信时间**

![Exported image](Exported%20image%2020260730180459-37.png)

✅ **要点总结：**

- 两个阶段相加，性能约为 one-to-all broadcast 的两倍。
- 由于链路带宽限制（link capacity），这是**渐进下界（****asymptotic lower bound****）**。
 ![Exported image](Exported%20image%2020260730180500-38.png)

非常好，这一部分介绍的是 **One-to-all Personalised communication****（即** **Scatter** **操作）**，也就是广播的“个性化”版本。下面我为你整理成与前面一致的**中英双语笔记格式**，带有公式、结构分析和拓扑对比表，方便直接加入你的课堂整理文档。
 
**All-to-all Broadcast – Hypercube**  
（全广播通信 —— 超立方体结构）

![Exported image](Exported%20image%2020260730180501-39.png)

**7****Concept Overview**  
（概念概述）

- **Hypercube** **版本（****Hypercube version****）** 是从 **mesh****（网格）版本** 推广而来的。
- ==若系统中有== ==( p = 2^d )== ==个处理器（====processors====），====￼====则该== **d** **维超立方体（****d-dimensional hypercube****）****￼**==可视为一个在每个方向上都有== **2** **个处理器** ==的== **d** **维网格（****mesh****）**==。==
 
**Communication Phases**  
（通信阶段）

- 全广播（all-to-all broadcast）在超立方体中分为 **d = log p** **个阶段（****phases****）**。
- 在第 ( i ) 个阶段（phase ( i )）中，￼每个处理器会发送 **1** **条消息（****message****）**，￼该消息的大小为：
- ![Exported image](Exported%20image%2020260730180502-40.png)
- 其中 ( m ) 为单条原始消息大小。
 
**Total Communication Time**  
（总通信时间公式）

![Exported image](Exported%20image%2020260730180503-41.png)

**Asymptotic Complexity**  
（渐近复杂度）

![Exported image](Exported%20image%2020260730180504-42.png)

**解释：**

- ==启动延迟（====startup latency====）随着阶段数增加呈对数增长；==
- ==消息传输量（====message volume====）随处理器数量线性增长；==
- ==整体复杂度受通信数据量主导。==
 
**Summary**  
（小结）

- 超立方体拓扑能有效支持 **并行广播（****parallel broadcast****）** 操作；
- 每轮通信使得消息数据量指数级增长；
- 相比网格结构，Hypercube 能在更少阶段内完成全广播；
- 但在大规模系统中，**消息扩散量（****message expansion****）** 导致总体通信时间仍为 **Θ(m p)**。
   

**9. One-to-all Personalised Communication — Scatter**  
**一对多个性化通信（****Scatter****）**

![Exported image](Exported%20image%2020260730180505-43.png)

**📘** **概念简介**  
**Scatter****（分发）** 是 **broadcast****（广播）** 的“个性化”版本：

- 源处理器（source processor）拥有一组数据，每个目标进程（destination process）都应接收不同的消息。
- 因此，Scatter 是一种 **one-to-all personalised communication****（个性化一对多通信）** 操作。
 
**性能下界（****Lower Bound****）**

![Exported image](Exported%20image%2020260730180531-44.png)

**🕒** **性能分析（****Performance by Topology****）**  
**1. Ring Topology —** **环形拓扑**

![Exported image](Exported%20image%2020260730180532-45.png)

- 每次传输一个块，每个进程接收到自己对应的数据块。
- 由于每条链路顺序传输，时间与进程数线性增长。
 
**2. Mesh Topology —** **网格拓扑**

![Exported image](Exported%20image%2020260730180533-46.png)

- 分阶段进行（先沿行广播，再沿列分发）。
- 尽管拓扑复杂，但渐进复杂度仍然与线性模型一致。
 
**3. Hypercube Topology —** **超立方体拓扑**

![Exported image](Exported%20image%2020260730180534-47.png)

- 拥有 ( \log_2 p ) 个阶段（phases）。
- 每个阶段中：
    - 一部分进程发送（send），另一部分进程接收（receive）；
    - 每阶段发送的消息尺寸（message size）会**逐步减小**（halve each round）。
 
**Scatter in Hypercube — Phase-based Structure****￼**

![Exported image](Exported%20image%2020260730180535-48.png)

**超立方体** **Scatter** **的阶段结构**

|   |   |   |   |
|---|---|---|---|
|**阶段** **(Phase)**|**参与进程数**|**消息大小**|**操作说明**|
|第 1 阶段|1 → 1|全部数据|源进程将一半数据发送给 partner|
|第 2 阶段|2 → 2|½ 数据|各自向另一半发送|
|…|…|…|…|
|第 log p 阶段|p/2 → p/2|1/p 数据|所有进程均已拥有自己的消息|

⚙️ 这种分阶段的结构使得通信保持高效并充分利用并行带宽。
 
**⚠️** **注意事项**

- 上述分析均假设使用 **SF****（****Store-and-Forward****）** 路由方式。
- 如果使用 **CT****（****Cut-Through****）** 路由，通信代价将显著降低。
- 阅读教材或论文时，应**注意上下文中使用的路由模型**（SF 或 CT），以免混淆复杂度公式。
 
**📊** **小结与比较****￼**

![Exported image](Exported%20image%2020260730180536-49.png)  

|   |   |   |   |
|---|---|---|---|
|**拓扑结构**|**通信阶段数**|**时间复杂度**|**特点**|
|Ring|(p - 1)|( \Theta(m p) )|简单线性结构|
|Mesh|2 阶段（行 + 列）|( \Theta(m p) )|分层传输|
|Hypercube|(\log p)|( \Theta(m p))|分阶段减半传输|
|—|—|**下界：** (\Omega(t_w m (p - 1)))|源端最小可达代价|

![[Lec9 Communication_Primitives - Ink.svg]]

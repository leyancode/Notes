**The LogP Model**  
**Design and Analysis of Parallel Algorithms**  
（并行算法的设计与分析）
 
**Papers for the Course So Far**  
（课程至今涉及的论文）  
[1] **S. Fortune and J. Wyllie (1978)**  
“Parallelism in random access machines,” _Proceedings of the 10th Annual ACM Symposium on Theory of Computing (STOC ’78)_, pp. 114–118.  
提出了早期 **RAM****（随机访问机器）** 的并行扩展思想，为并行计算模型奠定理论基础。  
[2] **P. B. Gibbons (1989)**  
“A more practical PRAM model,” _Proceedings of the 1st ACM Symposium on Parallel Algorithms and Architectures (SPAA ’89)_, pp. 158–168.  
改进了 PRAM 模型，使其更贴近实际计算系统。  
[3] **C. P. Kruskal, L. Rudolph, and M. Snir (1990)**  
“A complexity theory of efficient parallel algorithms,” _Theoretical Computer Science_, vol. 71, no. 1, pp. 95–132.  
建立了并行算法的复杂性理论，为衡量效率提供了基础框架。
 
**Papers Published Afterwards**  
（后续发展文献）  
[4] **D. Culler et al. (1993)**  
“LogP: towards a realistic model of parallel computation,” _PPoPP ’93_, vol. 28, no. 7, pp. 1–12.  
提出了著名的 **LogP** **模型** —— 一个更贴近真实并行系统的通信模型。  
[5] **A. Alexandrov et al. (1995)**  
“LogGP: incorporating long messages into the LogP model,” _SPAA ’95_, pp. 95–105.  
在 LogP 基础上加入了 **长消息（****long messages****）** 的处理机制，形成 **LogGP** **模型**。  
[6] **F. Ino et al. (2001)**  
“LogGPS: a parallel computational model for synchronization analysis,” _PPoPP ’01_, vol. 36, no. 7, pp. 133–142.  
进一步扩展了模型，用于 **同步分析（****synchronization analysis****）**。  
[7] **T. Hoefler et al. (2010)**  
“LogGOPSim – Simulating Large-Scale Applications in the LogGOPS Model,” _HPDC ’10_, p. 597.  
引入 **LogGOPS** **模型**，可用于模拟大规模高性能计算应用。
 
**Recall – PRAM Setup / Assumptions**  
（回顾：PRAM 模型的设定与假设）

![Exported image](Exported%20image%2020260730181504-0.png)

**PRAM****（****Parallel Random Access Machine****）** 是一种**理想化的共享内存计算模型**。  
其主要假设如下：

- **p processors****（处理器）**==：所有处理器同步运行（====synchronised====）。==
- **m memory locations****（内存单元）**==：每次全局访问的代价固定且为单位时间（====unit cost====）。==
- **任何操作（****computation or communication****）** ==的时间开销均为== ==1==。

**内存访问冲突的解决方式（****Memory Clash Resolution****）**：

- **EREW** – _Exclusive Read, Exclusive Write_（独占读写）
- **CREW** – _Concurrent Read, Exclusive Write_（并发读，独占写）
- **CRCW** – _Concurrent Read, Concurrent Write_
    - 常见的冲突解决策略包括：
        - **Common**（公共写）
        - **Arbitrary**（任意写）
        - **Priority**（优先级写）
        - **Associative**（结合写）
 
**Recall – Message-Passing SF vs. CT**

![Exported image](Exported%20image%2020260730181505-1.png)

￼￼mesh:￼当任意切断两个链接，网络仍将保持联系￼它有每个邻居的恒定数量 constant numbers of neigbors for each￼￼hypercube:￼非常快，易于用每个节点的二进制表示￼  
we conveniently can use the representation, the  
binary representation of each node for routing.
 
￼（回顾：消息传递模型中的 SF 与 CT）

![Exported image](Exported%20image%2020260730181507-2.png)

在消息传递系统中：

- 每条消息都会产生一个**固定的启动开销（****fixed start-up cost****）**；
- **大消息（****big messages****）** 比 **小消息（****short messages****）** 需要更长的传输时间。
 
**Store-and-Forward (SF)**  
（存储-转发机制）

- 整条消息必须**完全到达中间节点后**才能继续转发到下一个节点。

![Exported image](Exported%20image%2020260730181508-3.png)

**Cut-Through (CT)**  
（直通转发机制）

- 消息的不同部分（bits）可在**不同链路上同时传输**。
- 不必等待整条消息到达再转发下一段。
- **通信时间公式：**
- ![Exported image](Exported%20image%2020260730181509-4.png)
- **表明传输时间与消息长度呈线性关系，但不再受路径长度** **( l )** **的乘性影响****。**
 
**LogP – Abstract**  
（LogP 模型概述）

![Exported image](Exported%20image%2020260730181510-5.png)

早期的并行计算模型存在两种极端问题：

- **过于理想化（****overly simplistic****）** ==——== ==典型如== ==PRAM== ==模型，忽略了通信成本；==
- **过于具体（****overly specific****）** ==——== ==难以代表真实系统。==

**LogP** **模型的目标：**  
提出一个**更现实的并行机器模型（****a realistic model of parallel computation****）**，  
在**细节（detail）**与**简化（simplicity）**之间取得平衡，  
从而揭示系统中的关键瓶颈而不使分析变得复杂难解。  
**四个关键参数（****four parameters****）**：

- ==计算带宽== ==(==**computing bandwidth**==)==
- ==通信带宽== ==(==**communication bandwidth**==)==
- ==通信延迟== ==(==**communication delay**==)==
- ==通信与计算耦合的效率== ==(==**efficiency of coupling communication and computation**==)==
 
**LogP – View of Computer Architecture**  
（LogP 模型下的计算机体系结构视角）

![Exported image](Exported%20image%2020260730181525-6.png)

（图示幻灯片说明：LogP 将整个并行系统抽象为若干节点，每个节点具备计算、存储和网络通信能力，由统一延迟与带宽参数刻画。）￼  
它展示的是：  
**现代大规模并行计算机（****MPP****：****Massively Parallel Processors****）**  
**的典型硬件结构。**  
这类机器包括：

- Intel iPSC
- Intel Delta
- Intel Paragon
- Thinking Machines CM-5
- Cray T3D
- Meiko Computing Surface
- Parsytec GC￼等 HPC（高性能计算）大型并行机。

图反映了**几乎所有分布式内存并行机的基本结构**。  
**1****）****Microprocessor****（微处理器）**  
每个节点都包含一个 CPU（可能是当时的 RISC / MIPS / Alpha / SPARC）。  
每个 CPU 独立运行程序。  
**2****）****Cache Memory****（缓存）**  
处理器本地的高速缓存，用于减少访问 DRAM 的延迟。  
每个节点都有自己的 cache（没有共享内存）。  
**3****）****DRAM Memory****（本地主存）**  
每个节点都有独立的 DRAM（主存），  
**其他节点无法直接访问**—这就是“分布式内存架构”。  
这和 PRAM 完全不一样。  
**4****）****Network Interface****（网络接口）**  
每个节点通过网络接口卡（NIC）连接到互联网络。  
MPI 程序中的 send/recv 就是通过这东西实现的。  
**5****）****Interconnection Network****（互连网络）**  
图右边的椭圆表示**节点之间的网络结构**。  
网络拓扑可能是：

- Mesh（二维网格）
- Hypercube（超立方体）
- Fat-tree（胖树）
- Torus（环面）
- Butterfly（蝴蝶网络）
- 等等

这些拓扑决定通信瓶颈、延迟、带宽。
 
**图表达的核心思想（****1** **句话总结）**
 
**MPP =** **多个带私有内存的处理器，通过高速网络互联，靠显式通信进行协作的并行系统。**  
==这正是== ==MPI== ==程序运行的架构。==
 ![Exported image](Exported%20image%2020260730181526-7.png)  

**LogP – What’s Different, Why Change?**  
（LogP 有何不同，为什么需要变革？）

![Exported image](Exported%20image%2020260730181527-8.png)

随着**商用硬件（commodity hardware）**的发展，  
==并行系统的趋势发生了根本变化==：

- 每个节点类似于一台**独立工作站（****stand-alone workstation****）**；
- **大众市场（****mass market****）** ==需求改变了优化方向：==
    - ==处理器速度（====processor speed====）== ==\>== ==网络速度（====network speed====）==
    - ==内存容量（====memory capacity====）== ==\>== ==内存速度（====memory speed====）==
- **关键论断（****Assertions****）：**
    1. 节点数量不会扩展到上百万规模；
    2. 网络延迟（latency）高，带宽（bandwidth）低；
    3. 系统需要支持自适应路由（adaptive routing）以应对性能与故障问题。

因此，算法分析必须考虑这些现实因素，  
并假设每个处理器需处理**大量数据元素（****many data elements per processor****）**。
 
**LogP – The New Machine Model**  
（LogP：新的并行机器模型）

![Exported image](Exported%20image%2020260730181529-9.png)

LogP 模型用于描述**网络通信性能特征（****network performance characteristics****）**，  
但**不关心具体的网络结构（****topology****）**。  
**模型参数定义：**

|   |   |   |
|---|---|---|
|**参数**|**含义**|**说明**|
|**L**|**Latency**|网络通信延迟的上界（upper bound on network latency）|
|**o**|**Overhead**|每次发送或接收消息产生的处理器开销（processing overhead per message）|
|**g**|**Gap**|连续两条消息之间必须间隔的最短时间（minimum time gap between messages）|
|**P**|**Processors**|系统中的处理器数量（number of processors）|

==⚠️== ==注意：此处的== **L ≠ tₛ**==（启动时间）！==  
==实际上，==**tₛ ≈ L + 2 × max(o, g)**==。==
 
以下是这一部分幻灯片的中英双语整理笔记版本，延续前面相同格式，保留关键英文术语（如 _hypercube, hops, diameter, latency, overhead, topology_ 等）以保持学术准确性。
 
**LogP – Example with Diagram**  
（LogP 示例与图解）
 
**Comparison with PRAM**  
（与 PRAM 模型的对比）

- **问题：** LogP 与 PRAM 有何不同？
- 在 **PRAM** **模型** 中，==我们并未假设特定的网络拓扑结构（====topology====）==。
- 而 **LogP** **模型** 强调网络通信的实际代价，因此网络结构起作用。
- 在常见的拓扑中，最接近我们之前讨论的模式是 **hypercube****（超立方体网络）**。

![Exported image](Exported%20image%2020260730181530-10.png)

**. “Each operation by a processor takes unit time: a cycle”**  
每个处理器执行自己的指令用 1 个周期 —— 这与 PRAM 类似。  
**解释**：  
LogP 假设处理器很快、代价可以忽略不计，但通信代价不能忽略。  
**意义**：  
算术操作 vs 通信操作  
→ 通信才是瓶颈。
 
**2. “Other parameters are expressed in cycles”**  
所有时间单位都用“周期”衡量，包括 L、o、g。  
**解释**：  
整个 LogP 模型统一用一个时间尺度，方便分析。
 
**3. “The network always does store-and-forward routing”**  
网络使用“存储-转发”路由。  
数据必须在每个节点 **收下** **→** **再发出**。  
**解释**：  
真实的 HPC 网络并不是瞬间把消息送到目标，而是逐跳传递。  
每跳要花 o、L 这样的成本。  
**区别** **PRAM**：  
PRAM 中通信没有成本，而 LogP 强调通信开销。
 
**4. “The network has a finite capacity”**  
==网络带宽有限，不能无限同时传消息。==  
**完整解释：**

- ==在任意一条链路上，最多只能有== **L/g** ==条消息正在====“====飞行====”====（====in flight====）==
- ==如果你发送太多消息，会阻塞（====stall====）==

==例如：==

- ==L = 100 cycles====（消息延迟）==
- ==g = 10 cycles/word====（消息之间需要间隔）==

==你最多能并行传== ==L/g = 10== ==条消息==  
再多就会卡住。  
**意义**：  
分布式系统无法像 PRAM 那样让无限处理器同时通信。  
通信是瓶颈。
 
**5. “Processors are asynchronous, i.e., not in lock-step”**  
处理器是 **异步** 的，不同时钟，不同步执行。  
与 PRAM 的区别：

- PRAM 中所有处理器 **同步执行指令** **i → i+1**
- 分布式系统中不同处理器可能：
    - 提前完成
    - 等待消息
    - 阻塞
    - 延迟不同

这才符合真实 HPC 系统。
 
**6. “Some messages can arrive out of order”**  
消息可能乱序到达。  
**解释：**  
在真实网络中，同一源节点发送的两条消息，可能因为路由不同、拥塞不同而：

- 后发先到
- 先发后到

**PRAM** **永远不会出现这种情况**  
因为 PRAM 不考虑通信。  
**LogP** **强调网络不可靠性和乱序，使模型更接近** **MPI** **程序的实际行为。**

![Exported image](Exported%20image%2020260730181531-11.png)   ![Exported image](Exported%20image%2020260730181532-12.png)

￼￼

![Exported image](Exported%20image%2020260730181601-13.png)  
![Exported image](Exported%20image%2020260730181602-14.png)  
![Exported image](Exported%20image%2020260730181603-15.png)   
**Hypercube Communication Example**  
（超立方体通信示例）
 ![Exported image](Exported%20image%2020260730181604-16.png)   
假设系统采用 **3** **维** **hypercube** 结构：

- 节点通信路径（3 hops）：￼P0 → {P4, P2, P1}￼P4 → {P6, P5}￼P6 → {P7}

![Exported image](Exported%20image%2020260730181605-17.png)

- **通信路径长度（****number of hops****）：****Θ(log p)**。
- **通信时间：**
- ![Exported image](Exported%20image%2020260730181607-18.png)
- **可见该通信速度相对较慢（****slower****）。**
 
==解释：：：==
 ![Exported image](Exported%20image%2020260730181608-19.png)  
![Exported image](Exported%20image%2020260730181634-20.png)  
![Exported image](Exported%20image%2020260730181635-21.png)  
![Exported image](Exported%20image%2020260730181636-22.png)  
![Exported image](Exported%20image%2020260730181637-23.png)

￼￼为什么要乘以 3 hops？￼￼￼

![Exported image](Exported%20image%2020260730181638-24.png)  
![Exported image](Exported%20image%2020260730181639-25.png)  
![Exported image](Exported%20image%2020260730181640-26.png)

**￼****Is This Communication Pattern Always Optimal?**  
（这种通信模式总是最优的吗？）  
在 LogP 模型下，不同拓扑结构的通信时间可以表示为：￼￼==一个是====mesh,====一个是====hypercube==

![Exported image](Exported%20image%2020260730181733-27.png)

- ==如果设定参数关系== **L \< o \< g**==，====￼====即表示一个== **MPI** **开销（****overhead****）** ==大于网络传输时间（====network transmission time====）的系统，====￼====那么通信瓶颈将主要由处理器开销决定，而非网络延迟==。￼￼就是**o \< g****，所以我们说****mpi** **overhead****比****network overhead****差**
- ![Exported image](Exported%20image%2020260730181734-28.png)
 
**LogP – Does Network Topology Matter?**  
（网络拓扑是否重要？）  
下图展示了不同网络拓扑下，**直径（****diameter****）** 随节点数量（number of nodes）变化的对比关系：

|   |   |
|---|---|
|**Network Topology**|**Diameter Growth**|
|2D Mesh|最慢，增长近似为 Θ(√p)|
|3D Mesh|中等增长 Θ(p^(1/3))|
|6D Mesh|更快接近 Θ(log p)|
|Hypercube|最优拓扑之一，Θ(log p)|

**图** **1****：** 不同网络拓扑的直径与节点数量关系图  
显示出随着维度增加，网络直径减小，通信路径缩短。
 
**LogP – Dragonfly ≈ Hypercube**

![Exported image](Exported%20image%2020260730181735-29.png)

（Dragonfly 网络与 Hypercube 的近似关系）  
进一步比较不同高性能计算系统的实际网络拓扑：

|   |   |   |
|---|---|---|
|**系统**|**拓扑类型**|**通信直径特征**|
|**ARCHER**|Dragonfly|类似 hypercube，低直径、高连通性|
|**CIRRUS**|Dragonfly|结构紧凑，通信高效|
|**K Computer**|6D Mesh|稍大直径，通信距离较远|

**图** **2****：** 常见 HPC 系统的网络直径与节点数量对比图  
Dragonfly 结构在性能上接近 Hypercube，但更易于物理实现与扩展。
 
**LogP – Summary**  
（LogP 模型总结）

![Exported image](Exported%20image%2020260730181736-30.png)

- **LogP** 是一种**更细致的并行通信模型（****a more detailed communication model****）**。
- 它使用四个关键参数：
    - **L** — Latency（通信延迟）
    - **o** — Overhead（处理器开销）
    - **g** — Gap（消息间隔）
    - **P** — Processors（处理器数）
- **特点：**
    - 忽略具体网络拓扑结构（topology-agnostic）；
    - 忽略消息长度（message size-independent）；
    - 能区分不同算法的**代价最优性（****cost-optimality****）**。
 
是否希望我继续整理下一节（通常是 _LogGP and LogGOPS extensions_ 或 _Implications for parallel algorithm design_）？
 \> 来自 \<[https://chatgpt.com/c/690c8731-4168-832d-9af4-92b8734718dc](https://chatgpt.com/c/690c8731-4168-832d-9af4-92b8734718dc)\>

![[Lec12 The logP model - Ink.svg]]

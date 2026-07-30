**Message Passing Parallelism****（消息传递并行性）****￼**

![Exported image](Exported%20image%2020260730175039-0.png)

- 一种最简单的方式是：￼**忽略网络拓扑（****ignore the network topology****）**，￼仅仅根据消息大小（message size）来建模进程间的通信成本。
- 这种方式假设：
    - 所有进程之间的通信成本相同（model all process-to-process interactions as equal）。
    - 时间仅依赖于消息大小，而与通信路径无关。
 
- 我们可以做得更好一些（we can do a little better）：￼将 **网络拓扑（****network topology****）** 考虑进来。
- 对于 **大规模网络（****large topologies****）**：
    - 网络结构可能会对算法的性能产生 **渐近影响（****asymptotic impact****）**。
    - 不同拓扑结构

**Network Topologies****（网络拓扑结构）****￼**

- 实际和理论中存在多种网络拓扑（network topologies）。
- 本课程重点讨论两种：
    - **Mesh****（网格）**
    - **Binary Hypercube****（二进制超立方体）****￼****￼**

- 可能对应不同的消息传递延迟。￼￼
- ![Exported image](Exported%20image%2020260730175041-1.png)
-   
    
![Exported image](Exported%20image%2020260730175042-2.png)  
![Exported image](Exported%20image%2020260730175044-3.png)

- **Mesh**
    - 有时会“wrap around”变成 **Torus****（环面结构）**。
    - 这种结构与矩阵计算（matrix-based computations）有天然的对应关系。
- **Binary Hypercube**
    - 与 **divide and conquer algorithms****（分治算法）** 有天然的对应关系。
    - 通过 **递归构造（****recursive construction****）** 形成更高维的拓扑结构。
 
- ==每个节点（====node====）通过固定模式与邻居相连，通常是二维或三维网格。==
- ==特点：==
    - **局部通信（****local communication****）** ==高效。==
    - ==节点度数较小（====degree of each node is small====）。==
    - ==适合矩阵计算（如矩阵乘法、====PDE====求解等）==。￼
    - ![Exported image](Exported%20image%2020260730175044-4.png)

==虚线说明了方向==，第二张图说明不同球球之间可以环绕，比如最左边和最右边，他们从后面绕过了中间的球￼

![Exported image](Exported%20image%2020260730175056-5.png)  
![Exported image](Exported%20image%2020260730175058-6.png)

￼￼  
**Network Topologies – Hypercube****（二进制超立方体）****￼**

![Exported image](Exported%20image%2020260730175058-7.png)     

- 通过 **递归构造（****recursive construction****）** 形成：
    - 1维：2个节点
    - 2维：4个节点
    - 3维：8个节点，以此类推。
- 特点：
    - **对数级（****logarithmic****）直径** ==→== ==路径短。==￼比如进程数为p,那么它的直径就是logp
    - 节点间最大跳数为 log2(N)。
    - 非常适合分治算法和并行递归通信模式。
- ![Exported image](Exported%20image%2020260730175100-8.png)
 
- ==精确建模消息传递成本几乎不可能（====precise modelling is practically impossible====）==。
- 标准近似模型（standard approximations）考虑两个主要因素：
    1. 每条消息都有一个 **固定启动开销（****fixed start-up cost****）**。
    2. ==大消息传输时间长于小消息（====big messages take longer====）==。
- 当消息需要经过多个跳（multiple hops）时，有两种常见的路由模型：
    - **Store-and-Forward (SF)** Routing（存储转发）
    - **Cut-Through (CT)** ==Routing====（直通路由==）
 
**Message Passing Costs – Routing Models****（路由模型）**

![Exported image](Exported%20image%2020260730175101-9.png)

- 消息在每一跳（hop）上必须 **完整传输完成** ==后，才能进入下一跳。==
- 特点：
    - 简单，容易实现。
    - 延迟（latency）随跳数线性增长。
 
- 消息可以在多个链路上 **同时处于传输中**。
- 对于足够大的消息，**总路由时间（****routing time****）** 可以**与跳数无关**（asymptotically independent of number of links）。
- 特点：
    - 高吞吐（high throughput）。
    - 适合高性能计算系统中的快速通信。

**￼** **Message Passing Costs****（消息传递成本）**

![Exported image](Exported%20image%2020260730175102-10.png)   
- ==在== **无拥塞（****absence of congestion****）** ==的情况下，一条大小为== ==( m )== ==个== ==word== ==的消息穿过== ==( l )== ==条链路（====links====）==所需时间为：

- ⚠️ 注意：这个模型可能 **无法很好地惩罚** 那些包含许多短消息（short messages）的算法。
 
**2. Cut-Through (CT) Routing****￼**

- 同样在无拥塞情况下，CT 路由的时间：

- ⚠️ 注意这里使用的是 **“****∝****” (proportional)**，而不是等号。
- 当：
    - 消息很大（large message）
    - 链路数很小（small number of links）￼时，渐进近似为：

![Exported image](Exported%20image%2020260730175103-11.png)

👉 与 SF 路由相比，CT 路由对于大消息的总时间对跳数 ( l ) 不敏感。
 
**3.** **简化假设（****Simplifying Assumption****）**

- 为了简化分析，我们通常 **假设所有进程之间直接相连**，即：

- 因此，无论 SF 还是 CT 路由，都有：

- 该假设要求：
    - 每个进程在一个 timestep 内 **只发送****/****接收少量常数条消息（****a constant number of messages****）**。
    - 常见情况：每个 timestep 仅一条消息。
 
**4. Example: Sum Reduction on a Binary Hypercube****（二进制超立方体上的求和归约）****￼**

![Exported image](Exported%20image%2020260730175128-12.png)

第二步中，偶数的邻居再次交流

![Exported image](Exported%20image%2020260730175130-13.png)

￼==总而言之，就是把前一步中所存储的消息传递给下一个，就这样一步步传递，直到传递给第一个==

![Exported image](Exported%20image%2020260730175131-14.png)  

设处理器数 ( n = 16 )：

![Exported image](Exported%20image%2020260730175132-15.png)

- 超立方体结构的通信直径为 ( \log n )，因此归约操作所需时间与 (\log n) 成正比。
- 随着 ( n ) 增加，通信步骤增加，效率下降，但加速比仍能随着 ( n ) 增长。
 
**✅** **小结（****Summary****）**

|   |   |   |   |
|---|---|---|---|
|**项目**|**SF Routing**|**CT Routing**|**简化假设**|
|公式|( T_{\mathrm{SF}} = t_s + lmt_w )|( T_{\mathrm{CT}} \propto t_s + l + m t_w )|( T = \Theta(m) )（当 ( l = 1 )）|
|特点|简单实现，延迟随跳数线性增长|高吞吐，延迟对跳数不敏感|方便理论分析|
|适用场景|小规模网络/短消息|大消息传输，高性能通信|理论建模、简化算法分析|

**1. Basic Idea****（基本思想）**
 
**2. Better Model****（改进的建模方式）**

![Exported image](Exported%20image%2020260730175133-16.png)      
**Network Topologies – Mesh****（网格拓扑）**
      
![Exported image](Exported%20image%2020260730175135-17.png)  

**Message Passing Costs****（消息传递开销）**
 
**1. Store-and-Forward (SF) Routing**
 
**2. Cut-Through (CT) Routing**
   

**1. Store-and-Forward (SF) Routing**

![Exported image](Exported%20image%2020260730175136-18.png)  
![Exported image](Exported%20image%2020260730175312-19.png) ![Exported image](Exported%20image%2020260730175314-20.png)  
![Exported image](Exported%20image%2020260730175315-21.png) ![Exported image](Exported%20image%2020260730175317-22.png) ![Exported image](Exported%20image%2020260730175318-23.png)   
**解释：**

![[Lec6 Network_Topologies_and_Message_Passing_Costs - Ink.svg]]

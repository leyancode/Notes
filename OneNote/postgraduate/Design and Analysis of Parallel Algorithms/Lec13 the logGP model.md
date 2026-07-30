**Design and Analysis of Parallel Algorithms**  
**The LogGP Model****（课堂笔记整理）**  
**1. LogGP** **模型概述（****LogGP – abstract****）****￼**

![Exported image](Exported%20image%2020260730181740-0.png)

- LogGP 是对 LogP 模型的扩展，用于更精准地刻画并行机器的通信行为。
- 核心新增点：支持对 **long messages****（长消息）** 的建模。
- 模型使用五个参数，分别描述：
    - computing bandwidth（计算带宽）
    - communication bandwidth（通信带宽）
    - communication delay（通信延迟）
    - coupling efficiency between computation and communication
    - 对 long messages 的线性模型
 
**2.** **长消息的线性模型（****LogGP – long messages linear model****）**

![Exported image](Exported%20image%2020260730181741-1.png)

经典长消息通信成本的线性模型如下：  
==t = t0 + tB * n==

- t0：startup time（启动开销）
- tB：time per byte（每字节通信延迟）
- n：消息大小（字节数）

课堂关联：

- 之前从 PRAM 扩展到包含通信拓扑时，我们使用了类似公式。
- PRAM 中常见的消息传输模型：
    - ==Store-and-forward routing====：====T = ts + l * m * tw==
    - ==Cut-through routing====：====T = ts + l + m * tw==
        - ts：startup time
        - tw：time per byte
        - m：消息大小（对应 long-message 模型中的 n）
        - l：路径跳数（课堂中常忽略 l）
 
**3. LogGP** **机器模型（****LogGP – the new machine model****）**  
LogGP 除了描述性能参数，不规定网络拓扑结构。￼

![Exported image](Exported%20image%2020260730181743-2.png)

**五个参数：**

- **L**：upper bound on network latency（网络延迟上界）
- **o**：per-message processing overhead（每条消息的软件/CPU开销）
- **g**：gap between messages（发送或接收两条消息之间的最小间隔）
- **G**：gap per byte for long messages（长消息每字节的最小间隔，新增加参数）
- **P**：processors 数量
 
**4.** **为什么需要** **LogGP****（****LogGP – what’s different, why change?****）**

![Exported image](Exported%20image%2020260730181836-3.png)

在 LogP 中，大消息只能被看成由许多小消息组成，但会产生以下问题：

- 在 LogP 中，消息间隔为 max(o, g)，这导致**最大消息发送速率被严重限制**。
- LogP 隐含假设：CPU 需要完成所有通信相关工作。
- ==但现代系统中，====NIC====（====network interface card====）可以独立执行大量通信任务，支持== **pipelining****（流水线通信）**。
- 因此：
    - large message 可以分片发送
    - 每个片段能比单个小消息更快处理
    - 所以 **G \< max(o, g)**

LogGP 引入 G 解决了这一限制，使模型更真实地反映现代高性能系统。
 
**5.** **长消息传输示意图（****LogGP – diagram for a long message****）****￼**

![Exported image](Exported%20image%2020260730181837-4.png)  
![Exported image](Exported%20image%2020260730181838-5.png)  
![Exported image](Exported%20image%2020260730181839-6.png)  

==Small g refers more to the network bandwidth, how manymessages we could send==  
==G refers to A single message like to the== ==computation bandwidth of sending a big message.==
   
![Exported image](Exported%20image%2020260730181840-7.png) ![Exported image](Exported%20image%2020260730181841-8.png)  
![Exported image](Exported%20image%2020260730181842-9.png)  
![Exported image](Exported%20image%2020260730181926-10.png)  
![Exported image](Exported%20image%2020260730181928-11.png)

￼（课堂提到的图主要说明：

- startup 阶段受到参数 o 影响
- 每个字节的发送遵循 G 的间隔
- 总延迟包含 L￼图示用于理解“消息以流水方式被推进”的概念。）
 
**6. LogP** **中的单位与参数限制（****LogP – units & limits for parameters****）****￼**

![Exported image](Exported%20image%2020260730181929-12.png)

- 处理器每个操作用 1 cycle 表示，因此所有参数以 cycle 为单位表达。
- 网络采用 ==store-and-forward routing==。
- 网络容量有限：==每条链路最多只能同时有== ==L/g== ==条消息在飞行；超出会== ==stall processor==。
- 处理器是 asynchronous（异步），可能出现消息乱序到达。
- 这一点与最初的 LogP 模型相同。
   

**Design and Analysis of Parallel Algorithms**  
**The LogGP Model****（课堂笔记整理** **·** **第二部分）**  
**7. LogGP** **模型验证（****LogGP – validating the model****）****￼**

![Exported image](Exported%20image%2020260730181930-13.png)

课堂说明：LogGP 通过与实际系统测量的通信时间进行比较，用于验证其准确性。  
核心思想：

- 对不同大小的消息测量通信开销
- 验证模型中 o, g, G, L 是否能准确拟合这些测量结果
- 特别关注 long messages 是否呈线性增长（验证 t = t0 + tB n 的合理性）

（课堂上的第 8 页主要指出：模型不仅是理论构造，还必须通过实验数据进行验证。）
 
**8.** **使用** **LogGP** **分析** **Scatter****（****LogGP – applying to Scatter****）**
 ![Exported image](Exported%20image%2020260730181933-14.png)

Scatter 操作：

- ==root== ==将== ==p== ==个数据项分发给== ==p== ==个处理器，每个处理器得到一个== ==item==。
- 不同模型、不同拓扑下的通信成本不同。 
**整体概念**  
==这页叫：==  
**LogGP – applying to Scatter****（将** **LogGP** **模型应用于** **Scatter** **操作）**  
==目的：==

1. ==在== **LogP/LogGP** **模型下，****Scatter** **操作有不同算法**
2. ==算法的好坏依赖消息大小（====k=1== ==还是== ==k== ==很大）==
3. ==小消息与大消息的最佳算法不同==
4. ==当用== ==LogGP== ==而不是== ==LogP== ==时，效果又不同==
 
**Page** **上有四张图**

- 上面两张：**Short-message algorithm****（适用于小消息的** **Scatter****）**
- 下面两张：**Binomial-tree algorithm****（二叉树** **Scatter****，适用于大消息）**

- 左侧：使用 **完整** **LogGP****（包括** **o****、****L****、****g****、****G****）**
- 右侧：使用 **简化模型（****L** **大，****g** **取固定值）**
 
**先理解****“Short-message algorithm”**  
上面两张图展示：

- 每个节点接收 1 个 item
- P=4 个处理器（P0、P1、P2、P3）
- 根节点把每个 item 单独发送出去
- 用的是“逐个 send 给每个节点”的简单版本 Scatter（简称小消息版）

- P0 先 send 给 P1（有 o 和 g 的开销）
- 再 send 给 P2
- 再 send 给 P3
- 所以是顺序的、线性的
- 总时间就是：(P-1)*(o+g)

对小消息，这种方法反而是最优的（因为没有大消息的 G 和 L 的影响）。
 
**上半部分** **左图** **vs** **右图** **的区别**  
**左：使用** **full LogGP**  
参数：L=0, o=5, g=10, G=1

- 有一个 startup o
- 每条小消息之间间隔 g
- 斜线很密因为 L=0
- 所以看起来像阶梯状消息发送

**右：使用** **simplified LogGP**  
参数：L=15, g=10

- L 变成一个不可忽略的大延迟
- 斜线变缓、整体像被“拉伸”
- 虽然算法还是一样，但执行时间变长

右边图的目的不是展示新算法，而是：  
同一个算法在 LogGP 模型不同参数下会表现不同。
 
**再理解****“Binomial Tree Algorithm”**  
下面两张图展示：

- 同样是 k=1 item
- P=4
- 使用二叉树型传播（P0→P1，然后两者同时→P2,P3）

1. P0 把数据交给 P1
2. P0 和 P1 各自负责给另一个节点传数据
3. 所以两轮就能散播完全部数据

与上面线性发送相比，这是树形并行传播。
 
**下半部分** **左图** **vs** **右图** **的区别**  
和上半部分一样：  
**左：****full LogGP**

- 每轮送出一条消息
- 但消息间隔 g
- 延迟 L 很短，所以图很密

- L 增大
- 所有向下的斜线变长
- 传播更慢
- 但结构（两轮传播）保持不变
 
**最重要对比：小消息** **vs** **大消息**

1. ==小消息最好用：====￼==**Short-message algorithm****（一次发一条）**
2. ==大消息最好用：====￼==**Binomial Tree algorithm****（树型并行发送）**

为什么？

- 小消息主要受 **o** **和** **g** 限制
- 大消息主要受 **G****（****per-byte** **间隔）和** **L****（网络延迟）** 限制
- 树形扩散能让大消息在网络中**并行传播**，节省时间
- 小消息则不值得拆分或构建树，因为消息太小

这正是 LogGP 的意义：  
**不同消息大小需要不同算法。**
 
1. Scatter 有两种主要方式：线性发送 vs 二叉树发送。
2. 对小消息，线性发送更优，因为 g 主导成本。
3. 对大消息，二叉树更优，因为 G 和 L 主导成本，树能并行扩散。
4. LogGP 模型能准确区分这些情况（比 LogP 更真实）。
5. 左右图对比说明参数变化对算法影响很大。
 
==告诉你：==
 
它们的结构是：  
每一对图都是：
 
**情景**  
图上展示的是：
 
你会看到：  
简化之后：
 
**情景**  
流程：
 
你能看到：  
**右：****simplified LogGP**
 
==这页图想告诉你的主要思想是：==  
因为：
 
**核心** **takeaway****（考试必写）**
    
**9. Scatter** **示例：与** **PRAM****（拓扑模型）对比（****LogGP – Scatter example****）**

![Exported image](Exported%20image%2020260730181934-15.png)

课堂回顾：==在之前的== ==PRAM== ==拓扑分析中，不同网络结构得到的== ==scatter== ==时间==：

- Ring：￼T = (p − 1) ts + (p − 1) m tw
- Mesh：￼T = 2(√p − 1) ts + (p − 1) m tw
- Hypercube：￼T = (log p) ts + (p − 1) m tw
- Binomial tree：结构与 hypercube 使用相同链路，因此：￼T = (log p) ts + (p − 1) m tw

课堂提出的问题：

- 上面的 ts 和 tw 在 LogP / LogGP 中应该如何解释？
 
**10. Scatter** **在** **LogP** **模型下的时间（****LogGP – Scatter example, LogP****）**

![Exported image](Exported%20image%2020260730181936-16.png)

LogP 的假设：所有 messages 都是 small messages。

- Scatter 中 root 要发出 p 条消息，每条消息只携带一个 item。

对于第 i 条消息：  
Ti = (2 o + L) + (m) g

- 2 o：发送和接收的 software overhead
- L：latency
- m：消息大小，这里是一个 item
- g：gap between messages（每条 small message 之间的最小间隔）

所有 p 条消息从 root 发出：  
T = Σ Ti = p (2 o + L) + p m g = Θ(mp)  
因此在 LogP 中：  
ts = 2 o + L  
tw = g
 
**11. Scatter** **在** **LogGP** **模型下的时间（****LogGP – Scatter example, LogGP****）**

![Exported image](Exported%20image%2020260730182141-17.png)

LogGP 的关键区别：某些消息不再是小消息，而是大消息。  
Scatter 的传播方式：

- 第一跳：root 发送 p m / 2 的数据给第一层
- 第二跳：每个中间节点继续 forward p m / 4
- 依此类推，共 log p 轮

因此：  
T1 = (2 o + L) + (p m / 2) G  
T2 = (2 o + L) + (p m / 4) G  
…  
总时间：  
T = Σ Ti  
= (log p)(2 o + L) + (p m) G  
= Θ(mp)  
并可得参数：  
==ts = 2 o + L==  
==tw = G====（====long-message== ==每字节== ==gap====）==
 
**12.** **结论：****LogGP** **对** **Scatter** **的影响**

![Exported image](Exported%20image%2020260730182142-18.png)

LogGP 的关键优势：

- 模型鼓励将 small messages 合并为 large messages￼因为：G \<\< g
- 原因来自现代硬件：NIC 支持 DMA、pipelining
- 因此：
    - 硬件能力改变 → 模型参数必须改变（g → G）
    - 模型参数改变 → 最优算法结构也必须改变
    - 促使算法采用 message agglomeration（消息聚合）

这是 LogGP 优于 LogP 的核心原因。
 
**13. LogGP** **总结（****LogGP – summary****）**

![Exported image](Exported%20image%2020260730182144-19.png)

- LogGP 是一个更细致的并行通信模型。
- 使用五个参数：Latency, overhead, gap（small messages）, gap per byte（large messages）, number of processors。
- 不描述网络拓扑结构。
- 考==虑消息大小对通信行为的影响==。
- 能区分哪些算法是 cost-optimal。
- 比 LogP 更准确，也更复杂。
 
如果你愿意，我可以继续为你：

- 做成一页 cheat sheet
- 做成表格对比：PRAM vs LogP vs LogGP
- 画出逐步传输示意图（文字版）
- 制作适合课堂复习的精简卡片版

告诉我你想要哪种即可。
 \> 来自 \<[https://chatgpt.com/c/691864fd-bdc8-8333-ba0a-a9b46a976d3a](https://chatgpt.com/c/691864fd-bdc8-8333-ba0a-a9b46a976d3a)\>  

![Exported image](Exported%20image%2020260730182145-20.png)

![[Lec13 the logGP model - Ink.svg]]

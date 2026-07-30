下面给你一份 **最终版** **Final Cheat Sheet****（中英双语）**，覆盖你整个课程核心内容：  
PRAM、Topologies、Pointer Jumping、Parallel Sorting、LogP、LogGP、Scatter、长消息模型、SF/CT、Mesh/Hypercube、Small vs Large Message 散播、考试常考推导、符号解释。  
格式：**一页内容**，考前 10 分钟读完全部关键点。
 
**Final Cheat Sheet**  
**Design & Analysis of Parallel Algorithms**  
**中英双语终极速记版**  
=====================================================

1. PRAM 模型（PRAM Models）￼=====================================================

PRAM 四种模型（按读写冲突能力）  
PRAM variants by memory concurrency:

1. **EREW**（Exclusive Read Exclusive Write）￼不允许任何并发读写。￼No concurrent read or write.
2. **CREW**（Concurrent Read Exclusive Write）￼可并发读，不可并发写。￼Concurrent reads allowed, writes exclusive.
3. **CRCW**（Concurrent Read Concurrent Write）￼并发读写均可；写冲突需规则（common/min/max）。￼CR allowed, CW allowed using rules (common/min/max).

**复杂度记法**  
Work = p × Tp  
Cost-optimal ⇔ Work = Θ(Ts)  
Pointer jumping（指针跳跃）  
计算 predecessor/successor 链长度：  
Pointer jumping halves the distance each round → O(log n)。  
Prefix sums（前缀和）  
EREW：上升树 + 回溯 → O(log n) time, O(n) processors。  
Sort（并行排序）  
Parallel sort 常为 O(log n) 时间，O(n log n) work。  
Many are not cost-optimal.  
**=====================================================**  
**2.** **通信拓扑** **& Scatter****（****PRAM** **拓扑）**  
**Topologies & Scatter in topology models**  
经典拓扑：Ring / Mesh / Hypercube  
Classic topologies:  
Scatter（one-to-all personalised communication）：  
root 有 p 条不同消息要发给 p 个进程。  
下界：Ω(tw m (p−1)) (必须把数据送出去)  
Lower bound: must send total data.  
**Ring****（环）**  
T = (p−1) ts + (p−1) m tw = Θ(mp)  
**2D Mesh**  
直径 diameter = 2(√p − 1)  
Diameter of mesh = 2(√p − 1)  
T = 2(√p − 1) ts + (p−1) m tw  
**Hypercube**  
d = log p  
T = (log p) ts + (p−1) m tw  
**Binomial Tree**  
与 hypercube 同链路  
T = (log p) ts + (p−1) m tw  
**=====================================================**  
**3. Routing** **模型（****SF / CT****）**  
**Store-and-forward vs Cut-through**  
**Store-and-forward** (SF)  
每一 hop 都要重新 buffer：  
T = ts + l m tw  
**Cut-through** (CT)  
头部一旦进入链路就可向下游推进：  
T = ts + l + m tw  
l 是 hop 数，常对大消息忽略 l。  
**=====================================================**  
**4. LogP** **模型（****LogP Model****）**  
四个参数：  
Four parameters:

- **L**：Latency（网络延迟）
- **o**：Overhead（端点 CPU 软件代价）
- **g**：gap between small messages（小消息最小间隔）
- **P**：processors 数量

Small message 成本：  
Small message cost:  
T = (2o + L) + m g  
Scatter (linear) in LogP:  
root 发 p 条 small message  
T = p[(2o + L) + m g] = Θ(mp)  
关键理解：  
LogP 只能表示 small messages，不适合长消息（因为 gap g 迫使 root 串行）。  
**=====================================================**  
**5. LogGP** **模型（****LogGP Model****）**  
在 LogP 基础上增加 **G**：  
Introduces **G** for long messages.  
参数：  
L: latency  
o: overhead  
g: small message gap  
G: gap per byte for long messages（大消息逐字节延迟）  
P: number of processors  
为何需要 G？  
Why G?  
现代系统的 NIC 支持 DMA/分片/流水线发送  
Large message 不必拆成 small messages；  
G \< g，因此长消息更快。  
长消息线性模型：  
Long message linear model:  
T = t0 + tB n  
t0 = 2o + L  
tB = G  
n = bytes  
**=====================================================**  
**6. Scatter: Small vs Large messages**  
**在** **LogP / LogGP** **下的** **Scatter** **选择**  
**小消息：****linear scatter** **最优**  
Small messages: linear best  
因为 g 远大于 per-byte 部分，tree 引入额外 overhead，不划算。  
时间（LogP）：  
T = p[(2o + L) + m g]  
**大消息：****tree / hypercube scatter** **最优**  
Large messages: tree best  
因为大消息主要是 pmG，tree 可并行扩散减少 rounds。  
时间（LogGP）：  
T = (log p)(2o + L) + (p m) G  
两部分含义：  
startup: 每轮一个 (2o+L)  
per-byte: pmG（几何级数求和）  
**=====================================================**  
**7. Small vs Large message** **的算法选择**  
**Algorithm selection**  
Small message dominated by: o + g

- linear wins
- sending p−1 messages sequentially is fine
- tree introduces unnecessary overhead

Large message dominated by: G × size

- tree wins
- pmG / parallel broadcast
- G \<\< g → 优化效果大

**=====================================================**  
**8.** **关键符号（****Symbols****）**  
ts：startup time（拓扑模型）  
tw：per-byte cost（拓扑模型）  
L：latency  
o：endpoint overhead  
g：small-message gap  
G：long-message per-byte gap  
p：number of processors  
m：message size (bytes)  
l：path length (hops)  
**=====================================================**  
**9.** **最典型考点总结**  
**Ultra-high probability exam topics**  
必考 1：  
Small message vs large message Scatter → 哪个用线性，哪个用树？  
Small: linear  
Large: tree  
必考 2：  
mesh 的直径为何是 2(√p − 1)?  
因为从 (0,0) 到 (√p−1, √p−1) 要走两段 Manhattan distance。  
必考 3：  
LogP 的 small message 成本  
T = (2o + L) + m g  
必考 4：  
LogGP 的 long message 成本  
T = (2o + L) + m G  
必考 5：  
tree scatter in LogGP  
T = (log p)(2o + L) + pmG  
必考 6：  
SF vs CT 区别  
SF：T = ts + l m tw  
CT：T = ts + l + m tw  
必考 7：  
PRAM 拓扑下的三大 scatter 公式  
Ring： (p−1) ts + (p−1) m tw  
Mesh： 2(√p−1) ts + (p−1) m tw  
Hypercube： (log p) ts + (p−1) m tw  
必考 8：  
prefix sums（前缀和）EREW  
O(log n) time with n processors.  
**=====================================================**  
**10.** **考前** **30** **秒终极背诵（关键** **12** **句）**

1. LogP 只有 small message，LogGP 才能建模 long message。
2. small message gap = g；large message per-byte gap = G。
3. G \<\< g，长消息更适合 tree。
4. small message scatter 用 linear。
5. large message scatter 用 binary tree / hypercube。
6. LogP small message cost = (2o + L) + m g。
7. LogGP long message cost = (2o + L) + m G。
8. tree scatter in LogGP = (log p)(2o + L) + pmG。
9. mesh diameter = 2(√p −1)。
10. Ring scatter = Θ(mp)。
11. Mesh scatter = Θ(mp)。
12. Hypercube scatter = Θ(mp) with startup log p。

==============================================  
如果你愿意，我可以给你做：

1. 一张一页版 PDF（可打印）
2. 一张“只有公式 + 关键字”的超精简版本
3. 一张“纯英文版”
4. 一张“做题时如何判断用 linear 还是 tree 的决策图”

告诉我你想要哪一版，我立即给你生成。
 \> 来自 \<[https://chatgpt.com/g/g-p-691b09a2e7c88191ba7715bbbfb3f193-dapatest/c/691bb5d9-9ecc-8330-a474-4ab8883f86e5](https://chatgpt.com/g/g-p-691b09a2e7c88191ba7715bbbfb3f193-dapatest/c/691bb5d9-9ecc-8330-a474-4ab8883f86e5)\>
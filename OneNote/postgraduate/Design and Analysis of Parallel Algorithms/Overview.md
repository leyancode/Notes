好的 ✅ 我帮你把课堂笔记整理成中英双文的格式，保留关键的英文术语（如 parallelism, Big-O, PRAM, etc.），并用条目化的方式，让结构清晰、便于复习。下面是整理后的版本：
 
**课程内容** **(What is this course about?)**

- 设计算法 (Devising algorithms)，使得 **many processors** 可以协同工作：
    - 解决相同的问题，但 **faster**
    - 或者在相同时间里解决更大/更难的问题
- 与单处理器 (single processor) 的对比体现出优势。
 
**为什么这门课重要** **(Why is this course important?)**

- 这是一个 **intellectual challenge**，很有趣味性。
- **Parallelism** 无处不在。
- 需要 **parallel algorithms** 才能真正利用并行性。
- 好的算法远优于坏的算法 (**good algorithms** **≫** **bad algorithms**)。
 
**Parallelism** **无处不在** **(Parallelism everywhere)**

- **Global scale**: 数据中心 (data centres), 分布式计算 (distributed computing)，如 Facebook, Google, Amazon
- **Supercomputer scale**: 参考 Top 500 列表；高性能计算 (**High-Performance Computing, HPC**) 应用于科学模拟 (simulation)、建模 (modelling)、可视化 (visualisation)
- **Desktop scale**: 多核 PC、笔记本、平板、手机；应用于游戏、日常软件
- **Specialised hardware**: 定制化电路 (customised circuits)，例如加密 (encryption)、多媒体 (multimedia)、网络 (networking)、数字信号处理 (digital signal processing)
- _参考_: [https://www.top500.org/](https://www.top500.org/) (Frontier, ORNL, ExaFlop/s, 2022)
 
**课程运作方式** **(How does this course work?)**

- **Design techniques**: 自顶向下 (Top-down) & 自底向上 (Bottom-up) 方法
- **Machine models**: 以抽象方式描述基本操作
- **Cost models**: 为操作分配代价 (cost)，用资源衡量
- **Analysis techniques**: 将算法映射为代价 (cost)，保证一定精度
- **Evaluation metrics**: 区分代价，如性能 (performance) vs. 可扩展性 (scalability)
 
**设计** **(Design)**

- 没有固定规则，依赖 **intuition, experience, imagination**
- **Top-down techniques**:
    - Divide & Conquer
    - Pipelining
    - Step-by-step
    - Amdahl’s Law 与 Gustafson’s Law
- **Bottom-up primitives**:
    - Reduction & Prefix
    - Pointer-jumping
    - Broadcast, Scatter, Gather
 
**分析** **(Analysis)**

- 基于一些非正式但有形式化基础的规则
- **Asymptotic analysis**: Big-O notation
- **PRAM model** (适用于 shared-memory 架构)
- **Metrics**:
    - Cost
    - Speedup
    - Efficiency
    - Cost-optimality
- **Brent’s Theorem**: 高效缩减规模 (scaling down efficiently)
- **Message-passing**:
    - Network topology: ring, mesh, hypercube
    - Simple cost models
    - Mapping between topologies
    - Advanced cost models
 
要不要我帮你再画一张 **知识框架图** **(mind map)**，把 _Design–Analysis–Evaluation_ 这些内容结构化展示？这样你的复习更直观。
 \> 来自 \<[https://chatgpt.com/c/68d93c90-b160-8324-beb8-b613ea3bf743](https://chatgpt.com/c/68d93c90-b160-8324-beb8-b613ea3bf743)\>
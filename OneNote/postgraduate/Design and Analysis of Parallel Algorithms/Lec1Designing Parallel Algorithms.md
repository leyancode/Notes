**Designing Parallel Algorithms (****并行算法设计****)****￼**

![Exported image](Exported%20image%2020260730174328-0.png)

**General Principles (****总体原则****)**

- **Challenging**: 没有严格规则，依赖直觉 (intuition)、经验 (experience)、创造力 (creativity)
- 存在一些可借鉴的方法论和技巧 (methodologies & techniques)
- 常见互补方法：
    - **Top-down**: 使用概念范式 (conceptual paradigms) 找到主要结构
    - **Bottom-up**: 使用基本原语 (primitives)，并进行适当组合
 
**Partitioning / Decomposition (****划分与分解****)****￼**

![Exported image](Exported%20image%2020260730174329-1.png)

- ==基本计算单元是什么？====(tasks)==
- 划分方式：
    - 输入数据 (input data)
    - 输出数据 (output data)
    - 基于函数 (functions) → 关注不同但相互作用的活动
- 任务间交互 (task interaction)：
    - 是否存在依赖关系 (dependencies)?
    - 可用 **Directed Acyclic Graph (DAG)** 表达
    - 表现形式：通信 (communication)、同步 (synchronisation)、共享 (sharing)
    - 取决于 **machine model**
 
**Task Granularity (****任务粒度****)****￼**

![Exported image](Exported%20image%2020260730174330-2.png)

- 粒度过小：任务过多 → 高开销 (overheads)，需要聚合 (agglomeration) 成 **super-tasks**
- 粒度过大：任务过少 → 负载不均衡 (load imbalance)
- 问题：如何将任务分配 (assign) 给处理器？
 
**Owner Computes Rule (****所有者计算规则****)****￼**

![Exported image](Exported%20image%2020260730174332-3.png)

- 在基于数据划分 (data-oriented partitioning) 的算法中自然适用
- 需要保证任务交互 (task interaction) 的高效性
- 与 **scaling-down** 问题相关，不同因素之间常存在张力 (tension)
 
**Divide & Conquer (****分而治之****)****￼**

![Exported image](Exported%20image%2020260730174332-4.png)

**基本思想**

- 使用递归问题分解 (recursive decomposition)
- 将原问题拆分为相同类型的子问题 (sub-problems)，并递归求解
- 合并子问题的解 (sub-solutions) → 得到原问题解
- 定义 **base case**，直接求解简单实例

**常见例子**

- Quicksort
- Mergesort
- 矩阵乘法 (Matrix Multiply, Strassen 算法)
 
**Parallelising Divide & Conquer (****并行化分治法****)****￼**

![Exported image](Exported%20image%2020260730174334-5.png)

- ==使用进程====/====线程树== ==(tree of processes/threads)== ==映射到可用处理器==
- 挑战 (Challenges)：
    - 根节点 (root) 可能成为串行瓶颈 (sequential bottleneck)
    - 并行化高效算法可能需要 **nested parallelism** (嵌套并行)
    - 小问题不值得分配 (distribution) → 需要权衡分配成本 (distribution costs) 与重新计算成本 (recomputation costs)
 
**Pipelining (****流水线****)**  
**基本思想** **(Basic Idea)****￼**

![Exported image](Exported%20image%2020260730174335-6.png)

- 源自 **工厂生产线** **(factory production line)** 的模型。
- 适用于：需要对一长串数据项 (long sequence of data items) 执行复杂操作的情况。
- **工作方式** **(How it works):**
    - ==将操作分解== ==(decompose)== ==为一系列== ==p== ==个子操作== ==(sub-operations)====。==
    - ==每个子操作对应一个流水线阶段== ==(pipeline stage)====。==
    - ==各阶段串联组成流水线== ==(chain together processes)====。==
 
**性能特点** **(Performance Characteristics)****￼**

![Exported image](Exported%20image%2020260730174336-7.png)

- **单个任务** **(single task)** 的完成时间：最佳情况下与顺序执行相同 (通常更差)。
- **整体任务** **(overall completion)**：对于 n 个输入，吞吐量 (throughput) 明显提升。
- 如果各子操作耗时均衡 (well-balanced)，当 n 很大时，性能接近 **p** **倍加速** **(p-fold speed-up)**。
 
**示例** **(Example)****￼**

![Exported image](Exported%20image%2020260730174340-8.png)  
![Exported image](Exported%20image%2020260730174341-9.png)  

- ![Exported image](Exported%20image%2020260730174342-10.png)
- ]
 
**图表示意** **(Graph Interpretation)****￼**

![Exported image](Exported%20image%2020260730174343-11.png)

1. **运行时间** **(Runtime vs. Number of tasks)**
    - 顺序执行：( T_s(n) = 30n )
    - 流水线执行：( T_p(n) = 7n + 28 )
2. **加速比** **(Speed-up vs. Number of tasks)**
    - ==随着任务数== ==(n)== ==增加，====Speed-up== ==趋近于== ==( 30/7 \approx 4.29 )====。==
 
**总结** **(Summary)**

- Pipelining 的优势在于 **大规模任务** **(large n)** 下的吞吐量提升。
- 核心在于：
    - **任务拆分** **(decomposition)**
    - **阶段均衡** **(balance of sub-operation times)**
- 本质：**将****每个任务分成多个步骤** **(break each task into steps)**==，提升并行度。==

好的 ✅ 我把 **Pipelining – break each task into steps** 这一部分整理成清晰的中英双语笔记，保留公式和流水线推进的逻辑。
 
**Pipelining – Break Each Task into Steps (****流水线** **–** **将任务拆分为步骤****)**  
**基本设定** **(Setup)****￼**  
**执行示意图（****Execution Flow Example****）****￼****注意是将一个过程分化为多个进程去执行对应的操作去完成**  
Steps →￼P0: 7 7 7 7 7 7 7￼P1: - - 7 7 7 7 7￼P2: - - - 7 7 7 7￼P3: - - - - 7 7 7￼P4: - - - - - 7 7￼  
每个“7”表示一个执行步骤（step of time s=7），  
“-”表示该处理器尚未开始执行任务。

![Exported image](Exported%20image%2020260730174344-12.png)

- 每个任务被分为 **m = 5** 个阶段 (stages)。
- 每个阶段耗时 **s = 7**。
- 流水线由 **5** **个处理器** **(P0–P4)** 组成，每个处理器负责一个阶段。￼
- ![Exported image](Exported%20image%2020260730174345-13.png)
 ![Exported image](Exported%20image%2020260730174346-14.png)  
![Exported image](Exported%20image%2020260730174351-15.png)  
![Exported image](Exported%20image%2020260730174353-16.png)  
![Exported image](Exported%20image%2020260730174354-17.png)  
![Exported image](Exported%20image%2020260730174357-18.png)  
![Exported image](Exported%20image%2020260730174358-19.png)  
![Exported image](Exported%20image%2020260730174358-20.png)

**时间分析** **(Time Analysis)**  
**第** **1** **个任务** **(First task)**

- 需要经历所有阶段。￼[
- ![Exported image](Exported%20image%2020260730174400-21.png)
- ￼]
 
**第** **2** **个任务** **(Second task)**

- 在第 1 个任务完成后，只需再等 1 个阶段。￼[￼
- ![Exported image](Exported%20image%2020260730174405-22.png)
- ]
 
**第** **3** **个任务** **(Third task)**

- 同样地，只需再等 1 个阶段。￼[￼
- ![Exported image](Exported%20image%2020260730174407-23.png)
- ￼]
 
**推广** **(Generalisation)**

- 对于第 (k) 个任务：￼[￼
- ![Exported image](Exported%20image%2020260730174409-24.png)
- ]
- 即：￼
- ![Exported image](Exported%20image%2020260730174409-25.png)

**直观理解** **(Intuition)****￼****10. Pipelining — Breaking Each Task into Steps**  
**流水线（****Pipelining****）：将任务分解为多个阶段**
 
**概念简介**  
**Pipelining****（流水线）** 是一种将一个完整任务划分为多个连续步骤（steps）的并行化技术。  
每个步骤可以由不同的处理器（processors）并行执行，从而提高总体吞吐率（throughput）。  
在这种模型中：

- 每个任务包含 **m** **个阶段（****steps****）**；
- 每个阶段执行时间为 **s****（****step time****）**；
- 系统包含多个处理器 ( P_0, P_1, P_2, \dots )，依次处理这些阶段。

￼**示例计算**

![Exported image](Exported%20image%2020260730174411-26.png)  
![Exported image](Exported%20image%2020260730174412-27.png)  
![Exported image](Exported%20image%2020260730174413-28.png)

**￼****￼** **一般公式（****Counting Equation****）****￼**

![Exported image](Exported%20image%2020260730174418-29.png)

对于总共 **n** **个任务（****tasks****）**：

- 需要等待第一个任务经过所有 **m** 阶段；
- ==对于其余的== ==(n - 1)== ==个任务，每个只需再等待一个阶段==。

因此：

![Exported image](Exported%20image%2020260730174419-30.png)

将示例参数 ( s = 7, m = 5 ) 代入：

![Exported image](Exported%20image%2020260730174421-31.png)

**💡** **结果分析（****Interpretation****）**

|   |   |   |
|---|---|---|
|**参数**|**含义**|**说明**|
|( s )|Step time|每个阶段的执行时间|
|( m )|Steps per task|每个任务包含的阶段数|
|( n )|Number of tasks|总任务数|
|( T_n )|Total time|执行完 n 个任务的总时间|

- **启动阶段（****startup time****）**：第一个任务需要完整通过所有步骤。
- **稳态阶段（****steady state****）**：之后每个任务只增加一个阶段的时间。
- 当任务数量 (n) 较大时，**平均执行时间趋近于** **s****（每个任务一个阶段）**。
 
**✅** **要点总结**

![Exported image](Exported%20image%2020260730174422-32.png)  
![Exported image](Exported%20image%2020260730174423-33.png)  
![Exported image](Exported%20image%2020260730174424-34.png)  
![Exported image](Exported%20image%2020260730174425-35.png)

**￼****性能影响** **(Performance Impact)****￼**

![Exported image](Exported%20image%2020260730174449-36.png)

- 在流水线中，**最慢阶段** **(bottleneck stage)** 限制了整体吞吐量。
- 吞吐率 (throughput) → ( 1 / 20 ) per unit time。
- 与均衡流水线 (每阶段 7) 相比，加速比明显下降。
 
**图表解读** **(Graph Interpretation)****￼**

![Exported image](Exported%20image%2020260730174451-37.png)  
![Exported image](Exported%20image%2020260730174452-38.png)

✅ 总结：

- **均衡流水线**：接近理想加速比。
- **瓶颈阶段过慢**：整个流水线的速度被拖累。
- 设计并行算法时 → **必须平衡各阶段耗时**，否则 pipeline 效率会大打折扣。

￼**Step-by-Step Parallelisation (****逐步并行化****)****￼**

![Exported image](Exported%20image%2020260730174453-39.png)  
![Exported image](Exported%20image%2020260730174454-40.png)  
![Exported image](Exported%20image%2020260730174455-41.png)

**￼****挑战** **(Challenges)**

- 将 **顺序程序** **(sequential programs)** 并行化非常困难：
    - 存在许多复杂依赖 (complex dependencies)
    - 甚至可能有 **伪依赖** **(false dependencies)**
 
**顺序算法的并行化** **(Parallelising Sequential Algorithms)**

![Exported image](Exported%20image%2020260730174456-42.png)

- 有时更容易，因为可以考虑 **coarse grain phases (****粗粒度阶段****)**：
    - **识别阶段** **(identify phases)**
    - **并行化每个阶段** **(parallelise each phase)**
    - 在阶段之间保持 **顺序控制流** **(sequential control flow)**
 
**限制与困难** **(Limitations & Difficulties)**

- **阶段划分** **(phases)** 并不总是容易识别。
- 即使识别出来，也可能很难实现并行化。
 
✅ 总结：  
Step-by-Step 并行化方法强调 **逐阶段分解和并行化**，而不是试图一次性将整个顺序程序并行化。这种方式能够简化问题，但前提是能合理地找到程序中的 **并行阶段** **(parallelisable phases)**。

==28====是一开始延迟所用的时间==

==由最长的时间来决定==

**并行执行（任务重叠）**  
来看一个时间轴例子：

|   |   |   |   |
|---|---|---|---|
|**时间**|**P0****（读取）**|**P1****（计算）**|**P2****（输出）**|
|t₁|任务1|—|—|
|t₂|任务2|任务1|—|
|t₃|任务3|任务2|任务1|
|t₄|任务4|任务3|任务2|
|…|…|…|…|

可以看到：

- **第****1****个任务**：从 P0 → P1 → P2，逐步流动；
- **第****2****个任务**：在第1个任务进入下一阶段时即可启动；
- **第****3****个任务**：继续接着进入流水线；
- 之后每个时间片，都有一个任务完成。

这就是所谓的 **阶段并行（****stage parallelism****）** —— 不同的阶段在同时工作。
 
**⚙️** **二、是不是要****“****先装满任务****”****再开始？**  
这个是很多同学一开始的误解。  
答案是：❌ **不需要全部装满再处理。**
 
**🕒** **1.** **流水线的三个阶段**  
流水线的执行可以分为三个时期：

|   |   |   |
|---|---|---|
|**阶段**|**说明**|**状态**|
|**Startup****（启动期）**|第一个任务进入流水线，还没填满所有阶段|阶段逐渐被占用|
|**Steady state****（稳态期）**|流水线已满，每个时刻都有任务在进行|最大并行效率|
|**Drain****（排空期）**|没有新任务进入，流水线逐渐清空|结束执行|

⚙️ ==所以流水线是== **边装边处理（****streaming****）** ==的，不是====“====装满再启动====”====。==  
每个任务进入的同时，上一个任务已经在下一阶段执行。
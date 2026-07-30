以下为你整理后的课堂笔记内容，保持清晰结构、专业表达，并保留关键英文术语。你可以直接将其作为笔记使用。
 
**Design & Analysis of Parallel Algorithms**  
**Dense Matrix Algorithms: Matrix Multiplication**  
（并行算法设计与分析：稠密矩阵算法）
 
**1. Dense Matrix Algorithms****（稠密矩阵算法）**  
**1.1** **背景**

- 矩阵在科学与工程中无处不在，许多数值计算代码本质上是由一系列矩阵操作组成的。
- 因此，高效的矩阵算法非常关键。

**1.2 BLAS** **标准**

- BLAS（Basic Linear Algebra Subprograms）定义了向量与矩阵操作的标准接口。
- 其实现由库开发者负责，例如：
    - Intel MKL
    - NAG Library
    - Nvidia cuBLAS
    - AMD CML
    - OpenBLAS
    - ATLAS
- 虽然单个调用的性能提升可能有限，但矩阵例程通常会被调用大量次数，因此整体性能影响显著。
 
**2.** **矩阵类型说明**  
在本课程中，我们讨论的矩阵满足以下条件：  
**2.1 Dense****（稠密）**

- 与 sparse（稀疏）矩阵相对。
- 稀疏矩阵的零元素结构可以被系统性利用，而稠密矩阵的零太少无法使用这些技巧。

**2.2 Square****（方阵）**

- 虽然一般化到 rectangular（非方阵）矩阵是可行的，但过程繁琐，因此这里只关注方阵。

**2.3** **数据划分（****Data Partitioning****）**  
在 message passing（消息传递）算法中，矩阵如何划分是关键。常见分布方式：

- Block distribution（块分布）：相邻元素在同一进程内。
- Cyclic distribution（循环分布）：相邻元素被分散到不同进程。
- Row-wise（按行分布）、Column-wise（按列分布）、Chequerboard/Block-checkerboard（棋盘式分块）。
 
**3. Matrix Multiplication****（矩阵乘法）****￼**

![Exported image](Exported%20image%2020260730182407-0.png)

**3.1** **基本概念**

- 矩阵元素使用两个索引：i（行）与 j（列）。
- 当我们将矩阵 A 与矩阵 B 相乘时，结果矩阵 C 的每个元素 Cᵢⱼ 来自：
    - A 的第 i 行
    - B 的第 j 列
- ![Exported image](Exported%20image%2020260730182408-1.png)

**3.2** **维度要求****￼**

![Exported image](Exported%20image%2020260730182409-2.png)

- A 的列数必须等于 B 的行数。
- 结果矩阵 C 的维度为：
    - 行数：与 A 相同
    - 列数：与 B 相同

**3.3** **特殊化到方阵**

- 这里仅讨论 square matrices，因此维度匹配条件不会造成限制。
   

以下是对你提供内容的系统化课堂笔记整理，保持课堂风格、中英双语表达、保留关键英文术语，并使推导结构更清晰。
 
**Matrix Multiplication****（矩阵乘法）**  
**1.** **基本定义**

![Exported image](Exported%20image%2020260730182514-3.png)

**2. Sequential Algorithm****（顺序算法）**

![Exported image](Exported%20image%2020260730182516-4.png)  
![Exported image](Exported%20image%2020260730182517-5.png)

**Parallel Matrix Multiplication****（并行矩阵乘法）**  
**3. Blocked Form****（块划分形式）**  
在并行环境中，我们将矩阵分割为 blocks，并在一个概念上为 2D mesh（二维网格）的处理器拓扑上执行。￼

![Exported image](Exported%20image%2020260730182519-6.png)  
![Exported image](Exported%20image%2020260730182520-7.png)

**4.** **数据依赖（****Data Dependencies****）**
   
![Exported image](Exported%20image%2020260730182521-8.png)

问题：如何最有效地满足这些数据依赖？
 
**5. Simple Communication Strategy****：****Row-wise + Column-wise Broadcast**  
**5.1** **处理器网格示意**

![Exported image](Exported%20image%2020260730182522-9.png)  
![Exported image](Exported%20image%2020260730182626-10.png)

**￼****￼****5.2 Simple Approach**

![Exported image](Exported%20image%2020260730182628-11.png)

**6. Computation Cost****（计算成本）**  
一个进程需要执行：

![Exported image](Exported%20image%2020260730182629-12.png)

**7. Total Cost****（总成本分析）**

![Exported image](Exported%20image%2020260730182630-13.png)

如果你需要，我可以继续整理：

- Cannon’s algorithm
- SUMMA algorithm
- Block-cyclic distribution
- 并行矩阵乘法的 scalability、communication complexity 对比

告诉我是否需要继续扩展。  
下面为你整理后的 Cannon’s Algorithm 笔记内容，结构清晰、保持中英双语、保留重要英文术语，并与前面笔记风格一致。
 
**Cannon’s Algorithm****（****Cannon** **算法）**  
**1. Algorithm Overview****（算法概述）****￼**

![Exported image](Exported%20image%2020260730182631-14.png)

Cannon’s algorithm 仍然采用 block decomposition（块划分），但与简单的 all-to-all 广播算法相比，它具有以下显著特点：  
**1.1 Reduced Memory Use****（更低的内存占用）****￼**

![Exported image](Exported%20image%2020260730182632-15.png)

- Simple algorithm 会将每一行和每一列的所有 blocks 复制到每个相关进程。
- Cannon’s algorithm 通过 **cycling blocks**（沿行和列周期性移动 blocks）避免了这种数据重复。

**1.2 Interleaving Computation and Communication**

- 算法在每一步都将通信与计算交错进行，提高资源利用率。

![Exported image](Exported%20image%2020260730182633-16.png)

**2. Initial Alignment****（初始对齐** **/ Skewing Phase****）**  
因为 block 要根据 Cannon’s algorithm 的规则移动，所以在开始计算前需要对 block 进行 cyclic skew（循环错位）。￼

![Exported image](Exported%20image%2020260730182726-17.png)

**2.1 Skew Rules****（对齐规则）**

![Exported image](Exported%20image%2020260730182728-18.png)

]  
此步骤确保每个进程在计算开始前具有正确的 block alignment。
 
**3. Main Iteration****（主循环）**  
完成 initial alignment 之后，算法进入 ( p ) 步迭代（初始对齐后的第一次计算 + 后续 ( p - 1 ) 次循环）。  
**每次迭代包括两步：**  
**Step 1: Compute**

![Exported image](Exported%20image%2020260730182729-19.png)

。  
**Step 2: Communicate**

![Exported image](Exported%20image%2020260730182730-20.png)

**4. Performance Analysis****（性能分析）****￼**

![Exported image](Exported%20image%2020260730182731-21.png)  
![Exported image](Exported%20image%2020260730182732-22.png)

**5. Comparison with Simple Algorithm****（与简单算法的比较）**

|   |   |   |
|---|---|---|
|**属性**|**Simple Algorithm**|**Cannon’s Algorithm**|
|数据依赖满足方式|row-wise + column-wise 广播|block cycling|
|内存占用|高（需要复制 blocks）|低（无复制）|
|通信次数|每行/每列一次广播，代价高|每步局部通信 (nearest-neighbour)|
|计算与通信重叠|几乎无|可以重叠|
|可扩展性（Scalability）|差|好|
|理论复杂度|Θ(n³ + p² log p + pn²)|Θ(pn² + n³)|
|实际效果|通常不实用|实际可扩展性好|
 
**6. Key Insight****（核心思想）****￼**

![Exported image](Exported%20image%2020260730182733-23.png)

课程特别强调：

- 该问题的瓶颈必然是 **communication**
- 没有办法让所有计算数据自然对齐到同一进程上
- Simple algorithm 与 Cannon’s algorithm 理论复杂度非常接近
- **实际表现**：
    - Simple algorithm 因为广播代价极高而几乎不可用
    - Cannon’s algorithm 更易扩展且通信模式更合理
- 没有 magic，最终通信的数据量是相同的，但组织方式更高效
 
下面为你整理后的课堂笔记内容，保持与你前面整理过的风格一致，中英双语表达、逻辑清晰，并保留关键英文术语。
 
**Matrix Multiplication****（矩阵乘法）**

![Exported image](Exported%20image%2020260730182758-24.png)

**1. Communication as the Bottleneck****（通信是主要瓶颈）**

- 不出意外，矩阵乘法的并行瓶颈来自 communication，而不是 computation。
- 在这一问题中，无法将所有数据依赖自然地分解到同一个进程上 —— 也就是说，**结果矩阵的任意一个** **block** **都依赖多个在其他进程上的数据**。
- 因此，communication 是不可避免的核心负担。
 
**2. Two Parallel Algorithms****（两种并行算法）**  
课程介绍了两种典型的并行矩阵乘法算法：  
**2.1 Simple Algorithm****（简单算法）**

- 需要对每一行和每一列执行 collective broadcast。
- 这意味着通信开销极高。
- 数据复制在行和列之间发生大量冗余，严重限制程序的 scalability（可扩展性）。
- 虽然理论上的 asymptotic complexity 与 Cannon 类似，但实际上几乎不可用。

**2.2 Cannon’s Algorithm**

- 通过 cyclic shift（循环移动）和 initial alignment（初始错位）减少数据复制。
- 更容易 overlap computation and communication。
- 内存使用更低，可扩展性明显优于 simple algorithm。
- 没有本质魔法，最终还是必须通信同样数量的数据，只是 communication pattern 更高效。
 
**Beyond O(n³)?****（突破** **O(n³)** **的序列算法）**  
**3. Fast Matrix Multiplication Algorithms****（快速矩阵乘法算法）**  
注意：此前讨论 cost optimality 时使用的 sequential baseline 是 Θ(n³) 时间。但实际上已有更快的理论序列算法：  
**3.1 Theoretical Fast Algorithms**  
存在时间复杂度低于 n³ 的矩阵乘法算法：

- Coppersmith-Winograd
- Stothers
- Williams
- Le Gall

![Exported image](Exported%20image%2020260730182759-25.png)

但由于其**常数因子极其巨大**，在实际应用中并不使用。  
**3.2 Practically Useful: Strassen’s Algorithm**

![Exported image](Exported%20image%2020260730182800-26.png)

**4. Impact on Parallel Algorithms****（对并行算法的影响）**  
Cannon’s algorithm 内部需要执行 block-level 的矩阵乘法：

- 若 block-size 较大时，使用更快的 serial matrix multiply 会带来实际速度提升。
- 因此：**改进序列矩阵乘法，也能提高并行算法性能。**
 
**5. Hybrid Parallelism?****（那么** **Hybrid** **并行呢？）**  
这一部分通常引出进一步讨论：

- 在现代 HPC 环境中，节点内通常有共享内存（可用 OpenMP / threads），节点间使用 MPI。
- 若将 Cannon 作为 MPI 层上的分布式算法，在 block-level 的矩阵乘法内部使用多线程加速（如 OpenMP、CUDA、MKL multithreading），即可实现 hybrid parallelism。
- 这种方式能显著提高性能，并降低 MPI 通信开销对整体运行时间的占比。

若课程继续，会进入以下主题：

- MPI + OpenMP hybrid model
- Node-level BLAS acceleration
- GPU 加速（如 cuBLAS + MPI）

你若需要，我可以继续整理：

1. Hybrid parallelism 的完整笔记
2. Cannon + OpenMP 如何结合
3. Cannon 与 SUMMA 的对比总结
4. 整套 Matrix Multiplication 并行算法复习提纲（适合考试前）

告诉我你想继续哪部分。
 \> 来自 \<[https://chatgpt.com/c/69320b86-8d44-8325-8d43-1458078f817a](https://chatgpt.com/c/69320b86-8d44-8325-8d43-1458078f817a)\>   \> \>
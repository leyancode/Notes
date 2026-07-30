下面为你整理后的 **Distributed Data Structures****（分布式数据结构）章节课堂笔记**，保持与你之前笔记完全一致的格式，中英双语表达、重点清晰，并保留必要英文术语。
 
**Distributed Data Structures: An Overview**  
分布式数据结构：概述
 
**1. Roadmap****（本章路线图）**  
本章将介绍以下内容：

- Motivation（动机：为什么需要分布式数据结构）
- Historical Example: Numerical Weather Forecasting（历史案例：数值天气预报）
- Distributed Matrix and Mesh using PETSc（使用 PETSc 的分布式矩阵与网格）
- PGAS Programming Model using Fortran Coarrays（使用 Fortran Coarrays 的 PGAS 模型）
- Distributed Graph in a Language Runtime System（语言运行时中的分布式图结构）
- Distributed Hash Table（分布式哈希表）
- Summary（总结）
 
**2. Motivation****（动机：为什么需要分布式数据结构）****￼**

![Exported image](Exported%20image%2020260730183138-0.png)

**2.1 Large-scale data challenge****（大规模数据问题）**

- 大规模科学计算的数据量可能 **无法放入单个节点的内存**
- 要使用拥有成千上万节点的超级计算机，必须将数据进行分布式拆分（domain decomposition、partitioning、agglomeration）

**2.2 Communication dominates cost****（通信是主要成本）**  
在分布式环境中：

- 计算相对于通信通常更便宜
- 若数据被切分跨节点存储，访问或更新 remote elements 必须通信
- 因此通信会成为程序性能的主要瓶颈

**2.3 Memory hierarchy****（内存层次结构）**  
图中展示了不同存储介质的典型延迟（2021 年数据）：

|   |   |
|---|---|
|**存储介质**|**典型延迟**|
|CPU Register|~2 ns|
|L1 / L2 Cache|~5–20 ns|
|DRAM|~100 ns|
|Network (intra-cluster)|~0.1–10 ms|
|WAN|~150 ms|

跨节点通信比本地访存慢 **数百万倍**。这也是为什么需要精心设计分布式数据结构。
 
**3. Historical Example: Origins of Numerical Weather Prediction****￼**

![Exported image](Exported%20image%2020260730183139-1.png)

数值天气预报的起源（历史案例）  
Lewis Fry Richardson（1881–1953）在其著作 _Weather Prediction by Numerical Process_（1922）中提出：

- 将世界划分成许多空间区域（spatial partitioning）
- 每个“计算者”（人类）负责处理一个小区域
- 各个区域之间需要交换边界数据
- 需要一个中央协调器来调度和同步计算

这是现代 **分布式计算与域分解方法（****domain decomposition****）** 的早期思想雏形。  
图中的插画展示了 Richardson 想象中的“天气预报工厂”。
 
**4. Distributed Data Structure****（分布式数据结构）****￼**

![Exported image](Exported%20image%2020260730183141-2.png)

一个分布式数据结构（如 mesh 网格）具有以下特性：  
**4.1 Local partition****（本地分区）**

- 每个进程存储整个数据结构的一部分，例如网格的部分 cells 或部分节点。

**4.2 Mapping from global IDs to local IDs**

- 需要维护全局编号到本地编号的映射，使得跨节点通信成为可能。

**4.3 Remote access requires communication**

- 若某个远程元素不在本地内存中，则必须通过 MPI 或 RDMA 获取。

**4.4 Halo regions****（幽域区域）**  
Halo（或 ghost region）用于保存邻域进程的数据副本：

- 本地 partition 的边界需要邻居数据
- 消除频繁的小通信
- Halo 更新通常在每个计算步骤开始时执行一次

示例图片展示了：

- 一个网格及其 halo outline
- 一个 variable-resolution Voronoi mesh（非规则网格结构）
 
**5. Distributed Matrix: Example Using PETSc****￼**

![Exported image](Exported%20image%2020260730183219-3.png)

使用 PETSc 表示分布式矩阵（Matrix）  
PETSc（Portable Extensible Toolkit for Scientific Computation）是科学计算中常用的分布式线性代数库。  
用户只需调用 API，内部的分布式存储、索引映射、通信都被抽象掉：  
Mat A;￼MatCreate(PETSC_COMM_WORLD,&A);￼MatSetSizes(A,PETSC_DECIDE,PETSC_DECIDE,N,N);￼MatSetFromOptions(A);￼// e.g. -mat_type sbaij for symmetric block format￼MatSetUp(A);￼  
// ... fill entries of A ... (assemble)￼// ... solve system with A ...￼  
MatDestroy(&A);￼  
**关键特性：**

- PETSc 自动根据进程数量决定本地矩阵分块大小
- 用户不需要显式处理 global-to-local mapping
- 内部使用 optimized sparse / dense formats、MPI 通信
- 可与 Krylov solvers、preconditioners 等组件组合使用

下面为你整理后的 **PGAS****、****Coarray Fortran****、****Distributed Graph****、****Distributed Hash Tables** **以及** **Summary** 的完整课堂笔记内容，内容风格与前面所有笔记一致，中英双语、重点清晰、保留关键英文术语，适合作为你的课堂笔记直接使用。

![Exported image](Exported%20image%2020260730183940-4.png)

**Partitioned Global Address Space (PGAS)**  
分区全局地址空间模型￼

![Exported image](Exported%20image%2020260730184041-5.png)

**1. PGAS Programming Model****（编程模型）**  
PGAS 是一种并行编程模型，提供：

- **Global view of data structures****￼**程序员看到的是“全局数据”，虽然它们底层是 **partitioned / distributed** 到多个 ranks、节点或线程上的。
- **Communication hidden by the API****￼**通信由语言或运行时隐藏，程序员以访问共享变量的方式编写代码。
- **Affinity explicitly managed****￼**程序员可显式管理数据的 _affinity_（数据归属的执行单元），从而优化 locality。

**支持** **PGAS** **的语言**

- UPC (Unified Parallel C)
- Chapel
- X10
- Coarray Fortran（Fortran 2008 标准引入）

**Advantages****（优点）**

- 编写方式类似共享内存（shared memory），更方便
- 程序员可以控制数据的放置（affinity control）
- 能利用网络的 RDMA（Remote Direct Memory Access）特性￼→ 低开销通信

**Disadvantages****（缺点）**

- 若数据划分不合理，性能会急剧下降
- 同步原语使用不当可能导致性能问题或死锁
- 模型“看似简单”，但高性能仍需要精心设计局部性与同步

图示展示了 PGAS 中 _images_ 的概念：

- 每个 image 是独立地址空间
- 全局数据结构被复制 / 分区到所有 image
- 程序员通过本地访问和远程访问（remote access）读写数据
 
**Coarray Fortran Model****（****Coarray Fortran** **模型）****￼**

![Exported image](Exported%20image%2020260730184346-6.png)

Coarray Fortran 是 Fortran 语言的 PGAS 扩展。  
**1. Data Distribution****（数据分布）**

- 每个 image 都包含相同大小的 coarray
- 数据可以按 cyclic 或 block-wise 方式分布访问
- SPMD（Single Program Multiple Data）风格

**2. Syntax****（基本语法）**  
声明：  
real, dimension(4,4), codimension[*] :: x￼  
关键区分：

- x(i,j) → local data（本地元素）
- x(i,j)[k] → remote access（访问 image k 上的元素）

**3.** **特点**

- 使用简单，接近共享内存编程体验
- 每个变量天然带有全局索引维度
- 远程访问通过底层通信自动完成（MPI, GASNet 等）
 
**Distributed Graph in a Parallel Runtime System**  
分布式图结构（以 Parallel Haskell 为例）  
图示来自：  
J. Berthold et al., _PAEAN: Portable and Scalable Runtime Support for Parallel Haskell Dialects_, JFP, 2016￼

![Exported image](Exported%20image%2020260730184348-7.png)

**运行时结构**

- 并行语言的 runtime system 本身由多个实例组成
- 并行任务在执行过程中自然形成一个 **distributed graph**
- 图节点可能位于不同计算节点，需要动态解析远程依赖

**Global In-Pointer Table (GIT)**

- FetchMe 表示一个指向某项远程计算结果的全局索引
- 当执行 FetchMe 时：
    1. 发送请求到拥有该数据的进程（owner）
    2. owner 解析索引并返回结果
    3. runtime 将其整合到本地图中

这个机制类似分布式图数据库中的 remote pointer dereference。
 
**Distributed Hash Tables****（分布式哈希表）****￼**

![Exported image](Exported%20image%2020260730184350-8.png)

分布式哈希表（DHT）广泛用于：

- P2P 网络
- Grid / Cloud computing
- 大规模 key-value stores

典型实现：Chord、Kademlia、CAN、Pastry、Tapestry。
 
**Example: Chord Protocol****（****Chord** **协议）**

![Exported image](Exported%20image%2020260730184352-9.png)

**Summary****（总结）****￼**

![Exported image](Exported%20image%2020260730184357-10.png)

本节总结了并行计算中常见的分布式数据结构：  
**1.** **分布式数据结构是可扩展应用的构建基石**  
包括 mesh、matrix、graphs、hash tables 等。  
**2. Communication dominates in distributed algorithms**  
通信是性能的主要限制因素。  
**3. Design Principles****（设计原则）**

- **尽量限制通信到邻居节点（****locality-aware design****）**
- **分析时需要理解实现细节**（如 PETSc 中的 ghost regions）
- **使用** **profiling** **工具评估真实性能**

**4.** **使用现有库需谨慎**  
例如 PETSc、DHT frameworks、Coarray Fortran 等：

- 虽然接口抽象简单
- 但底层分布策略与通信模式决定性能
- 因此必须理解数据如何 partition、通信如何触发
 
如果你需要，我可以继续整理：

1. 整合所有 _Distributed Data Structures_ 的一页 cheat sheet
2. 完整 PGAS vs MPI vs Shared-memory 对比表
3. 帮你把以上内容转成 LaTeX 讲义格式
4. 为考试复习总结所有分布式数据结构的核心考点

告诉我你希望继续哪部分。
 \> 来自 \<[https://chatgpt.com/c/69320b86-8d44-8325-8d43-1458078f817a](https://chatgpt.com/c/69320b86-8d44-8325-8d43-1458078f817a)\>
**Sequential Computing** **（顺序计算）**  
**精确建模的困难****￼**

![Exported image](Exported%20image%2020260730174718-0.png)

- 对顺序算法进行 **precise cost modelling****（精确代价建模）** 非常困难。
- 需要考虑的因素包括：
    - **Cache memory hierarchy**（缓存层次结构），
    - **Instruction scheduling**（指令调度），
    - **Compiler optimisations**（编译器优化），
    - 以及其他硬件/系统因素。
 
**顺序矩阵乘法示例**

- 经典实现（时间复杂度 Θ(n³)）：
![Exported image](Exported%20image%2020260730174719-1.png)

![Exported image](Exported%20image%2020260730174720-2.png)  

**更快的顺序矩阵乘法**

![Exported image](Exported%20image%2020260730174721-3.png)

- **Blocking / Tiling** **优化**（分块算法），减少 cache miss：
- 依然是 Θ(n³)，但在实际硬件上更快。

![Exported image](Exported%20image%2020260730174722-4.png)  

**简化建模方法**

![Exported image](Exported%20image%2020260730174743-5.png)

- 为了使建模问题可处理，需要一个简单的模型：
    - ==例如== **LLVM cost model**==：==
        - **free (0)**==：无代价指令，==
        - **basic (1)**==：基本代价指令，==
        - **expensive (4)**==：高代价指令。==
- ==真实硬件过于复杂，而== ==LLVM== ==模型过于简化。==
- ==我们需要借用== **complexity theory****（复杂性理论）** ==的思想。==
 
**A Brief Intro to Computational Complexity Theory**  
==（计算复杂性理论简介）==  
**核心问题**

![Exported image](Exported%20image%2020260730174749-6.png)

- 计算复杂性理论试图回答：
    - “哪些问题可以高效计算？”
    - “一个给定问题，最高效的计算方法是什么？”
- 这些问题看似简单，但答案并不容易。

**基本定义**

- 为了回答这些问题，必须先定义：
    - **A computer**（计算模型），
    - **A problem**（问题形式化），
    - **Efficiency**（效率标准）。
 
**k-tape Turing Machine** **（****k-****带图灵机）****￼**

![Exported image](Exported%20image%2020260730174751-7.png)

- 输入带（input tape）：只读。
- 输出带（output tape）：只写。
- 工作带（work tapes）：读写，可有 **k-2** 条，且长度无限。
- 每个 tape 单元存储来自有限字符集的符号，例如 {\>, 0, 1}。
- 图灵机包含一个寄存器，用于存储当前状态。

图示（示意）：  
\> 0 1 0 1 1 1 0 1 1 （input tape）￼\> 0 1 1 0 1 （work tape 1）￼\> 0 0 0 1 0 1 （work tape 2）￼  
好的 Leyan 👍 我来帮你把这一部分 **Computational Complexity Theory** 的介绍整理成课堂笔记风格，中英双语，并保留关键英文词汇。
 
**A Brief Intro to Computational Complexity Theory**  
（计算复杂性理论简介）￼

![Exported image](Exported%20image%2020260730174752-8.png)

**Turing Machine** **的建模特性**

- **Universal****（通用性）**：
    - 任意图灵机都可以被另一台图灵机模拟，方法是用磁带描述另一台图灵机的操作与状态。
- **Church–Turing thesis****（丘奇****-****图灵论题）**：
    - 虽未被严格证明，但普遍认为任何可计算函数都能在图灵机上计算。
 
**定义** **Problem** **与** **Algorithm**

- 在复杂性理论的语境下：
    - **Problem** 定义为：将某个 **input** 映射到 **output** 的函数（在图灵机磁带、符号和指令范围内）。
    - **Algorithm** 定义为：一组指令，用于根据图灵机的 **当前状态** 及其 **input/work tape**，计算对应的函数。

![Exported image](Exported%20image%2020260730174753-9.png)

**定义** **Efficient****（高效）**

- 没有唯一的定义，但复杂性理论提供了 **复杂性类（****complexity classes****）** 的层次结构：
    - 基于输入规模 **n** 下所需的渐近运算次数（time complexity）。
    - 或所需的渐近工作带大小（space complexity）。
- ==典型复杂性类：==
    - **PTIME (P)**==：所有能在== **多项式时间** **polynomial time** ==内解决的问题。==
        - 最优算法的运行时间至多为 **c·nᵏ**，其中 k 为常数。
    - **PSPACE**：在输入大小为 n 的情况下，所需空间不超过 **多项式规模** 的问题集合。
 
**进一步阅读**

- 在线资料：[Complexity Zoo](https://complexityzoo.net/)
- 书籍：_Computational Complexity: A Modern Approach_ by Sanjeev Arora, Boaz Barak （爱丁堡大学图书馆可获取数字版）
 
**Efficiently Solvable** **的定义****￼**

![Exported image](Exported%20image%2020260730174754-10.png)

- Alan Cobham 与 Jack Edwards 提出：
    - **所有属于** **PTIME** **或更低复杂性类的问题可视为** **efficiently solvable****（高效可解）**==。==
    - 更悲观的说法是 “feasibly computable（可行计算）”。
 
**从复杂性理论借鉴的思想****￼**

![Exported image](Exported%20image%2020260730174755-11.png)

- 需要为 **parallel computer****（并行计算机）** 建立模型。
- 区分 **time complexity** 与 **space complexity**，但两者在某些情况下可以相互转化。
- 关注 **最坏情况输入** 所需的渐近时间或空间资源。
- **目标**：力求多项式时间（polynomial time）或更优。
 
**Complexity Theory vs Parallel Algorithms**  
（复杂性理论 vs 并行算法）￼

![Exported image](Exported%20image%2020260730174801-12.png)  

|   |   |   |
|---|---|---|
|**方面**|**Computational Complexity Theory**|**Design & Analysis of Parallel Algorithms**|
|**Model**|Universal Turing Machine|Parallel Random Access Machine (PRAM)|
|**Space**|工作带大小（memory usage）|并行进程数量|
|**Time**|基本操作数量（basic ops）|串行操作数量|
|**Goal**|分类并理解哪些问题是理论上可高效解决的|分类并比较并行算法在实际机器上的可扩展性|
 
**Asymptotic Analysis** **（渐近分析）**

![Exported image](Exported%20image%2020260730174802-13.png)

- ==简化内存模型为== **RAM (Random Access Machine)**==：==
    - 假设所有内存访问代价相同（unit cost）。
- 不再精确计数操作，而是关注资源使用随 **problem size** 的 **增长率（****growth rate****）**。
- 当相同规模的输入运行时间不同：
    - 通常考虑 **worst case run-time**（最坏情况）。
    - 特殊情况可考虑 **average case run-time**（平均情况）。
   

**Asymptotic Analysis** **（渐近分析）**  
**简化计算模型**

- 使用 **RAM (Random Access Machine)** 模型：
    - 假设每次 **memory access** 代价相同（unit cost）。
- 精确计数操作难且无意义 → 关注 **增长率** **(growth rate)**。
- 运行时间在同一规模输入下可能不同：
    - 通常考虑 **worst-case run-time**（最坏情况）。
    - 特殊情况下考虑 **average-case run-time**（平均情况）。
 
**Asymptotic (“Big-O”) Notation****￼**

![Exported image](Exported%20image%2020260730174804-14.png)

- 用来描述函数随输入规模增大时的增长情况。
- 忽略常数因子（constant factors），例如：
    - 实际上 **10000n \> 10n**，但渐近分析认为都是 **O(n)**。

**定义**

- **Big-O (Upper Bound** **上界****)**
- ![Exported image](Exported%20image%2020260730174805-15.png)
- **表示** **f(n)** **不超过** **g(n)****。**
    - **g(n)** 是上界（upper bound）。
- **Big-Omega (Lower Bound** **下界****)**
- ![Exported image](Exported%20image%2020260730174806-16.png)
    - 表示 **f(n)** **不小于** **g(n)**。
    - **g(n)** 是下界（lower bound）。
- **Big-Theta (Tight Bound** **紧界****)**
- ![Exported image](Exported%20image%2020260730174807-17.png)
- **表示** **f(n)** **与** **g(n)** **渐近相等（常数因子范围内）。**
    - **g(n)** 是紧界（tight bound）。￼￼
 
以下是整理后的课堂笔记版本（中英双文对照、保留数学符号与关键术语、便于理解与考试复习）：
 
**Asymptotic Analysis****（渐近分析）**
 
**Big-O, Omega, and Theta Notation****（****O****、****Ω****、****Θ** **表示法）**
 
**1. Big Omega (Ω)** **表示法** **—** **下界（****Lower Bound****）**

![Exported image](Exported%20image%2020260730174809-18.png)

**2. Big Theta (Θ)** **表示法** **—** **紧确界（****Tight Bound****）**

![Exported image](Exported%20image%2020260730174829-19.png)

**总结表（****Summary Table****）**

|   |   |   |   |   |
|---|---|---|---|---|
|**符号**|**名称**|**含义**|**边界类型**|**示例**|
|( O(g(n)) )|Big-O|( f(n) ) 增长 **不快于** ( g(n) )|上界（Upper bound）|( n = O(n^2) )|
|( \Omega(g(n)) )|Big-Omega|( f(n) ) 增长 **不慢于** ( g(n) )|下界（Lower bound）|( n^2 = \Omega(n) )|
|( \Theta(g(n)) )|Big-Theta|( f(n) ) 与 ( g(n) )**增长同阶**|紧确界（Tight bound）|( 3n^2 + 2n = \Theta(n^2) )|
 
**要点总结（****Key Takeaways****）：**

- **Big-O** → 描述上界，算法复杂度的“最多”增长速度。
- **Big-Omega** → 描述下界，算法复杂度的“最少”增长速度。
- **Big-Theta** → 同时描述上下界，是算法复杂度的**准确量级**。
- 在渐近分析中，我们通常**忽略常数因子与低阶项**。
   

**Asymptotic Bounds** **总结****￼**

![Exported image](Exported%20image%2020260730174830-20.png)

- **Θ(N)**
    - 渐近与 N 相同，阶为 N（tight bound）。
- **Ω(N)**
    - 至少是 N，不小于 N（lower bound）。
- **O(N)**
    - 不超过 N，不比 N 更差（upper bound）。
 
**常见渐近复杂度**

![Exported image](Exported%20image%2020260730174832-21.png)

- **Θ(1) – Constant****（常数级）**
    - 例如固定次数操作。
- **Θ(log N) – Logarithmic****（对数级）**
    - 例如二分查找。
- **Θ(N) – Linear****（线性级）**
    - 例如单层循环遍历。
- **Θ(N²) – Quadratic****（二次方级）**
    - 例如双重循环（矩阵运算）。
 
**常见函数增长比较****￼**

![Exported image](Exported%20image%2020260730174833-22.png)

（图形展示函数随 N 增长的趋势）

- Constant（常数） → 水平线。
- Logarithmic（对数） → 缓慢增长。
- Linear（线性） → 匀速增长。
- Quadratic（二次方） → 快速增长。

第一部分：常见复杂度大小关系  
1 \< log n \< n \< n log n \< n² \< n³ \< nᵏ \< cⁿ \< nⁿ  
log 永远最小  
指数永远大于多项式  
nⁿ 永远爆炸级别
 
第二部分：for 循环（线性递增）  
for i = 1 to n: → 执行 n 次 → O(n)  
for i = 1 to n²: → O(n²)  
for i = 1 to n^k: → O(nᵏ)  
结论：看循环次数 = O(终值)
 
第三部分：嵌套循环（内外相乘）  
for i = 1..n  
for j = 1..n  
→ O(n·n) = O(n²)  
for i = 1..n  
for j = 1..n²  
→ O(n³)  
结论：嵌套 = 内层 × 外层
 
第四部分：while 循环三大模式

1. 等差递增：j = j + c￼执行 n/c 次 → O(n)
2. 等比增长：j = 2j￼执行 log n 次 → O(log n)
3. 减半：j = j/2￼也执行 log n 次 → O(log n)

结论：等差 n，等比 log n
 
第五部分：综合嵌套规律  
外层 n 次 × 内层 log n 次 → O(n log n)  
外层 n 次 × 内层 n 次 → O(n²)
 
第六部分：log 的常见重写  
log(n²) = 2 log n  
log(n³) = 3 log n  
log base 无所谓（换底公式）  
考试里只需要认：log n 是否比 n 小 → 是的，永远小
 
第七部分：多项式与指数比较  
2ⁿ \> n⁴ \> n³ \> n² \> n log n \> n \> log n  
结论：出现 2ⁿ 立刻判断最慢
 
第八部分：多个 term 取最大项  
T = n³ + n² + 100 → O(n³)  
T = 2ⁿ + n¹⁰ → O(2ⁿ)  
结论：Big-O 只看最大项
 
第九部分：加法 vs 乘法原则  
串行执行：加法 → O(f(n) + g(n)) = O(max(f,g))  
嵌套执行：乘法 → O(f(n)·g(n))  
（把程序结构看成树很有帮助）
 
第十部分：特殊套路  
当 i 每次加 2：  
i = i + 2 → 执行 n/2 次 → O(n)  
当 i 递增为平方：  
i = i² → 执行 log log n 次  
当有两个 while 从偶数/奇数访问：  
共计仍为 O(n)
**Scaling Laws** **（扩展规律）**

![Exported image](Exported%20image%2020260730174507-0.png)

- 对理解 **parallel performance**（并行性能）至关重要。
- 假设有一个问题需要解决：
    - 在 **1 core** 上运行时间是 **600 seconds (10 minutes)**。
    - 如果用 **2 cores**？运行时间会是 **300 seconds** 吗？也许。
    - 如果用 **600 cores**？运行时间会是 **1 second** 吗？不太可能……
- 为什么不可能？我们能预测需要多少时间吗？
 
**Scaling Laws —** **关键考虑**

![Exported image](Exported%20image%2020260730174508-1.png)

- 问题中实际存在多少 **parallelism****（并行性）**？
- 算法能暴露多少 **parallelism**？
- 硬件能利用多少 **parallelism**？
- 在这里，我们主要考虑 **第二点**（算法暴露的并行性），不过在真实情况下，结果取决于以上三点。

👉 接下来介绍 **Amdahl’s Law**。
 
**Amdahl’s Law** **（安姆达尔定律）****￼**

![Exported image](Exported%20image%2020260730174510-2.png)

- 一个关于 **parallel speedup under strong scaling**（强扩展下的并行加速比）的简单模型。
- **Strong scaling**：问题规模固定不变。
- 具体假设：
    - ==程序中有一部分== **α** ==是不可避免的== **serial****（串行）**。
    - ==其余部分== **(1 − α)** ==可以完全== **parallelisable****（并行化）**。
    - 并行部分的 **speedup** 与进程数 **P** 成线性关系。
    - ==串行运行时间记作== **Tₛ**==。==
 
**Amdahl’s Law —** **并行运行时间与加速比****￼**==The problem size is fixed (strong scaling): we run exactly the same total amount of work, just on more processors.==

![Exported image](Exported%20image%2020260730174511-3.png)  
![Exported image](Exported%20image%2020260730174512-4.png)  

- ![Exported image](Exported%20image%2020260730174513-5.png)
- → 加速比被 **串行部分的倒数** 所限制。
 
**Amdahl’s Law —** **实际偏差原因****￼**

![Exported image](Exported%20image%2020260730174514-6.png)

- 真实的强扩展结果可能不同，原因包括：
    - ==并行部分并非完全可并行化。==
        - ==如果所有并行任务是完全==独立的，那么加速比近似线性成立；如果不是，就会偏离。
    - 串行部分的估计不准确。
        - 串行比例 α 并不容易估算。
        - 串行部分可能并非固定，例如：随着进程数增加，会引入额外 **overheads****（开销）**，从而增大串行比例。

￼￼
 
**Gustafson’s Law** **（古斯塔夫森定律）****￼**

![Exported image](Exported%20image%2020260730174537-7.png)

- 一个关于 **parallel speedup under weak scaling**（弱扩展下并行加速比）的简单模型。
- 假设：**problem size**（问题规模）随着 **processes** **数量** **P** 线性增长。

**具体假设**

- 并行部分的运行时间与 **problem size** 成线性关系。
- 并行部分是完全 **parallelisable****（可并行化）**。
- 串行部分的运行时间在任意 **problem size** 与 **processes** **数量** 下保持常数。
- 总运行时间 **Tₛ** 对所有进程数保持不变。
- 与 **Amdahl’s Law** 不同，这里采用 **scaled speedup**（缩放加速比）：
    - 即比较单个进程完成 **P** **个进程工作量** 的运行时间与 **P** **个进程并行完成** 的运行时间。
 
**Gustafson’s Law —** **数学表达****￼**

![Exported image](Exported%20image%2020260730174538-8.png)

==所以这里实际上是让====n=p====来完成公式的推导的==
 
- ![Exported image](Exported%20image%2020260730174540-9.png) - ![Exported image](Exported%20image%2020260730174540-10.png) - ![Exported image](Exported%20image%2020260730174542-11.png) - ![Exported image](Exported%20image%2020260730174543-12.png) - ![Exported image](Exported%20image%2020260730174544-13.png)
- 特点：
    - ==与== **problem size N** ==无关（因我们设定== **P = N**==）。==
    - 当 **P → ∞** 时，￼
    - ![Exported image](Exported%20image%2020260730174558-14.png)
    - → 理论上可以得到 **无限加速比**！
       
**Gustafson’s Law —** **实际偏差原因****￼**

![Exported image](Exported%20image%2020260730174559-15.png)  
 - ![Exported image](Exported%20image%2020260730174600-16.png)
- 现实中的弱扩展结果可能不同，原因包括：
    - 并行部分并非完全可并行化。
    - 串行部分比例（serial fraction）估计不正确。
    - 尽管 Gustafson’s Law 考虑了 **problem size**，但仍假设串行部分在进程数与规模下保持常量，这在现实中并不总成立。
    - 问题规模并非严格线性扩展：
        - 例如，==输入参数翻倍时，实际问题规模可能是== **quadratic****（二次方增长）** ==而非线性增长。==
 
**Scaling Laws —** **综合模型**

![Exported image](Exported%20image%2020260730174601-17.png)

- **Amdahl’s Law** 和 **Gustafson’s Law** 都可以看作是更通用并行扩展模型的特例。
- ==一般的并行运行时间模型为：==
- ![Exported image](Exported%20image%2020260730174602-18.png)
- ==重要的综合公式==
    
**Gustafson’s Law —** **推导过程****￼**

![Exported image](Exported%20image%2020260730174603-19.png)  
![Exported image](Exported%20image%2020260730174604-20.png)  
![Exported image](Exported%20image%2020260730174611-21.png)  
![Exported image](Exported%20image%2020260730174612-22.png)

**￼****Scaling Laws —** **非理想情况一：对数型并行部分****￼**

![Exported image](Exported%20image%2020260730174613-23.png)  
 - ![Exported image](Exported%20image%2020260730174615-24.png)
- 想象一个程序：
    - 串行部分优化很差（poorly optimised serial component）。
    - 并行部分是强耦合的（tightly coupled），其扩展方式是 **logarithmic scaling****（对数型扩展）**，而不是随 **1/P** 线性缩减。
- 启示：并不是所有东西在并行化后都会更快。
    - 有时我们必须分布式处理问题，仅仅因为它太大了，无法放在单个节点中。
- 数学模型：￼
- ![Exported image](Exported%20image%2020260730174616-25.png)
- “Which log? Any log!”（对数的底数无关紧要）
    - 由于采用对数型扩展，得到 **T(1,N) = 0**，需要思考其实际含义。
    - 当 **P → ∞** 时，运行时间会如何？
- Speedup 公式：￼
- ![Exported image](Exported%20image%2020260730174617-26.png)

**Scaling Laws —** **非理想情况二：有限分割的工作量**

![Exported image](Exported%20image%2020260730174618-27.png)  
 - ![Exported image](Exported%20image%2020260730174635-28.png)
- ￼有些情况下，==特定问题规模下的工作量只能被分割有限次==。所以不同的规则无法被贯彻执行到底
- 这种情况非常常见，例如 **domain decomposition****（区域分解）**。￼
- ![Exported image](Exported%20image%2020260730174636-29.png)
- 数学模型：￼
- ![Exported image](Exported%20image%2020260730174637-30.png)
- ==特点：==
    - ==在进程数== **P ≤ N** ==之前，近似线性加速。==
    - ==当进程数== **P \> N** ==时，运行时间会出现== **指数级惩罚（****exponential penalty****）**==。====￼==
    - ![Exported image](Exported%20image%2020260730174638-31.png)
-   
    
- ![Exported image](Exported%20image%2020260730174639-32.png)
- ￼Speedup 公式：
- ![Exported image](Exported%20image%2020260730174640-33.png)
 
**Scaling Laws —** **反思与总结**

![Exported image](Exported%20image%2020260730174641-34.png)

- 这些模型真的有用吗？
    - 有时是的，如果你有理由相信 **Amdahl** 和 **Gustafson** 不适用，并且需要一个特定的预测模型。
- 但前提是：
    - 你需要清楚说明为什么选择这种 **T_S(N)** 和 **T(P,N)** 的形式。
- 大多数情况下，你应该直接比较 **Amdahl’s Law** 或 **Gustafson’s Law**。
- ==经典结论：==
    - **All models are wrong, but some are useful.**
    - ==理解它们的假设，有助于理解你自己的加速比结果。==
 
**Case Study: Flood Simulation** **（洪水模拟案例研究）**  
**模型特点****￼**

![Exported image](Exported%20image%2020260730174658-35.png)

- 使用 **gridded cellular model**（网格细胞模型）。
- 模拟 **water and erosion****（水与侵蚀）**。
- 问题：随着进程数 **P** 增加，洪水预测能更快吗？

![Exported image](Exported%20image%2020260730174659-36.png)  

**Strong Scaling** **案例****￼**

![Exported image](Exported%20image%2020260730174701-37.png)  

- 将整个网格划分给不同的 **tasks/processors (P)**。
- 问题规模固定，增加进程数，观察运行时间缩短情况。

![Exported image](Exported%20image%2020260730174702-38.png)

**Weak Scaling** **案例****￼**

![Exported image](Exported%20image%2020260730174703-39.png)

- 当问题规模（grid cells 数量）增加时，进程数 **P** 也随之增加。
- 保持 **每个** **CPU** **处理大约** **150** **万** **cells** 不变。
- 观察整体运行时间是否保持近似不变。
 
**并行化的现实挑战**

![Exported image](Exported%20image%2020260730174704-40.png)

- 并行化现有代码并非易事：
    - **Load imbalance**（负载不均衡）。
    - **Communication overhead**（通信开销）。
    - 需要深入理解代码，才能有效并行化。
 
**Summary — Scaling Laws** **总结**

![Exported image](Exported%20image%2020260730174705-41.png)

**Amdahl’s Law** **（强扩展，****Strong Scaling****）**

- 公式：￼
- ![Exported image](Exported%20image%2020260730174714-42.png)
- 假设：
    - 程序中有一部分 **α** 是串行的。
    - 串行部分的运行时间是常数。
    - 并行部分可以完全并行化。
    - **Problem size** 固定不变。
 
**Gustafson’s Law** **（弱扩展，****Weak Scaling****）**

- 公式：￼
- ![Exported image](Exported%20image%2020260730174715-43.png)
- 假设：
    - 程序中有一部分 **α** 是串行的。
    - 总运行时间在任意进程数下保持常数。
    - 并行部分可以完全并行化。
    - **Problem size** 随进程数线性扩展。

前面增加很快，但后面逐渐平坦，效果降低

![[Lec2 Scaling laws - Ink.svg]]

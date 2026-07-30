**Metrics for Parallel Algorithms** **（并行算法的度量标准）**  
**比较并行算法的方式**

![Exported image](Exported%20image%2020260730174921-0.png)

- 一种方式：假设 **𝑝** **=** **𝑛**，只关注运行时间（run-time），与最优的顺序算法（sequential algorithm）比较。
- 现实情况：大多数系统中 **𝑝**（进程数）是固定的，如果视为常数，在渐近分析中会消失。
- 我们希望算法 **随进程数增长而具备良好的可扩展性（****scalability****）**。
- 从 **scaling laws****（扩展定律）** 可知，进程数对性能影响显著。

![Exported image](Exported%20image%2020260730174922-1.png)

**将** **𝑝** **视为变量**

- 将==进程数== **𝑝** ==作为变量分析。==
- 目标：算法在 **𝑝** **增长时**仍表现良好。
- 可以在不同问题规模 **𝑛** 下分析。
- 也可同时变化 𝑛 和 𝑝（得到 **surface plots****（三维曲面图）**），但复杂度较高。
- 实际上，通常只考虑现实范围内的 **𝑝** **值**。
- 定义两个核心度量指标：**cost****（代价）** 与 **efficiency****（效率）**。
 
**Cost** **（代价）****￼**

![Exported image](Exported%20image%2020260730174923-2.png)

- 定义：==并行算法的== **代价** **C** ==为运行时间== **𝑇𝑃** ==与进程数== **𝑝** ==的乘积：==
- ![Exported image](Exported%20image%2020260730174924-3.png)
- 这相当于同时考虑了算法的时间复杂度和空间复杂度。
- 示例：**CRCW constant time sorting**（常数时间排序）虽然运行快，但 **代价昂贵**。
 
**Cost Optimality** **（代价最优）****￼**

![Exported image](Exported%20image%2020260730174925-4.png)

- 定义：如果一个并行算法的代价等于最优顺序算法的运行时间 **𝑇𝑆**，则称其为 **cost optimal****（代价最优）**。￼
- ![Exported image](Exported%20image%2020260730174935-5.png)
- 该指标会惩罚 **过度使用时间或进程数** 的算法。
- 示例：**CRCW constant time sorting** 虽然快，但 **并非代价最优**。
 
**Speedup** **（加速比）****￼**

![Exported image](Exported%20image%2020260730174936-6.png)

- 定义：并行算法的 **speedup S** 为最优顺序算法运行时间 **𝑇𝑆** 与并行算法运行时间 **𝑇𝑃** 之比：￼
- ![Exported image](Exported%20image%2020260730174937-7.png)
- 如果算法是 **代价最优的**，则其 speedup 满足：￼
- ![Exported image](Exported%20image%2020260730174938-8.png)

**Efficiency** **（并行效率）****￼**

![Exported image](Exported%20image%2020260730174939-9.png)

- 定义：**parallel efficiency E** 为 speedup 与进程数 **𝑝** 的比值：￼
- ![Exported image](Exported%20image%2020260730174940-10.png)
- 等价于 **顺序代价与并行代价之比**。
- 如果算法 **代价最优**，则：￼
- ![Exported image](Exported%20image%2020260730174941-11.png)

好的，我会把 **Scaling Down Efficiently** 和 **Brent’s Theorem** 部分整理成中英双语课堂笔记，保持要点清晰，并保留关键英文术语。￼￼  
**Metrics for Parallel Algorithms****（并行算法的性能度量）**

![Exported image](Exported%20image%2020260730174947-12.png)

- 在本课程中，我们主要关注算法的**渐近性能（****asymptotic performance****）**，￼即当问题规模 ( n \to \infty ) 时，运行时间和代价的增长趋势。
 
- ![Exported image](Exported%20image%2020260730174949-13.png)
 
**3. Example: Parallel Sorting Algorithm 1****（并行排序算法示例** **1****）**

![Exported image](Exported%20image%2020260730174950-14.png)

**4. Example: Parallel Sorting Algorithm 2****（并行排序算法示例** **2****）**

![Exported image](Exported%20image%2020260730174951-15.png)

- 此算法的通信/同步开销更高（log^2 n）。
- 因此，为保持代价与串行算法同阶，必须使用更少的处理器。￼￼==使得总开销与串行算法保持同阶，即可达到算法代价最优==
 
- ![Exported image](Exported%20image%2020260730174952-16.png)
- ￼故此算法**永远无法代价最优（****never cost-optimal****）**。
 
**6. Summary: Desirable Parallel Algorithm Properties****（理想并行算法特性）**
 
- **“log log n”** 通常记作：￼
- ![Exported image](Exported%20image%2020260730174953-17.png)
- 表示增长非常缓慢的函数，常用于分析极高并行度算法的复杂度。
    
**Scaling Down Efficiently** **（高效缩减并行度）****￼**

- ￼通常更容易先设计一个 **针对无限制进程数** **p** **的并行算法**，然后再缩减到实际可用的进程数 **p′**。
- 一种简单方法：**Round Robin Scheduling****（轮转调度）**
    - 每个真实进程（real process, 数量为 p′）执行多个逻辑进程（logical process, 数量为 p）的工作。
    - 每个真实进程依次为多个逻辑进程执行操作。
    - **运****行时间增加一个因子** **(\frac{p}{p′})**==，同时进程数减少到== ==p′====，因此== **代价** **cost** **保持不变**。￼
 
**Round Robin** **的问题****￼**

- **Naïve round robin** 会暴露 **非代价最优（****non cost optimal****）** 算法的低效性。
- 示例：之前的 **EREW summation algorithm**。
    - 在某些步骤中，部分进程没有操作（标记为 **“S” → slack****（空闲）**）。
    - 这些 slack 导致资源利用率低下。

![Exported image](Exported%20image%2020260730174954-18.png)

**去除** **Slack** **的可能性****￼**

- 在某些情况下，可以通过缩减非最优算法并 **去除** **slack** 来降低代价（cost）。
- 前提：必须 **不破坏算法中的依赖关系（****dependencies****）**。
- 示例：**EREW summation** 在仅使用 **两个处理器** 时接近 **代价最优（****cost optimal****）**。
- ==总结：是否能成功缩减，取决于== ==slack== ==的数量和分布==。

![Exported image](Exported%20image%2020260730175000-19.png)

**Brent’s Theorem** **（布伦特定理）**  
**m=Ts****￼****t=Tp****￼**

- **作用**：帮助判断某个 PRAM 算法是否有进一步优化的可能。
- **定理内容**：
    - 一个 PRAM 算法若需要 **t** **步（****time steps****）**，并总共执行 **m** **次操作（****operations****）**，
    - ==则在== **p** **个处理器** ==上可以在不超过以下时间内完成：==
    - ![Exported image](Exported%20image%2020260730175001-20.png)
    - ==注意事项：==
    - ==区分== **steps****（时间步）** ==与== **operations****（操作总数）**==。==
    - ==Brent’s Theorem== ==告诉我们可能存在更优的算法，但不会直接给出具体形式。==
    - **Round robin scheduling** ==和== **Brent’s Theorem** ==仅在== **CRCW-associative algorithms** ==的渐近分析中成立==。
 
好的，我会帮你把 **Brent’s Theorem** **应用于** **EREW summation** 部分整理成中英双语课堂笔记，保持公式和要点完整。
 
**Brent’s Theorem** **应用：****EREW Summation****￼**

![Exported image](Exported%20image%2020260730175002-21.png)

- 使用 **p = Θ(n)** 个处理器，运行时间 **Tₚ = Θ(log n)**。
- **代价（****Cost****）**：￼
- ![Exported image](Exported%20image%2020260730175003-22.png)
- 顺序算法的代价是 **Θ****(n)**。
- 结论：该算法 **不是** **cost optimal****（代价最优）**。
 
**应用** **Brent’s Theorem**

![Exported image](Exported%20image%2020260730175003-23.png)  
![Exported image](Exported%20image%2020260730175004-24.png)

**￼****Brent’s Theorem** **的启发**

- 思路：寻找更优算法。
- 优化方法：
    - 每个处理器先顺序求和 **log n** **个元素**（分组处理）。
    - 然后对所有处理器的 **部分和（****partial sums****）** 进行归约（reduction）。￼

- 新算法的运行时间：￼
- ![Exported image](Exported%20image%2020260730175005-25.png)
- 新算法的代价：￼
- ![Exported image](Exported%20image%2020260730175015-26.png)
- 当进程数满足：￼
- ![Exported image](Exported%20image%2020260730175016-27.png)
- 时，该算法是 **cost optimal****（代价最优）**。
- 结论：这与 Brent’s Theorem 的预测一致。
 
要不要我帮你把 **“****原始** **EREW Summation vs. Brent** **优化算法****”** 做一个对比表格（处理器数 / 运行时间 / 代价 / 是否 cost optimal），方便复习？
 \> 来自 \<[https://chatgpt.com/c/68dae8a1-b1c0-832e-aaee-c6601b222921](https://chatgpt.com/c/68dae8a1-b1c0-832e-aaee-c6601b222921)\>  

**1. Focus on Asymptotics****（关注渐近复杂度）**
 
**2. Example: Sequential Sort****（示例：串行排序）**
 
**解释（****Explanation****）：**
 
**5. Non–Cost-Optimal Example****（非代价最优示例）**
 
|   |   |
|---|---|
|**性质**|**含义**|
 ![Exported image](Exported%20image%2020260730175017-28.png)

✅ **最佳并行算法**同时满足高速度、代价最优和良好可扩展性。
 
**7. Notation Shortcut****（符号简写）**
    ![Exported image](Exported%20image%2020260730175018-29.png)  
![Exported image](Exported%20image%2020260730175020-30.png) ![Exported image](Exported%20image%2020260730175020-31.png) ![Exported image](Exported%20image%2020260730175022-32.png)   
**原始** **EREW Summation** **算法**
   
![Exported image](Exported%20image%2020260730175034-33.png) ![Exported image](Exported%20image%2020260730175035-34.png)   \> 来自 \<[https://chatgpt.com/c/68dae8a1-b1c0-832e-aaee-c6601b222921](https://chatgpt.com/c/68dae8a1-b1c0-832e-aaee-c6601b222921)\>

![[Lec5 Metrics and Brent’s Theorem - Ink.svg]]

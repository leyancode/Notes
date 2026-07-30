![Exported image](Exported%20image%2020260730182148-0.png)  

1111￼

![Exported image](Exported%20image%2020260730182149-1.png)

**分布式并行排序题型**  
**（****Class Test Cheat Sheet —** **完整答案模板）**
 
**1.** **必考概念：****PRAM vs Distributed Memory****（****MPI****）**  
**必背答案模板：**  
在 PRAM 模型中，所有处理器共享同一内存，因此

- 数据分布无需考虑
- compare-exchange 可以直接访问共享数组
- 并发读写由 PRAM 模型自动处理

而在分布式内存（MPI）中

- 每个处理器只能访问本地数据，没有共享内存
- compare-exchange 必须用显式通信实现
- 数据的分布、交换伙伴以及结果收集都需要明确说明

**因此，考题中若从** **PRAM** **迁移到** **MPI****，必须回答三个额外问题：**

1. 初始输入如何分布（scatter？already distributed？）
2. 中间阶段如何通信（send/recv？谁和谁交换？交换多少？）
3. 最终结果如何收集（gather？broadcast？distributed output？）

**1)** **初始输入如何分布？**  
**标准答案（英文）**：  
In the distributed-memory setting, each processor only has access to its ==local memory.==  
Therefore, we must specify how the initial input array is distributed:

- If all input resides on rank 0, we must explicitly scatter it to all processors￼(e.g., using MPI_Scatter or point-to-point sends).
- If the problem statement assumes an already balanced block distribution,￼then each rank initially holds n/p items.
- We must clearly state the distribution scheme (usually block distribution).
 
**2)** **中间阶段如何通信？**  
**标准答案（英文）**：  
In contrast to PRAM where processors access shared memory directly,  
in distributed memory processors must explicitly communicate to perform  
operations such as compare-exchange or compare-split.  
Thus we must specify:

- Which processors communicate with each other (e.g., odd-even neighbors).
- The amount of data to exchange (usually n/p items per compare-split).
- The communication primitive used (MPI_Sendrecv, MPI_Isend/Irecv, etc.).
- How processors merge and retain the correct half of the data.

Essentially, all shared-memory interactions must be replaced by explicit message passing.
 
**3)** **最终结果如何收集？**  
**标准答案（英文）**：  
We must specify how the final sorted result is represented:

- If only rank 0 must output the full sorted array, we gather all blocks￼using MPI_Gather or MPI_Gatherv.
- If each processor should retain a portion of the sorted array (distributed output),￼then no gathering is needed, but we must explicitly state this.
- If all ranks must have access to the final result, we may broadcast it￼using MPI_Bcast or perform all-gather.

The output distribution must be clearly stated.
 
**2.** **题型** **A****：解释** **compare-split** **的步骤** **+** **成本**  
**必背答案模板：**  
==compare-split(Pi, Pj)== ==的步骤：==

1. Pi 和 Pj 各自有 n/p 个有序元素
2. 交换整个块（each sends n/p items）
3. 合并两个长度 n/p 的有序数组 → 成本 Θ(n/p)
4. Pi 保留前 n/p 个（较小部分）
5. Pj 保留后 n/p 个（较大部分）

**整轮并行时间** **= Θ(n/p)**  
因为所有 pairs 同时执行。  
考试可能问：“为什么不是 Θ((n/p) log(n/p))？”  
答案：因为 merge 两个有序数组是线性的。
 
**3.** **题型** **B****：解释** **odd-even transposition sort** **在分布式系统中如何工作**  
这是最常见的考试题。  
**必背答案模板：**  
对 p 个处理器，每个存 n/p 个已排序的本地元素：

1. 执行 p 轮 odd-even sweeps
2. 奇数轮：奇数编号处理器与右邻交换并 compare-split
3. 偶数轮：偶数编号处理器与右邻交换并 compare-split
4. 每轮的通信成本为交换 n/p 元素，合并成本为 Θ(n/p)
5. 因此，整个并行阶段的时间为

![Exported image](Exported%20image%2020260730182210-2.png)  

**4.** **题型** **C****：为什么** **p = O(log n)** **才** **cost-optimal****？**  
考试最爱问。  
**必背答案模板：**  
在 p 个处理器中：

- compare-split 阶段需要 Θ(n) 时间
- cost = p · T = Θ(pn)

为了达到 cost-optimal（与最优顺序排序 n log n 相同阶）：

![Exported image](Exported%20image%2020260730182307-3.png)

等价地：

![Exported image](Exported%20image%2020260730182308-4.png)

**解释：**  
处理器数不能太大，否则通信代价主导，总成本比 n log n 差。
 
**5.** **题型** **D****：分布式排序的通用问题****（考试** **90%** **会出）**  
题目形式通常是：  
“给你一个共享内存/PRAM 的排序算法，请说明在分布式内存系统上需要考虑哪些额外问题。”  
**必背答案模板：**  
在分布式内存系统上必须考虑：

1. **数据初始分布**
    - 数据是否都在 rank 0？
    - 是否需要 MPI_Scatter？
    - 是否需要 block distribution（每个 rank n/p 个）？
2. **中间阶段的通信模式**
    - 哪些处理器配对交换？（odd-even pairing）
    - 交换的数据量是多少？（n/p 元素）
    - 用哪种通信？（send/recv 或 MPI_Sendrecv）
3. **最终输出的分布**
    - 结果是否要收集到 rank 0？（MPI_Gather）
    - 还是保持分布式输出（each rank owns a sorted block）？
    - 是否需要 broadcast？
4. **避免共享内存冲突****￼**PRAM 中 trivial 的 shared-memory writes 在 MPI 环境下需要通过通信实现。

以上三点必须全部回答，才是满分答案。
 
**6.** **题型** **E****：比较** **PRAM compare-exchange** **与** **MPI compare-split**  
**必背答案模板：**  
PRAM 中的 compare-exchange(i,j)：

- 单个元素交换，常数时间
- 直接访问共享数组

MPI 中的 compare-split(i,j)：

- 交换整个块，大小为 n/p
- 合并两个长度 n/p 的有序数组 → Θ(n/p)
- 必须显式通信（send/recv）
- Pi 保留小的一半，Pj 保留大的一半
 
**7.** **题型** **F****：输入、输出、通信量计算（老师爱考）**  
**必背答案模板：**

- 每一轮的消息大小：n/p
- 每个处理器每轮只收发一次 → 通信量 Θ(n/p)
- 共 p 轮 → 总通信 Θ(n)

如果题目问 bandwidth cost：

- 每轮 bandwidth cost = 2 × (n/p)￼（send + recv）
 
**8.** **题型** **G****：如果输入未排序，每个处理器如何预处理？**  
**必背答案模板：**  
每个处理器先本地排序 n/p 个元素：

![Exported image](Exported%20image%2020260730182309-5.png)

然后再进入 p 轮 odd-even compare-split。
 
**9.** **题型** **H****：解释图** **6.2****（****compare-split** **图）**  
可能会要求你解释图中的每一步：  
**必背解释：**  
Step 1: Pi 和 Pj 互相发送他们各自的块  
Step 2: 两者都拥有对方的数据，并分别与自己的合并  
Step 3: 排序后的合并数组被分成前半/后半  
Step 4: Pi 保留小元素，Pj 保留大元素￼￼￼  
**极背诵总结（最可能出现的** **5** **大考题）**  
如果你时间紧，只背这 5 个问题 + 模板答案就够了：  
**Q1****：在分布式环境实现** **PRAM** **比较交换算法，需要考虑哪些额外问题？**  
A：初始分布、通信伙伴和通信量、最终收集方式。  
**Q2****：解释** **compare-split** **并给出其时间复杂度。**  
A：交换 n/p 元素，merge 两个有序数组，成本 Θ(n/p)。  
**Q3****：为什么** **odd-even sort** **在** **p** **个处理器上的时间是** **Θ(n)****？**  
A：p 轮，每轮 compare-split 成本 Θ(n/p)，并行同步 → 总共 Θ(n)。  
**Q4****：为什么** **p = O(log n)** **才** **cost-optimal****？**  
A：cost = pn，不超过 n log n → p ≤ log n。  
**Q5****：****compare-exchange****（****PRAM****）和** **compare-split****（****MPI****）有什么区别？**  
A：PRAM 交换单元素；MPI 必须交换整个块并 merge，成本 Θ(n/p)。
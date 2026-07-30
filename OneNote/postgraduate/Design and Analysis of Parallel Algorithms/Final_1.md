给出2024和2023等近4年的试卷的试卷以及近4年的主要题型和答案，然后你再去回顾一下之前我给你的所有资料，请你帮我预测一下今年可能会出什么样子的题型，以及我该重点复习哪一方面，我明天就要考试了，我相信你可以预测，然后我们2022年课改，所以重点是2023和2024，但我相信你的感觉：  
**￼****￼****一、****Amdahl** **定律** **·** **强****/****弱扩展性** **· Speedup & Efficiency**  
**1. Amdahl** **定律** **&** **强扩展** **(Strong Scaling)****￼**

- **[2021 Q1(a)]****￼**Recall Amdahl’s Law for strong scaling.￼(a) Imagine a parallel program which exhibits perfectly linear strong scaling.￼What can you infer about the structure of the program, and the communication pattern between parallel processes? Explain your answer.￼￼

==A program with perfectly linear strong scaling must have an almost-zero serial fraction and negligible communication or synchronisation overhead.==  
==Its workload is fully parallelisable and perfectly balanced, with minimal data dependencies (essentially embarrassingly parallel).==￼

- **[2021 Q1(b)]****￼**Consider the below strong scaling results.￼Number of Cores | Runtime (s) | Parallel Speedup | Parallel Efficiency￼1 | 186.3636 | |￼2 | 102.6866 | |￼4 | 63.8433 | |￼8 | 41.7138 | |￼16 | 31.5816 | |￼32 | 27.5055 | |￼64 | 24.2415 | |￼128 | 23.8810 | |￼(b) State the formulae for parallel speedup and parallel efficiency. Calculate the parallel speedup and parallel efficiency of the program for each value of the number of nodes. You may use a spreadsheet program or calculator.￼

![Exported image](Exported%20image%2020260730184401-0.png)

**Calculations (T(1)=186.3636)**

|   |   |   |   |
|---|---|---|---|
|**Cores**|**Runtime (s)**|**Speedup S(P)=T1/Tp**|**Efficiency E(P)=S/P**|
|==1==|==186.3636==|==1.000==|==1.000==|
|==2==|==102.6866==|==1.815==|==0.907==|
|==4==|==63.8433==|==2.920==|==0.730==|
|==8==|==41.7138==|==4.469==|==0.559==|
|==16==|==31.5816==|==5.902==|==0.369==|
|==32==|==27.5055==|==6.780==|==0.212==|
|==64==|==24.2415==|==7.689==|==0.120==|
|==128==|==23.8810==|==7.807==|==0.061==|

- **[2021 Q1(c)]****￼**(c) Use this data to estimate the serial fraction of the code. Show your working.￼

![Exported image](Exported%20image%2020260730184403-1.png)

￼￼

![Exported image](Exported%20image%2020260730184403-2.png)

- **[2023 Q1(a)(i)]****￼**(a) Consider the results of a strong scaling experiment below.￼Number of processes | Runtime (sec) | Parallel Speedup | Parallel Efficiency￼1 | 745.45 | |￼2 | 410.75 | |￼4 | 255.37 | |￼8 | 166.86 | |￼16 | 126.33 | |￼32 | 110.02 | |￼i. State the formulae for parallel speedup and parallel efficiency. Calculate the parallel speedup and parallel efficiency for each value of the number of processes. You may use a calculator.￼

T(1)=745.45  
**Speedup & Efficiency**

|   |   |   |   |
|---|---|---|---|
|**Procs**|**Runtime**|**Speedup S(P)=T1/Tp**|**Efficiency E(P)=S/P**|
|1|745.45|1.000|1.000|
|2|410.75|1.815|0.907|
|4|255.37|2.920|0.730|
|8|166.86|4.467|0.558|
|16|126.33|5.903|0.369|
|32|110.02|6.778|0.212|

**These results have the same structure as the 2021 data: clear saturation and decreasing efficiency.**￼

- **[2023 Q1(a)(ii)]****￼**ii. How many processes would you recommend running this code on and why?￼

**How many processes should be used?**  
**Answer (exam-ready):**  
==Based on the efficiency results, I would recommend running the code on 8 or 16 processes. At 8 processes, efficiency is still reasonably high (~56%), and at 16 processes, efficiency remains acceptable (~37%).==  
==Beyond 16 processes, efficiency drops sharply (to ~21% at 32 processes), indicating diminishing returns and poor resource utilisation.==  
==Thus,== **8–16 processes** ==is the recommended range.==￼

- **[2023 Q1(a)(iii)]****￼**iii. Explain what a weak scaling experiment would entail.￼==Weak scaling keeps== **work per process constant**==, increases total problem size with P, and checks whether runtime stays constant.==￼￼
- **[2023 Q1(a)(iv)]****￼**iv. Briefly discuss whether these results are consistent with Amdahl’s Law.￼==Yes, the results are consistent with Amdahl’s Law. Speedup increases sub-linearly and eventually saturates, and parallel efficiency decreases as the number of processes grows. This behaviour indicates the presence of a non-zero serial fraction and increasing communication overheads, both of which are exactly what Amdahl’s Law predicts.==￼
 
**二、并行复杂度分析** **·** **成本** **(Cost) ·** **成本最优性** **· Brent** **定理**  
**1.** **一般复杂度** **&** **成本最优性判断****￼**  
**一个简单类比帮助理解**  
调和级数：  
1 + 1/2 + 1/3 + ... + 1/B ≈ log B  
增长较快。  
素数倒数：  
1/2 + 1/3 + 1/5 + 1/7 + ... ≈ log log B  
增长极慢，甚至 B=10^24，log log B 也不到 4。  
所以：  
素数倒数之和的增长速度 = log(log(B))
 
- **[2021 Q2(a)]****￼**Algorithm 1: Sieve of Eratosthenes (serial).￼(a) What is the asymptotic time complexity of this algorithm? Explain your result.￼
- ![Exported image](Exported%20image%2020260730184404-3.png)
-   
    
- **[2021 Q2(c)]****￼**(c) What is the asymptotic time complexity of your parallel algorithm? What is the cost? Explain your result.￼
- **[2021 Q2(d)]****￼**(d) Is your parallel algorithm cost optimal? Explain your result.￼**￼** ![Exported image](Exported%20image%2020260730184405-4.png)

**￼****￼**

![Exported image](Exported%20image%2020260730184406-5.png) ![Exported image](Exported%20image%2020260730184411-6.png)

- **[2021 Q2(e)]****￼**(e) Parallelise the outer loop of this optimised Sieve of Eratosthenes on a shared memory machine. What is the time complexity and cost of this parallel algorithm? Is it cost optimal? Explain your answer. Remember to state your hardware model.￼

![Exported image](Exported%20image%2020260730184412-7.png)  
![Exported image](Exported%20image%2020260730184413-8.png)

- **[2022 Q1(a)]****￼**(a) What is the serial time complexity of the 2D DFT?￼￼
- ![Exported image](Exported%20image%2020260730184414-9.png)
-   
    
- **[2022 Q1(d)]****￼**(d) What is the time complexity of your algorithm? Is it cost optimal?￼（针对 2D DFT 在 N×N mesh 上的并行算法）￼

![Exported image](Exported%20image%2020260730184415-10.png)

- **[2022 Q1(f)]****￼**(f) What is the time complexity of your algorithm? Is it cost optimal?￼（针对 2D DFT 在 ring 上的并行算法）
 ![Exported image](Exported%20image%2020260730184416-11.png)

￼￼

![Exported image](Exported%20image%2020260730184418-12.png)  
![Exported image](Exported%20image%2020260730184423-13.png)  
![Exported image](Exported%20image%2020260730184424-14.png)  

- **[2022 Q2(a)]****￼**(a) What is the time complexity of this algorithm? Explain your answer.￼（Dijkstra’s shortest distance algorithm）

- **[2022 Q2(c)]****￼**(c) Rewrite this algorithm to parallelise it so that it runs on p processors, where p \< N. What is the time complexity of this algorithm? For what values of p is it cost optimal?

![Exported image](Exported%20image%2020260730184425-15.png)

- **[2022 Q2(f)]****￼**(f) Modify Algorithm 2 to find the shortest distance from all nodes to every other node. What is the time complexity of your new algorithm? How would you trivially parallelise this over N processors while keeping the cost constant?

![Exported image](Exported%20image%2020260730184426-16.png)  
![Exported image](Exported%20image%2020260730184427-17.png)

- **[2023 Q1(b)(i)]****￼**(b) A serial algorithm for a single iteration of Conway’s Game of Life is presented in Algorithm 1.
- i. What is the asymptotic complexity of this algorithm? Explain your result. You can assume that M and N are of similar order.￼
- ![Exported image](Exported%20image%2020260730184428-18.png)

￼￼

- **[2023 Q1(b)(ii)]****￼**ii. Devise a parallel algorithm for a PRAM. Describe the choice of the variant of PRAM and number of processors used. What is the time complexity of your algorithm? Explain your result.
- **[2023 Q1(b)(iii)]****￼**iii. Is your algorithm cost-optimal? Explain your reasoning.￼
- ![Exported image](Exported%20image%2020260730184429-19.png)

- **[2023 Q1(b)(v)]****￼**v. What is the asymptotic complexity of your algorithm? Explain your result.￼（分布式 Game of Life 算法）

![Exported image](Exported%20image%2020260730184433-20.png)  
![Exported image](Exported%20image%2020260730184434-21.png)  

-   
    
- **[2023 Q2(a)]****￼**Algorithm 2: k-means clustering (serial).￼(a) Propose an algorithm for a CREW PRAM. What is its asymptotic time complexity? Explain you calculation. We assume k is a small constant and that the algorithm converges after L iterations (where L \<\< N).

![Exported image](Exported%20image%2020260730184436-22.png)

- **[2023 Q2(b)]****￼**(b) Is your algorithm cost-optimal? Explain your reasoning and how the choice of p may affect the outcome.

![Exported image](Exported%20image%2020260730184437-23.png)

- **[2023 Q2(c)]****￼**(c) Design an algorithm for a distributed architecture where processors are arranged in a 2D mesh topology. Describe a suitable decomposition, assuming p \< N. What is the algorithm’s asymptotic time complexity, including communication? State any further assumptions.

![Exported image](Exported%20image%2020260730184437-24.png)

- **[2024 WordCount Q(a)]****￼**Algorithm 1: Word count (serial).￼(a) State the asymptotic complexity of the sequential algorithm, assuming there are O(M) texts and O(N) words in each text. Explain your calculation and state any additional assumptions you are making. Briefly outline how the (word, count) pairs are stored.￼
- ![Exported image](Exported%20image%2020260730184439-25.png)
- ￼￼
- **[2024 WordCount Q(c)]****￼**(c) Analyse your algorithm. What is the asymptotic complexity? Is your algorithm cost-optimal? Explain your calculations.￼
- ![Exported image](Exported%20image%2020260730184440-26.png)
- ￼￼

- **[2024 WordCount Q(d)]****￼**(d) Define Brent’s theorem for PRAM and discuss whether your algorithm can be scaled down to obtain a cost-optimal solution.
 
- **[2024 MatrixMult Q(b)]****￼**Algorithm 2: Matrix multiplication (serial).￼(b) What is the asymptotic complexity of the serial algorithm shown in Algorithm 2? Explain your calculations and assumptions.￼
- ![Exported image](Exported%20image%2020260730184444-27.png) - **[2024 MatrixMult Q(c)]****￼**(c) Propose an algorithm for a distributed architecture with a network topology and routing scheme of your choice. Justify your choice and use pseudocode and diagrams to illustrate the algorithm. What is its asymptotic time complexity? Explain your calculation.
- **[2024 MatrixMult Q(d)]****￼**(d) Is your algorithm cost-optimal? Explain your reasoning and how the choice of the number of processes p may affect the outcome. For simplicity you may use the time complexity of Algorithm 2 as the serial time.

![Exported image](Exported%20image%2020260730184547-28.png)

**三、并行模型：****PRAM / CREW / CRCW · Shared vs Distributed****￼**

![Exported image](Exported%20image%2020260730184548-29.png)  
![Exported image](Exported%20image%2020260730184550-30.png)

**￼****￼****￼****1. PRAM / CREW / CRCW** **模型设计**

![Exported image](Exported%20image%2020260730184551-31.png) ![Exported image](Exported%20image%2020260730184552-32.png)

- **[2021 Q2(b)]****￼**(b) Devise a parallel version of this algorithm for a shared memory parallel machine. Describe your algorithm using pseudocode and diagrams. Clearly state the hardware model assumed by your algorithm.￼（Sieve of Eratosthenes 并行版本）￼
- ![Exported image](Exported%20image%2020260730184553-33.png)
      
- ![Exported image](Exported%20image%2020260730184557-34.png) ![Exported image](Exported%20image%2020260730184558-35.png)

- **[2021 Q2(e)]****￼**（同上，优化版 Sieve，在 shared memory 上并行 outer loop，并要求说明 hardware model）￼

**硬件模型（共享内存）**

- ==多个处理器可能同时写== ==primes[x] \<- false==
- ==写同一个值== ==false== ==是幂等写（====benign race====），在== ==CRCW== ==下合法====￼====￼==

![Exported image](Exported%20image%2020260730184600-36.png)  
![Exported image](Exported%20image%2020260730184601-37.png)  
![Exported image](Exported%20image%2020260730184602-38.png)  
![Exported image](Exported%20image%2020260730184603-39.png)  
![Exported image](Exported%20image%2020260730184604-40.png)  
![Exported image](Exported%20image%2020260730184608-41.png)

==￼====￼====￼====￼====￼====￼==**[2023 Q1(b)(ii)]**

![Exported image](Exported%20image%2020260730184610-42.png)  
![Exported image](Exported%20image%2020260730184611-43.png)  
![Exported image](Exported%20image%2020260730184612-44.png)

ii. Devise a parallel algorithm for a PRAM. Describe the choice of the variant of PRAM and number of processors used. What is the time complexity of your algorithm? Explain your result.￼（Game of Life）
 ![Exported image](Exported%20image%2020260730184614-45.png)  
![Exported image](Exported%20image%2020260730184615-46.png)

if (stats_enabled):  
total_num_alive = ==parallel_sum==(partial[0..P-1]) // reduction over P items  
print(total_num_alive)  
￼==因为是并行计算的，所以最后====logk====轮就可以全部计算完，时间复杂度就是====log k==￼￼￼第二种：用 idx，把二维网格“拉直”成一维（更利于分析与实现）￼

![Exported image](Exported%20image%2020260730184617-47.png)  
![Exported image](Exported%20image%2020260730184706-48.png)  
![Exported image](Exported%20image%2020260730184707-49.png)  
![Exported image](Exported%20image%2020260730184709-50.png)  
![Exported image](Exported%20image%2020260730184710-51.png)

￼￼￼

![Exported image](Exported%20image%2020260730184711-52.png) ![Exported image](Exported%20image%2020260730184712-53.png)

- **[2023 Q2(a)]****￼**

**一、****m[0]** **到底是什么意思？为什么一定要先算它？**  
**1. m[j]** **是什么**

- m 是一个长度为 k 的数组
- m[j] 表示 **第** **j** **个簇的质心（****centroid / mean****）**

- m[0]：第 0 个簇的中心
- m[1]：第 1 个簇的中心
- …
- m[k-1]：第 k−1 个簇的中心

注意：下标从 0 开始只是编程习惯，没有任何数学特殊含义。
 
**2. current_min_dist ← distance(items[i], m[0])** **在干什么？**  
因为你后面要做的是：
 
if dist \< current_min_dist then￼ current_min_dist ← dist￼ items[i].closest ← j  
那就必须先有一个 current_min_dist，否则你没法比较。
 
- ![Exported image](Exported%20image%2020260730184713-54.png) - ![Exported image](Exported%20image%2020260730184803-55.png) - ![Exported image](Exported%20image%2020260730184805-56.png)
- **￼**Propose an algorithm for a CREW PRAM.￼（k-means clustering）￼
- ![Exported image](Exported%20image%2020260730184806-57.png)

**￼****￼****(a1) CREW PRAM** **算法（每一轮）**  
设有 p 个处理器，编号 0..p-1，把 N 个点均匀分块，每个处理器负责约 N/p 个 items。  
**Step 1****：并行分配（无写冲突）**
 
==parallel for i = 0..N-1:====￼== ==best = 0====￼== ==bestDist = distance(items[i], m[0])====￼== ==for j = 1..k-1:====￼== ==d = distance(items[i], m[j]) //== ==并发读== ==m[j]== ==没问题====￼== ==if d \< bestDist:====￼== ==bestDist = d====￼== ==best = j====￼== ==items[i].closest = best //== ==每个== ==i== ==只写自己，满足== ==Exclusive Write==

- 每个 i 做 O(k)=O(1) 工作
- 并行后每个处理器做 N/p 个点
- 所以 Step 1 时间是 **O(N/p)**

**Step 2****：并行更新质心（用归约避免并发写）**

1. 每个处理器对自己那一块 items 先算本地部分和：
 ![Exported image](Exported%20image%2020260730184807-58.png)

- 每个处理器扫描 N/p 个 items
- 所以是 **O(N/p)**
- 把所有处理器的 local_sum[c], local_count[c] 做树形归约（reduction）：
- 第 1 轮：处理器 0 和 1 合并到 0；2 和 3 合并到 2；…
- 第 2 轮：0 和 2 合并到 0；4 和 6 合并到 4；…
- 共 **O(log p)** 轮
- 每一轮每个“接收者”写一次自己的位置，不存在并发写同一单元，满足 CREW

归约完成后，由一个处理器（例如 0）更新质心：
 
if r == 0:￼ m[c] = local_sum[c] / local_count[c]  
这一步是 O(1)。

- **O(N/p + log p)****￼**因为 k 是常数，所以对所有簇的总更新时间仍是：
- **O(N/p + log p)**

**合并：每轮总时间**

- Step1: O(N/p)
- Step2: O(N/p + log p)

- **T_iter(p) = O(N/p + log p)**

- **T(p) = O(L (N/p + log p))**

这是一个标准、满足 CREW 的 k-means PRAM 复杂度表达。

- 取 p = N，则每轮时间 O(1 + log N) = O(log N)
- 总时间 **O(L log N)**￼￼￼￼￼￼￼

- **[2024 WordCount Q(b)]****￼**
- ![Exported image](Exported%20image%2020260730184808-59.png)
- **注意****M****和****N****的差别，****WC[M][N]****和****WC[I]****完全不是一个东西，****N****是所有单词的数量**
- ![Exported image](Exported%20image%2020260730184809-60.png) - ![Exported image](Exported%20image%2020260730184811-61.png)
- **￼**(b) Based on Algorithm 1, design a parallel algorithm for a CREW PRAM and provide its pseudocode and any explanatory diagrams.（Word count）

![Exported image](Exported%20image%2020260730184846-62.png) ![Exported image](Exported%20image%2020260730184848-63.png)  
![Exported image](Exported%20image%2020260730184849-64.png)  
![Exported image](Exported%20image%2020260730184850-65.png)  
![Exported image](Exported%20image%2020260730184851-66.png)  
![Exported image](Exported%20image%2020260730184852-67.png)

- **[2024 WordCount Q(e)]****￼**(e) Can a better algorithm be obtained by using CRCW PRAM? Would a company working on text analytics be excited to hear about such an algorithm? Justify your answer.￼

**2. Shared Memory vs Distributed Memory** **对比****￼**

![Exported image](Exported%20image%2020260730184853-68.png)

- 所有 **偶数** **\> 2** **一定不是素数**
- ==所以数组== ==primes[]====：==
    - ==不再表示== ==0..B==
    - ==而是只表示== **奇数**
- 映射关系：
    - index i 表示数 2i + 1
    - 所以：
        - primes[0] 表示 1
        - primes[1] 表示 3
        - primes[2] 表示 5

这就是为什么数组长度是 B/2
 
**优化** **2****：从** **i²** **开始筛（时间）**  
为什么不从 2i 开始？

- 所有 \< i² 的倍数
- 一定已经被更小的素数筛过了

这是**经典筛法优化**，考试必提。

![Exported image](Exported%20image%2020260730184941-69.png)  
![Exported image](Exported%20image%2020260730184942-70.png)  

- **[2021 Q2(e)]**（shared memory 上优化 Sieve 外层循环）￼
- ![Exported image](Exported%20image%2020260730184944-71.png)

- **[2021 Q2(f)]****￼**(f) Parallelise the optimised Sieve of Eratosthenes to be implemented on a distributed parallel machine, such that the primes array is distributed. State clearly the network topology you assume, and the primitive operations used. Describe qualitatively how you would expect the scalability of this version to differ from the shared memory algorithm. Explain your answer.￼￼
- ![Exported image](Exported%20image%2020260730184945-72.png) - ![Exported image](Exported%20image%2020260730184946-73.png) - ![Exported image](Exported%20image%2020260730184947-74.png)
- ￼￼￼
- ![Exported image](Exported%20image%2020260730184948-75.png) - ![Exported image](Exported%20image%2020260730185008-76.png)
- **￼****[2022 Q2(d)]****￼**(d) While useful for shared-memory systems the PRAM model does not realistically scale to large numbers of processors. Let’s now consider a distributed architecture. What primitive communication operations does your distributed algorithm require? What are the expected time costs of each primitive communication operation you have used? What is the total communication time complexity? Make sure to state your network topology.
  - ![Exported image](Exported%20image%2020260730185009-77.png)
- **￼****[2023 Q1(b)(iv)]****￼**iv. Design an algorithm for a distributed memory machine with p \<\< M×N (i.e. where p is much smaller than M×N), where each processor handles a partition of the grid. Describe how the work is decomposed and assume statistics output is required and is done by process 0.￼（Game of Life 分布式）

- **[2023 Q2(c)]****￼**分布式 2D mesh 上的 k-means。

-   
    

- **[2024 MatrixMult Q(c)(e)]****￼**(c) 分布式矩阵乘法算法（自选拓扑和路由）￼(e) Discuss how a combination of shared and distributed memory parallelism could be exploited to improve the algorithm.
 
- **[2021 Q1(f)]****￼**(f) What is the asymptotic cost of the parallel matrix-vector product you have designed, assuming a 2D torus network topology, and store-and-forward routing? Explain your answer.
- **[2021 Q1(g)]****￼**(g) Give the asymptotic bound on the number of parallel processes required to simulate N qubits.￼（量子态向量存储与并行）
- **[2021 Q1(h)]****￼**(h) Design a parallel algorithm to calculate the matrix-vector product using the distribution described above, where every process has a compressed local copy of the matrix, and the vector is distributed across processes. Assume a ring topology for the network. You may assume the compressed matrix can be indexed in to as normal, in O(1) time. Remember that every process has a copy of A, but the result vector y, and the input vector x should be distributed across processes.
- **[2021 Q1(i)]****￼**(i) What is the time complexity of this algorithm assuming store-and-forward routing? Is it cost optimal?
- **[2022 Q1(b)]****￼**(b) An all–gather operation (equivalently, an all–to–all broadcast) has asymptotic costs of Θ(sp) on both ring and mesh networks with store–forward routing, where s is message size and p is the number of processes. Show why this is the case for a 2D mesh network.
- **[2022 Q1(c)]****￼**(c) Parallelise Algorithm 1 using N² processes arranged on a N×N mesh network with store–forward routing. Each process has 1 element of data initially. The result dft2d should end up on a single process. The order of operations is allowed to differ from the serial algorithm.￼（2D DFT on mesh）
- **[2022 Q1(e)]****￼**(e) Now parallelise Algorithm 1 using N processes arranged on a ring network with store–forward routing. Each process has N elements of data initially. The result dft2d should end up on a single process. The order of operations is allowed to differ from the serial algorithm.￼（2D DFT on ring）
- **[2022 Q2(d)]****￼**（Dijkstra 分布式版本的通信原语及其复杂度，含网络拓扑）
- **[2023 Q2(c)]****￼**（k-means 在 2D mesh 上，包括通信复杂度）
- **[2024 MatrixMult Q(c)]****￼**（要求自己选网络拓扑和路由来设计分布式矩阵乘法）
 
**五、线性代数类：矩阵****-****向量** **·** **矩阵乘法** **·** **量子态矢量** **· DFT**

- **[2021 Q1(d)]****￼**In matrix-vector multiplication Ax = y, the result vector y is calculated by,￼( y_i = \sum_j A_{i,j} x_j ).￼The serial algorithm for an m×n matrix and n×1 vectors has an asymptotic time complexity of Θ(mn). To do this operation on a distributed parallel machine, an algorithm is designed in which every process has a full copy of both the input vector x and the result vector y, while matrix A is distributed across processes.￼(d) Given the above, how would you partition and distribute the matrix A? Explain why you have chosen that partitioning.
- **[2021 Q1(e)]****￼**(e) Design a parallel algorithm which functions as described above, performing a matrix-vector product. You should use pseudocode and may use diagrams to explain your algorithm. Remember that every process should have a full copy of both vectors, and part of the matrix.
- **[2021 Q1(f)]****￼**（如上，计算该并行矩阵–向量乘的代价，在 2D torus + store-and-forward 下）
- **[2021 Q1(g)(h)(i)]****￼**量子状态向量存储和稀疏电路矩阵：￼(g) Give the asymptotic bound on the number of parallel processes required to simulate N qubits.￼(h) Design a parallel algorithm to calculate the matrix-vector product using the distribution described above, where every process has a compressed local copy of the matrix, and the vector is distributed across processes. Assume a ring topology for the network.￼(i) What is the time complexity of this algorithm assuming store-and-forward routing? Is it cost optimal?

- **[2022 Q1(a)]****￼**Algorithm 1: 2-dimensional Discrete Fourier transform.￼(a) What is the serial time complexity of the 2D DFT?
- **[2022 Q1(c)]****￼**(c) Parallelise Algorithm 1 using N² processes arranged on a N×N mesh network with store–forward routing. Each process has 1 element of data initially. The result dft2d should end up on a single process. The order of operations is allowed to differ from the serial algorithm.
- **[2022 Q1(d)]****￼**(d) What is the time complexity of your algorithm? Is it cost optimal?
- **[2022 Q1(e)(f)]****￼**(e) Parallelise Algorithm 1 using N processes on a ring;￼(f) What is the time complexity of your algorithm? Is it cost optimal?

- **[2024 MatrixMult Q(a)]****￼**Matrix multiplication is a common task in scientific and engineering simulations that run on modern high-performance clusters.￼(a) Name three application domains which make extensive use of matrix algebra.
- **[2024 MatrixMult Q(b)]****￼**(b) What is the asymptotic complexity of the serial algorithm shown in Algorithm 2? Explain your calculations and assumptions.
- **[2024 MatrixMult Q(c)]****￼**(c) Propose an algorithm for a distributed architecture with a network topology and routing scheme of your choice. Justify your choice and use pseudocode and diagrams to illustrate the algorithm. What is its asymptotic time complexity? Explain your calculation.
- **[2024 MatrixMult Q(d)]****￼**(d) Is your algorithm cost-optimal? Explain your reasoning and how the choice of the number of processes p may affect the outcome.
- **[2024 MatrixMult Q(e)]****￼**(e) Discuss how a combination of shared and distributed memory parallelism could be exploited to improve the algorithm.
 
- **[2021 Q2(a)]****￼**Algorithm 1: Sieve of Eratosthenes.￼(a) What is the asymptotic time complexity of this algorithm? Explain your result.
- **[2021 Q2(b)]****￼**(b) Devise a parallel version of this algorithm for a shared memory parallel machine. Describe your algorithm using pseudocode and diagrams. Clearly state the hardware model assumed by your algorithm.
- **[2021 Q2(c)]****￼**(c) What is the asymptotic time complexity of your parallel algorithm? What is the cost? Explain your result.
- **[2021 Q2(d)]****￼**(d) Is your parallel algorithm cost optimal? Explain your result.
- **[2021 Q2(e)]****￼**Algorithm 2: Optimised Sieve of Eratosthenes，time complexity O(B log log B)。￼(e) Parallelise the outer loop of this optimised Sieve of Eratosthenes on a shared memory machine. What is the time complexity and cost of this parallel algorithm? Is it cost optimal? Explain your answer. Remember to state your hardware model.
- **[2021 Q2(f)]****￼**(f) Parallelise the optimised Sieve of Eratosthenes to be implemented on a distributed parallel machine, such that the primes array is distributed. State clearly the network topology you assume, and the primitive operations used. Describe qualitatively how you would expect the scalability of this version to differ from the shared memory algorithm. Explain your answer.
 
- **[2022 Q2** **整题****]****￼**背景：加权图、邻接矩阵表示、Algorithm 2 为 Dijkstra’s shortest distance algorithm。￼关键小问：
    - **[2022 Q2(a)]****￼**(a) What is the time complexity of this algorithm? Explain your answer.
    - **[2022 Q2(b)]****￼**(b) Explain why it is significantly easier to parallelise the inner “for u = 0 to N−1” loop than the outer “while not all vertices have been visited” loop.
    - **[2022 Q2(c)]****￼**(c) Rewrite this algorithm to parallelise it so that it runs on p processors, where p \< N. What is the time complexity of this algorithm? For what values of p is it cost optimal?
    - **[2022 Q2(d)]****￼**(d) While useful for shared-memory systems the PRAM model does not realistically scale to large numbers of processors. Let’s now consider a distributed architecture. What primitive communication operations does your distributed algorithm require? What are the expected time costs of each primitive communication operation you have used? What is the total communication time complexity? Make sure to state your network topology.
    - **[2022 Q2(e)]****￼**(e) Assuming that the message sizes are constant and much smaller than N (e.g. m \<\< N), what number of processors will make this cost optimal?
    - **[2022 Q2(f)]****￼**(f) Modify Algorithm 2 to find the shortest distance from all nodes to every other node. What is the time complexity of your new algorithm? How would you trivially parallelise this over N processors while keeping the cost constant?
 
- **[2023 Q1(b)]****￼**Algorithm 1: Conway’s Game of Life (serial; single iteration)。￼相关小问：
    - **[2023 Q1(b)(i)]****￼**i. What is the asymptotic complexity of this algorithm? Explain your result. You can assume that M and N are of similar order.
    - **[2023 Q1(b)(ii)]****￼**ii. Devise a parallel algorithm for a PRAM. Describe the choice of the variant of PRAM and number of processors used. What is the time complexity of your algorithm? Explain your result.
    - **[2023 Q1(b)(iii)]****￼**iii. Is your algorithm cost-optimal? Explain your reasoning.
    - **[2023 Q1(b)(iv)]****￼**iv. Design an algorithm for a distributed memory machine with p \<\< M×N (i.e. where p is much smaller than M×N), where each processor handles a partition of the grid. Describe how the work is decomposed and assume statistics output is required and is done by process 0.
    - **[2023 Q1(b)(v)]****￼**v. What is the asymptotic complexity of your algorithm? Explain your result.
 
- **[2023 Q2(a)]****￼**Algorithm 2: k-means clustering (serial)。￼(a) Propose an algorithm for a CREW PRAM. What is its asymptotic time complexity? Explain you calculation. We assume k is a small constant and that the algorithm converges after L iterations (where L \<\< N).
- **[2023 Q2(b)]****￼**(b) Is your algorithm cost-optimal? Explain your reasoning and how the choice of p may affect the outcome.
- **[2023 Q2(c)]****￼**(c) Design an algorithm for a distributed architecture where processors are arranged in a 2D mesh topology. Describe a suitable decomposition, assuming p \< N. What is the algorithm’s asymptotic time complexity, including communication? State any further assumptions.
 
- **[2024 WordCount** **全题****]****￼**Algorithm 1: Word count (serial).
    - **[2024 WordCount Q(a)]****￼**(a) State the asymptotic complexity of the sequential algorithm, assuming there are O(M) texts and O(N) words in each text. Explain your calculation and state any additional assumptions you are making. Briefly outline how the (word, count) pairs are stored.
    - **[2024 WordCount Q(b)]****￼**(b) Based on Algorithm 1, design a parallel algorithm for a CREW PRAM and provide its pseudocode and any explanatory diagrams.
    - **[2024 WordCount Q(c)]****￼**(c) Analyse your algorithm. What is the asymptotic complexity? Is your algorithm cost-optimal? Explain your calculations.
    - **[2024 WordCount Q(d)]****￼**(d) Define Brent’s theorem for PRAM and discuss whether your algorithm can be scaled down to obtain a cost-optimal solution.
    - **[2024 WordCount Q(e)]****￼**(e) Can a better algorithm be obtained by using CRCW PRAM? Would a company working on text analytics be excited to hear about such an algorithm? Justify your answer.

![Exported image](Exported%20image%2020260730185010-78.png)

￼￼
    ![Exported image](Exported%20image%2020260730185012-79.png)  
![Exported image](Exported%20image%2020260730185013-80.png)

我们从 **串行算法** 开始，再到 **并行算法**，这样你会清楚“并行只是分摊了什么”。
 
**Step 1****：初始化部分**
 
for i = 0 to B−1 do￼ primes[i] ← true;￼end

- 循环执行 B 次
- 每次 O(1)

时间复杂度：

![Exported image](Exported%20image%2020260730185014-81.png)  

**Step 2****：主循环结构（关键）**
 
for i = 2 to B-1 do￼ j ← 2i;￼ while j \< B do￼ primes[j] ← false;￼ j ← j + i;￼ end￼end  
我们**不能一眼写** **loglogB**，必须拆。
 
**Step 2.1****：固定一个** **i****，看** **while** **循环跑几次**  
对某个固定的 i：

- j = 2i, 3i, 4i, …
- 直到 j \< B

次数大约是：

![Exported image](Exported%20image%2020260730185015-82.png)

也就是说：

- i = 2 → ~ B/2 次
- i = 3 → ~ B/3 次
- i = 10 → ~ B/10 次
- i = B/2 → ~ 2 次
 
**Step 2.2****：把所有** **i** **的工作量加起来**

![Exported image](Exported%20image%2020260730185050-83.png)                 
![Exported image](Exported%20image%2020260730185051-84.png) ![Exported image](Exported%20image%2020260730185052-85.png)   ![Exported image](Exported%20image%2020260730185053-86.png)     
![Exported image](Exported%20image%2020260730185054-87.png)      
**2. Brent** **定理与** **PRAM** **缩放**
 ![Exported image](Exported%20image%2020260730185055-88.png)           

for i = 2 to B-1 do  
parallel for p = 1 to P do  
start \<- max(2*i, ceil(L_p / i) * i)  
j \<- start  
while j \<= R_p do  
primes[j] \<- false  
j \<- j + i  
end  
end  
End￼

![Exported image](Exported%20image%2020260730185056-89.png)  
![Exported image](Exported%20image%2020260730185135-90.png)  
![Exported image](Exported%20image%2020260730185136-91.png)

==我采用== **PRAM-CRCW****（****common write** **或** **arbitrary write** **都可）**==：==
                   
￼￼￼￼￼￼￼￼￼￼￼￼￼￼  
在算法里：  
所以：
 
这一行的**核心作用只有一个**：  
给“当前最小距离”一个**合法的初始值**
         

时间：  
对每个簇 c=0..k-1（k 是常数，整体乘 k 仍是常数因子）：
 
这一步时间：
 
因此 Step 2（对单个 c）时间是：  
每轮 = Step1 + Step2  
所以每轮时间：  
总共 L 轮：  
如果你想用“最大并行度”给一个更具体的 bound：
       
￼￼￼  
**优化** **1****：只存奇数（空间** **+** **时间）**
 
因为：
   
![Exported image](Exported%20image%2020260730185137-92.png)        
![Exported image](Exported%20image%2020260730185138-93.png)  
![Exported image](Exported%20image%2020260730185140-94.png) ![Exported image](Exported%20image%2020260730185141-95.png)  
![Exported image](Exported%20image%2020260730185142-96.png)  

**四、通信原语** **·** **网络拓扑** **(ring / mesh / torus / all-gather)**
 
**1.** **矩阵****-****向量乘法** **&** **稀疏矩阵****/****量子模拟**  
**2. 2D DFT**  
**3.** **矩阵乘法** **(Matrix Multiplication)**
 
**六、数论与筛法：****Sieve of Eratosthenes**
 
**七、图算法：****Dijkstra** **最短路** **& All-pairs Shortest Paths**
 
**八、格点与元胞自动机：****Conway’s Game of Life**
 
**九、聚类与机器学习：****k-means** **并行**
 
**十、文本分析与计数：****Word Count** **并行化**

![[Final_1 - Ink.svg]]

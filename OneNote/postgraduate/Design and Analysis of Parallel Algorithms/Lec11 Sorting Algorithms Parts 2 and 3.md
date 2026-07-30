**Sorting: Parts 2 & 3**  
**Design & Analysis of Parallel Algorithms**  
（并行算法的设计与分析）  
**Total number of slides:** #
 
**Quicksort: Recap**  
（快速排序：回顾）

![Exported image](Exported%20image%2020260730181040-0.png)  

**CRCW Quicksort – Tree of Pivots**  
（CRCW 快速排序——枢轴树结构）

![Exported image](Exported%20image%2020260730181042-1.png)

左图展示了顺序 Quicksort 执行过程的概念性进展，其==中每个被圈出的节点为一个== **pivot****（枢轴）**。  
从另一种角度来看，这一过程实际上是在构建一棵 **pivot tree****（枢轴树）**，如右图所示。  
在 **CRCW****（****Concurrent Read, Concurrent Write****，并发读写）****Quicksort** 中，我们利用这棵枢轴树的思想作为并行化的基础。
 
**CRCW Quicksort**  
**核心思想：**

![Exported image](Exported%20image%2020260730181043-2.png)

在 **CRCW****（****arbitrary resolution****）****Quicksort** ==中，我们先构建一棵枢轴树（====pivot tree====）==，  
随后利用该树来计算每个元素的最终位置（**rank**），  
再根据这些排名将元素移动到正确的位置上。  
**实现方式：**

- 树结构通过数组索引进行描述，包含：
    - 一个共享变量 root（整棵树的根节点）
    - 数组 leftchild 和 rightchild（分别表示左右子节点）
    - 每个元素的局部变量 parent（指向其父节点）

**写入机制：**

- ==使用== **arbitrary write mechanism****（任意写机制）** ==来随机化== ==pivot== ==的选择。==
- ==对于重复值，会被随机分配到左右两侧，以保持工作负载平衡==。￼￼一开始进程可能会有冲突，并且只有一个，可以获胜去写pivot，but we don care, 无论谁赢，都将是第一个根

**复杂度：**

- 平均树深度（即平均运行时间）为 **Θ(log n)**。
 
**CRCW Quicksort – Implementation Notes**

![Exported image](Exported%20image%2020260730181044-3.png)

（CRCW 快速排序：实现要点）

- **Line 5****：** 使用 CRCW “arbitrary” 机制选出一个进程作为根节点（root process）
- **Line 13****：** 使用 CRCW “arbitrary” 机制选出新的 pivot
- **Line 19****：** 使用 CRCW “arbitrary” 机制选出另一个新的 pivot
- **进程终止点（****Processes stop taking part****）：**
    - 根进程（root process）在第 9 行停止参与
    - 子进程（child processes）在第 14 行或第 19 行停止参与
 ![Exported image](Exported%20image%2020260730181045-4.png)

￼￼￼==这个是通过编号来看，注意这里并不是从====0====开始，而是从====1====开始==。￼  
**CRCW Quicksort – Compute Ranks**  
（CRCW 快速排序：计算排名）
 
**树结构示意（****Tree Structure Example****）**  
1 2 3 4 5 6 7 8 : Element [ ]￼: Value { }￼leftchild rightchild￼leftchild rightchild￼
 
**Step 1 – Compute Sub-tree Sizes**  
（步骤 1 – 计算子树大小）  
在已知 **pivot tree****（枢轴树）** 的情况下，我们分两步计算每个节点的 **rank****（排名）**：

![Exported image](Exported%20image%2020260730181131-5.png)

1️⃣ **计算每个节点的子树大小（****sub-tree size****）**

- 对于每个节点 _i_，分别计算其左右子树的节点数，并存储在数组：
    - leftsize[i]
    - rightsize[i]
- 即：leftsize[i] 表示以 _i_ 为根的左子树包含的节点数量，￼rightsize[i] 表示右子树的节点数量。
 
**Step 2 – Compute Ranks by Sweeping the Tree**  
（步骤 2 – 通过树的扫描计算排名）

- 通过 **从根到叶（****from root to leaves****）** 的一次遍历（sweep），为每个节点分配其最终的 **rank**。
- 该过程的时间复杂度与树的深度成正比，平均为 **Θ(log n)**。

![Exported image](Exported%20image%2020260730181133-6.png)

**Computing Sub-tree Sizes**  
（计算子树大小的原理）

![Exported image](Exported%20image%2020260730181134-7.png)

1️⃣ 将树中的每条边（edge）看作两条边：

- 一条“向下”（down）
- 一条“向上”（up）
-   
    
 
2️⃣ 在执行 **深度优先遍历（****depth-first traversal****）** 时，￼==左边优先==￼  
可以将遍历经过的边序列视为一个列表（list）：

- ==“====向下====”====边标记为== **0**
- ==“====向上====”====边标记为== **1****￼**
![Exported image](Exported%20image%2020260730181135-8.png)  
 ![Exported image](Exported%20image%2020260730181136-9.png)  
![Exported image](Exported%20image%2020260730181137-10.png)      
3️⃣ **子树大小的计算方法：****￼**

![Exported image](Exported%20image%2020260730181138-11.png)  
![Exported image](Exported%20image%2020260730181240-12.png)

**￼****参照前图直接相加就行，非常简单**

![Exported image](Exported%20image%2020260730181241-13.png)  
 - ![Exported image](Exported%20image%2020260730181242-14.png) - ![Exported image](Exported%20image%2020260730181243-15.png)
- 对于某一子树，其大小等于列表中“从进入该子树的向下边（down edge）到离开该子树的向上边（up edge）之间（含该边）”出现的 **向上边数量（****up edges****）**。
- 具体计算方式：
    - 对整个序列执行一次 **prefix addition****（前缀加法）** 操作；
    - 然后对每个节点，用“离开时的计数”减去“进入时的计数”，即可得到该子树的大小。
 
下面是你这部分课堂笔记的中英双语整理版，格式与前面一致，保留关键英文术语如 _prefix, processor, compare-exchange, diameter, cost-optimality_ 等，以便技术准确：
 
**Example: Computing Sub-tree Sizes**  
（示例：计算子树大小）

- 例如：
    - 含有 **33** 的上层节点（upper node）作为根节点，其子树大小为 **5**，即 _(5 - 0)_。
    - 含有 **82** 的节点作为根节点，其子树大小为 **2**，即 _(7 - 5)_。
- 对于根节点（root），这是一个特殊情况：￼其子树大小为 **最终** **prefix** **值** **+ 1**。
 
**Sorting: Part 3**  
**Design & Analysis of Parallel Algorithms**  
（并行算法的设计与分析）  
**Total number of slides:** #
 
**Parallel Sorting with Message Passing**  
（基于消息传递的并行排序）

![Exported image](Exported%20image%2020260730181244-16.png)

对于 **message passing****（消息传递）算法**，还需要额外考虑如何定义“排序问题”。  
当系统中存在多个独立内存时，**数据排序（****data sorted****）** 的定义是什么？  
**假设条件：**

- ==每个== **processor****（处理器）** ==初始时各自存储相等数量的数据项，即每个处理器拥有约== _n/p_ ==个元素；==
- 排序的目标是重新分配这些元素，使得：
    - 每个处理器依然持有 _n/p_ 个元素；
    - 但数据整体上已按顺序排列，例如：
        - ==最小的== _n/p_ ==元素在== **processor 0** ==上==；
        - 接着较大的部分依次分配给 processor 1, 2, …

**理论下界：**  
==系统网络的== **diameter****（直径）**==——====即任意两个节点间的最短路径中的最长者====——==  
==定义了算法运行时间的==**下界（****lower bound on run time****）**==。==
 
**Odd-Even Transposition Sort**  
（奇偶换位排序）

- 该算法是顺序 **Bubblesort****（冒泡排序）** 的一种并行变体，￼映射到一维处理器阵列（1-D array）上实现。
- 它通过一系列**不重叠的相邻交换（non-overlapping neighbour swaps）**并行完成排序。
- 其核心操作是 **pairwise compare-exchange****（成对比较****-****交换）** 步骤。

![Exported image](Exported%20image%2020260730181245-17.png) ![Exported image](Exported%20image%2020260730181246-18.png)  
![Exported image](Exported%20image%2020260730181430-19.png)

**Alternating Swap Windows**  
（交替交换窗口）  
为了使元素能够向正确方向移动（从无序到有序），  
需要交替改变“交换窗口（swap window）”的位置。  
这确保了算法在多轮比较-交换过程中能够持续推进，  
而不会出现死锁或停滞的情况。

![Exported image](Exported%20image%2020260730181431-20.png)

**Analysis**  
（算法分析）

![Exported image](Exported%20image%2020260730181432-21.png)  
![Exported image](Exported%20image%2020260730181433-22.png)  
 - ![Exported image](Exported%20image%2020260730181434-23.png)
- ￼当使用 **n** **个处理器（****processors****）** 时，￼==该算法的运行时间在该体系结构上是==**最优的（****optimal****）**==。==￼但从**代价（cost）**角度看，它**不是代价最优（not cost-optimal）**的。

![Exported image](Exported%20image%2020260730181436-24.png)

**Using p \< n Processors**

![Exported image](Exported%20image%2020260730181437-25.png)

（当使用 p \< n 个处理器时）

- 每个处理器需顺序排序 _n/p_ 个元素；
- 系统运行 **p** **次迭代（****iterations****）**，￼将顺序的 **compare-exchange** 操作替换为 **compare-split****（比较****-****分割）** 操作；
- 每次操作都消耗一定的时间开销。
 
**Ensuring Asymptotic Cost-Optimality**  
（保证渐近代价最优性）  
为了确保算法在渐近意义下是**代价最优（asymptotically cost-optimal）**的，  
必须满足一定的关系条件（即：_时间复杂度_ _×_ _处理器数_ _≈_ _顺序算法复杂度_）。
 ![Exported image](Exported%20image%2020260730181501-26.png)

![[Lec11 Sorting Algorithms Parts 2 and 3 - Ink.svg]]

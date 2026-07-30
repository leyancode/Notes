**Sorting: Case Study**  
课程：**Design & Analysis of Parallel Algorithms**  
主题：**Sorting****（排序）** **– Part 1**
 
**1. Sorting** **概述**

![Exported image](Exported%20image%2020260730180539-0.png)

- Sorting（排序）是一个**简单但极具代表性**的问题，用于将一组元素按照某种顺序（例如升序 ascending、降序 descending、字典序 lexicographic）进行排列。
- 研究排序问题可以帮助我们应用多种算法分析与优化技术。
- 问题定义：给定一组可比较的元素集合 ( S = { a_1, a_2, …, a_n } )，以及一个排序关系（ordering relation），生成一个有序集合￼
- ![Exported image](Exported%20image%2020260730180606-1.png)
- 并且 ( S' ) 是 ( S ) 的一个排列（permutation）。
- 本课程只考虑**整数（****integers****）**，但所使用的技术对所有**可比较对象（comparable entities）**都通用。
- 顺序排序的时间复杂度（sequential time complexity）：￼
- ![Exported image](Exported%20image%2020260730180607-2.png)
-   
    
 
**2. Sequential Sorting Algorithms****（顺序排序算法）****￼**

![Exported image](Exported%20image%2020260730180609-3.png)

1. **Enumeration Sort****（枚举排序）**
    - 基本思想：先计算每个元素的最终位置（rank），然后根据 rank 把元素放到正确的位置。
    - 时间复杂度：￼
    - ![Exported image](Exported%20image%2020260730180611-4.png)
    - Mergesort（归并排序）
    - 一种典型的 **divide & conquer****（分治）** 算法，主要工作在 **combine****（合并）** 步骤中完成。
    - 总体复杂度：￼
    - ![Exported image](Exported%20image%2020260730180612-5.png)
    - Quicksort（快速排序）
    - 同样是 **divide & conquer** 算法，但主要工作在 **divide****（划分）** 步骤中。
    - 核心思想：选择一个 **pivot****（基准值）**，将数据分为“小于或等于 pivot”与“大于 pivot”的两部分。
    - pivot 的选择至关重要。
    - 平均时间复杂度：￼
    - ![Exported image](Exported%20image%2020260730180613-6.png)
    - 最坏情况：￼
    - ![Exported image](Exported%20image%2020260730180615-7.png)
    - 若采用随机化 pivot 选择（randomized pivot choice），则几乎总能得到良好的性能表现。
 
**3. Enumeration Sort****（枚举排序）****￼**

![Exported image](Exported%20image%2020260730180700-8.png)

**算法思想：**  
对每个元素 ( a[i] )，计算有多少个元素比它小（即它的 rank），再根据 rank 将元素放到正确位置。  
**伪代码（****Pseudocode****）：**  
enumeration_sort(a, len)￼ for i = 0 to len￼ for j = 0 to len￼ if (a[j] \< a[i] || (a[j] == a[i] && i \< j))￼ r[i] = r[i] + 1￼ for k = 0 to len￼ b[r[k]] = a[k]￼ return b￼  
**时间复杂度：** Θ(n²)￼

![Exported image](Exported%20image%2020260730180701-9.png)

**示例：**  
输入：  
a = [7, 1, 5, 3, 4, 6, 2]￼r = [0, 0, 0, 0, 0, 0, 0]￼  
比较结果：  
7: r = [6,0,0,0,0,0,0]￼1: r = [6,0,0,0,0,0,0]￼5: r = [6,0,4,0,0,0,0]￼3: r = [6,0,4,2,0,0,0]￼4: r = [6,0,4,2,3,0,0]￼6: r = [6,0,4,2,3,5,0]￼2: r = [6,0,4,2,3,5,1]￼  
根据 rank 进行重新排列：  
b[r[k]] = a[k]￼→ b = [1, 2, 3, 4, 5, 6, 7]￼  
输出结果（0-indexed）：  
**[1, 2, 3, 4, 5, 6, 7]**
 
**4. Mergesort****（归并排序）****￼**

![Exported image](Exported%20image%2020260730180702-10.png)

**算法思想：**

- 将输入序列不断分割（split）为小块，直到每块容易排序（例如只含一个元素）；
- 然后在合并阶段（combine）逐步合并成整体的有序序列。

**伪代码（****Pseudocode****）：**  
merge_sort(a)￼ (l, r) = split(a)￼ sl = merge_sort(l)￼ sr = merge_sort(r)￼ b = combine(sl, sr)￼ return b￼  
**时间复杂度：** ==Θ(n log n)==
 
**5. Mergesort** **示例（****Example****）**  
**示例（****Example****）**：￼

![Exported image](Exported%20image%2020260730180703-11.png)

给定输入数组 a = [7, 1, 5, 3, 6, 4, 2]  
请思考（Exercise）：

- 设计 **split**（分割）与 **combine**（合并）函数的算法。
    - **split(a)**：将数组分为左右两部分。
    - **combine(l, r)**：将两个有序子序列合并为一个整体有序序列。
 
**6. Quicksort****（快速排序）****￼**

![Exported image](Exported%20image%2020260730180704-12.png)

**基本思想：**

- ==根据选定的基准值== **pivot**==，将输入划分为两个子集合：==
    - **l**==：所有小于或等于== ==pivot== ==的元素==
    - **r**==：所有大于== ==pivot== ==的元素==

**伪代码（****Pseudocode****）：**  
quicksort(a)￼ (l, r) = divide(a)￼ sl = quicksort(l)￼ sr = quicksort(r)￼ b = concatenate(sl, p, sr)￼ return b￼  
**时间复杂度（****Time Complexity****）：**

- 平均情况（average case）： ( O(n \log n) )
- 最坏情况（worst case）： ( O(n^2) )

**思考问题（****Question****）：**  
在什么情况下会出现最坏情况？  
→ 当每次选择的 **pivot** 恰好是当前子序列中的最小值或最大值时（即划分极不平衡），会导致最坏性能。
 
**7. Quicksort** **示例（****Example****）****￼**

![Exported image](Exported%20image%2020260730180706-13.png)

**示例：**  
输入 a = [7, 1, 5, 3, 6, 4, 2]  
**练习（****Exercise****）：**  
设计以下三个辅助算法：

- **divide(a)**：将数组按 pivot 值划分为左右两部分。
- **append(x, list)**：在序列尾部添加元素。
- **concatenate(l, p, r)**：将三部分连接为完整序列。

💡 _提示：考虑使用_ _in-place swapping__（原地交换）优化内存使用。_
 
**8. PRAM Algorithms Reminder****￼**

![Exported image](Exported%20image%2020260730180707-14.png)

**PRAM****（****Parallel Random Access Machine****）并行算法回顾**

- PRAM 是一种**共享内存并行抽象模型**，忽略同步与通信开销，但允许任意数量的处理器并行工作。
- ==PRAM== ==模型根据==**读写并发性**==的不同进行区分：==
    - **E** === Exclusive====（独占）==
    - **C** === Concurrent====（并发）==
    - **R/W** === Read / Write==

==由此产生以下模型类型：==

- **EREW**==：====Exclusive Read, Exclusive Write====（最严格）==
- **CREW**==：====Concurrent Read, Exclusive Write====（允许并发读）==
- **ERCW**==：====Concurrent Write====（通常不考虑）==
- **CRCW**==：====Concurrent Read, Concurrent Write====（最宽松）==
 
**9. CREW Mergesort****（并行归并排序）****￼**

![Exported image](Exported%20image%2020260730180733-15.png)

**思想核心：**

- ==在== **CREW PRAM** ==模型下，将归并排序的== **divide-and-conquer** ==结构并行化。==
- ==同时并行化== ==merge== ==阶段，避免在根节点（====root node====）出现瓶颈。==

**关键观察（****Key Observation****）：**  
==在合并两个序列时：==  
==每个元素在合并后的序列中的== ==rank === ==自身在本序列的== ==rank +== ==在另一序列中的== ==rank==  
**实现思路：**

- 为每个元素分配一个处理器（processor per item）来计算其 rank。
- 通过 **binary search****（二分查找）** 计算其在另一序列中的 rank。
- 并行二分查找依赖于 **CREW PRAM** 模型，以允许多个处理器同时读取数据。
- 若存在重复元素（duplicate elements），算法需作额外调整以保证稳定性。
 
**10. Analysis of CREW Mergesort****（性能分析）**

![Exported image](Exported%20image%2020260730180734-16.png)  
![Exported image](Exported%20image%2020260730180735-17.png)  
![Exported image](Exported%20image%2020260730180736-18.png)  
![Exported image](Exported%20image%2020260730180737-19.png)      
这样时间复杂度是最低了，但是空间复杂度非常高，没有达到cost optuimal
 ![Exported image](Exported%20image%2020260730180738-20.png)  

**11. CREW Mergesort Cost Optimality****（成本最优性分析）****￼**

![Exported image](Exported%20image%2020260730180739-21.png)  
![Exported image](Exported%20image%2020260730180853-22.png)     
![Exported image](Exported%20image%2020260730180854-23.png)     

**12. CRCW Quicksort — Tree of Pivots****（并发读写快速排序：枢轴树思想）****￼**

![Exported image](Exported%20image%2020260730180854-24.png)

在左图中，我们看到**顺序** **Quicksort** 的执行过程，==圆圈标记的部分表示每一步选取的== **pivot****（基准值）**。  
另一种理解方式是：==这个过程实际上在==**构建一棵** **pivot** **树（****tree of pivots****）**。  
在 **CRCW****（****Concurrent Read, Concurrent Write****）****Quicksort** 中，我们以 pivot 树为并行化的核心思想基础。  
每个 pivot 节点对应一个分区操作，从而形成层次化的并行划分。
 
**13. CRCW Quicksort** **原理（****Principles****）**

![Exported image](Exported%20image%2020260730180856-25.png)

在 **CRCW Quicksort****（****arbitrary resolution model****）** 中，算法主要分为以下步骤：

1. **构建** **pivot** **树（****Build Pivot Tree****）**：￼通过“任意写入”（arbitrary write）机制选取多个 pivot，形成一棵层次化的 pivot 树。
2. **计算最终位置（****Compute Ranks****）**：￼使用 pivot 树确定每个元素的**最终秩（****rank****）**，然后将其移动到正确位置。

**数据结构（****Data Structures****）：**

- ==root====：全局变量，表示整棵== ==pivot== ==树的根节点；==
- ==leftchild[], rightchild[]====：数组，记录每个节点的左右子节点索引；==
- ==parent====：每个进程的局部变量，指示当前元素的父节点。==

**关键特性（****Key Features****）：**

- ==使用== ==CRCW “arbitrary”== ==机制随机化== ==pivot== ==选择，以平衡负载；==
- ==对重复值（====duplicate values====）进行随机分配，使左右子树大小更均衡；==
- ==平均树深度与运行时间为：==
- ![Exported image](Exported%20image%2020260730180857-26.png)

**14. CRCW Quicksort** **执行逻辑（****Execution Steps****）****￼**

![Exported image](Exported%20image%2020260730180858-27.png)

**算法中关键并发步骤（****Key Parallel Operations****）：**

- **Line 5**：利用 CRCW “arbitrary” 机制选出一个进程作为根节点（root）。
- **Line 13 & 19**：分别在左右子树中选取新的 pivot。
- **Line 9**：根进程完成后退出计算。
- **Line 14/19**：左右子树的子进程在完成本层分区后退出。

通过这种机制，每一层的 pivot 选择与分区操作均可**并行进行**，从而形成多层次并行执行的 pivot 树。

![Exported image](Exported%20image%2020260730180859-28.png)

**15. CRCW Quicksort – Compute Ranks****（计算秩的并行过程）**

![Exported image](Exported%20image%2020260730180935-29.png)

在 pivot 树构建完成后，算法继续进行**秩（****rank****）计算阶段**。  
整个计算分为两个主要步骤：

1. **计算每个节点的子树大小（****Compute Subtree Sizes****）**
    - 对于树中每个节点 _i_，计算其左子树与右子树的节点数量。
    - 结果分别存储在数组 leftsize[i] 与 rightsize[i] 中。
2. **从根向叶遍历计算秩（****Sweep Down the Tree****）**
    - 从根节点（root）开始，沿着 pivot 树向下遍历，￼根据上层的子树大小累积偏移量（offset），￼从而为每个节点分配一个最终秩（rank）。

**时间复杂度（****Time Complexity****）：**

- 整个秩计算阶段的复杂度与树的深度成正比：￼
- ![Exported image](Exported%20image%2020260730180936-30.png)
- 由于树深度期望为 Θ(log n)，因此整体并行执行时间也为 Θ(log n)，￼达到了极高的并行效率。

![Exported image](Exported%20image%2020260730180937-31.png)   
**16. CRCW Quicksort – Compute Ranks****（并发快速排序中的秩计算）**  
在 **CRCW Quicksort** 中，pivot 树（tree of pivots）建立完成后，接下来的任务是为每个节点（即输入数据项）计算其在最终有序序列中的位置（rank）。  
**计算步骤分为两部分（****Two-Step Rank Computation****）：**

1. **计算每个节点的子树大小（****Compute Sub-tree Sizes****）**
    - 对于每个节点 _i_，分别计算其左、右子树的节点数。
    - 结果存储在数组：￼
    - ![Exported image](Exported%20image%2020260730180938-32.png)
    - 由根向叶递推计算秩（Sweep Down the Tree）
    - 从根节点（root）开始，沿树向下遍历。
    - 每个节点的 rank 由其父节点的 rank 加上左子树的大小等信息推导得出。

**时间复杂度（****Time Complexity****）：**

- 与树的深度成正比：
- ![Exported image](Exported%20image%2020260730180939-33.png)
- 因为随机化 pivot 选择的树深度期望为 Θ(log n)，所以整个并行秩计算也在该数量级内完成。
 
**17. Sweeping Down the Tree****（树的向下遍历）**  
在 **sweep down** 过程中：

- 遍历从根节点出发，逐层传递 rank 偏移量（offset）；
- 每个节点的 rank = 父节点 rank + 左子树大小（或其前缀和）；
- 当到达叶节点时，rank 即为其在全序列中的最终位置。

这一步可以看作一次**前序遍历（****pre-order traversal****）**，在每个节点访问时传播 rank 值。
 
**18. Computing Sub-tree Sizes****（计算子树大小）**

![Exported image](Exported%20image%2020260730180939-34.png)

为了高效计算每个节点的子树规模，我们使用了一种基于树的**边序列（edge sequence）**的思想。  
**思路（****Key Idea****）：**

- 将树中的每条边视为两条：
    - 一条“向下”的边（down edge）
    - 一条“向上”的边（up edge）

因此，整个树的遍历可表示为一个带有 0/1 标签的边序列：

- **0** 表示 “向下（down）”
- **1** 表示 “向上（up）”
 
**19. Using Edge Sequences****（利用边序列计算子树大小）****￼**

![Exported image](Exported%20image%2020260730180940-35.png)

在深度优先遍历（Depth-First Traversal, DFS）中：

- 将所有边依次记录下来，形成一个边序列；
- 每个节点的子树大小可通过序列中“上下边”的前缀信息推导得到。

**计算规则（****Computation Rule****）：**

- 子树的大小 = 序列中该节点进入的“down edge”与离开的“up edge”之间的 **up edges** **数量**；
- 可以通过一次 **prefix sum****（前缀加法）** 操作实现：
    - 对整个边序列执行前缀累加；
    - 节点进入时的计数值从离开时的计数值中相减，即得到子树节点数量。

这类 prefix 计算可以在 **CRCW PRAM** 模型中并行完成，因此该步骤可高效并行化。
 
**20. Example****（示例说明）****￼**

![Exported image](Exported%20image%2020260730181033-36.png)

假设树中有以下节点：

![Exported image](Exported%20image%2020260730181036-37.png)

换句话说：  
根节点的子树大小等于整棵树的节点数；  
其余节点的子树大小通过上下边界差计算得到。
 
**小结（****Summary****）**

|   |   |   |   |
|---|---|---|---|
|**步骤**|**内容**|**并行特征**|**时间复杂度**|
|Step 1|Compute sub-tree sizes|使用 prefix sum 并行计算|Θ(log n)|
|Step 2|Sweep down tree to assign ranks|树状递推并行传播|Θ(log n)|

→ 因此整个 **CRCW Quicksort** **秩计算过程** 的总复杂度为：  
[  
Θ(\log n)  
]  
并行性能优越，结构清晰，且能自然扩展到更复杂的排序结构（如 **Sorting Networks**）。
 
是否希望我接下来继续整理 **Sorting Networks****（排序网络）** 的内容？  
我可以延续这种中英对照、结构清晰的风格，为你完整整理下一章（Bitonic Sort 与 Odd-Even Mergesort）。
 \> 来自 \<[https://chatgpt.com/c/6907a4a5-05a8-8331-83c6-7c39244ee197](https://chatgpt.com/c/6907a4a5-05a8-8331-83c6-7c39244ee197)\>

![[Lec10 Sorting_Case_Study - Ink.svg]]

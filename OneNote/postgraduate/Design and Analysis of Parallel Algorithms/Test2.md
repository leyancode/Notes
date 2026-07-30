给你再做一张完整的 **Sorting Final Cheat Sheet****（排序总复习单）**，尽量压缩到你考前五到十分钟就能扫完一遍的密度。  
结构和前一张 LogP/LogGP 的风格一致，中英双语、突出关键公式和考点。
 
**Sorting Final Cheat Sheet**  
**Design & Analysis of Parallel Algorithms**  
**排序总复习单（顺序** **+ PRAM + CRCW + Message Passing****）**  
==================================================

1. Sorting 问题定义（Problem Definition）￼==================================================
2. 输入（Input）：集合 S = {a₁,…,aₙ}，元素可比较。￼A set of comparable elements.
3. 输出（Output）：有序序列 S′，并且 S′ 是 S 的一个排列。￼A sorted permutation S′ of S.
4. 顺序算法下界（Sequential lower bound）：￼比较排序的决策树（decision tree） ⇒ Ω(n log n)。

**==================================================**  
**2.** **顺序排序算法（****Sequential Algorithms****）**

1. Enumeration Sort（枚举排序）
2. 思路：对每个 a[i]，计数有多少元素比它小（用 rank[i]）；再按 rank 把元素放入结果数组。￼For each element count how many are strictly smaller (plus tie-breaking by index) then place it.
3. 时间：Θ(n²)
4. 空间：额外 O(n) 存 r[] 和 b[]。
5. Mergesort（归并排序）
6. Divide & Conquer：
    - split(a) → 左右两半；
    - 递归排序；
    - combine(sl,sr) 归并两个有序子序列。
7. 时间：T(n) = 2T(n/2) + Θ(n) ⇒ Θ(n log n)。
8. 稳定排序，易于并行化（merge 阶段可并行）。
9. Quicksort（快速排序）
10. Divide & Conquer：
    - 选 pivot；divide(a) → l (≤pivot) 和 r (\>pivot)；
    - 递归排序；
    - concatenate(l, pivot, r)。
11. 平均时间 average：Θ(n log n)。
12. 最坏 worst：Θ(n²)，当每次 pivot 是当前区间最小/最大值时。
13. 随机化 pivot 或“三数取中”降低最坏情况概率。

**==================================================**  
**3. PRAM** **回顾与并行排序（****PRAM + Parallel Sorting****）**  
PRAM 模型：共享内存、忽略显式通信成本。

- EREW：Exclusive Read / Exclusive Write
- CREW：Concurrent Read / Exclusive Write
- CRCW：Concurrent Read / Concurrent Write（需要写冲突规则）

Work = p · Tₚ。  
Cost-optimal ⇔ Work = Θ(顺序时间) = Θ(n log n)。  
**==================================================**  
**4. CREW PRAM Mergesort****（并行归并排序）**  
思想（Idea）：

- 利用 merge 阶段的“rank = 本序列 rank + 另一序列中的 rank”。
- 为每个元素分配一个处理器，做 **并行二分查找**（parallel binary search）到另一有序子列中，找到它应插入的位置。

关键步骤（Key steps）：

1. 两个已排序子列 A, B，长度 k。
2. 每个元素 x∈A：
    - 在 B 中二分搜索 x 的插入位置 ⇒ rank_B(x)。
    - 全局 rank(x) = index_in_A(x) + rank_B(x)。
3. 同理对 B 做一次到 A 中的搜索。
4. 全局 rank 已知后，每个处理器并行写入最终数组。

复杂度（Complexity）：

- 每个元素一次 binary search：Θ(log n) 时间。
- 使用 n 个处理器：Tₚ = Θ(log n)。
- Work = n log n（忽略常数） ≈ 顺序 mergesort 工作量 ⇒ 接近 cost-optimal。
- 真正实现中递归树 +辅助数组会增加空间复杂度，但大 O 形式仍是 Θ(n log n)。

**==================================================**  
**5. CRCW Quicksort****：****Pivot Tree****（枢轴树）**  
核心概念（Core idea）：  
顺序 quicksort 调用链可以视为构建一棵 **pivot tree**。  
CRCW（arbitrary write）版本先构建 pivot tree，再计算所有元素的最终 rank。  
构建 pivot 树（Building the pivot tree）：

- 使用 CRCW arbitrary 机制：多进程竞争写 pivot 位置，任意一个成功即可成为 pivot。
- 对当前子数组：
    1. 选 pivot（通过 arbitrary 写）
    2. 每个元素根据与 pivot 的比较，决定走 leftchild 还是 rightchild
    3. 递归在子区间重复
- duplicate values 随机分配到左右两边以平衡负载。
- 期望树高（expected depth）：Θ(log n)。

树构建完后：每个输入元素对应树中的一个节点。  
**==================================================**  
**6. CRCW Quicksort****：****Compute Ranks****（秩计算）**  
两步法（Two-step rank computation）  
Step 1: Compute sub-tree sizes（子树大小）

- 对每个节点 i，计算：￼leftsize[i] = 左子树节点数￼rightsize[i] = 右子树节点数
- 技巧：使用 **edge sequence + prefix sum**
    - 深度优先遍历 DFS，记录边序列：
        - down edge 标记为 0
        - up edge 标记为 1
    - 对 0/1 序列做 prefix sum。
    - 某节点子树大小 = 对应 down/up 位置的前缀和差值。
- 在 CRCW PRAM 上，prefix sum 可在 Θ(log n) 完成。

Step 2: Sweep down the tree（向下扫描树）

- 从 root 开始向下传播 rank 偏移量（offset）：
    - root 的 rank 取决于左子树大小；
    - 每到一个子节点，其 rank = 父节点的 base rank + 左/右子树偏移。
- 预序遍历（pre-order-like traversal）并行完成。
- 时间与树高成正比 ⇒ Θ(log n)（期望）。

总复杂度（Total complexity）：

- 枢轴树构建 + 子树大小 + sweep down
- 期望并行时间 Tₚ = Θ(log n)
- 使用 n 个处理器，work ≈ n log n ⇒ 与顺序 quicksort 同阶，平均意义下 cost-optimal。

**==================================================**  
**7. Sorting with Message Passing****（消息传递排序）**  
问题定义（Distributed sorted definition）：

- p 个处理器，每个初始持 n/p 个元素。
- 排序后：
    - 每个处理器仍有 n/p 个元素；
    - 且处理器 0 持有全局最小的 n/p 个元素，处理器 1 持下一个 n/p，依此类推。

网络直径（network diameter）给出并行排序时间的下界。
 
**Odd-Even Transposition Sort**  
**奇偶换位排序**

1. 模型：一维处理器线（1-D processor array），每个处理器只与左右邻居通信。
2. 核心操作：pairwise compare-exchange / compare-split
3. 邻居之间交换各自局部数组的一部分。
4. 对单元素版本：每轮比较相邻元素并交换。
5. 算法步骤（单元素版本）：
6. 有 n 个处理器，每个持 1 个元素。
7. 执行 n 轮（phases）：
    - 奇数轮：处理器 (1,2), (3,4), … compare-exchange
    - 偶数轮：处理器 (2,3), (4,5), … compare-exchange
8. 最终序列有序。
9. 复杂度：
10. p = n 个处理器：
    - 每轮 O(1) 时间，总共 n 轮 ⇒ Tₚ = Θ(n)
    - Work = p · Tₚ = n · n = Θ(n²)
    - 顺序最佳算法为 Θ(n log n) ⇒ 奇偶换位排序 **不是** **cost-optimal**。
 
**Using p \< n processors****（****p** **小于** **n** **的情况）**

- 每个处理器持有 n/p 个元素：
    1. 本地顺序排序：O((n/p) log(n/p))
    2. 进行 p 轮 compare-split 阶段，让局部数组逐步全局有序。
- 理论上要满足 cost-optimality：￼p · Tₚ = O(n log n)￼这给出对 p 的约束：p 不能太大（典型结果：p ≤ O(n / log n) 时有机会接近代价最优）。

高层记忆句：

- n 个处理器运行 odd-even transposition sort ⇒ 时间 Θ(n)、代价 Θ(n²) ⇒ 不代价最优。
- 想要代价最优，必须减少处理器数量、让每个处理器多干活，满足 p·Tₚ ≈ n log n。

**==================================================**  
**8.** **考前必记核心点（****Sorting** **部分）**

1. 顺序排序下界：Ω(n log n)。
2. Enumeration sort：Θ(n²)。
3. Mergesort：Θ(n log n)，天然适合 parallel merge。
4. Quicksort worst case：当 pivot 总是最小或最大 ⇒ Θ(n²)。
5. CREW mergesort：并行 merge 用“本序列 rank + 另一序列中的 binary search rank”，Tₚ = Θ(log n) 用 n 处理器。
6. CRCW quicksort：pivot tree + subtree sizes + sweep down，期望 Tₚ = Θ(log n)。
7. edge sequence 技巧：down=0, up=1，prefix sum 差值给子树大小。
8. Message passing sorting：定义“每个处理器持有一段全局有序片段”。
9. Odd-even transposition：p=n 时运行时间 Θ(n)，work Θ(n²)，不是 cost-optimal。
10. cost-optimality 判断：检查 p·Tₚ 是否为 Θ(n log n)。

这张单就可以和你前面那张 LogP/LogGP Cheat Sheet 配套，一张负责通信模型，一张负责排序和 PRAM/CRCW 思想。你可以打印或在脑中只记粗体词和公式，考场上题目基本都会落在这些骨架上。
 \> 来自 \<[https://chatgpt.com/g/g-p-691b09a2e7c88191ba7715bbbfb3f193-dapatest/c/691bb5d9-9ecc-8330-a474-4ab8883f86e5](https://chatgpt.com/g/g-p-691b09a2e7c88191ba7715bbbfb3f193-dapatest/c/691bb5d9-9ecc-8330-a474-4ab8883f86e5)\>
**Reduction Operations****（归约操作）****￼**

![Exported image](Exported%20image%2020260730175511-0.png)

- ![Exported image](Exported%20image%2020260730175512-1.png)
- 通常，我们希望 **所有进程（****processes****）** 在归约完成后都拥有这个结果值。
- 有时，也可能只希望 **一个进程** 拥有这个值。
- ⚠️ ==然而，无论是== **reduce-to-all** ==还是== **reduce-to-one**==，其== **计算代价（****cost****）是相同的**。
- 备注：我们之前在 PRAM（Parallel Random Access Machine）模型中已经见过这种原语的快速实现。￼
- ![Exported image](Exported%20image%2020260730175514-2.png)
 
**Prefix Operations****（前缀操作** **/** **扫描** **Scan****）****￼**

![Exported image](Exported%20image%2020260730175515-3.png)

- 给定一组值 ( x_1, x_2, x_3, \dots, x_n ) 和一个 **associative operator** ⨁。
- ![Exported image](Exported%20image%2020260730175517-4.png)
- 每个进程完成时拥有的值不同：
    - ==最后一个进程获得的值== **与** **Reduction** **结果相同**==。==
    - ==其他进程获得的是== **部分归约值（****partial reductions****）**。
- 👉 问题：我们是否可以为此设计一个高效的 PRAM 或 **message-passing** 算法？
 
**Prefix Example****（前缀计算示例）****￼**

![Exported image](Exported%20image%2020260730175518-5.png)  

|   |   |   |   |
|---|---|---|---|
|**Processor**|**Input**|**Prefix Expression**|**Result**|
|P0|1|1|1|
|P1|2|1+2|3|
|P2|3|1+2+3|6|
|P3|4|1+2+3+4|10|
|P4|5|1+2+3+4+5|15|
|P5|6|1+2+3+4+5+6|21|
|P6|7|1+2+3+4+5+6+7|28|
|P7|8|1+2+3+4+5+6+7+8|36|

✅ 注意：最后一个进程 P7 的结果即为整体的 Reduction 值（36）。
 
**Prefix on Hypercube****（超立方体上的前缀计算）****￼**

![Exported image](Exported%20image%2020260730175610-6.png)

- 假设 ( p = n )（一个进程对应一个输入）。
- Inputs 和 Results 分布在各个处理器上（one per processor）。
- ==需要== ==( \log p )== ==次迭代（====iterations====）==。
- ![Exported image](Exported%20image%2020260730175612-7.png)

![Exported image](Exported%20image%2020260730175613-8.png)  

**Prefix for Hypercube****（超立方体前缀求和示意）****￼**

![Exported image](Exported%20image%2020260730175614-9.png)

- 在 8 个处理器的 hypercube 上进行 prefix sums 计算。
- 每个处理器的状态表示如下：
    - ==方括号== ==[]====：本地== ==prefix sum====（已累积结果缓冲区）。==
    - ==圆括号== ==()====：发往下一个通信步骤的== ==message buffer== ==内容。==

（参考教材：Copyright © 1994 Benjamin/Cummings）￼

![Exported image](Exported%20image%2020260730175615-10.png)  
![Exported image](Exported%20image%2020260730175616-11.png)  
![Exported image](Exported%20image%2020260730175617-12.png)  
![Exported image](Exported%20image%2020260730175646-13.png)  
![Exported image](Exported%20image%2020260730175647-14.png)

**￼****￼****￼****￼****(Fractional) Knapsack Problem****（分数背包问题）****￼**

![Exported image](Exported%20image%2020260730175647-15.png)

- 问题描述：￼给定 ( n ) 个物品，每个物品有 **weight ( w )** 和 **value ( v )**，以及一个容量为 ( c ) 的背包。￼目标是在不超过容量的前提下，选择物品或其一部分，使背包总价值最大化。
- 假设物品可以 **按比例分割（****fractionally****）**。

**例子**：  
背包容量 ( c = 15 )，物品如下：

![Exported image](Exported%20image%2020260730175649-16.png)

最优选择：

- 全部取第一个物品 (5, 100)
- 全部取第三个物品 (9, 135)
- 取一半的第四个物品 (2, 26)

![Exported image](Exported%20image%2020260730175650-17.png)

✅ **总结** **Summary**：

- **Reduction**：单值聚合，结果相同，代价相同。
- **Prefix (Scan)**：多值输出，部分结果用于并行算法加速。
- **Hypercube prefix**：利用对数步通信降低计算时间。
- **Knapsack**：经典优化问题，在并行计算中也有高效实现方案。
    
**(Fractional) Knapsack Problem: Parallel Algorithm****（分数背包问题的并行算法）****￼**

![Exported image](Exported%20image%2020260730175651-18.png)  
![Exported image](Exported%20image%2020260730175652-19.png)  
![Exported image](Exported%20image%2020260730175725-20.png)  
![Exported image](Exported%20image%2020260730175726-21.png)  
![Exported image](Exported%20image%2020260730175727-22.png)  
![Exported image](Exported%20image%2020260730175728-23.png)  
![Exported image](Exported%20image%2020260730175729-24.png)  
![Exported image](Exported%20image%2020260730175730-25.png)  
![Exported image](Exported%20image%2020260730175731-26.png)     

- 我们可以通过 **step-by-step** **并行化** 来加速求解。
- 主要步骤及其时间复杂度如下（假设使用 **CREW PRAM** 且 ( p = n )）：

![Exported image](Exported%20image%2020260730175816-27.png)  
![Exported image](Exported%20image%2020260730175818-28.png)

注：通过并行计算排序和前缀和，我们实现了比顺序算法更高效的复杂度。
 
**Pointer Jumping****（指针跳跃）****￼**

![Exported image](Exported%20image%2020260730175819-29.png)

- 假设 PRAM 上有一个 **linked-list****（链表）** 结构：
    - ==每个节点存储一个== **指向下一个节点的指针（****pointer****）**==；==
    - ==最后一个节点的指针为== ==null====。==
- 我们可以找到任意节点，但不能直接跳过去，只能沿着指针一步步移动。
- ==顺序查找第== ==( n )== ==个节点需要== ==( n )== ==步。==
- ==但我们可以通过== **pointer jumping****（指针跳跃）** ==技术来并行完成如== ==prefix== ==这样的操作。==
 
**Pointer Jumping – List Ranking****（链表排序****/****排名）****￼**

![Exported image](Exported%20image%2020260730175820-30.png)

- 目标：计算每个节点到链表尾端的距离。
- 这是一个 **list ranking** 问题。

![Exported image](Exported%20image%2020260730175821-31.png)

⚠️ Side note：这种方法更快，但它是否 **cost optimal****（代价最优）** 仍需考虑。
 
**Pointer Jumping – List Ranking, Pseudo-code****（伪代码）****￼**

![Exported image](Exported%20image%2020260730175822-32.png)  
![Exported image](Exported%20image%2020260730175823-33.png)  
![Exported image](Exported%20image%2020260730175933-34.png)  
![Exported image](Exported%20image%2020260730175934-35.png)

**￼****￼****算法思想**：每一步，每个进程“跳”两步而不是一步，从而指数级地减少迭代次数。

![Exported image](Exported%20image%2020260730175935-36.png)

- **Step 1:** 将当前节点的值加上其所指向节点的值
- **Step 2:** 将当前指针更新为“下下个”节点的指针

✅ 经过 ( \log n ) 轮迭代后，所有节点的距离都被计算出来。￼

![Exported image](Exported%20image%2020260730175935-37.png)  
![Exported image](Exported%20image%2020260730175937-38.png)  
![Exported image](Exported%20image%2020260730175937-39.png)  
![Exported image](Exported%20image%2020260730175938-40.png)

**Pointer Jumping** **总结** **Summary**

![Exported image](Exported%20image%2020260730180018-41.png)

Pointer Jumping 是许多 PRAM 算法（如 **list ranking****、****tree contraction****、****prefix computation**）的核心技巧。
   

**Pointer Jumping – List Ranking, Another View****（另一种角度的链表排名）****￼**

![Exported image](Exported%20image%2020260730180019-42.png)

- 指针跳跃不仅可以用于 list ranking（链表排名），也可以扩展用于前缀计算。
- 通过操作链表中的 **successor****（后继节点）** 值，可以高效地进行并行前缀计算。
 
**Pointer Jumping – Prefix Operation****（指针跳跃前缀计算）****￼**

![Exported image](Exported%20image%2020260730180020-43.png)

- 我们可以在链表中的元素上执行 **prefix computation****（前缀计算）**。
- 与 list ranking 不同的是：
    - list ranking 处理的是 **自身的值**，
    - prefix computation 则操作 **后继节点（****successor****）的值**。
- 该操作可以使用任意 **associative operator****（结合运算符）**，例如 Op。
 
**Pointer Jumping Prefix – Cost Optimal?****（成本最优性分析）****￼**

![Exported image](Exported%20image%2020260730180022-44.png)

- ==单独使用== ==pointer jumping prefix== ==算法时，==**并不是** **cost optimal** ==的。==

![Exported image](Exported%20image%2020260730180024-45.png)

然而，在 fractional knapsack problem 中：

- 排序步骤（sort）是主导耗时，其顺序代价为 ( \Theta(n \log n) )。
- ==pointer jumping prefix== ==虽然成本不是最==优，但其 ( O(n \log n) ) 成本与排序相同量级，￼==因此不会成为性能瓶颈==。
- 可以将这种并行 prefix 算法整合进 knapsack 的并行求解流程中，从而实现总体 **cost optimal parallel algorithm****（成本最优的并行算法）**。
 \> 来自 \<[https://chatgpt.com/c/68f0d7c5-c2c4-832e-8d74-2f74385d76e5](https://chatgpt.com/c/68f0d7c5-c2c4-832e-8d74-2f74385d76e5)\>

![[Lec8 Computation_Primitives - Ink.svg]]

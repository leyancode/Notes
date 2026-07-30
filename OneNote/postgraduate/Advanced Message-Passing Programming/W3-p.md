下面我按你之前的课堂笔记格式，把这份 **AMPP Practical: User-defined reduction operations** 用中文为主（保留关键英文术语）整理成可直接放进笔记本的版本。
 
**AMPP Practical**  
主题：User-defined reduction operations  
讲师：Alexei Borissov  
日期：30 January 2026
 
**Introduction** **介绍**  
本文件是课程 **Advanced Message Passing Programming** 中 lectures “User-Defined Reduction Operations” 对应的 practical 练习说明。  
实践要求：

- 每个 exercise 建议从 scratch 开始实现
- 尽量避免使用 pre-existing codes，也不要直接 copy/paste 现成结构
- 目标是让你熟悉 user-defined reduction operator 的设计与实现细节
 
**Exercise 1: Your own sum**  
自定义求和算子（必做核心）  
任务描述：

- 需要实现一个你自己的 **summation operator**
- 每个 MPI process 提供 **1** **个** **integer** 参与 reduction
- 选择一个 root process（例如 rank 0）
- root process 打印最终 reduced value

关键信息提炼：

- 输入：每个进程 1 个 int
- 操作：user-defined reduction operator（sum）
- collective：MPI_Reduce
- 输出：root 打印总和

实现目标（概念层面）：

- 使用 MPI_Op_create 创建自定义 MPI_Op
- 在 MPI_Reduce 中使用该 MPI_Op
 
**Exercise 2: A new operator (optional)**  
自定义 AVG 算子（可选挑战题）  
说明：

- 该题 optional
- 难度超出 quiz 评估范围
- 面向想要额外挑战的同学

目标：

- 设计一个 MPI_AVG operator
- 功能：计算参与 reduction 的所有值的 average

主要挑战（slides 明确指出两点）：

1. 需要知道有多少 MPI processes 参与 reduction
    - 即 communicator size（例如 MPI_Comm_size 的结果）
    - 但注意：user-defined operator 的回调函数本身并不会直接把 communicator 传进来
2. 需要跟踪 partial reductions 的完成次数
    - 因为 reduction 是分阶段合并的（tree-based / hierarchical）
    - 你必须在数据结构中携带“累计了多少个值”这类信息，否则无法正确求平均
 
**Note** **重要备注**

- 你的 user-defined reduction operator 只需要保证在 MPI_Reduce 中正确即可
- 不要求在 MPI_Scan 等 prefix reduction 中正确
    - 但你可以自行尝试扩展
 
**复习关联点（建议你在笔记里标注）**  
与 lecture 的直接关联概念：

- MPI_Op_create(MPI_User_function* user_function, int commutativity, MPI_Op* handle);
- reduction operator 必须满足 **associativity**
- 若设置 commutativity = true，则还要求 **commutative**
- reduction 的组合顺序与 MPI 实现有关（tree/segmented），因此 average 这种“需要全局信息”的 operator 更棘手
 
如果你接下来要把这份 practical 也写成“可执行的标准模板”，我可以直接给你一个最小可用的 C 结构（包括 MPI_Op_create、user function 的参数解释、以及 MPI_Reduce 的调用方式），并把 Exercise 2 的“正确设计思路”也整理成笔记版（例如用 pair (sum, count) 作为 datatype）。
 \> 来自 \<[https://chatgpt.com/c/6981233c-2a2c-8396-a4be-9bbe06444d7a](https://chatgpt.com/c/6981233c-2a2c-8396-a4be-9bbe06444d7a)\>  

￼为什么 MPI user-defined reduction operator 必须 associative？￼  
Because MPI may perform reductions using different reduction trees or hierarchical strategies, the order in which operands are combined is not fixed.  
Therefore, the user-defined reduction operator must be associative to ensure a correct and deterministic result.￼￼如果我把 commutativity 设为 true，但 operator 实际上不可交换，会发生什么？￼￼If commutativity is set to true but the operator is not actually commutative, MPI is allowed to reorder operands arbitrarily, which may lead to incorrect or undefined results.￼￼  
*MPI_MINLOC 中：

1. 第二个元素类型必须是什么？
2. 是否可以使用普通 MPI_DOUBLE 作为 datatype？**

**你的回答**  
INT，不可以  
**评价**  
结论 **完全正确**，但考试里建议写完整一点。  
**标准答案**

1. 第二个元素必须是 int
2. 不可以使用普通 MPI_DOUBLE，必须使用 (value, int) 对应的 datatype，例如 MPI_DOUBLE_INT
   

**为什么** **average reduction** **不能只用一个** **int** **或** **double** **来做？**  
**你的回答**  
为了针对不同数据的 data type，防止不同格式直接合并，造成错误  
**评价**  
这是 **猜测性回答，不成立**，但你意识到“average 不简单”是对的。  
**正确核心原因（一定要记）**  
**Average ≠** **可直接归约的运算**  
因为：

- reduction 是 **分阶段进行的**
- 中间阶段你不知道：
    - 已经合并了多少个值

所以你必须携带：

- partial sum
- count（合并了多少个值）

**标准答案（推荐）**  
Because an average requires both the sum of values and the number of values combined.  
During a reduction, partial results are combined in stages, so a single int or double does not carry enough information.  
Therefore, a compound datatype (e.g. sum and count) is required.
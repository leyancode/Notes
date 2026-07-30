**Advanced Message-Passing Programming**  
**Advanced Derived Datatypes Practical**  
**Instructor**  
Alexei Borissov  
**Date**  
26 January 2026
 
**Introduction**  
本实验文档对应课程 **“Advanced Derived Datatypes”**，隶属于模块 **Advanced Message-Passing Programming**。  
**实验目标**

- 练习使用课堂中介绍的 **MPI datatype constructors**
- 创建多种 **MPI derived datatypes**
- 理解 derived datatypes 在 **collective operations** 和 **structured data communication** 中的实际作用
 
**运行环境要求**

- 所有实验需在 **Cirrus compute nodes** 上运行
- 请不要使用 --exclusive 标志
    - 允许作业使用 partial nodes
- 各个练习可以任意顺序完成
- 练习整体设计为 **难度递增**
 
**Exercise 1: Scattering along non-contiguous dimensions**  
**背景知识：****C** **的内存布局（****row-major order****）**  
在 C 语言中：

- 二维数组采用 **row-major order**
- 同一行（row）中的元素在内存中是连续的
- 同一列（column）中的元素在内存中是 **非连续的**
 
**二维数组声明示例**  
int my_array[row_count][column_count];￼  
含义：

- my_array[i][j] 和 my_array[i][j+1] 在内存中是连续的
- my_array[i][j] 和 my_array[i+1][j] 在内存中不是连续的
 
**通信难点分析**

- 发送一整行：
    - 内存连续
    - 使用 **MPI_Type_contiguous** 即可
- 发送一整列：
    - 内存非连续
    - 需要 **MPI derived datatype**
 
**本练习的目标**

- 使用 **MPI_Type_vector**
- 构造一个表示“列（column）”的 derived datatype
- 使用 **MPI_Scatter**
    - 由 root（MPI process 0）
    - 将二维数组按列分发
    - 每个 MPI process 接收一列
 
**实验关键提示（非常重要）**

- 在 collective operations 中使用 derived datatypes
- 必须考虑 **datatype replication mechanism**
- 特别是：
    - interleaving
    - displacement 计算
- 很可能需要结合课堂中讲过的：
    - extent
    - MPI_Type_create_resized
 
**Exercise 2: Communicating structures**  
本练习关注 **struct** **类型的通信**，强调 MPI derived datatypes 在结构化数据中的作用。
 
**结构体定义（逻辑层面）**  
该结构体按顺序包含：

1. 一个 double
2. 一个 int
3. 一个 char

结构体成员的具体语义不重要，重点在于：

- 成员类型不同
- 成员大小不同
- 存在 alignment 与 padding 的可能性
 
**Case 1:** **单次** **MPI_Send** **发送结构体**  
目标：

- 构造一个 MPI derived datatype
- 能够表示该 struct
- 使用 **一次** **MPI_Send**
    - 而不是分别发送每个成员

原因：

- 多次 MPI_Send 会多次支付 network latency
- 不符合高性能通信的设计原则
 
**Case 2:** **结构体成员顺序改变**  
变化点：

- 结构体成员顺序被重新排列
- 其余逻辑保持不变

要求：

- 在 Case 1 的代码基础上
- 调整 derived datatype 的构造
- 使程序仍然能够正确通信
 
**思考问题（非常关键）**  
如果你的代码在 Case 2 中不再工作，可能原因包括：

- displacement 依赖于原始成员顺序
- 忽略了 alignment / padding
- 使用了假定固定 offset 的 manual displacement
- datatype 定义与实际 struct 内存布局不一致
 
**Exercise 3: Extensions****（可选拓展）**  
该部分为 **可选练习**，难度更高，适合深入理解 MPI derived datatypes。
 
**AoS to SoA** **转换**  
**概念说明**

- **AoS (Array of Structures)****￼**一个数组，每个元素是一个 struct
- **SoA (Structure of Arrays)****￼**每个成员单独形成一个数组

不同布局在不同场景下：

- cache 行为不同
- 通信与计算性能不同
 
**练习目标**

- 构造两个 MPI derived datatypes
- 能够：
    - 发送 AoS 布局的数据
    - 接收并存储为 SoA 布局
- 结果应为：
    - 所有第一个成员聚集在一起
    - 所有第二个成员聚集在一起
    - 以此类推
 
**Update the coursework****（综合挑战）**

- 你现在已经掌握：
    - 在 collective operations 中使用 derived datatypes
- 额外挑战：
    - 重写上一学期的 MPP coursework
    - 使用 MPI vectors
    - 直接在 MPI_Scatter / MPI_Gather 中通信
    - 而不是手动 pack / unpack 数据
 
**实验部分总目标总结**

- 熟练使用 MPI derived datatypes
- 理解 non-contiguous data 的通信方式
- 将课堂理论（extent、replication、resized、lower bound）
    - 应用于真实 MPI 程序
- 提升程序的：
    - 性能
    - 可移植性
    - 可维护性
 
如果你愿意，下一步我可以帮你：

- 为 **Exercise 1 / 2** 写一份完整的实现思路（不直接给答案版 / 可给提示版）
- 把实验目标整理成 **checklist**
- 帮你把实验和课堂知识一一对应，形成“知识映射表”
- 或在你写完代码后，帮你逐行检查 derived datatype 是否设计正确
 \> 来自 \<[https://chatgpt.com/c/6977f6fa-ba28-8326-a74f-7c7e1d21f2a4](https://chatgpt.com/c/6977f6fa-ba28-8326-a74f-7c7e1d21f2a4)\>
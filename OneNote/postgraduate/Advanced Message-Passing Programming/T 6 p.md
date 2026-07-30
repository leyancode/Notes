。

![Exported image](Exported%20image%2020260730161116-0.png)

**AMPP Practical**  
Neighbourhood collective operations and persistent communication requests  
Ludovic Capelli  
February 26, 2024
 
**背景说明**  
你将复用上周创建的 graph topology：

- Undirected edges
- 即 A—B 等价于 A→B 和 B→A
- 必须使用带 topology 的 communicator

假设：  
你已经创建了 graph communicator，例如：  
MPI_Graph_create 或 MPI_Dist_graph_create
 
**Exercise 1**  
**题目要求**  
每个 MPI process：

- 持有 1 个 int
- 值等于 rank
- 发送给所有 neighbour
- 使用 MPI_Neighbor_allgather
 
**应选择的操作**  
MPI_Neighbor_allgather  
原因：

- 每个进程发送相同数量元素（1 个）
- 每个 neighbour 接收数量一致
- 满足 allgather 语义
 
**实现逻辑**

1. sendcount = 1
2. sendbuf = &rank
3. recvcount = 1
4. recvbuf size = neighbour_count

关键点：  
先用 MPI_Graph_neighbors_count 获取邻居数量。
 
**Exercise 2**  
**新条件**  
每个 MPI process：

- 发送元素数量 = 自己的 rank
- 元素值仍然等于 rank

示例：

- rank 0 → 发送 0 个
- rank 1 → 发送 1 个
- rank 2 → 发送 2 个
- rank 3 → 发送 3 个
 
**问题**  
每个 neighbour 发送的数据大小不同。  
MPI_Neighbor_allgather 不再适用。
 
**应选择的操作**  
MPI_Neighbor_allgatherv  
原因：

- 接收数量随 neighbour 不同而变化
- 允许 recvcounts[] 和 displs[]
 
**关键理解**  
sendcount 仍然是固定的。  
这里 tricky 点：  
Neighbour_allgatherv 允许接收端大小不同，但发送端 sendcount 仍固定。  
因此：  
这里每个进程发送给所有 neighbour 的元素数必须相同。  
由于题目说每个进程发送 rank 个元素给所有 neighbour：  
这符合 allgatherv 的语义。
 
**Exercise 3**  
**新条件**  
每个进程：

- 向每个 neighbour 发送 1 个元素
- 但不同 neighbour 发送不同值
 
**应选择的操作**  
MPI_Neighbor_alltoall  
原因：

- 每个 neighbour 对应不同数据块
- 每个 neighbour 发送数量相同（1）
- 适合 alltoall
 
**数据布局**  
sendbuf 大小 = neighbour_count  
sendbuf[i] = 针对第 i 个 neighbour 的值  
recvbuf 同样按 neighbour 顺序组织。
 
**Exercise 4**  
**新条件**  
每个进程：  
向 neighbour j 发送：  
数量 = rank(j)  
例如：  
process 0：

- 向 1 发送 1 个
- 向 2 发送 2 个
- 向 3 发送 3 个

总发送 = 6 个
 
**应选择的操作**  
MPI_Neighbor_alltoallv  
原因：

- 每个 neighbour 发送数量不同
- 每个 neighbour 接收数量不同
- 类型相同
 
**数据布局逻辑**  
sendcounts[i] = rank(neighbour[i])  
sdispls[] = 前缀和  
例如：  
neighbours = {1,2,3}  
sendcounts = {1,2,3}  
sdispls = {0,1,3}  
sendbuf 总长度 = 6
 
**Exercise 5**  
**新条件**  
不同 neighbour 发送不同类型：

- 若 neighbour rank 为 odd → 发送 uint32_t
- 若 even → 发送 uint64_t
 
**应选择的操作**  
MPI_Neighbor_alltoallw  
原因：

- 类型可变
- displacements 以字节为单位
- sendtypes[] 可变
 
**数据布局关键点**

1. 必须分配 char* buffer
2. 手动控制字节偏移
3. sdispls 类型为 MPI_Aint
4. 单位为 bytes
 
**内存对齐问题**  
uint32_t = 4 bytes  
uint64_t = 8 bytes  
必须注意：

- 计算正确的字节偏移
- 防止错位
- 合理填充 buffer
 
**示例布局思路**  
假设：  
neighbours = {1,2,3}

- 1 → odd → uint32_t
- 2 → even → uint64_t
- 3 → odd → uint32_t

若 sendcounts = {1,2,3}  
则：  
总字节数 =  
1×4 + 2×8 + 3×4 = 4 + 16 + 12 = 32 bytes  
然后构造：  
sdispls = {0,4,20}
 
**Persistent Communication Requests**  
虽然题目标题提到 persistent communication requests，  
但 exercises 1–5 重点是 neighbourhood collectives。  
若扩展：  
MPI 提供 persistent 版本：  
MPI_Neighbor_alltoall_init  
MPI_Start  
MPI_Wait  
优势：

- 避免重复 setup cost
- 适用于循环 halo exchange
 
**实践实现顺序建议**

1. Exercise 1 先调试 topology
2. 打印 neighbour 顺序
3. 确认 recvbuf 布局
4. 再逐步升级到 v 和 w 版本
 
**总体知识结构图**  
Neighbourhood collectives 进阶路径：  
allgather  
→ allgatherv  
→ alltoall  
→ alltoallv  
→ alltoallw  
复杂度逐步上升：  
固定大小 → 可变大小 → 可变类型
 
如果你愿意，我可以：

- 给你一个完整代码骨架模板
- 或帮你设计一个统一框架，一次性支持 1–5 五个版本切换
- 或帮你画出完整内存布局逻辑图帮助你理解 sendbuf / recvbuf 对应关系
 \> 来自 \<[https://chatgpt.com/c/699b2583-cd64-838b-8f1b-528280f0848e](https://chatgpt.com/c/699b2583-cd64-838b-8f1b-528280f0848e)\>
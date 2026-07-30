下面是根据你贴的 PPT 要点 + 你 ex1–ex5 的实现风格，整理的“课堂测验可能出的问答题”，中英双语对照，答案尽量用考试可直接抄的表述。
 
**A.** **基础概念与动机**

1. 问：为什么 MPI 3.0 引入 neighbourhood collective operations？￼Q: Why were neighbourhood collective operations introduced in MPI 3.0?￼答：为了在已知拓扑（topology）下，只在邻居范围内执行 collective，避免为每个邻域创建大量 sub-communicator，并让 MPI runtime 能利用全局连接信息做优化。￼A: To perform collectives restricted to neighbours using topology information, avoiding thousands of sub-communicators and enabling runtime optimisations using full connectivity data.
2. 问：Neighbourhood collectives 是不是 collective？不全调用会怎样？￼Q: Are neighbourhood collectives still collective operations? What if not all ranks call them?￼答：是。communicator 内所有 rank 必须调用；否则可能死锁或未定义行为。￼A: Yes. All processes in the communicator must call them; otherwise deadlock or undefined behaviour may occur.
 
**B. Neighbour_allgather / Neighbour_allgatherv**

1. 问：MPI_Neighbor_allgather 的限制是什么？￼Q: What restriction does MPI_Neighbor_allgather impose?￼答：每个进程必须从每个 neighbour 接收相同数量的元素（固定 recvcount）。￼A: Each process receives the same number of elements from every neighbour (fixed recvcount).
2. 问：什么时候必须用 MPI_Neighbor_allgatherv？￼Q: When must MPI_Neighbor_allgatherv be used?￼答：当从不同 neighbours 接收的数量不同，或希望控制每个 neighbour 数据在 recvbuf 的放置位置时。￼A: When receive counts differ per neighbour or you need per-neighbour placement control via recvcounts[] and displs[].
3. 问：MPI_Neighbor_allgatherv 里哪些东西能“按 neighbour 变化”？哪些不能？￼Q: In MPI_Neighbor_allgatherv, which parameters can vary per neighbour, and which cannot?￼答：recvcounts[] 和 displs[] 可按 neighbour 变化；发送端 sendcount/sendtype 对所有 neighbours 相同。￼A: recvcounts[] and displs[] vary per neighbour; sendcount/sendtype are the same for all neighbours.
4. 问：Neighbourhood allgather 有“按 neighbour 改 datatype”的版本吗？￼Q: Is there a neighbourhood allgather variant that allows varying datatypes per neighbour?￼答：没有。PPT 明确说明不存在允许 varying element types 的 neighbourhood allgather 版本。￼A: No. The slides explicitly state there is no neighbourhood allgather variant with varying element types.
5. 问：在 Neighbor_allgatherv 中 displs[] 的单位是什么？如何计算？￼Q: What is the unit of displs[] in Neighbor_allgatherv, and how is it computed?￼答：单位是“元素个数”（不是字节）。通常用前缀和：displs[0]=0; displs[i]=displs[i-1]+recvcounts[i-1]。￼A: In units of elements (not bytes). Typically computed as prefix sums: displs[0]=0; displs[i]=displs[i-1]+recvcounts[i-1].
 
**C. Neighbour_alltoall / alltoallv / alltoallw**

1. 问：MPI_Neighbor_alltoall 与 MPI_Neighbor_allgather 的关键差异？￼Q: Key difference between MPI_Neighbor_alltoall and MPI_Neighbor_allgather?￼答：allgather 向所有 neighbours 发送同一份数据；alltoall 可以对不同 neighbours 发送不同元素。￼A: allgather sends the same data to all neighbours; alltoall can send different elements to different neighbours.
2. 问：MPI_Neighbor_alltoall 的限制是什么？￼Q: What restriction does MPI_Neighbor_alltoall impose?￼答：每个进程必须向每个 neighbour 发送相同数量的元素（固定 sendcount），并从每个 neighbour 接收相同数量（固定 recvcount）。￼A: Each process sends the same number of elements to every neighbour (fixed sendcount) and receives a fixed recvcount from every neighbour.
3. 问：什么时候用 MPI_Neighbor_alltoallv？￼Q: When do you use MPI_Neighbor_alltoallv?￼答：当 send/recv 的 counts 或 displacements 需要按 neighbour 变化（但 datatype 仍固定）时。￼A: When per-neighbour counts/displacements must vary (but datatype remains fixed).
4. 问：MPI_Neighbor_alltoallw 比 alltoallv 多了什么能力？￼Q: What extra capability does MPI_Neighbor_alltoallw provide compared to alltoallv?￼答：允许每个 neighbour 使用不同 datatype；因此 displs 不再按元素计，而是按字节（MPI_Aint）。￼A: Allows varying datatypes per neighbour; therefore displacements are in bytes (MPI_Aint), not in elements.
5. 问：为什么 alltoallw 的 displacement 是 bytes？￼Q: Why are displacements in bytes for alltoallw?￼答：因为每个 neighbour 的 datatype 可能不同，元素大小不同，无法用“元素个数”作为统一位移单位。￼A: Because datatypes (and element sizes) may differ per neighbour, so a single element-based displacement unit is not meaningful.
 
**D. Neighbour ordering****（考试高频）**

1. 问：Graph topology (MPI_Graph_create) 下 neighbour 的顺序由什么决定？MPI 会排序吗？￼Q: In graph topologies created with MPI_Graph_create, what determines neighbour order? Does MPI sort them?￼答：由创建时提供的 CSR（indexes/edges）顺序决定；MPI 按提供顺序迭代，不保证排序。￼A: Determined by the CSR input order (indexes/edges); MPI iterates in that order and does not guarantee sorting.
2. 问：为什么 neighbour ordering 对 recvbuf[i]/recvcounts[i]/displs[i] 很关键？￼Q: Why is neighbour ordering critical for recvbuf[i]/recvcounts[i]/displs[i]?￼答：这些数组的下标 i 对应“第 i 个 neighbour”，不是 neighbour 的 rank；顺序变了，语义就变。￼A: Index i refers to the i-th neighbour in the topology ordering, not the neighbour’s rank; changing ordering changes the mapping.
3. 问：Cartesian topology 里 neighbour 的顺序规则是什么？缺失 neighbour 怎么处理？￼Q: What is the neighbour ordering rule in Cartesian topologies, and how are missing neighbours handled?￼答：按维度顺序迭代；每一维先负方向再正方向。边界缺失 neighbour 时视作 MPI_PROC_NULL，但 buffer 仍要按“存在该 neighbour”的结构组织。￼A: Iterated by dimension; in each dimension negative direction first. Missing neighbours behave as MPI_PROC_NULL, but buffers must be structured as if those neighbours existed.
 
**E.** **非阻塞版本与语义**

1. 问：Neighbourhood collectives 的 non-blocking 版本有哪些？使用时的 buffer 规则？￼Q: What are the non-blocking neighbourhood collective variants and what buffer rule applies?￼答：MPI_Ineighbor_allgather/_allgatherv/_alltoall/_alltoallv/_alltoallw。语义与普通 non-blocking 一致：在对应 request 完成（wait/test）前不得覆写相关 buffer。￼A: MPI_Ineighbor_allgather/_allgatherv/_alltoall/_alltoallv/_alltoallw. Same semantics as other non-blocking calls: do not modify the involved buffers until the request is completed (wait/test).
 
**F.** **结合** **ex1–ex5** **的****“****应用型问答****”****（最贴近你代码）**

1. 问：Exercise 2 为什么不能用 MPI_Neighbor_allgather，必须用 MPI_Neighbor_allgatherv？￼Q: Why does Exercise 2 require MPI_Neighbor_allgatherv instead of MPI_Neighbor_allgather?￼答：因为从不同 neighbours 接收的数量不相同（例如从 rank0 收 0 个，从 rank3 收 3 个），allgather 要求固定 recvcount。￼A: Because receive counts differ per neighbour; allgather requires a fixed recvcount.
2. 问：Exercise 3 为什么用 MPI_Neighbor_alltoall？￼Q: Why use MPI_Neighbor_alltoall in Exercise 3?￼答：因为需要对不同 neighbours 发送不同元素，但每 neighbour 发送数量固定为 1；alltoall 正好是“每 neighbour 一段”的发送语义。￼A: Because you must send different elements to different neighbours while sending 1 element per neighbour; alltoall matches that per-neighbour slot semantics.
3. 问：Exercise 4 为什么必须用 MPI_Neighbor_alltoallv？￼Q: Why must Exercise 4 use MPI_Neighbor_alltoallv?￼答：因为对不同 neighbours 的发送数量不同（send_counts varies），而 alltoall 要求固定 sendcount。￼A: Because per-neighbour send counts vary; alltoall requires a fixed sendcount.
4. 问：Exercise 5 为什么必须用 MPI_Neighbor_alltoallw？￼Q: Why must Exercise 5 use MPI_Neighbor_alltoallw?￼答：因为不仅 counts/displacements 变化，datatype 也按 neighbour 奇偶变化；只有 alltoallw 支持 per-neighbour types，且位移需用 bytes。￼A: Because datatypes vary per neighbour as well as counts/displacements; only alltoallw supports per-neighbour types and byte displacements.
5. 问：在 alltoallw 中，为什么 displs 要用 MPI_Aint 而不是 int？￼Q: Why are displs typed as MPI_Aint in alltoallw rather than int?￼答：位移以字节计，可能需要表达更大的地址偏移，且 MPI 标准指定使用 MPI_Aint 表示地址/字节位移。￼A: Displacements are in bytes and may require address-sized representation; the MPI standard specifies MPI_Aint for address/byte displacements.
 
如果你希望我把这些问答再按“高频必背 15 题”压缩成一页速记版（更像测验复习卡片），我也可以直接给你精简版。
 \> 来自 \<[https://chatgpt.com/g/g-p-697bedcd2a608191bd4f46274b4a3e7c-ampp/c/699ebf4b-a5d4-838c-ae8a-59b8d89ec8c8](https://chatgpt.com/g/g-p-697bedcd2a608191bd4f46274b4a3e7c-ampp/c/699ebf4b-a5d4-838c-ae8a-59b8d89ec8c8)\>
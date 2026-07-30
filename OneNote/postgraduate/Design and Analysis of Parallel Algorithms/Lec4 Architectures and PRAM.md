**Parallel Computer Structures** **（并行计算机结构）****￼**

![Exported image](Exported%20image%2020260730174836-0.png)

- 两种主要的并行编程模型（parallel programming models）反映了底层体系结构的分歧：
    - **S****hared Memory model****（共享内存模型****）**：
        - 线程（threads）通过公共内存位置直接交互。
        - 需要小心避免同时访问同一内存位置。
    - **Message Passing model****（消息传递模型）**：
        - 每个进程（process）拥有独立的地址空间。
        - 需要小心确保数据在正确的时间发送到正确的地方。
- 思考：如何在机器和成本模型（machine and cost models）中反映这些？
- 不同并行体系结构的结构性差异使得建模更加困难。
 
**Shared Address Space Parallelism** **（共享地址空间并行）**

![Exported image](Exported%20image%2020260730174837-1.png)

- 实际成本复杂：涉及 **cache coherence****（缓存一致性）** 和 **network congestion****（网络拥塞）**。
- ==这些实际上属于== **隐藏通信（****hidden communications****）**==，因此算法的耦合性比表面上更强==。
- 在算法设计中，通常会简化：忽略这些因素。
- ==理论模型：==**Parallel Random Access Machine (PRAM)**。
 
**Typical Shared Memory Architectures** **（典型共享内存体系结构）****￼**

![Exported image](Exported%20image%2020260730174838-2.png)

- **Uniform-memory shared-address-space computer** （统一内存共享地址空间计算机）
- **Non-uniform-memory-access (NUMA) with local & global memories** （非统一内存访问，含本地与全局内存）
- **Non-uniform-memory-access with local memory only** （非统一内存访问，仅有本地内存）

图示来源：Benjamin/Cummings Publishing Co, 1994

==M====是====memory, P====是====proces==s
   

**The PRAM model****￼**

![Exported image](Exported%20image%2020260730174848-3.png)

- ==PRAM== ==是== **共享地址空间计算机的理想化模型**==，由随机存取机（====Random Access Machine, RAM====）模型扩展而来。==
- 假设条件：
    - **p processes**：进程数为 p，步调一致（lock-step），可随时免费同步。
    - **m shared memory locations**：共享内存位置 m 个。
    - 访问任意内存位置的代价为单位时间。
    - ==内存冲突（====memory clash====）解决方式：==
        - **EREW**==: Exclusive-Read, Exclusive-Write== ==（独占读写）==
        - **CREW**==: Concurrent-Read, Exclusive-Write== ==（并发读，独占写）==
        - **CRCW**==: Concurrent-Read, Concurrent-Write== ==（并发读写）==

**combining write****（合并写）是什么意思？**  
最重要的一句话：  
**当多个处理器同时向同一个地址写不同的值时，不是选一个，而是把这些值用某个操作组合成一个结果，然后写进去。**  
这个“操作”可以是：

- 加法（+）
- 乘法（×）
- 最大值（max）
- 最小值（min）
- OR（逻辑或）
- AND（逻辑与）

这些操作必须满足两个性质：

1. **结合律（****associative****）**：￼(a ⊕ b) ⊕ c = a ⊕ (b ⊕ c)
2. **交换律（****commutative****）**：￼a ⊕ b = b ⊕ a

这样，不管多少处理器写，不管写入顺序是什么，结果都是一样的。  
这就是为什么合并写（combining write）是安全的、可定义的。
 
**一个超级形象的类比：**  
想象有一个“神奇的桶”（内存单元），同时有多个工人（处理器）往里面“丢数字”。  
CRCW(+) 就是：  
桶自动把所有丢进来的数字 **加起来**。  
多个写操作 **不是冲突**，而是 **自动被****“****合并****”**。  
例如：
 
P0: 写入 2 ￼P1: 写入 7 ￼P2: 写入 5 ￼  
CRCW(+):  
sum = 2 + 7 + 5 = 14  
CRCW(max):  
sum = max(2, 7, 5) = 7  
CRCW(*)：  
sum = 2 * 7 * 5 = 70  
CRCW(OR)：  
OR(true, false, true) = true  
==所以，====CRCW== ==不是====“====选谁写进去====”====；==  
==而是== **所有写的值一起合并成一个值。**
   

**CRCW PRAM Conflict Resolution** **（****CRCW** **冲突解决策略）**

![Exported image](Exported%20image%2020260730174849-4.png)

- 在 **CRCW PRAM** 中，必须指定==内存冲突的解决策略==。
- 四种常见变体：
    1. **Common**==：所有进程必须写相同的值。==
    2. **Arbitrary**==：任意选一个进程写入，其余失败。==
    3. **Priority**==：按进程优先级====/====编号决定写入成功。==
    4. **Associative**==：通过== **associative reduction****（结合性归约）** ==来解决写入冲突（如加法）==。
 
**Summing** **𝑛** **integers** **（****n** **个整数求和）****￼**

![Exported image](Exported%20image%2020260730174851-5.png)

- 考虑 **CRCW (associative: +)** 模型下的整数求和算法：
- 如果进程数 **p = n**，则存在一个 **Θ(1)** **时间复杂度** 的算法。

int a[n], sum;￼for (i = 0; i \< n; i++) do in parallel:￼ sum = a[i];￼  
==用来思考最大并行度是多少==
 
**Summing** **𝑛** **integers** **（****n** **个整数求和）****￼**

![Exported image](Exported%20image%2020260730174852-6.png)

- **EREW algorithm** （独占读写算法）
    - 时间复杂度：Θ(log 𝑛)
    - 需要 **𝑝** **=** **𝑛****/2 processes****￼**

**题目的算法感觉实际上有点问题，应该是****j****在循环的外面，但实际上表达的意思都是一样的：****￼**

![Exported image](Exported%20image%2020260730174853-7.png)  
![Exported image](Exported%20image%2020260730174854-8.png)  

**数学推导**

![Exported image](Exported%20image%2020260730174855-9.png)  
![Exported image](Exported%20image%2020260730174902-10.png)

**在代码中的含义**  
伪代码里写的是：
 
for j = 1 to log(n) do:￼  
其实对应的是：

- 当 j=1 → 间隔 1 → 每 2 个相加
- 当 j=2 → 间隔 2 → 每 4 个相加
- 当 j=3 → 间隔 4 → 每 8 个相加
- …直到 2^(j−1) = n/2 为止

这样，最后一轮就会把整个数组的结果合并成一个值。

![Exported image](Exported%20image%2020260730174903-11.png)

int a[n], temp[n], sum;￼  
// 初始化：并行复制￼for i = 0 to n-1 do in parallel:￼ temp[i] = a[i]￼  
// 逐层归约：对数轮次￼for j = 1 to log(n) do:￼ for i = 0 to n-1 do in parallel:￼ if (i mod (2^j) == 2^j - 1) then:￼ temp[i] += temp[i - 2^(j-1)]￼  
// 最终结果￼sum = temp[n-1];￼
 
**Is PRAM too powerful?** **（****PRAM** **是否过于强大？）****￼**

![Exported image](Exported%20image%2020260730174904-12.png)

- PRAM 的变体选择会影响 **渐近性能（****asymptotically achievable performance****）**。
- 最强大的 **CRCW** 模型能得到令人惊讶的结果：
    - ==例如：在== **CRCW (associative: +)** ==PRAM== ==中，若给定== **𝑛****²** **个进程**==，可以在== **常数时间（****constant time****）完成排序**==。==
- 不过：
    - PRAM 虽然是理想化模型，但提供了一个 **有用的起点**。
    - 我们可以先用 PRAM 设计新算法，然后再改进成更实用的形式。
 
**Constant Time Sorting** **（常数时间排序）**

- 模型：**CRCW (associative: +) PRAM**
- 思路：并行比较所有元素，通过“胜场（wins）”数组决定排名。￼
- ![Exported image](Exported%20image%2020260730174905-13.png)
 ![Exported image](Exported%20image%2020260730174906-14.png)  
![Exported image](Exported%20image%2020260730174907-15.png)

￼￼￼int A[n], wins[n];  
// 并行比较：计算每个元素的胜场数￼for i = 0 to n-1 do in parallel:￼ for j = 0 to n-1 do in parallel:￼ if (A[i] \> A[j]) or (A[i] == A[j] and i \> j) then:￼ wins[i] = 1;￼ else:￼ wins[i] = 0;￼  
// 按胜场数写入排序后的位置￼for i = 0 to n-1 do in parallel:￼ A[wins[i]] = A[i];￼

- **说明**：
    - 每个元素通过并行比较，得到它在排序结果中的位置。
    - 需要 **𝑛****²** **次并行比较**，但由于有 **𝑛****²** **个进程**，所以运行时间为常数级 Θ(1)。
 
要不要我帮你把 **EREW** **和** **CRCW** **求和算法** 做一个对比表格（时间复杂度 / 所需进程数 / 并行程度），这样更直观？
 \> 来自 \<[https://chatgpt.com/c/68dae8a1-b1c0-832e-aaee-c6601b222921](https://chatgpt.com/c/68dae8a1-b1c0-832e-aaee-c6601b222921)\>

![[Lec4 Architectures and PRAM - Ink.svg]]

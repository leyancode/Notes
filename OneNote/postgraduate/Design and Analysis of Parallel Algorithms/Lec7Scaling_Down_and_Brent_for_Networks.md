**Message Passing Costs**  
**Sum Reduction Example****（求和归约示例）**

![Exported image](Exported%20image%2020260730175321-0.png)  

- ![Exported image](Exported%20image%2020260730175322-1.png)
- 。
 
**Scaling Down****（缩放）**

- 与 PRAM 算法相比，缩小消息传递算法规模更加复杂，因为：
    - ==数据位置（====location of data==）会变得重要。
- **Quotient networks** 可以将大规模网络映射到较小的网络：
    - ==进程映射是平衡的（====balanced mapping of processes====）。==
    - ==不存在边扩张（====no edge dilation====），即边最多映射为一条边。==
    - ==采用轮转模拟（====round-robin emulation====）可以保持常数开销。==
    - ==因此，对于大== ==( p )== ==的高效算法，也可以缩小为小== ==( p' )== ==的高效算法。==
 
**Quotient Networks****（商网络）**

![Exported image](Exported%20image%2020260730175346-2.png)

- 原来是8*8.现在分成4个4*4
- Mesh（网格）和 Hypercube（超立方体）都可以用于构造 quotient networks。
- 通过 quotient network，我们可以将大规模拓扑结构映射到较小拓扑上，从而实现高效缩放。
 
**Brent for Networks****（****Brent** **缩放定律在网络中的应用）****￼**

- ==当针对大== ==( p )== ==的算法本身并不高效时，有时缩放到== ==( p' )== ==进程后反而可以得到更高效的算法。==
- ==数据放置（====data placement====）与串行部分的效率对整体性能至关重要。==
- 但这种情况并非总是成立，不能依赖它。
 
**Brent for Networks Example****（示例）**

![Exported image](Exported%20image%2020260730175348-3.png)

==考虑在一个== ==( p )== ==进程的== ==hypercube== ==上对== ==( n )== ==个数求和（====( n \> p )====）。==

- ==将== ==( n )== ==个数据项按照== ==round-robin== ==方法分配给== ==( p )== ==个进程。==
- 通信步骤：
    1. 先在网络上通信（reduce over network）
    2. 再在本地计算（reduce locally）￼￼因为已经将n个数据分配给p个进程，所以开始每个进程都有n/p个数据量
      
    
- ![Exported image](Exported%20image%2020260730175349-4.png)
- 因此，该算法 **不是** **cost optimal**。
   
 - ![Exported image](Exported%20image%2020260730175350-5.png)
- ==￼====也就是说前面一段时间通信完之后，后面只需要单进程，不用再像树结构那样去考虑通信成本====￼====￼==
- ![Exported image](Exported%20image%2020260730175351-6.png)
 
**小结（****Summary****）**

|   |   |   |
|---|---|---|
|**内容**|**概念**|**关键点**|
|Sum reduction|超立方体通信|并行时间随 (\log n) 增长，效率降低|
|Scaling down|缩小规模|数据位置重要，quotient network 可帮助缩放|
|Quotient networks|拓扑映射|mesh 和 hypercube 都适用|
|Brent for Networks|缩放原理|并非所有算法缩小规模后都更高效|
|示例|( n ) 个数在 ( p ) 进程上求和|总成本 (\Theta(n \log p))，非成本最优|

以下是整理后的课堂笔记内容，延续前面《Brent for Networks》的格式，不使用特殊符号，保留英文术语与公式：
 
**Brent for Networks****（****Brent** **缩放定律在网络中的进一步优化）****￼**  
**下面的图就是上面的示例，上面的****round-robin****是累计的不做其它处理，下面是直接相加****￼**

![Exported image](Exported%20image%2020260730175352-7.png)  
![Exported image](Exported%20image%2020260730175353-8.png)  
![Exported image](Exported%20image%2020260730175437-9.png)  
![Exported image](Exported%20image%2020260730175439-10.png)  
![Exported image](Exported%20image%2020260730175440-11.png)

**因为在这里，相加的成本是****o(1),****所以可以忽略****￼**

![Exported image](Exported%20image%2020260730175440-12.png)  
![Exported image](Exported%20image%2020260730175441-13.png)  

- ![Exported image](Exported%20image%2020260730175443-14.png)
- 这与 **共享内存版本（****shared memory version****）** 的结论一致。
 
**Inter Model Emulation****（模型间仿真）****￼**

![Exported image](Exported%20image%2020260730175444-15.png)

- 我们不需要为每一种消息传递（message passing）机器体系结构重新设计算法。
- 只需要：
    - ==将进程映射到进程（====map processes to processes====）；==
    - ==将链路====/====内存映射到链路====/====内存（====map links/memory to links/memory====）；==
    - ==计算每一步的额外开销（====calculate the overhead involved in each step====）==。
- 在某些特殊情况下，还可以进行额外优化：
    - 例如，当某些链路被使用得更频繁时（if some links are used more than others），￼可以将这些使用分散得更均匀（spread out evenly），￼以提高算法整体性能（improve the algorithm）。
 
**小结（****Summary****）**

![Exported image](Exported%20image%2020260730175509-16.png)

本门课中，我们基本不讨论load balancing,因为这个非常复杂

![[Lec7Scaling_Down_and_Brent_for_Networks - Ink.svg]]

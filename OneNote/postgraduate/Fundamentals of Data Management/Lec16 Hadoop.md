下面为你继续按照课堂笔记的格式，将 Hadoop 相关内容系统地整理成中文或中英双语的复习笔记。结构与前面 Fabric 整理部分保持一致，内容清晰、可直接用于复习。
 
**Hadoop****（讲义整理版）**  
**Fundamentals of Data Management**

1. What is Hadoop?

![Exported image](Exported%20image%2020260730192430-0.png)

Hadoop =  
• **HDFS****（****Hadoop Distributed File System****）**  
• **MapReduce Framework**  
• Supporting functionality，例如：  
– **YARN**（Resource management & scheduling）  
– **Hadoop Ozone**（2020 引入的 Object Store）  
Hadoop 的核心理念：  
• 处理大规模数据（Big Data）  
• 将数据与计算分布到大量 commodity nodes  
• 高容错性、可扩展性
 
1. Hadoop Distributed File System（HDFS）

![Exported image](Exported%20image%2020260730192431-1.png)

1. 使用场景
 
• **Typical use: write once, read many**  
– 大文件一次写入、多次读取  
• **Computation runs on Data Nodes**  
– 计算尽量移动到数据处（data locality）

1. 特性

![Exported image](Exported%20image%2020260730192431-2.png)

• **Distributed****（分布式）**  
• **Data redundancy****（数据冗余、常见** **3** **副本策略）**  
• **Cluster of commodity nodes****（普通硬件组成集群）**  
• **Designed to withstand failure**  
– DataNode 故障不影响系统  
– 早期 NameNode 是 single point of failure，后来通过 HA 改进  
• **Optimised for tasks in hand**  
– 非 POSIX file system（非标准文件系统语义）  
• **Placement strategies aware of data centre topology**  
– 数据放置可基于机架信息（rack awareness）

1. 文件写入过程（示意）
 
一个大文件被切分成 blocks（如 64MB），并分布到多个 Data Nodes。  
NameNode 负责管理 metadata（存储哪些 block 在哪些节点）。
 
1. Hadoop Framework（MapReduce 执行流程）

![Exported image](Exported%20image%2020260730192432-3.png)  

1. MapReduce 执行逻辑（基于图示）
 
每个 Node 执行以下流程：  
Node 1 / Node 2：  
• Local Data  
• Read  
• **Map**  
• Sort  
• Combine（可选）  
• Partition  
• Shuffle（跨节点数据交换）  
• Merge Sort  
• **Reduce**  
• Write  
• Result  
完整流程：  
**Map → Shuffle/Sort → Reduce**  
Map 阶段本地执行，Reduce 阶段可能跨节点汇总。
 
1. Reading & Writing the Data（输入与输出格式）

![Exported image](Exported%20image%2020260730192433-4.png)

1. InputFormat 接口
 
常见实现：  
• **TextInputFormat**  
– key：byte offset  
– value：line text  
• **KeyValueTextInputFormat**  
– 每行包含 key/separator/value  
• **SequenceFileInputFormat**  
– Hadoop 的二进制压缩格式  
• **NLineInputFormat**  
– 多行作为一个 InputSplit

1. OutputFormat 接口
 
• **TextOutputFormat**  
– 每条记录输出一行，key/separator/value  
• **SequenceFileOutputFormat**  
– 压缩二进制格式  
• 输出文件统一命名为：  
**part-xxxx**（xxxx 为 partition ID）
 
1. Combiner（组合器）优化
 
1. Combiner 的目的
 
• Map 阶段会产生大量中间数据  
• Combiner 在 Map 节点本地执行，起到预聚合作用  
• 减少 Shuffle 阶段数据量，提高性能

1. 示例

![Exported image](Exported%20image%2020260730192435-5.png)

Map 输出：  
• “A boy drove a car” → [\<a,1\>, \<boy,1\>, \<drove,1\>, \<a,1\>, \<car,1\>]  
• “A car drove at a bus” → ...  
Combiner 输入：  
• \<a, [1,1]\>  
• \<car, [1,1,1]\>  
• \<boy, [1,1]\>  
Combiner 输出：  
• \<a, [2]\>  
• \<car, [3]\>  
• \<boy, [2]\>  
效果：减少大量重复 key 的 value 列表，降低网络传输成本。￼￼￼

1. Combiner 特性

![Exported image](Exported%20image%2020260730192539-6.png)

• 仅为优化  
– 框架可能执行 0 次、1 次或多次  
• **不能改变最终结果**  
• **Keys** **不得被修改**  
• Hadoop 不会在 Combiner 后重新排序（reduce 前的顺序仍来自 Map）

1. Map → Combine → Reduce 的数据类型
 
==• Map====：==  
==\<Key1, Value1\> → List(\<Key2, Value2\>)====￼==  
==• Combine====：==  
==\<Key2, List(Value2)\> → \<Key2, List(Value2)\>====￼==  
==• Reduce====：==  
==\<Key2, List(Value2)\> → List(\<Key3, Value3\>)====￼====￼==  
下面为你继续按课堂笔记格式，将 **Partitioner****、****Chaining MapReduce****、****Iterative Limitations****、****Programming Hadoop****、****Java Mapper/Reducer****、****Streaming Mapper/Reducer** 完整整理成结构化、清晰且便于复习的内容。
 
**Hadoop: Partitioner****（分区器）****￼**

![Exported image](Exported%20image%2020260730192540-7.png)

1. Partitioner 作用
 
• 决定某个 \<key, value\> 最终被发送到哪个 Reducer。  
• Map 阶段输出在 Shuffle 阶段根据 Partitioner 的规则进行分区。

1. 类型
 
• **Hash Partitioner****（默认）**  
– 根据 key.hashCode() 对 Reducer 数量取模  
– 简单快速，但在数据 skew 时会导致负载不均衡  
• **Total Order Partitioner**  
– 保证 key 的全局排序（global ordering）  
– 适用于需要排序输出的作业  
– 可以配置使分区划分均匀  
• **Bespoke / Custom Partitioner**  
– 当数据高度 skew 时，Hash Partitioner 可能导致某些 Reducer 负载过重  
– 可以编写自定义分区器来改善均衡性  
– 例如：某些 key 需要特殊处理（如热门 key）
 
**Chaining MapReduce Jobs****（作业链式执行）****￼**

![Exported image](Exported%20image%2020260730192541-8.png)

1. 一个单独的 MapReduce Job 结构
 
• 1 个 Reduce 阶段  
• Reduce 前可以有多个 Map 阶段  
• Reduce 后也可以有 Map 阶段￼￼

1. 何时需要链式 Job

• 当算法需要 **多个** **Reduce** **阶段**  
– 因为 Reduce 会按照 key 对数据进行分组（grouping）  
• Job 之间通过 HDFS 交换数据  
• 前一 Job 的输出作为下一 Job 的输入  
例如：  
Job1: Map → Reduce  
Job2: Map → Reduce  
Job3: Map → Reduce
 
**Chain, but don’t iterate****（可链式，但不适合迭代计算）****￼**

![Exported image](Exported%20image%2020260730192542-9.png)

1. Hadoop 的设计限制
 
==•== **Each Hadoop job reads data from HDFS and writes results to HDFS**  
==•== ==不会在多个== ==job== ==之间保留内存状态==  
• 适合一些 pipeline 式的短链处理￼

1. 对迭代算法的影响

• 非常低效，因为：  
==–== ==每次迭代都要从磁盘重新读取所有数据==  
==–== ==即使数据完全不变，也无法缓存==  
==典型迭代算法（如== ==K-means====、====PageRank====）非常受损。==  
对比示例：  
• HPC：可在内存中反复更新模型  
• Hadoop：每步都要读写 HDFS（磁盘），代价巨大￼

1. 替代方案

• **Spark** – 支持 caching（RDD cache / persist），非常适合迭代  
• **Twister** – 支持 iterative MapReduce
 
**Programming Hadoop****（编写** **Hadoop** **程序）****￼**

![Exported image](Exported%20image%2020260730192543-10.png)

1. MapReduce 程序通常用 Java 编写

• Hadoop 框架本身由 Java 编写  
• 两种方式实现 Map/Reduce：  
– Java Class 实现 Mapper 和 Reducer  
– Hadoop Streaming（用脚本即可）￼

1. Hadoop Streaming

• 使用标准输入和标准输出进行数据交换  
• 输入输出格式：  
key TAB value￼  
• Mapper：  
– 逐行处理，无跨行状态（stateless）￼  
• Reducer：  
– 同一 key 的行是连续的  
– 当 key 变化时，可以认为前一个 key 的所有值已完整读取  
– Reducer 可对同一 key 的值保持本地状态，但不能跨 key 保持状态
 
**Java Mapper****（示例）****￼**

![Exported image](Exported%20image%2020260730192545-11.png)

示例：交换 key 和 value  
public static class MapClass￼extends Mapper\<Text, Text, Text, Text\>￼{￼ public void map(Text key, Text value, Context context)￼ {￼ context.write(value, key);￼ }￼}  
Mapper 模板：  
Mapper\<  
InputKeyType, InputValueType,  
OutputKeyType, OutputValueType  
特点：  
• 可以调用 context.write() 多次 → 输出 List(\<K2,V2\>)  
• 必须实现 map() 方法  
• Map 阶段不保证输出有序
 
**Java Reducer****（示例）****￼**

![Exported image](Exported%20image%2020260730192546-12.png)

示例：将同一 key 的 value 合并成 CSV  
public static class Reduce￼extends Reducer\<Text, Text, Text, Text\>￼{￼ public void reduce(Text key, Iterable\<Text\> values, Context context)￼ {￼ String csv = "";￼ for (Text val : values)￼ {￼ if (csv.length() \> 0) csv += ",";￼ csv += val.toString();￼ }￼ context.write(key, new Text(csv));￼ }￼}  
Reducer 模板：  
Reducer\<  
InputKeyType, InputValueType,  
OutputKeyType, OutputValueType  
特点：  
• values 是 Iterable，可处理超大规模 value 列表  
• context.write() 可调用多次  
• Reducer 只知道同一 key 的所有值都会连续到达  
注意：示例中的 CSV 拼接不是低内存实现（真实环境需更优化）。
 
**Streaming Mapper****（示例说明）****￼**

![Exported image](Exported%20image%2020260730192649-13.png)

输入：  
1 A long time ago￼2 in a galaxy far￼3 far away  
输出（word count mapper）：  
a 1￼long 1￼time 1￼ago 1￼...  
特点：  
• 完全 stateless  
• 每行独立处理
 
**Streaming Reducer****（示例说明）****￼**

![Exported image](Exported%20image%2020260730192651-14.png)

输入（按 key 分组排序后）：  
a 1￼a 1￼a 1￼far 1￼far 1￼time 1￼  
Reducer：

![Exported image](Exported%20image%2020260730192652-15.png)

• 同一 key 的所有值连续出现  
• 当 key 改变时，知道上一 key 的所有值已经全部接收完毕  
• 可对同一 key 维护状态（如做 sum）  
• 不可跨 key 维持状态  
输出：  
a 3￼far 2￼time 1
 
如果你愿意，我可以继续整理以下部分：  
• Shuffle & Sort 的完整流程  
• Combiner vs Reducer 的区别总结（考试高频）  
• Hadoop vs Spark 对比总结  
• MapReduce 的容错机制  
• Hadoop 架构思维导图版笔记  
你需要继续整理哪一部分？
   

**Hadoop vs MPI / HPC****（对比总结）****￼**

![Exported image](Exported%20image%2020260730192653-16.png)

1. Fault Tolerance（容错能力）
 
• **Hadoop**  
– 专为容错设计（HDFS 副本、任务失败自动重试）  
– 可在 commodity hardware 上可靠运行  
==•== **MPI / HPC**  
==–== ==几乎不提供容错机制==  
==–== ==多数== ==MPI== ==程序假设硬件不会失败==  
==–== ==节点故障通常导致整个作业终止==
 
1. Specific vs General（专用 vs 通用）
 
• **Hadoop**  
– 专门用于一种数据处理模式：Map → Shuffle → Reduce  
– 适用于批处理大规模数据  
• **MPI**  
– 通用并行编程模型  
– 理论上可实现任何算法  
– 对性能和控制力要求高的程序更适合 MPI
 
1. Iterative Algorithms（迭代算法）
 
• **Hadoop****：表现很差**  
– 每次迭代都必须从 HDFS 重新读取数据  
– 不支持内存缓存 → 迭代时磁盘 I/O 成本极高  
• **MPI****：表现出色**  
– 内存常驻数据结构  
– 非常容易实现大量迭代（例如 K-means、PDE 解、PageRank）
 
1. Speed（速度）
 
• HPC 系统上的优化 MPI 程序通常比 Hadoop 快得多  
– HPC 有高速互联（Infiniband）  
– MPI 没有磁盘 I/O 瓶颈  
• Hadoop 适合吞吐量优先的作业，但延迟较高
 
**Hadoop vs MPI / HPC****（续）****￼**

![Exported image](Exported%20image%2020260730192653-17.png)

1. Cost（成本）
 
• **Hadoop**  
– 能在 commodity hardware 上运行  
– 成本低、易部署  
• **MPI / HPC**  
– 典型运行在昂贵的 HPC 集群  
– 虽可运行在云平台，但用户需自建容错机制
 
1. Dynamic Nature of Data（数据动态性）
 
• **Hadoop**  
– 擅长处理海量数据  
– 数据“写一次、多读多处理”场景最适合  
– 足以承载 PB 级数据上传  
• **HPC / MPI**  
– 对超大规模数据上传不一定扩展良好  
– 核心用途不是海量数据管理，而是高性能数值计算
 
**Hadoop Ecosystem****（生态系统）****￼**

![Exported image](Exported%20image%2020260730192655-18.png)

1. HBase
 
• 分布式、可扩展的大数据存储  
• Column-oriented（列式数据库）  
• 类似 Google Bigtable

1. PIG
 
• 高层次的数据流语言  
• 编译为 MapReduce 作业

1. Mahout
 
• 在 Hadoop 上运行的可扩展机器学习和数据挖掘库

1. Spark

![Exported image](Exported%20image%2020260730192656-19.png)

• 通用数据处理和机器学习框架  
• 可使用 HDFS 作为数据源  
• 支持 caching → 非常适合迭代计算  
• 比 MapReduce 更丰富的 API：  
– Transformations：map、filter、union、join、sample…  
– Actions：reduce、collect、count、foreach…

1. 其他组件
 
• Hive（数据仓库）  
• Drill（SQL 查询引擎）  
• Kafka（流数据平台）  
• Storm（实时流处理）  
• Twister（迭代型 MapReduce）
 
**Hadoop 1.x → 2.x → 3.x****（版本演进）****￼**

![Exported image](Exported%20image%2020260730192800-20.png)

1. Hadoop 1
 
• 开源版 Google MapReduce + HDFS  
• 存在关键缺陷：**单** **NameNode****（****SPOF****）**  
• 架构：MRv1（固定 Map，Reduce slot）

1. Hadoop 2
 
• 引入 **YARN v1** 作为资源管理系统  
• 支持多类型应用，不再局限于 MapReduce  
• NameNode 增加单点 Failover 机制  
• 支持更大规模集群

1. Hadoop 3
 
• YARN v2（更强扩展性、更多 scheduler 策略）  
• 支持 **多个** **NameNode failover**，真正解决 SPOF  
• 数据 resilience 引入除 replication 以外的新方法（例如 Erasure Coding）  
– 降低存储成本（EC 比 3 副本节省空间）
 
**Additional Reading****（扩展阅读）**  
原始论文（非常重要且常被引用）：  
• **Google File System****（****GFS****）**  
[http://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf](http://static.googleusercontent.com/media/research.google.com/en/archive/gfs-sosp2003.pdf)  
• **MapReduce: Simplified Data Processing on Large Clusters**  
[http://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf](http://static.googleusercontent.com/media/research.google.com/en/archive/mapreduce-osdi04.pdf)  
教材：  
• _Hadoop in Action_（Manning）  
• _Hadoop: The Definitive Guide_（O'Reilly）  
课程说明：  
• 不要求阅读额外材料，考试以讲义与实验内容为主。
 
Acknowledgements  
• 幻灯片由 Adam Carter（EPCC）基于 Ally Hume 的原始课程内容整理  
• © 2015–2025 The University of Edinburgh
 
如果你需要，我可以继续为你：  
• 整理一个 “Hadoop 全章节超级总结表格版”  
• 完整 Hadoop vs Spark 对比表（考试高频）  
• 给你生成思维导图结构版本  
• 整理所有 MapReduce 考点（InputFormat、Combiner、Partitioner、Shuffle、Reducer 等）  
你希望下一步生成哪一种？
 \> 来自 \<[https://chatgpt.com/c/6932e9eb-3324-8333-b65c-cc6f6dc173b0](https://chatgpt.com/c/6932e9eb-3324-8333-b65c-cc6f6dc173b0)\>

![[Lec16 Hadoop - Ink.svg]]

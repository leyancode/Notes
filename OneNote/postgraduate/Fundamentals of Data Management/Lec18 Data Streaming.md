下面为你整理成与前面一致的课堂笔记格式，中文为主、关键英文词汇保留，逻辑清晰、适合复习或作为课程总结使用。
 
**Data Streaming****（数据流处理）**  
**Lecture Outline****（大纲）**

- What is data stream processing
- Why use streaming
- Strategies for data stream processing
- Examples
 
**1. What is Data Streaming****（什么是数据流处理）**  
Data Streaming 是一种用于处理超大规模数据集的策略，尤其适用于 Big Data 的典型三要素（Three Vs）：  
**Three Vs of Big Data**

- **Volume**：数据量非常大
- **Variety**：数据类型多样
- **Velocity**：数据产生速度极快

特点

- 数据流处理允许系统在数据产生时即时处理，而不是先将其完整存储再处理。
 
**2. Why Data Streaming****（为什么需要数据流处理）**  
原因主要来自数据规模、速度和实时需求：  
**数据量过大**

- 某些数据集大到无法完整存储
- 因此需要新的处理模型，应对“不能落盘的海量数据”

**数据产生速度极快**

- 数据到达速度远高于存储速度

**新型需求**

- **Process as data is produced**：边到达边处理
- **Compression****、****stacking****、****aggregation**：通过实时压缩、聚合减少存储与计算压力
- **Realtime computation of sensor data**：传感器数据必须实时处理
- **Short response times**：对低延迟需求极高的应用需要流式处理
 
**3. Examples****（案例）**  
**Example 1: LHC****（****Large Hadron Collider****）**  
LHC 是数据流处理的经典极端案例：

- 150 million sensors
- 每秒触发 40 million 次
- 数据量：**1 PB per second****￼**（如此速率下，不可能将全部数据写入磁盘）

应对策略：

- 预选策略（pre-selection）
    - 1/10,000 事件被快速筛选
- 进一步快速处理
    - 剩余部分中仅 1% 进入更深入分析
- 最终产出约 **30 PB per year**

总结：  
流式筛选是唯一可行路径，否则根本无法存储也无法处理。
 
**Example 2: X****（****formerly Twitter****）**  
X 也是典型的高速度数据流系统：

- 6000 tweets per second
- 超过 500 million per day

系统特点：

- tweets 到达即分析（analytics performed as they arrive）
- 存储在 MySQL 数据库
- 每条 tweet 有全球唯一 ID（globally unique ID）
- 使用 hash 决定 tweet 落到哪一个数据库
- 通过增加数据库数量提升处理吞吐

关键思想：水平扩展（scale-out）
 
**4. Strategies for Data Stream Processing****（数据流处理策略）**  
数据流处理需满足以下需求：  
**核心特征**

- 处理高容量（volume）或高速（velocity）流数据
- **No persistence**：不保存全部原始数据
- 使用 **sliding window****（滑动窗口）** 来代替完整历史记录
- 主要目标是实时信息提取
- 必须具备可扩展性（scalability），以应对峰值负载
- 最终目标：**short response times**

**应用方向**

- Data stream mining
- Real-time analytics
- Event detection
- Monitoring systems（网络流量、金融交易、传感器网络等）
 
**5. Strategies for Data Stream Mining****（数据流挖掘）**  
数据挖掘是数据处理的一个典型例子，但传统算法不适于数据流环境。  
**挑战**

- 多数 machine learning 算法需要多次扫描数据（multiple passes）
- 数据流太大时无法 random access，也不能多次读取

**解决方案**

- 使用 **single-pass algorithms****（单遍算法）**
- 维护 summary 或 sketches（小型摘要）
- 使用 approximate computing（近似计算）
- 使用在线学习（online learning）模型

这些方法允许系统在数据到达时就处理，而不依赖全局数据存储。  
下面为你整理成清晰、结构化的课堂笔记格式，中文为主、关键英文术语保留，与前面保持一致。
 
**Machine Learning Techniques in Data Streaming****（数据流中的机器学习技术）**
 
**1. Online Learning vs Offline Learning****（在线学习与离线学习）**  
**Online Learning****（在线学习）**  
特点

- 数据 **sequentially** 到达
- 模型 **持续更新（****continuously updated****）**
- 数据使用后即被丢弃
- 适用于无法多次查看数据的大规模数据流场景

优点

- 能处理无限数据流
- 适合实时应用
- 更新成本低
 
**Offline / Batch Learning****（离线或批处理学习）**  
特点

- 使用完整数据集训练模型
- 数据集静态、不随时间变化
- 但数据集可能过大，无法 random access

适用场景

- 数据可完全加载
- 模型训练不要求实时
 
**2. Sequential vs Random Access****（顺序访问** **vs** **随机访问）**  
**Sequential Access****（顺序访问）**

- 模型逐步增量更新（incremental update）
- 可以单步更新（single step）或使用 sliding windows（mini-batch）
- 常用于在线学习和数据流算法

**Random Access****（随机访问）**

- 可对数据进行多次扫描（multiple passes）
- 能支持复杂算法，如：
    - k-means
    - 递归算法
 
**3. Multipass Algorithms****（多遍算法）**  
特点

- 需要多次访问数据
- 通常 rely on random access
- 仅适用于数据能被重复读取的情况
- 理想状况是数据可放入内存

示例

- K-means

限制

- 不适用于数据太大的流式场景
 
**4. Single-Pass Algorithms****（单遍算法）**  
特点

- 输入数据按顺序读取一次
- 模型在每一步更新
- 模型必须可由少量可更新的统计量表示
- 时间复杂度通常为 O(n)
- 理想情况下，空间复杂度 O(1)

示例

- Naïve Bayes

优势

- 在流式数据处理中非常重要
- 可处理无限数据
- 无需存储历史记录
 
**5. Online Learning Community****（在线学习研究方向）**  
近年来，越来越多的研究者开发面向 data streams 的 incremental algorithms。  
目标

- 将传统需要多遍扫描的数据挖掘和机器学习算法转化为可在线、可增量的方法

研究领域包括

- Streaming clustering（流式聚类）
- Online K-means
- Online regression
- Online classification
 
**6. Data Streaming Challenges****（数据流处理中的挑战）**  
主要问题包括：  
**1. Joins****（关联操作）**

- Windowed join：基于时间窗口
- Stream–stream join
- Stream–static join

**2. Out-of-order arrival****（乱序到达）**

- 数据可能与时间戳顺序不一致
- 日志数据常见
- 解决方法：watermarking（延迟窗口 + 允许一定滞后）

**3.** **需要低延迟与可扩展性**
 
**7. Data Streaming Frameworks****（数据流处理框架）**  
常见技术栈：

- Apache Storm
- Apache Spark Structured Streaming
- Google TensorFlow
- Amazon Kinesis
- Apache Hadoop / YARN
 
**8. Apache Storm****（聚焦示例）**  
Storm 是一个分布式实时计算系统，特征：

- Scalable（可扩展）
- Fault tolerant（容错）
- 多语言简洁 API
- 通过拓扑结构（topology）描述数据流
- 拓扑由 nodes（bolts、spouts）与 edges（streams）构成
 
**8.1 Storm Concepts****（核心概念）**  
**Topology**

- 数据流处理图
- 长期运行，类似 MapReduce 的持续版本

**Spouts**

- 数据源
- 产生数据流（streams of tuples）

**Bolts**

- 数据处理单元
- 收到 tuple 时触发处理
- 可以输出 0 个或多个新 tuple
 
**8.2 How Storm Processes Data****（处理流程）**

- 数据以 tuple 流的形式注入系统
- 每个 bolt 处理一个输入 tuple，输出新 tuples
- spout → bolt → bolt，以拓扑顺序流动

示例（Twitter 流分析）  
Twitter feed → Identify trends → Notify subscriber
 
**9. Parallelisation in Storm****（并行化机制）**  
Storm 采用 data parallelism：

- 每个节点有多个实例实现并行
- 数据根据 grouping（分组策略）分配给不同实例

常见 grouping 类型

- **Shuffle grouping**：随机分配，默认方式
- **Fields grouping**：按字段 hash 分组，如按 user-id
- **All grouping**
- **Global grouping****（****all-to-one****）**

其他特点

- Worker nodes 等待任务分配
- Topology 可动态调整（rebalanced），以增加或减少 worker 数量

下面为你继续整理课堂笔记，结构、风格与前面保持一致，中文为主、关键英文保留，不使用特殊符号。
 
**Data Parallelisation in Storm****（****Storm** **中的数据并行化）**  
Storm 使用 **data parallelism****（数据并行）** 来提升吞吐量与实时处理能力。  
示例结构：  
Twitter Feed → Identify trends￼  
在并行化执行中，会变成：  
Twitter Feed￼Twitter Feed￼Twitter Feed￼ → Identify trends￼ → Identify trends￼ → Identify trends￼ → Identify trends￼  
说明

- 每一个 spout 或 bolt 都可以被复制为多个实例
- 数据流根据 grouping 策略分配给不同实例
- 通过增加实例数即可水平扩展

Storm 的目标：在不改变逻辑结构的前提下，通过并行复制节点来提升处理速度。
 
**Fault Tolerance in Storm****（****Storm** **的容错机制）**  
Storm 被设计为高度容错的实时系统。  
**1. Fault Recovery****（故障恢复）**

- 当某个 worker 进程失败时，其未完成的消息会被重新分配
- 失败的 worker 会自动重启
- 系统确保所有数据最终都会被处理

**2. Fault Detection****（故障检测）**

- Storm 会跟踪每条数据 tuple
- 如果 tuple 未被完全处理，则会被重新播放（replayed）

**3. Fail-fast, stateless daemons**

- Storm 的守护进程是 stateless 的，这意味着：
    - 失败后可以立即重启
    - 重启后系统自动恢复运行状态，无需人工干预

总结  
Storm 的容错设计确保即使在高并发和实时环境中也能保持可靠性。
 
**Conclusion****（小结）**  
大数据挑战传统处理方式，核心难点来自 Big Data 的三大 V：

- **Variety****（多样性）**
- **Velocity****（高速）**
- **Volume****（海量）**

在数据不能完全持久化的情况下，**实时处理（****real-time processing****）** 成为必要策略。  
Storm、Spark 等框架应运而生，专为实时和大规模分布式处理而设计。
 
**Introduction to Apache Spark****（****Spark** **简介）**  
演讲者：Amy Krause, Principal Architect, EPCC  
课程：Fundamentals of Data Management
 
**1. Apache Spark** **概述**  
Spark 是一个 **fast****、****expressive****、开源、兼容** **Hadoop** **的分布式计算系统**。  
特点

- 比 Hadoop MapReduce 更快
- 提供更丰富、更易用的 API
- 拥有大型社区支持
- 支持多种计算模式，而不仅仅是批处理（batch）

Spark 支持以下工作负载：

- interactive queries（交互式查询）
- streaming（流处理）
- machine learning（机器学习）
- graph processing（图计算）
 
**2. Spark** **的核心特性**  
Spark 是一个 **general-purpose, cluster, in-memory computing system****（通用的集群内存计算系统）**。  
Spark 提供高层 API：

- Java
- Scala
- Python
 
**Use Cases****（使用场景）**  
Spark 广泛用于以下领域：

- 批处理
- 实时分析
- 机器学习训练与推理
- 图分析
- 日志分析
- IoT 数据处理
- 金融风控
- 科学计算
 
**How to Use Spark****（使用方式）**  
Spark 提供两种使用方式：  
**1. Interactive Mode****（交互式模式）**

- 适用于测试与开发
- 可在本地机器上运行
- 使用共享内存和多个 CPU cores

**2. Cluster Job Submission****（提交任务到集群）**  
Spark 可以运行在不同的 cluster manager 上：

- Spark Standalone
- Hadoop YARN
- Apache Mesos
- Kubernetes
- 云服务（各种大厂 Cloud providers）

下面为你继续整理课堂笔记，风格与前面保持一致，中文为主、关键英文术语保留，内容结构清晰、适合复习。
 
**Spark Functionality****（****Spark** **的主要功能）**  
Spark 提供广泛的数据处理能力：  
**1.** **多数据源访问**  
Spark 支持直接读取多种数据源，包括：

- HDFS
- HBase
- Amazon S3
- 其他分布式存储系统

**2.** **分布式并行计算**

- 自动将计算分布到集群中的多个 worker 节点
- 任务可并行执行，提升吞吐量和效率

**3.** **数据缓存（****Caching****）**

- RDD 或 DataFrame 可缓存到内存
- 适用于迭代算法，如机器学习与图计算
- 比 Hadoop MapReduce 快得多（尤其 iterative workflows）

**4. JVM** **环境下运行**

- Spark 运行在 Java VM 上
- 支持 Java、Scala、Python、R 四种主流语言
 
**Supported Languages****（支持语言）**

- Spark **本身由** **Scala** **编写**
    - Scala 语言： [https://www.scala-lang.org/](https://www.scala-lang.org/)
    - 编译为 Java bytecode
    - 运行在 JVM 上（跨平台）
- Spark 提供语言 API：
    - Scala（最完整）
    - Java
    - Python（PySpark）
    - R（SparkR）

注意

- 不同语言对 API 的覆盖率不同
- Python API（PySpark）仍不如 Scala 完整（尤其低层 API）
 
**Spark Basics: RDD****（****Spark** **基础：****RDD****）**  
RDD 全称 **Resilient Distributed Dataset**，是 Spark 的核心抽象。  
**特性概述**  
**1. Distributed****（分布式）**

- RDD 是一个分布式数据集合
- 数据被切分成 partitions 存储在多个 worker 上

**2. Read-only & Immutable****（只读且不可变）**

- RDD 不可修改
- 任何操作都会创建一个新的 RDD

**3. Fault Tolerant****（容错性强）**

- 通过 lineage（血统记录）恢复数据
- 若某个 partition 丢失，可根据 lineage 重新计算

**4. Parallel execution****（并行执行）**

- RDD 操作自动并行化

**5. Caching****（缓存能力）**

- 多种缓存级别可选（MEMORY、DISK 等）
- 适用于重复计算的场景
 
**Partition****（分区）**

- Spark 自动将 RDD 分成多个 partitions
- 每个 partition 由一个 worker 节点负责处理
- 并行度取决于 partition 数量
- 示例：3 workers 处理 3 partitions
 
**Transformations****（转换操作）**  
Transformations **不会立刻执行**，只是构建新的 RDD。  
常见操作：

- **map**：将每个元素转换为新元素
- **filter**：过滤不符合条件的元素
- **join**：基于 key 匹配两个 RDD
- **union**、**intersection**、**distinct**
- **groupByKey**、**reduceByKey**、**aggregateByKey**
- **sortByKey**

Transformation 操作之间可连续组合，例如：  
map → filter → groupBy → sort￼
 
**Actions****（行动操作）**  
Action 会触发实际计算，生成最终结果或产生副作用。  
常见 Actions：

- **reduce**
- **count****、****countByKey**
- **take****、****takeSample****、****takeOrdered****、****first**
- 保存结果
    - saveAsTextFile
    - saveAsSequenceFile
- **foreach**：对每个元素执行函数（常用于输出）

Transformations 是 lazy evaluation，而 Actions 触发执行。
 
**Execution Model****（执行模型）**  
Spark 的执行逻辑如下：

1. 用户编写 RDD Transformations
2. Spark 构建一个 operator DAG（血统有向无环图）
3. 执行 Action 时，DAGScheduler 将 DAG 切分为多个 stages
4. TaskScheduler 将任务调度到 cluster manager
5. Worker 节点执行 tasks
6. 若内存不足，中间数据 spill 到磁盘

关键特征

- 计算按需触发（lazy）
- 自动容错
- data locality aware（尽量把计算放在数据所在节点）
 
**Job Scheduling****（作业调度流程）**  
调度器流程（简化版）：  
RDD Objects￼ ↓ build operator DAG￼DAGScheduler￼ ↓ split graph into stages￼TaskScheduler￼ ↓ launch tasks￼Cluster manager￼ ↓ distribute tasks￼Workers￼ ↓ execute & store data blocks￼Block manager￼  
DAGScheduler

- 分析 dependences
- 将 job 分解为多个 stages

TaskScheduler

- 分配 task 给 workers
- 处理失败重试
- 处理 straggler（慢任务）

Worker

- 执行任务
- 将 block 存储在 Block Manager 中
 
**Job Submission to a Cluster****（集群任务提交）**  
流程：

- 将作业提交给 master 节点
- master 监听 host:port
- master 将任务分配给 worker 节点
- 可在 Web UI 中监控任务进度

任务分配考虑因素

- cluster 内的资源状况
- memory usage
- data locality

下面为你整理本节内容，延续之前笔记的结构与风格，中文为主、关键英文术语保留，并将图示内容（DAG、stage 划分）以文字形式清晰呈现。
 
**Example Application on YARN****（在** **YARN** **上运行** **Spark** **应用示例）**  
下图（你提供的截图）展示了一个 Spark Word Count 程序在 Hadoop YARN 上执行时的 DAG 可视化：  
**DAG** **分为两个** **Stages**  
**Stage 0**  
包含：

- textFile（读取 HDFS 文件）
- flatMap（将行拆分成单词）
- map（映射单词为键值对）

这些操作属于 **narrow transformations**，不会触发 shuffle，因此在同一个 stage 中完成。  
**Stage 1**  
包含：

- reduceByKey

此操作需要跨 partition 聚合，因此触发 **shuffle**。  
Spark 会创建 ShuffledRDD 并形成新 stage。  
总结：

- Stage 0：map-side 处理
- Stage 1：reduce-side 聚合（触发 shuffle）
 
**Spark Standalone Cluster****（****Spark** **独立集群模式）**  
Spark 可以以 Standalone 模式运行，不依赖 Hadoop。  
**启动方式**

- 启动 master 节点
- 启动 worker 节点
- workers 会根据 master URL 自动注册

**特征**  
优点

- 部署简单
- 易于快速运行小规模集群

缺点

- 鲁棒性较弱
- 配置管理不如 Hadoop YARN 完善
- 不适合复杂企业环境

适用场景

- 个人实验
- 教学
- 小型研究环境
 
**Running Spark on Hadoop YARN****（在** **YARN** **上运行** **Spark****）**  
Spark + YARN 是企业生产环境最常用的方案之一。  
**要求**

- 一个 Hadoop YARN 集群（包含 HDFS、ResourceManager、NodeManager）

**优势**

- 利用 Hadoop 提供的功能：
    - Node management
    - Cluster configuration
    - Distributed file system（HDFS）
    - Data replication
    - Fault recovery

适用场景

- 生产级大数据处理管线
- Hadoop 生态集成度要求高的环境
 
**Running Spark on Kubernetes****（在** **Kubernetes** **上运行** **Spark****）**  
Spark 可直接提交到 Kubernetes 集群。  
**提交流程**

- K8s 为 Spark driver 和 executors 分配 pods
- Spark driver 负责监督整个应用执行
- Executor pods 在 job 完成后自动清理
- 每个 pod 都基于 Docker image 创建

典型用途

- 云原生环境
- 动态伸缩
- 与容器化部署深度结合
 
**Example: Word Count on YARN****（示例：****Word Count** **程序在** **YARN** **上的执行）**  
示例代码：  
lines = spark.textFile("hdfs://...")￼errors = lines.filter(lambda s: s.startswith("ERROR"))￼messages = errors.map(lambda s: s.split('\t')[2])￼messages.cache()￼  
messages.filter(lambda s: "foo" in s).count()￼messages.filter(lambda s: "bar" in s).count()￼  
流程说明：  
**1.** **创建** **RDD**  
lines → errors → messages￼  
messages 被 cache，因此存入 worker 内存中（Cache 1, Cache 2, Cache 3）  
**2.** **执行** **actions**

- messages.filter("foo").count()
- messages.filter("bar").count()

由于 messages 已缓存，操作提高了速度，避免重复从 HDFS 读取和重复计算。  
**3.** **角色分工**

- **Driver**：负责调度任务
- **Workers**：执行 transformations 与 actions
- **Cache blocks**：存储到内存中的 RDD partitions
 
**Example: Mining Console Logs****（挖掘控制台日志）**  
典型步骤：

1. 将日志文件载入 Spark
2. 过滤 error messages（错误日志）
3. 缓存结果
4. 交互式搜索 patterns

Spark 的优势：

- 能够将日志加载到内存，大幅提升搜索速度
- 可交互分析（可用于安全分析、调试、系统监控）
 
**Spark Ecosystem****（****Spark** **生态系统）**  
Spark 核心由一系列模块组成：
 
**Spark Core**  
Spark Core 提供以下基础功能：

- Task scheduling（任务调度）
- Memory management（内存管理）
- Fault recovery（容错机制）
- 与存储系统交互（如 HDFS）

定义 Spark 最基本的抽象：

- **RDD****：****Resilient Distributed Dataset**

RDD 是 Spark 最底层、最重要的编程抽象。
 
**MLlib****（****Spark** **机器学习库）**  
MLlib 是 Spark 的机器学习库，提供：

- Basic statistics
- Classification（例如 Naïve Bayes、Decision Trees）
- Clustering（k-means、Gaussian mixture models）
- Recommendation algorithms
- Feature transformations
- Frequent updates

适用于大规模机器学习任务（相比 scikit-learn 更适合集群环境）。  
下面为你整理本节关于 SparkSQL、DataFrames、Streaming、Structured Streaming 的课堂笔记，风格统一、结构清晰、中文为主、关键英文词汇保留。
 
**SparkSQL and DataFrames****（****SparkSQL** **与** **DataFrame****）**  
SparkSQL 是 Spark 生态中处理结构化数据的核心组件，它让用户可以以关系型数据库的方式操作大规模数据。  
**主要特性**  
**1. View datasets as relational tables**

- 将数据集视为关系型表（table）
- 可以定义 schema（列名、类型）

**2. Schema support**

- 通过 StructType、StructField 或自动推断生成 schema
- 可以更安全、更高效地操作数据

**3. SQL queries**

- 支持标准 SQL 查询
- 例如：￼SELECT word, COUNT(*) FROM table GROUP BY word￼

**4. DataFrame API**

- 是高度优化的分布式数据结构
- 类似 R 或 Pandas 中的 DataFrame，但支持分布式计算

DataFrame 在 R 语言社区非常流行，Spark 的 DataFrame 是其在大数据领域的扩展版本。
 
**Spark Streaming****（传统流处理接口）**  
Spark Streaming 用于实时或准实时分析流式数据。  
**核心特征**  
**1.** **基于** **DStreams****（****Discretized Streams****）**

- DStream 是由小批次 RDD 组成的序列
- 每个 batch 对应一个固定时间窗口的数据
- 相当于把流切分成一系列微批处理（micro-batch）

**2. High-throughput and fault-tolerant**

- 高吞吐
- 容错强
- 可以处理连续的数据流，如日志、传感器数据、网络流量

**3. Window operations**

- 支持基于时间窗口的数据处理
- 如过去 10 秒内的 count、average 等操作

典型应用场景

- 实时日志监控
- 在线统计
- IoT 数据采集
 
**Structured Streaming****（新一代流处理框架）**  
Structured Streaming 是 Spark 在流式数据处理上的重大升级，基于 Spark SQL 引擎。  
**核心优势**  
**1. Built on SQL engine**

- 利用 Catalyst 优化器和 Tungsten 执行引擎
- 表达能力更强，性能更高

**2. Continuous & incremental computation**

- 读取 streaming data source
- 持续执行增量计算
- 类似持续更新的表（table），而不是 batch 的序列

**3. Results table updates**

- 当新数据到达时，会自动更新结果表
- 支持 append、update、complete 输出模式

**4. Window operations on event time**

- 支持 event-time 语义（而非仅处理时间）
- 更适用于乱序数据（常见于日志、用户事件）

**5. Time-window aggregations**

- 类似 group by，但按时间窗口聚合
- 如 10 分钟窗口的用户点击数

**6. Streaming joins**

- 支持 stream–stream join（基于时间窗口）
- 支持 stream–static join（与静态数据表关联）

**7. Watermarking**  
引入延迟容忍机制，应对乱序数据  
作用：

- 指定事件最大延迟
- 超过窗口和延迟的数据将被丢弃以控制状态大小

Structured Streaming 是 Spark 官方推荐的现代流处理方式。
 
**Conclusion****（总结）**

- **Apache Spark** 是一个功能全面的数据分析框架
- 容易学习、广泛使用、社区活跃
- 支持 SQL、机器学习、图计算、批处理、流处理
- 包含机器学习库（MLlib）、图计算库（GraphX）、流式处理（Structured Streaming）
- Spark 仍在持续开发与扩展，是大数据处理的主流技术之一
 
如需，我可以帮你把 Spark 全章节整理成一份完整的复习 cheat sheet（适合考试使用），或进一步补充：

- RDD vs DataFrame vs Dataset 对比
- Spark Shuffle 机制详解
- Structured Streaming 程序结构模板
- 水位线（watermarking）和时间窗口图示

告诉我你想继续哪部分。
 \> 来自 \<[https://chatgpt.com/c/6932ec39-7f6c-8330-8ab0-bfc2b384606a](https://chatgpt.com/c/6932ec39-7f6c-8330-8ab0-bfc2b384606a)\>
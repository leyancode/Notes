**What is Data Processing Architecture?** **（什么是数据处理架构？）**  
**定义**  
==数据处理架构是对以下环节如何相互连接的描述==：

- Data Sources（数据源）
- Data Ingestion / Buffering（数据引入 / 缓冲）
- Data Management（数据管理）
- Data Processing（数据处理：Batch / Stream）
- Data Analytics & Machine Learning（数据分析与机器学习）
- Reporting（报告输出）
 
==What is Data Processing== ==Architecture?==  
==•== ==A description of how data sources, data ingestion,==  
==data management, data processing and analysis are==  
==linked together.==

![Exported image](Exported%20image%2020260730190946-0.png)   
**Data Sources****（数据源）**  
**Real-Time Sources****（实时数据源）**

- 来自 IoT Devices（物联网设备）
- 卫星图像、Bio-Medical sensor（生物医学传感器）、汽车等
- Social media posts（社交媒体帖子）
    - Twitter, Facebook 等

**Static Sources****（静态数据源）**

- Application generated files（应用生成文件）
    - 如 Web server logs（服务器日志）、用户论坛数据等
- Database Management Systems (DBMSs)（数据库管理系统）
    - 例如：公司人力资源数据库
 
**Data Management****（数据管理）**  
**管理范围**

- 结构化、半结构化与非结构化数据

**Database Management Systems****（数据库管理系统）**

![Exported image](Exported%20image%2020260730190947-1.png)

- **Relational Database Management Systems (RDBMSs)**
    - e.g., MySQL, SQL Server, Oracle
- **NoSQL Database Management Systems**
    - e.g., MongoDB, Cassandra, Neo4J, Redis
- **RDF (Resource Description Framework) data stores**
    - e.g., Fuseki, Virtuoso, GraphDB

（以上内容会在后续课程中详细讲解）
 
**Data Management - Continued****（续）****￼**

![Exported image](Exported%20image%2020260730190949-2.png)

**Distributed storage****（分布式存储）**

- Hadoop Distributed File System (HDFS)
- Distributed Object Storage
    - e.g., Amazon S3 (Amazon Simple Storage Service)

**File storage****（文件存储）**

- 传统的层级式文件系统（通过目录和文件存取数据）
- 配置简单，但性能受限于单一路径
- 仅通过常见文件级协议运行
    - e.g., NTFS (Windows), NFS (Linux)
- 可能基于 NAS (Network Attached Storage)
- 高性能版本可提供并行 I/O，如 BeeGFS
 
**Data Management - Continued****（续）****￼**

![Exported image](Exported%20image%2020260730190950-3.png)

**Block storage****（块存储）**

- 将数据拆分为多个 **blocks**，每个块有唯一 ID
- 存储位置：
    - Storage Area Networks (SANs)
    - Cloud-based storage environments
- 特点：快速、高效、可靠
- 可跨不同操作系统使用
- 底层系统通过 block 映射来满足用户或应用请求
 
**Data Management - Continued****（续）****￼**

![Exported image](Exported%20image%2020260730190952-4.png)

**Object storage****（对象存储）**

- 将数据拆分为 **objects**（对象），例如一个文件
- 每个 object 拥有唯一 ID
- 应用需自行合并对象，例如将多个结果对象整合为单一文件
- 对象存储于单一容器内，可分布在多个系统中
- 无文件系统层级结构（no hierarchy）
- 优势：可扩展到 PB 级甚至更大
- 适合静态、不频繁变化的文件
    - 每次修改都会生成新的 object
- 示例：
    - AWS S3 (Simple Storage Service)
    - OpenStack Swift (Object Storage)

好的，我帮你把这一部分也整理成中文/中英双文课堂笔记风格，保留关键的英文术语。这样你整体的笔记会更统一：
 
**Data Management - Continued** **（续）**  
**Object Storage****（对象存储）**

- 将数据拆分为 **objects****（对象）**
- 每个 object 可能是应用中的一个文件
- 每个 object 拥有唯一 ID
- 用户或应用可直接处理 objects，例如：
    - 应用结果分散存储为多个 objects
    - 应用需要将多个 objects 合并为单一结果文件
- objects 存储在单一 container（容器）中，可分布在多个系统中
    - e.g., 分布式存储系统
- 无文件系统的层级结构（No hierarchy）
- 可扩展到 **petabytes** 级别或更大规模
- 最适合用于静态、不经常修改的文件
    - 文件的任何修改会生成新的 object
- 示例：
    - **AWS S3 (Simple Storage Service)**
    - **OpenStack Swift (Object Storage)**
 
**Data Warehouse****（数据仓库）****￼**

![Exported image](Exported%20image%2020260730190953-5.png)

- 基于==单一数据库管理系统==存储大规模历史数据
- 整合多个数据源的数据
- 数据经过转换并准备好用于分析（历史数据分析）

**数据仓库的组成**

- Relational database（关系型数据库）用于存储和管理数据
- ETL（Extract, Transform, Load）功能
- Statistical analysis, data mining, reporting（统计分析、数据挖掘和报表）
- 可视化和展示工具，供业务人员使用

**数据仓库的四大特征**

- **Subject-oriented****（面向主题）**：可分析特定主题的数据（如销售 Sales）
- **Integrated****（集成性）**：将来自不同来源的数据链接并保持一致性
- **Non-volatile****（非易失性）**：导入的数据稳定，不会频繁变动
- **Time-variant****（时变性）**：可基于时间变化进行分析
 
**Data Lake****（数据湖）****￼**

![Exported image](Exported%20image%2020260730190957-6.png)

- 大规模、容错的数据存储，基于分布式对象/文件存储
- 存储 **异构的、未过滤的、结构化****/****非结构化的原始数据**
- 数据以 **原始形式（****as-is****）** 存储
- 不需要提前设计数据存储方案
    - 捕获数据时不定义 Schema
    - 无需提前知道要执行什么查询
- 分析者需在使用时进行过滤、格式化、结构化和数据链接
- 可对原始数据执行多种数据分析
    - e.g., **Natural Language Processing (NLP)**, **Machine Learning (ML)**
 
**Data Buffering****（数据缓冲）**

![Exported image](Exported%20image%2020260730190958-7.png)

- 通常使用 **messaging system****（消息系统）**
- 将数据包作为消息暂存（buffer）
- 示例：**Kafka, RabbitMQ, ActiveMQ**
- **Producer****（生产者）**：从数据源生成数据
- Producer 将数据（消息）发送给 **Consumer****（消费者）**
- **Consumer**：对数据进行处理，用于 Data Analytics 或存储到 Data Management 系统

![Exported image](Exported%20image%2020260730190959-8.png)   
**Batch Processing****（批处理）****￼**

![Exported image](Exported%20image%2020260730191000-9.png)

**定义**

- 一次性处理在一段时间内收集的大量数据
- 规模可能是 **millions of records****（百万级记录）、****terabytes** **或** **petabytes****（****TB/PB** **级数据）**
- 常见操作：**filtering****（过滤）、****aggregation****（聚合）**，为数据分析做准备
- 通常是 **non-stop****（不中断执行）**，一次性完成

**示例**

- **Payroll processing****（工资处理）**

**执行特征**

- 通常可控制执行时间
    - e.g., 每天结束（end of the day）、每周、每月

**实现方式**

1. **高性能集中式服务器**
    - 本地附带大容量存储
    - 示例：IBM、Oracle 等提供的专用服务器硬件
2. **分布式系统**
    - 每个节点都有本地存储
    - 即 **distributed computing and storage system****（分布式计算与存储系统）**
    - 示例：Hadoop、Storm 等，使用普通硬件（commodity hardware）
   

**Stream Processing****（流处理）**

![Exported image](Exported%20image%2020260730191001-10.png)

**定义**

- 处理 **continuous stream of data****（连续数据流）**，通常是实时生成的
- 数据可能是 **high velocity****（高速度）**
    - 在极短时间内产生（毫秒级）

**特点**

- 执行 **filtering****（过滤）、****aggregation****（聚合）** 来为数据分析做准备
- 分析通常基于 **time-window****（时间窗口）**
    - e.g., 每 10 分钟、每小时
- 适用于以下场景：
    - 需要快速检测、分析并响应事件
    - 事件频繁发生且时间间隔很短
    - e.g., 网络故障、欺诈检测、网络安全
- 相较于 Batch Processing（批处理）更具挑战：
    - 如何处理 **高速度数据**
    - 已处理数据过大时的存储问题
 
**Lambda Architecture****（****Lambda** **架构）**

![Exported image](Exported%20image%2020260730191002-11.png)

**设计目标**

- 高吞吐量（High throughput）
- 低延迟（Low latency）
- 可忽略的错误率（Negligible errors）

**架构层次**

- **Batch Layer****（批处理层）**
    - 原始数据进行批处理
    - 结果存储为 Batch Views，可供查询
- **Speed Layer****（速度层）**
    - 实时分析数据
    - 特点：低延迟，但精确度较低
- **Serving Layer****（服务层）**
    - 对 Batch Views 建立索引，支持高效查询
    - Speed Layer 提供增量更新（incremental updates）

**数据流动**

- **Hot Path****（热路径）** → Stream Processing（流处理）
- **Cold Path****（冷路径）** → Batch Processing（批处理）
 
**Kappa Architecture****（****Kappa** **架构）**

![Exported image](Exported%20image%2020260730191003-12.png)  
![Exported image](Exported%20image%2020260730191007-13.png)

**￼****核心区别**

- 与 Lambda 架构相似，但 **数据流只经过单一路径**
- 不再重复执行两套逻辑
- 数据不会被处理两次

**特点**

- 数据作为 **stream of events****（事件流）** 输入到 **Unified log****（统一日志）**
- Unified log 是分布式的并具备容错性
- 事件是有序的（ordered events）
- 所有输入事件被处理并存储为 **real-time views****（实时视图）**
- 可从 Master Data **重放所有事件（****replay all events****）**
- 可以利用并行性提升计算效率

**优势**

- 架构更简单
- 无需重复处理相同数据
- 逻辑维护成本更低
   

**Data Analytics****（数据分析）**  
**定义****￼**

![Exported image](Exported%20image%2020260730191008-14.png)

- 分析由 **Batch Processing****（批处理）** 或 **Stream Processing****（流处理）** 得到的数据
- 提取 **Business Intelligence****（商业智能）**
- 获取 **insight****（洞察）**
- 根据数据进行 **反应（****react to data****）**
- 在某些情况下可接近实时
    - e.g., Fraud detection（欺诈检测）、network attacks（网络攻击）
- 训练 **Machine Learning (ML) models****（机器学习模型）**
 
**Data Analytics – Machine Learning (ML)**  
**用途**

- 存储和处理过的数据可用于训练 ML 算法
- 训练 ML 算法需要大量高质量数据
- 训练好的 ML 算法可对 **新到达的数据（****previously unseen data****）** 进行预测
- 帮助及时决策和事件响应

**应用示例**

- Fraud detection（欺诈检测）
- Detecting network attacks（检测网络攻击）
- Forecasting retail stock level（预测零售库存）
 
**ML Methods****（机器学习方法）****￼**

![Exported image](Exported%20image%2020260730191009-15.png)

- **Supervised ML****（监督学习）**
    - 使用已处理数据训练神经网络
- **Unsupervised ML****（无监督学习）**
    - 发现数据中的隐藏模式和组织结构
- **Decision Trees****（决策树）**
- **Statistical regression analysis****（统计回归分析）**
- **Etc.****（其他方法）**

**ML Frameworks****（框架）**

- **Neural Network (NN)** 常用框架：
    - TensorFlow
    - PyTorch
- **Data stream processing frameworks****（数据流处理框架）** 也提供 ML 能力
    - e.g., Spark MLlib
- 这些框架通常要求数据符合特定格式
 
**Data Analytics – Reporting****（报告生成）**  
**目的****￼**

![Exported image](Exported%20image%2020260730191010-16.png)

- 提取商业智能
- 识别数据模式（recognize patterns）
- 进行预测（make forecasts）
- 支持快速决策

**报告生成方式**

- 人工生成报告
    - 耗时且成本高
- 使用专业分析软件
    - e.g., SPSS
- 通过脚本自动生成报告
    - 快速、低成本
- 半自动化报告
    - e.g., 基于 Jupyter Notebook 的报告
 
**Summary****（总结）**

- **Data Processing Architecture****（数据处理架构）** 描述了从数据生成到分析的整个数据管道
- 在数据管道的不同阶段，数据需要被存储、管理或丢弃
- 可使用多种 **Data Management Systems****（数据管理系统）**
- **Analytics****（分析）** 用于：
    - 获取洞察（insight）
    - 提取商业智能（business intelligence）
    - 执行及时响应（reaction）
    - 做未来预测（predictions）
- 分析可能包括训练 ML 算法
- 在设计特定的数据处理架构时，以上所有环节都需被考虑

好的，我帮你把 References 部分也整理成课堂笔记风格，保持中英双文，同时统一排版，便于你最后整理到文档里：
 
**References****（参考资料）**

- **Big Data Architectures****￼**[https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/](https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/)
- **Real Time vs Batch Processing vs Stream Processing****￼**[https://www.bmc.com/blogs/batch-processing-stream-processing-real-time/](https://www.bmc.com/blogs/batch-processing-stream-processing-real-time/)
- **Block Storage****￼**[https://www.ibm.com/cloud/learn/block-storage](https://www.ibm.com/cloud/learn/block-storage)
- **What is a Data Warehouse****￼**[https://www.oracle.com/uk/database/what-is-a-data-warehouse/](https://www.oracle.com/uk/database/what-is-a-data-warehouse/)
- **What is a Data Lake****￼**[https://aws.amazon.com/big-data/datalakes-and-analytics/what-is-a-data-lake/](https://aws.amazon.com/big-data/datalakes-and-analytics/what-is-a-data-lake/)
- **A brief introduction to two data processing architectures – Lambda and Kappa for Big Data**
- **Lambda Architecture for Batch and Stream Processing****￼**[https://d1.awsstatic.com/whitepapers/lambda-architecure-on-for-batch-aws.pdf](https://d1.awsstatic.com/whitepapers/lambda-architecure-on-for-batch-aws.pdf)
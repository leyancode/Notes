下面为你整理成课堂笔记格式，使用清晰的小标题、条列式结构，并保留核心英文术语。语言为中文为主、关键概念中英双文呈现。无特殊符号。
 
NoSQL Databases  
A High-Level Overview  
Dr Charaka Palansuriya  
EPCC, University of Edinburgh
 
一、What is a NoSQL database?￼

![Exported image](Exported%20image%2020260730191346-0.png)  
![Exported image](Exported%20image%2020260730191347-1.png)  

1. 定义
2. ==NoSQL = Not Only SQL==
3. 用于指代非关系型数据库（non-relational databases）
4. 数据类型
5. JSON-like document databases
6. Graph databases
7. 其他非关系型数据存储形式
8. 设计目标
9. 分布式存储（distributed storage）
10. 高水平横向扩展性（high horizontal scalability）
11. 适用数据类型
12. 结构化（structured）
13. 半结构化（semi-structured）
14. 非结构化（unstructured）
15. 应用优势
16. 对结构化数据也适用，因为应用可以直接存储对象，无需 ORM（Object Relational Mapping）
 
二、NoSQL Databases 的一般特性￼

![Exported image](Exported%20image%2020260730191349-2.png)

1. Schema 可选
2. 多数 NoSQL 无需 schema
3. 同一集合可存不同结构的记录
4. 数据验证由应用层完成
5. 无 Join 操作
6. 多数不支持 SQL-style join
7. 某些模型天然可以连接数据，例如 Graph databases
8. 无传统 ACID 事务
9. 多数 NoSQL 不支持完整 ACID
10. 某些支持简化事务
    - 例如 Cassandra 支持 Atomicity, Isolation, Durability（不保证 Consistency）
 
==三、====NoSQL: Data Models== ==数据模型==  
==主要四类：==

1. ==Key-Value==
2. ==Document==
3. ==Column==
4. ==Graph==
 
四、Key-Value Data Model￼

![Exported image](Exported%20image%2020260730191350-3.png)

1. 定义
2. 数据以 Key-Value 方式存储
3. 本质类似一个持久化的 Hashtable
4. Key
5. 通常是数字或字符串
6. 用来快速检索对应 value
7. Value
8. 可以是任意类型：
    - 数字、字符串
    - 大型文档
    - 图片、视频
9. 示例
10. Amazon Dynamo（现代版本：Amazon DynamoDB）
11. 提供低延迟（single-digit millisecond latency）
 
五、Document Data Model￼

![Exported image](Exported%20image%2020260730191351-4.png)

1. 数据存储方式
2. 按 document（文档）组织
3. 文档集合称为 collection
4. 每个 document 有唯一 ID
5. 无 schema 强制结构 → 高灵活性
6. 访问方式
7. 通常一次检索返回完整文档
8. 可执行基于内容的搜索
9. 格式
10. 常用 JSON（key-value 形式，可嵌套）
11. XML 也可用，但较少采用￼
12. ![Exported image](Exported%20image%2020260730191453-5.png)
13. 示例（MongoDB 文档）

{￼ _Id: "53f72bd87cf6efbdac437613",￼ name: "John Smith",￼ contact: {￼ phone: "1234-4567",￼ email: "me@abc.com"￼ },￼ department: "Finance",￼ employeeID: 987￼}

1. 常见数据库
2. MongoDB
3. CouchDB
 
六、Column Data Model（列式数据模型）￼

![Exported image](Exported%20image%2020260730191457-6.png)

1. 主要思想
2. Columns 被独立存储
3. 列值在物理上连续存放（column files）
4. 可视为按列组织数据而非按行组织
5. 优势
6. 列式存储能更快处理聚合类查询（SUM、AVG、COUNT 等）
7. 可将行视为多个 column key-value pairs 的组合
8. 典型系统
9. Cassandra
10. HBase
11. 例子示意（课堂图形解释）￼
12. ![Exported image](Exported%20image%2020260730191558-7.png)
13. ￼Column Family（类似一张表）由多个行构成
14. partition key 用于分区，例如：101、103、104
15. 每行可具有不同的列集合（schema-less per row）￼例如：
16. 101 行：email, name, tel
17. 103 行：email, name, tel, tel2
18. 104 行：仅包含 name
 
七、Summary

- NoSQL 涉及一系列非关系型数据库技术
- 适合大规模、分布式、灵活结构的数据存储
- 四大数据模型：Key-Value、Document、Column、Graph
- 大多数无固定 schema、无 join、事务弱化
- 设计目标为高扩展性与高可用性
   

一、Graph Data Model 图数据模型￼

![Exported image](Exported%20image%2020260730191600-8.png)

1. 基本概念
2. Nodes 表示实体（entities）
    - 可包含属性，如 ID、Name、Age
3. Edges 表示关系（relationships）
    - 也可包含属性，如 ID、Knows、Member
4. 每个 node 和 edge 都有唯一 ID
5. Node 知道其邻接节点（adjacent nodes）
6. 模型特点
7. 以图结构存储数据，天然支持关联关系
8. 适合描述社交网络、知识图谱、推荐系统等场景
9. 高效执行 graph traversal，如 shortest path、friend-of-a-friend 等查询
10. 示例数据库
11. Neo4j
12. TigerGraph
13. 简例关系图（课堂示意）
14. Staff(Name: John, Age: 30)
    - TEACHES → Course(Name: Mathematics)
15. Student(Name: Jane, Age: 19)
    - ATTENDS → Course(Name: Mathematics)
 
二、Summary 总结

- NoSQL 包含多种类型的数据库：Key-Value、Document、Column、Graph
- 适合可扩展的半结构化或非结构化数据
- 对结构化数据也适用，应用可直接存储对象避免 ORM（Object Relational Mapping）
- 多语言编程支持，例如 Python、Java 等
- 强调扩展性、分布式和灵活的数据存储模型
 
三、MongoDB Database

1. 基本介绍￼
2. ![Exported image](Exported%20image%2020260730191601-9.png)
3. 开源 JSON-like document database
4. 文档以 BSON（Binary JSON）格式存储
    - 支持比 JSON 更多的类型，如 Date
5. 提供官方驱动支持多种语言：C、C++、Java、Python、PHP、Perl、Ruby、C#
6. 由 C++ 编写，性能较高
7. 使用 TCP sockets 进行高效 client-server 通信（非 REST）
8. 具备高可用性，可配置多种数据复制策略（replication）
 
四、MongoDB Document Model 文档模型￼

![Exported image](Exported%20image%2020260730191603-10.png)

1. 文档结构
2. 文档由 key-value pairs 组成
3. key 为字符串类型
4. _id 字段为 primary key，如未提供 MongoDB 会自动生成
5. field name 不可包含 dot(.) 或 null character
6. Size 限制
7. 单个 BSON 文档最大为 16MB
    - 可存储字符串、嵌套文档、较小的二进制对象
8. 对于大于 16MB 的文件，使用 GridFS
    - 将文件分块（chunks），每块作为独立文档存储
 
五、MongoDB: Data Modelling 数据建模￼

![Exported image](Exported%20image%2020260730191604-11.png)

1. Normalised Model（规范化模型）￼Pros
2. 无数据重复￼Cons
3. 更新复杂：
    1. 首先获取 references
    2. 再根据 references 获取相关文档
    3. 最后再更新这些文档
4. De-normalised Model（非规范化模型）

![Exported image](Exported%20image%2020260730191605-12.png)

Pros

- 单个文档包含所需信息
- 更新只需一次请求即可完成￼Cons
- 存在数据重复
- 由应用负责管理数据一致性与重复问题
 
六、MongoDB: Mongo Shell￼

![Exported image](Exported%20image%2020260730191608-13.png)

1. 定义
2. MongoDB 官方命令行工具
3. 提供交互式 JavaScript 环境
4. 支持 CRUD 四类基本操作：
    - Create（插入）
    - Read（查询）
    - Update（更新）
    - Delete（删除）
 
七、Mongo Shell 示例

![Exported image](Exported%20image%2020260730191609-14.png)

1. Create 插入文档

db.staff.insert({￼ name: "John Smith",￼ email: "me@my.com",￼ department: "Finance"￼})

1. Read 查询文档

db.staff.find({ name: "John Smith" })
   

一、MongoDB: Programming Support 编程支持￼

![Exported image](Exported%20image%2020260730191610-15.png)

1. 多语言支持
2. Python 是 MongoDB 应用开发中最常用语言之一
3. 官方推荐 Python 访问方式：Pymongo
    - 提供 API 与工具与 MongoDB 交互
4. Java 也提供官方 driver
    - 文档： [http://docs.mongodb.org/ecosystem/drivers/java/](http://docs.mongodb.org/ecosystem/drivers/java/)
5. 其他语言也有驱动：C、C++、C#、Node.js、Perl 等
6. 完整列表可参考： [http://docs.mongodb.org/ecosystem/drivers/](http://docs.mongodb.org/ecosystem/drivers/)
7. 特点
8. MongoDB 的驱动通常提供 CRUD、索引管理、连接池、会话处理等能力
9. 驱动是 MongoDB 融入软件系统的核心组件
 
二、MongoDB: A Python Example（示例）￼

![Exported image](Exported%20image%2020260730191611-16.png)

示例代码（使用 Pymongo 连接数据库、选择 collection、查询文档）：  
import pymongo￼from pymongo import MongoClient  
client = MongoClient('localhost', 27017) # 本地 MongoDB，默认端口￼db = client.testdb # 选择数据库￼customers = db.customers # 选择 collection  
# 工具函数：打印整个 customers collection￼def CUSTOMERS():￼ for customer in customers.find():￼ print(customer)  
CUSTOMERS() # 打印所有文档  
要点总结：

- MongoClient 用于建立连接
- 通过 db. 选择文档集合
- find() 用于检索所有文档
 
三、MongoDB: Indexing 索引

![Exported image](Exported%20image%2020260730191713-17.png)

1. 作用
2. 功能与传统关系数据库（RDBMS）索引类似
3. 避免对 collection 中所有文档进行全表扫描（collection scan）
4. 默认索引
5. 文档的 _id 字段默认已建立唯一索引
6. 索引范围
7. 可对字段（field）或子字段（sub-field）建立索引
8. 示例：employees collection 中字段 salary 建立索引后，可更快执行查询：

db.employees.find({ salary: { "lt": 20000 } })

1. 性能影响
2. 索引能显著提升常用查询性能
3. 建议为最常使用的查询模式建立索引
4. 注意：过多索引会增加写入成本（因为写入时需维护索引）
 
四、MongoDB: Data Backup and Restore 数据备份与恢复￼

![Exported image](Exported%20image%2020260730191714-18.png)

1. 备份工具选项￼MongoDB 提供多种备份方案，取决于数据库大小与需求：
2. Mongodump / Mongorestore
3. Mongodump：将数据库导出为 BSON 文件
4. Mongorestore：从 BSON 文件重建数据库
5. 适合中小型数据库
6. MongoDB Management Service (MMS)
7. 云端备份服务
8. 提供持续备份（continuous backup）
9. 可使用 snapshot 进行恢复
10. 链接：[https://mms.mongodb.com](https://mms.mongodb.com/)
11. OS-level 文件备份
12. 直接复制 MongoDB 的底层数据文件
13. 适用于停机备份场景
 
五、MongoDB: Summary 小结￼

![Exported image](Exported%20image%2020260730191715-19.png)

1. 特性概览
2. MongoDB 是高性能 NoSQL 数据库
3. 最流行的 NoSQL 之一
4. 采用 Document Data Model（文档模型）
    - JSON-like 文档，内部由 key-value pairs 组成
5. 适合 structured、semi-structured、unstructured 数据
6. 与应用集成优势
7. De-normalized model（非规范化模型）更贴近应用程序数据结构
8. 不需要 ORM，不需将应用对象拆分成多表结构
9. 编程语言支持
10. 几乎所有主流语言都有官方驱动
11. Python 常用 Pymongo driver
12. 性能优化
13. 利用 indexing 提升常见查询性能
14. 设计查询模式时需关注数据访问路径
 
如果你希望，我可以继续帮你整理下一章、制作整份 NoSQL + MongoDB 的完整复习笔记，或者将所有内容压缩成一页 cheat sheet。
 \> 来自 \<[https://chatgpt.com/c/6932ca11-7a88-8326-8ab4-55329269a886](https://chatgpt.com/c/6932ca11-7a88-8326-8ab4-55329269a886)\>

![[Lec11 NoSQL Databases - Ink.svg]]

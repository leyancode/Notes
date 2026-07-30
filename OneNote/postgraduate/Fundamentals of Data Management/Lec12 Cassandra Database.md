下面为你继续按照课堂笔记格式进行整理，保持清晰结构、中英双文、重点术语保留英文，适合复习或统一纳入你的笔记文档。
 
Cassandra Database  
Dr Charaka Palansuriya  
EPCC, The University of Edinburgh
 
一、Introduction to Apache Cassandra

1. 基本介绍
2. Apache Cassandra 是一种开源 column-oriented NoSQL database
3. 与传统关系型数据库（Relational Databases）有相似之处，但也存在显著差异
4. 设计目标是处理高速度、高体量的数据，并具备高扩展性与容错能力￼
5. ![Exported image](Exported%20image%2020260730191721-0.png)
 
二、Cassandra vs Relational Databases 对比

|   |   |   |
|---|---|---|
|**特点**|**Cassandra**|**Relational Databases**|
|数据类型|Structured & Unstructured|Structured|
|Schema|Flexible schema|Fixed schema|
|数据量与速度|适用于 high velocity / high volume|Low to medium velocity/volume|
|可扩展性|高水平横向扩展（commodity hardware）|依赖高性能专用硬件进行扩展|
|架构结构|Peer-to-peer，无单点故障|Master-slave，有单点故障|
|适用部署|去中心化部署|中心化部署|

总结：Cassandra 更适用于大规模分布式系统，而关系型数据库更强于结构化数据管理。
 
三、Cassandra Architecture（架构）

1. Peer-to-peer Cluster
2. 所有节点（nodes）拥有相同角色
3. 数据分布在整个集群中（distributed across nodes）
4. 任何节点都可处理 read/write 请求，即使目标数据位于其他节点
5. 容错性（Fault Tolerance）
6. 若某个节点失效，其他节点仍可提供服务
7. 集群整体具备高可用性（high availability）
8. 主要设计原则
9. Multi-master replication
10. Low-latency global availability
11. Horizontal scalability
12. Partitioned key-oriented queries
13. Flexible schema design
 
四、Cassandra: Key Concepts 核心概念

1. Keyspace
2. 类似数据库（database）
3. 定义 tables 及其数据的整体架构（schema）
4. Table
5. 由 rows 与 columns 组成
6. 分布式按 partitions 组织
7. Partition
8. 多行（rows）按 partition key 分组
9. 是 Cassandra 数据访问的基本单位
10. Row
11. 表中一条结构化数据记录
 
五、Cassandra Query Language (CQL)

1. CQL 特点
2. 类似 SQL 的声明式语言
3. 操作对象为 Tables
4. 用于创建 keyspaces、tables 以及执行 CRUD 操作
5. 工具
6. Cassandra 提供 CQL Shell（cqlsh）供命令行执行 CQL
7. 用法与 SQL 类似，但无 join、多表事务等操作
 
六、Cassandra CRUD Example 示例

1. 创建 Keyspace

CREATE KEYSPACE mycompany￼WITH replication = { 'class': 'SimpleStrategy', 'replication_factor': 1 };￼

1. 切换 Keyspace

USE mycompany;￼

1. 创建 Employee 表

CREATE TABLE Employee (￼ employee_id UUID PRIMARY KEY,￼ name TEXT,￼ address TEXT,￼ email TEXT,￼ telephone TEXT,￼ department_id UUID￼);￼  
说明：

- PRIMARY KEY 为 employee_id
- employee_id 也是 partition key
- 单列 primary key → 单分区结构
 
七、Insert 示例（展示 flexible schema）  
INSERT INTO Employee (employee_id, name, address, email, telephone, department_id)￼VALUES (uuid(), 'John Smith', '123 Main St, Cityville', 'john.smith@example.com', '555-1234', uuid());￼  
INSERT INTO Employee (employee_id, name, address, telephone, department_id)￼VALUES (uuid(), 'Alan Doe', '456 Elm St, Townsville', '555-5678', uuid());￼  
INSERT INTO Employee (employee_id, name, address, email, telephone, department_id)￼VALUES (uuid(), 'Michael Johnson', '789 Oak St, Villageton', 'michael.johnson@example.com', '555-9012', uuid());￼  
注意：

- 第二条 INSERT 省略了 email 字段
- 展现 Cassandra flexible schema 的特点
 
八、Read / Update / Delete 示例

1. Read 查询所有记录

SELECT * FROM Employee;￼

1. Update

UPDATE Employee￼SET name = 'Alan Erwine'￼WHERE employee_id = \<Alan Doe’s employee_id\>;￼

1. Delete

DELETE FROM Employee￼WHERE employee_id = \<John Smith’s employee_id\>;￼  
CRUD 操作都需基于 partition key 进行查询，这是 Cassandra 的关键约束之一。
 
下面继续为你整理成课堂笔记，延续你前面笔记的格式，中英双文呈现、结构清晰、保留关键英文术语。
 
一、Cassandra Partition Keys（分区键）

1. 多列 Primary Key 的结构￼若 Primary Key 由多个 column 组成，其中部分列会作为 Partition Key，其余列则作为 Clustering Key。

示例：  
CREATE TABLE Employee (￼ department_id UUID,￼ employee_id UUID,￼ name TEXT,￼ address TEXT,￼ email TEXT,￼ telephone TEXT,￼ PRIMARY KEY ((department_id, employee_id))￼);￼

1. 解释
2. Partition Key：department_id
3. Clustering Key：employee_id
4. Primary Key：由 (department_id, employee_id) 的组合构成
5. Partition Key 决定数据落在哪个 partition
6. Clustering Key 决定 partition 内的排序方式

该设计适合按 department 分区，同时支持按 employee_id 进行排序或高效查找。
 
二、Cassandra Installation（安装方式）

1. 最快上手方式：Docker
2. 官方提供 Cassandra Docker 容器
3. 适合快速尝试 CQL
4. 需要安装 Docker Desktop（Windows/MacOS/Linux）
5. 容器默认不对外开放端口
6. 安全且快速，但从程序访问较复杂（需额外配置）
7. 生产环境
8. 建议直接安装 Apache Cassandra 稳定版本
    - 官网：Cassandra.apache.org
9. 默认配置只允许 localhost 访问
    - 最安全的默认行为
 
三、Cassandra Programming Support（编程支持）

1. 语言支持
2. 驱动（drivers）适用于：Python、Java、C++ 等
3. Java
4. Cassandra 本身用 Java 实现
5. Java driver 最成熟，提供最细粒度控制（query tuning, consistency levels 等）
6. Python
7. 使用 DataStax Python Driver
8. 用法简单，适合应用开发
 
四、Cassandra Python Program 示例  
from cassandra.cluster import Cluster￼  
def query_all_employees():￼ cluster = Cluster(['localhost']) # 连接本机集群￼ session = cluster.connect('mycompany') # 连接到 keyspace￼  
query = "SELECT * FROM Employee"￼ result_set = session.execute(query)￼  
for row in result_set:￼ print("Employee ID:", row.employee_id)￼ print("Name:", row.name)￼ print("Address:", row.address)￼ print("Email:", row.email)￼ print("Telephone:", row.telephone)￼ print("Department ID:", row.department_id)￼  
session.shutdown()￼ cluster.shutdown()￼  
query_all_employees()￼  
要点总结：

- Cluster([...]) 建立集群连接
- session.connect(keyspace) 选择数据库
- execute(query) 执行 CQL
- 返回结果可迭代访问 row.
 
五、Backup and Restore（数据备份与恢复）  
Cassandra 提供两类备份方案：

1. Snapshots
2. 某一时刻的表数据副本
3. 可手动或自动创建
4. 可用来恢复到某个具体时间点的数据库状态
5. Incremental Backups
6. 默认关闭，需要显式启用
7. 首次备份是 full backup
8. 之后仅备份自上次以来的变化
9. 恢复时需要 full backup + 所有 incremental backups
10. 优点：减少存储成本和网络带宽消耗
 
六、Cassandra Summary 总结

- Cassandra 是 column-oriented 的 NoSQL 数据库
- 类似关系型数据库的 table 模型，但具备 flexible schema
- Peer-to-peer 架构，无单点故障
- 适用于高速度、高吞吐的数据流场景
- 强调分布式、可扩展性（horizontal scalability）
 
七、Neo4j Database（图数据库）

1. 基本介绍
2. 最受欢迎的原生图数据库（native graph database）
3. 使用 property graph model
    - nodes 和 relationships 都可包含 key-value 属性
4. 支持 ACID transactions
5. 提供开源版本（仅单节点）
6. 使用 Cypher 作为查询语言
7. Node 可通过 labels 标识类型：Person、Course、Department
    - labels 可用于 metadata（如 index、constraints）
8. 图示例（课程示意）
9. Staff(Name: John, Age: 30)
    - TEACHES → Course(Mathematics)
10. Student(Name: Jane, Age: 19)
    - ATTENDS → Course(Mathematics)
 
八、Neo4j vs Relational Databases

|   |   |   |
|---|---|---|
|**特点**|**Neo4j**|**Relational Databases**|
|实体表示|Nodes + Edges|Tables|
|关系表示|Edges 是一等公民|Joins 模拟关系|
|表达能力|适合深度关系（deeply linked data）|适合结构化、关系较浅的数据|
|查询方式|Pattern matching，可发现隐藏结构|需明确 joins，无法描述部分复杂关系|
|应用场景|社交网络、知识图谱、网络分析|商业数据库、ERP 系统等|

下面为你继续整理 Neo4j 相关课堂笔记，保持与前面一致的格式：结构清晰、中英双文、关键字保留英文、无特殊符号。
 
一、Neo4j: Cypher Query Language（Cypher 查询语言）

1. 基本特点
2. Cypher 是一种声明式查询语言（declarative language），类似 SQL
3. 主要优化用于 graph traversing（图遍历）
4. 可执行 CRUD（Create, Read, Update, Delete）操作
5. 使用 ASCII-art 风格语法描述图结构（例如 (a)-[:REL]-\>(b)）
6. 创建节点（Create Nodes）￼示例：创建两个 Person 节点：

CREATE (:Person {name: 'Bob', age: 19})￼CREATE (:Person {name: 'Eve', age: 20})￼

1. 创建关系（Create Relationships）￼示例：建立 FRIEND 关系：

MATCH (bob:Person {name: 'Bob'}), (eve:Person {name: 'Eve'})￼CREATE (bob)-[:FRIEND]-\>(eve)￼
 
二、Neo4j Cypher CRUD Operations

1. Create（创建节点和关系）￼示例：创建 Staff 和 Course 节点：

CREATE (:Staff {name: 'John'})￼CREATE (:Course {name: 'Mathematics'})￼  
示例：建立 TEACHES 关系：  
MATCH (staff:Staff {name: 'John'}), (course:Course {name: 'Mathematics'})￼CREATE (staff)-[:TEACHES]-\>(course)￼  
注意：

- Cypher 通过类似 ASCII-art 的方式表示图结构
- 如 (staff)-[:TEACHES]-\>(course)
- Read（读取数据）￼基于 graph pattern 进行匹配：

示例：查询所有选修数学课程的学生：  
MATCH (student:Student)-[:ATTENDS]-\>(course:Course {name: 'Mathematics'})￼RETURN student￼

1. Update（更新数据）￼更新学生的选课记录：

MATCH (student:Student {name: 'Eve'})-[oldRel:ATTENDS]-\>(oldCourse:Course)￼MATCH (newCourse:Course {name: 'Mathematics'})￼DELETE oldRel￼CREATE (student)-[:ATTENDS]-\>(newCourse)￼

1. Delete（删除）
2. 删除节点或关系需先匹配，再 delete
3. 注意：删除节点前通常需要先删除关系
 
三、Neo4j Desktop

1. 简介
2. Neo4j Desktop 是在本机运行 Neo4j 的最简单方式
3. 提供单节点版本 Neo4j 数据库
4. 跨平台支持 Windows、MacOS、Linux
5. 功能
6. 图形化管理控制台
7. 可创建数据库、管理配置
8. 内置 Neo4j Browser（图形化 Cypher 查询工具）
    - 通常在左侧 Graph Apps 面板
    - 可连接数据库并直接执行 Cypher 查询
 
四、Neo4j Browser（示意）

- 以可视化方式展示节点、关系
- 支持交互式查询
- 可查看节点属性、关系结构
- 非常适合学习 Cypher 与调试图模型
 
五、Neo4j Programming Support（编程支持）  
Neo4j 提供多种语言的官方驱动，包括：

- Python
- Java
- JavaScript
- .NET

Python 是常用的入门选择。
 
六、Neo4j Python Program 示例  
from neo4j import GraphDatabase￼  
uri = "bolt://localhost:7687"￼username = "my_username"￼password = "my_password"￼database = "mygraphdb"￼  
driver = GraphDatabase.driver(uri, auth=(username, password), database=database)￼  
query = "MATCH (student:Student) RETURN student"￼  
with driver.session() as session:￼ result = session.run(query)￼ for record in result:￼ student = record["student"]￼ print(student["name"])￼  
driver.close()￼  
要点：

- 使用 bolt 协议连接 Neo4j（默认端口 7687）
- session.run(query) 执行 Cypher
- record["student"]["name"] 获取属性值
 
七、Neo4j Backup and Restore（备份与恢复）  
Neo4j 提供命令行工具 neo4j-admin，可执行以下备份模式：

1. Full Backup（完全备份）
2. 初次备份必须完整备份
3. 适用于 offline dump 或 online backup
4. Backup Modes
5. online：数据库运行中执行备份
6. offline：使用 dump 创建静态备份文件
7. Incremental Backups
8. online 模式支持增量备份
9. 仅保存自上次备份之后的变化
10. 恢复需要：full backup + 所有 incremental backups
11. Restore
12. 使用 neo4j-admin restore 命令
13. 可恢复到相同或更高版本的 Neo4j 实例
 
八、Summary 总结

- Neo4j 是目前最流行的图数据库
- 使用 property graph model
- 原生图存储与图计算，节点和关系均包含属性
- 使用 Cypher 进行 CRUD 操作
- ASCII-art 风格语法自然表达图关系
- 擅长发现隐藏 patterns，特别适用于欺诈检测、社交网络分析等
- Panama Papers 案例是 Neo4j 图分析的典型示例
 
如果你需要，我可以把整个 NoSQL（MongoDB、Cassandra、Neo4j）内容进一步合并整理为：

1. 一份标准课堂笔记（多页）
2. 一页考试复习 cheat sheet
3. 一份中文或中英双语的总结比较表

告诉我你的偏好即可。
 \> 来自 \<[https://chatgpt.com/c/6932ca11-7a88-8326-8ab4-55329269a886](https://chatgpt.com/c/6932ca11-7a88-8326-8ab4-55329269a886)\>
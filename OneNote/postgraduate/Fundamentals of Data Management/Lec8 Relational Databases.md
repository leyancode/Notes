下面为你继续整理本节 **Relational Databases****（关系型数据库）** 的课堂内容，采用和之前一致的 **中英双语、结构化、关键英文术语保留、不添加特殊符号** 的笔记风格。你可直接将其拼入你的课程笔记体系。
 
**19 Relational Databases** **关系型数据库**  
Fundamentals of Data Management  
Dr Charaka Palansuriya
 
**19.1 Outline** **本讲内容****￼**
 
本节课程涵盖以下主题：

- What is a database 数据库是什么
- Keys and constraints 键与约束
- Design considerations 设计考虑
- Querying a relational database 查询关系型数据库
- Accessing relational databases via applications 应用程序访问数据库
- Transactions 事务
- Performance optimisation 性能优化
- Backup and restore 备份与恢复
 
**20 What is a Database?** **什么是数据库****￼**

![Exported image](Exported%20image%2020260730191237-0.png)

数据库是一种用于 **高效存储与访问数据** 的系统。  
**20.1 DBMS (Database Management System)** **提供的功能**  
DBMS 通常提供以下机制：

- Creating data structures 创建数据结构（如 Tables）
- Adding data 插入数据（包括 bulk loading）
- Querying and modifying data 查询与修改数据
- Performance optimisation 性能优化
- Backup and restore 数据备份与恢复
- Application access 支持应用访问（APIs, connectors）
- Authentication and Authorization 用户身份认证与权限管理

总结：DBMS 是数据库的核心软件层，为结构化数据提供统一存储与管理。
 
**21 Types of Databases** **数据库类型**

![Exported image](Exported%20image%2020260730191238-1.png)

本课程讨论：

- Relational Databases 关系型数据库
- NoSQL Databases 非关系型数据库
- RDF Databases RDF 图数据库

你还应了解：

- XML Databases（过去流行）
- Hybrid Databases（如 PostgreSQL：object-relational 支持对象关系模型）

本讲专注于：

- Relational Databases（目前仍最常见，如 Oracle、SQL Server、MySQL）
 
**22 What is a Relational Database?** **什么是关系型数据库****￼**

![Exported image](Exported%20image%2020260730191239-2.png)

关系数据库由以下组成：

- A collection of tables（又称 Relations）
- Relationships between tables（关联关系）

示例表（Employee 和 Department）：  
Employee  
Name | Department  
Andrew | Computers  
Bob | Finance  
Carol | Computers  
David | Management  
Department  
Department | HeadOfDept | Location  
Computers | Carol | Edinburgh  
Finance | Mike | Aberdeen  
Management | Ethel | Cardiff  
关系模型依赖：

- Primary Keys
- Foreign Keys
- Referential Integrity
 
**23 Primary Key, Foreign Key and Indexes** **主键、外键与索引****￼**

![Exported image](Exported%20image%2020260730191240-3.png)  
![Exported image](Exported%20image%2020260730191241-4.png)  
![Exported image](Exported%20image%2020260730191242-5.png)

**￼****23.1 Primary Key** **主键**

- Primary Key 是表中保证 **唯一标识** 每条记录的字段
- 可由系统自动生成（auto-increment, sequences）
- 必须唯一且不可重复

示例（Department 表）：

- Department 字段作为 Primary Key

**23.2 Foreign Key** **外键**

- Foreign Key 是引用另一个表的 Primary Key 的字段
- 保证两个表之间的关系一致性
- 用于建立关联关系（Employee.Department → Department.Department）

Employee 表中：

- Department 字段是 Foreign Key 指向 Department 表的主键

**23.3 Indexes** **索引**

- Index 是对表进行加速访问的数据结构
- 提高查询性能
- 常用于频繁搜索或排序的字段
- 索引存在额外存储开销
 
**24 Constraints** **约束****￼**

![Exported image](Exported%20image%2020260730191246-6.png)

约束保证数据库中的数据有效、可靠、一致。  
**24.1 Domain Constraints** **域约束**

- 字段的值必须来自预定义集合或范围￼例如：年龄必须在 0–120 之间

**24.2 Uniqueness Constraints** **唯一性约束**

- Key 字段不能重复￼例如 EmployeeID 必须唯一

**24.3 Null Constraint** **空值约束**

- 指定字段是否允许为空（NULL）￼例如 Name 不可为空，MiddleName 可为空

**24.4 Semantic Constraints** **语义约束**

- 根据业务规则进行限制￼例如：员工工资必须小于上司工资

**24.5 Dynamic Constraints** **动态约束**

- 限制数据如何随时间变化￼例如：工资只能增加不能减少

**24.6 Referential Integrity** **参照完整性****￼**

![Exported image](Exported%20image%2020260730191247-7.png)  
![Exported image](Exported%20image%2020260730191247-8.png)

￼要点：

- 当 Foreign Key 引用另一个表时：
    - 新记录必须引用一个有效的 Primary Key
    - 若 Primary Key 被删除，则所有引用该主键的记录必须处理（通常删除或置 NULL）

示例：  
Employee.Department 必须引用 Department 表中的一个有效部门名。  
参照完整性确保数据库中不同表之间的链接永远有效。  
下面为你整理 **Design Considerations & Normalisation****（数据库设计与范式）** 的课堂内容，完全对齐你上传的图片内容，采用与之前一致的 **中英双语、结构化笔记风格**，关键英文术语保留、不加入特殊符号。
 
**25 Design Considerations** **设计考虑****￼**

![Exported image](Exported%20image%2020260730191250-9.png)

数据库设计的核心目标是：

- Minimise data redundancy 最小化数据冗余
- Ensure consistency 保持一致性
- Avoid anomalies 避免更新、删除、插入异常

示例：冗余问题（Redundancy）  
Name | Company | Address | Postcode  
A. Smith | Smith & Son Ltd | 51 High Street | PC7 4LT  
J. Smith | Smith & Son Ltd | 51 High Street | PC7 4LT  
T. Jones | Smith & Son Ltd | 51 High St | PC7 4LT  
N. Dupont | Flash Lighting Co | 14 Howe Crescent | RN4 8PU  
同一公司信息被重复存储，导致：

- 存储浪费
- 更新困难（如地址变化）
- 数据不一致风险

解决方法：Data Normalisation 数据规范化（范式）
 
**26 Data Normalisation** **范式**  
Normalization 是减少冗余并确保数据一致性的正式方法。  
主要范式：

- First Normal Form (1NF)
- Second Normal Form (2NF)
- Third Normal Form (3NF)
- Higher forms exist（BCNF, 4NF, 5NF），但很少使用￼￼

![Exported image](Exported%20image%2020260730191251-10.png)  

**26.1 First Normal Form (1NF)** **第一范式**  
**规则：**

- Each cell contains only one value（单值性）
- 不允许重复列
- ==为每一行创建一个唯一标识（====Primary Key====）==

**示例（来自幻灯片）：**  
非 1NF 的情况：  
student | course | grade | course fee  
Alison Brown | Databases, AI | A | 100  
每个单元格必须只有一个值，因此应将多值拆成多行。  
1NF 之后：  
student | course | grade | course fee  
Alison Brown | Databases | A | 100  
Alison Brown | AI | A | 120
   
![Exported image](Exported%20image%2020260730191252-11.png)

这一张表中：  
student（学生姓名）不能作为主键，因为一个学生会出现多行。  
course（课程名称）也不能作为主键，因为多个学生会修同一门课。  
因此，**主键必须是组合主键**：  
**Primary Key = (student, course)**  
两个字段共同唯一标识一条记录。
    
**26.2 Second Normal Form (2NF)** **第二范式****￼**

![Exported image](Exported%20image%2020260730191254-12.png)

**前提：必须先满足** **1NF**  
**规则：**

- Non-primary key columns must depend on the entire primary key￼非主键字段必须依赖整个主键，而不是主键的一部分
- Applies when the primary key is composite（由多个字段组成）

**示例（你上传的图片内容）：**  
未满足 2NF 的表结构（Student course fees）：  
student | course | grade | course fee  
Alison Brown | Databases | A | 100  
Jason Liu | Mathematics | B | 150  
Mariah Hill | Databases | B+ | 100  
复合主键：(student, course)  
问题：

- course fee 仅依赖于 course，而不是整个主键(student, course)
- 违反 2NF

**分解后满足** **2NF****：**  
Student course  
student | course | grade  
Alison Brown | Databases | A  
Jason Liu | Mathematics | B  
Mariah Hill | Databases | B+  
Courses  
course | course fee  
Databases | 100  
Mathematics | 150  
这样每个非主键字段依赖完整主键或自身主键。
 
**26.3 Third Normal Form (3NF)** **第三范式**  
**前提：必须满足** **1NF + 2NF**  
**规则：**

- Non-primary key columns must depend only on the primary key￼非主键字段只能依赖主键
- 不允许 transitive dependency（传递依赖）￼
- ![Exported image](Exported%20image%2020260730191257-13.png)

依赖（functional dependency）应如何判断？  
这是数据库范式的核心。依赖的判断标准是：  
**语义** **+** **业务规则（****real-world meaning****）决定函数依赖****。**
 
什么叫“语义决定依赖”？  
例如：  
customer → customer_email  
为什么？  
因为现实中一个 customer 就对应一个 email。  
这叫语义上的业务规则。  
这个规则不依赖你看到的数据样本，而是逻辑关系。  
同样：  
order_id → date  
因为一个订单号对应一个唯一的日期。  
但下面 **不成立**：  
date → order_id  
即使表中看起来每天只有一个订单，也不能推断依赖。  
这不是语义规则，只是数据样本碰巧这样。
 
四、判断依赖的黄金法则  
要问自己一句话：  
如果我知道属性 A 的值，能否保证在业务上确定属性 B 的值？  
能 → A → B  
不能 → 不存在依赖  
例子：  
知道 student 能否确定 course？不能。  
知道 (student, course) 能否确定一行记录？可以。  
例子：  
知道 customer 能否确定 customer email？可以（一个 customer 一个 email）。  
所以 customer → customer_email  
**￼****￼****￼****示例（来自图片）：**  
未满足 3NF 的表：  
order_id | date | customer | customer email  
1/2020 | 2020-01-15 | Jason White | [white@example.com](mailto:white@example.com)  
2/2020 | 2020-01-16 | Mary Smith | [msmith@mailinator.com](mailto:msmith@mailinator.com)  
...  
问题：

- customer email 依赖 customer，而 customer 又依赖 order_id
- ==这是典型的传递依赖（====order_id → customer → customer email====）==
- 违反 3NF

**分解后的** **3NF****：**  
Orders  
order_id | date | customer  
1/2020 | 2020-01-15 | Jason White  
2/2020 | 2020-01-16 | Mary Smith  
3/3030 | 2020-01-17 | Jacob Albertson  
4/2020 | 2020-01-18 | Bob Dickinson  
Customers  
customer | customer email  
Jason White | [white@example.com](mailto:white@example.com)  
Mary Smith | [msmith@mailinator.com](mailto:msmith@mailinator.com)  
Jacob Albertson | [jasobal@example.com](mailto:jasobal@example.com)  
Bob Dickinson | [bob@fakemail.com](mailto:bob@fakemail.com)
 
**27 Referential Integrity** **参照完整性**  
你上传的图示说明以下内容：  
Department（主键表）  
Department | HeadOfDept | Location  
Computers | Carol | Edinburgh  
Finance | Mike | Aberdeen  
Management | Ethel | Cardiff  
Employee（外键表）  
Name | Department  
Andrew | Computers  
Bob | Finance  
Carol | Computers  
David | Management  
Stuart | Transport ← 问题：Transport 不存在于 Department 表  
**规则：**

- Foreign Key 必须引用 Primary Key 表中的有效值
- 若引用不存在的值，数据不得插入
- ==删除== ==Primary Key== ==时，需要确保处理所有相关外键记录==

在示例中，Stuart 的部门 Transport 无效，因此违反参照完整性。  
下面为你继续整理下一节内容 **Entity-Relationship Diagrams, SQL Querying**，保持之前一致的中文/中英双语笔记格式，关键英文术语保留，不加入特殊符号，可直接插入你的课堂笔记。
 
**28 Design Considerations: Entity-Relationship (ER) Diagrams****￼**

![Exported image](Exported%20image%2020260730191258-14.png)

实体-关系图  
ER 图用于描述现实世界中的数据结构，由：

- Entities（实体，对应数据库中的 tables）
- Relationships（实体之间的关系）
- Cardinality（基数：一对一、一对多、多对多）

**28.1 Types of Relationships** **关系类型**  
**Binary Relationship** **二元关系**  
两个实体之间的关系  
示例：  
Manager —— manages —— Employee  
**Tertiary Relationship** **三元关系**  
三个实体之间的关系  
示例：  
Sales Assistant —— sells —— Product —— to —— Customer  
（注：实际建模中通常会将三元关系拆成多个二元关系）
 
**29 Cardinality** **基数****￼**

![Exported image](Exported%20image%2020260730191259-15.png)

基数描述一个实体与另一个实体的数量关系：  
Type | Meaning  
1 1 | One-to-one  
1 m | One-to-many  
m 1 | Many-to-one  
m n | Many-to-many  
示例：

- Person has Address → 1 m
- Employee belongs to Department → m 1
- Product contains Ingredients → m n
- Organisation 1 1 Organization（例如唯一法人实体）
 
**30 How ER Diagrams Are Useful ER** **图的用途****￼**

![Exported image](Exported%20image%2020260730191300-16.png)

ER 图有助于：

- Producing a data model representing a real-world situation￼建立现实世界的数据模型
- Identifying important entities (tables)￼确定系统中的关键实体（即未来的数据库表）
- Identifying relationships between entities￼指明实体间的联系
- Simplifying the data model￼可以用于简化模型结构

**Removing Many-to-Many Relationships**  
多对多关系不能直接在关系数据库中实现，必须拆分为一个中间表￼

![Exported image](Exported%20image%2020260730191301-17.png)

（junction table）  
示例：  
Borrower —— loans —— Book  
m n  
拆分为：  
Borrower —— BorrowerBook —— Book  
1 m and m 1
 
**31 Querying a Relational Database**

![Exported image](Exported%20image%2020260730191302-18.png)

使用关系数据库查询  
关系数据库使用 SQL（Structured Query Language）进行查询。  
特点：

- 所有主流 RDBMS 支持 SQL
- 标准存在（如 SQL:2003），但各自有方言（MySQL, PostgreSQL, SQL Server 等）
- SQL 查询返回的结果是一个虚拟表（virtual table）
 
**32 SQL SELECT** **查询语句****￼**

![Exported image](Exported%20image%2020260730191303-19.png)

**基本查询** **Example**  
SELECT Name, Department￼FROM Employee￼WHERE Department = 'Computers';  
**打印所有字段：使用** *****  
SELECT *￼FROM Employee￼WHERE Department = 'Computers';  
结果示例：  
Name | Department  
Andrew | Computers  
Carol | Computers
 
**33 Joins** **连接查询**  
Join 用于从多个表中提取数据。￼

![Exported image](Exported%20image%2020260730191306-20.png)

**Example**  
SELECT￼ Name,￼ Employee.Department AS Dept,￼ Department.Location AS Location￼FROM Employee, Department￼WHERE Employee.Department = Department.Department;  
结果：  
Name | Dept | Location  
Andrew | Computers | Edinburgh  
Bob | Finance | Aberdeen  
Carol | Computers | Edinburgh  
David | Management | Cardiff  
（注意：现代 SQL 更推荐使用 JOIN 语法，但课程示例使用旧式语法）
 
**34 Boolean Logic, Comparisons** **布尔逻辑与比较运算****￼**

![Exported image](Exported%20image%2020260730191308-21.png)

**34.1 AND, OR**  
SELECT name, department￼FROM Employee￼WHERE name = 'Andrew' OR name = 'Bob';  
括号可用于逻辑分组。  
**34.2 Numerical Comparisons**  
支持 \<, \>, =, \>=, \<=, \<\>（或 !=）  
**34.3 NOT**  
SELECT student￼FROM class￼WHERE NOT(mark1 \> 50);  
等价于：  
SELECT student￼FROM class￼WHERE mark1 \<= 50;
 
**35 Arithmetic Operations** **数值运算****￼**

![Exported image](Exported%20image%2020260730191309-22.png)

SQL 可对数字字段执行常见运算：+，-，*，/  
**BETWEEN** **示例**  
SELECT name, salary￼FROM employee￼WHERE salary BETWEEN 10000 AND 20000;  
等价于：  
salary \>= 10000 AND salary \<= 20000;  
注意：BETWEEN 包含边界。
 
**36 Pattern Matching** **模式匹配****￼**

![Exported image](Exported%20image%2020260730191310-23.png)

SQL 支持简单模式匹配，通过 LIKE 使用两个通配符：  
Wildcard | Meaning  
% | 零个或多个字符  
_ | 精确匹配一个字符  
**Example**  
SELECT name￼FROM addressbook￼WHERE name LIKE 'Car%';  
匹配：Caroline, Carl, Carol, cAroLinE  
SELECT name￼FROM addressbook￼WHERE name LIKE 'A_a_';  
匹配：Adam, Alan  
不匹配：Armstrong（因为长度不符）  
下面继续为你整理 **Aggregation, Ordering, Application Access, Performance, Transactions, ACID, Backup** 等内容，保持与前文一致的 **中英双语课堂笔记风格**，关键英文术语保留，不添加特殊符号。
 
**37 Aggregation Functions** **聚合函数****￼**

![Exported image](Exported%20image%2020260730191311-24.png)

SQL 提供一组用于对数值数据进行汇总计算的函数：  
Functions | Meaning  
SUM | 求和  
AVG | 平均值  
COUNT | 计数  
MIN | 最小值  
MAX | 最大值  
**示例** **1****：统计所有客户数量**  
SELECT COUNT(*) FROM Customers;  
**示例** **2****：返回** **Customers** **表中** **Age** **字段的最大值**  
SELECT MAX(Age) FROM Customers;  
不同数据库可能提供额外的聚合函数（如 STDDEV, VARIANCE 等）。
 
**38 Ordering** **排序****￼**

![Exported image](Exported%20image%2020260730191312-25.png)

SQL 可对查询结果（而非原始表）进行排序。  
**基本排序**  
SELECT name, salary￼FROM employees￼ORDER BY salary, name;  
含义：

- 先按 salary 升序排序
- 若 salary 相同，再按 name 字母顺序排序

**降序排序（****DESC****）**  
SELECT name, salary￼FROM employees￼ORDER BY salary DESC, name;  
DESC 表示降序（descending）。
 
**39 Accessing Relational Databases via Applications****￼**

![Exported image](Exported%20image%2020260730191313-26.png)

通过应用程序访问关系数据库  
虽然可使用 RDBMS 提供的命令行工具，但生产环境中通常通过应用程序访问数据库。  
**常用标准接口**

- JDBC (Java Database Connectivity)￼面向 Java 应用
- ODBC (Open Database Connectivity)￼常用于 Microsoft 应用

**RDBMS** **驱动程序**  
应用程序必须使用由数据库供应商提供的 driver，例如：

- MySQL JDBC driver
- MySQL ODBC driver
- MySQL Connector/Python

这些驱动提供 API，使程序能够通过 SQL 访问数据库。
 
**40 Performance Optimization** **性能优化****￼**

![Exported image](Exported%20image%2020260730191316-27.png)

**Indexes** **索引**  
索引的作用类似于书籍的目录，使 SQL 查询可以快速定位所需行，而不需要扫描整个表。  
特点：

- 用于高频查询字段（如主键、姓名、ID）
- 大幅提升 SELECT 查询性能
- 插入更新操作略有额外成本（需要维护索引）

索引常用于：

- Primary key
- WHERE 子句中经常出现的字段

例：在 Employee 表的 name 字段上建索引可加速搜索。
 
**41 Transactions** **事务****￼**

![Exported image](Exported%20image%2020260730191317-28.png)

事务是作为一个整体执行的一组 SQL 操作，要么全部成功，要么全部撤销。  
事务的两种状态：

- committed（提交）
- rolled back（回滚）

典型示例：银行账户转账

- 从 A 扣钱、给 B 加钱必须作为单一事务执行

用途：

- Data concurrency（并发控制）
- Data consistency（保证一致性）

关系数据库一般都提供强事务支持。
 
**42 ACID Properties ACID** **特性****￼**

![Exported image](Exported%20image%2020260730191318-29.png)

事务必须满足 ACID 四大特性：  
**Atomicity** **原子性**

- 事务中的所有操作要么全部执行，要么全部不执行

**Consistency** **一致性**

- 事务必须使数据库从一个一致状态转变到另一个一致状态

**Isolation** **隔离性**

- 并发事务不得相互影响
- 一个事务未提交前，其修改对其他事务不可见

**Durability** **持久性**

- 事务提交后，其结果必须永久保存
- 即使系统崩溃也必须保证数据不丢失
 
**43 Backup and Restore** **备份与恢复****￼**

![Exported image](Exported%20image%2020260730191320-30.png)

关系数据库提供备份与恢复机制，用于：

- 从系统故障中恢复
- 数据归档

**MySQL** **示例**  
备份：  
mysqldump -u user -p database_name \> backup.sql  
恢复：  
在 MySQL CLI 中执行：  
source backup.sql
 
**44 Summary** **总结**  
本模块主要内容：

- Good database design requires thought￼数据库设计需要深思熟虑
- SQL is used to query relational databases￼SQL 是关系数据库的核心查询语言
- Most database access is done via applications￼实际使用中多通过应用程序访问数据库
- Large databases need performance optimisation￼大型数据库必须进行性能优化（索引等）
- Relational databases are one option among many￼关系数据库不是唯一方案
- NoSQL and other models may be better for certain workloads￼某些场景下 NoSQL 更合适

后续课程将进一步讨论其他数据管理解决方案。
 
**45 References** **参考资料**

- Database design basics￼[http://office.microsoft.com/en-us/access-help/database-design-basics-HA001224247.aspx](http://office.microsoft.com/en-us/access-help/database-design-basics-HA001224247.aspx)
- Oracle documentation￼[http://docs.oracle.com/cd/E11882_01/](http://docs.oracle.com/cd/E11882_01/)
- A relational model of data for large shared data banks￼[http://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf](http://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)
- Normal forms references￼[https://www.vertabelo.com/blog/normalization-1nf-2nf-3nf/](https://www.vertabelo.com/blog/normalization-1nf-2nf-3nf/)
 
如果你愿意，我可以继续为你整理：

1. **整套** **Relational Databases** **章节的总结版** **cheat sheet**
2. **ER diagrams + normalization** **的思维导图**
3. **SQL commands** **全表总结（****SELECT****、****JOIN****、****AGG****、****TRANSACTION****）**
4. **考试速记版** **+** **典型考题模板**

你想要哪一种？
 \> 来自 \<[https://chatgpt.com/c/69322758-4b90-832d-9d17-6e5cb2579c2b](https://chatgpt.com/c/69322758-4b90-832d-9d17-6e5cb2579c2b)\>

![[Lec8 Relational Databases - Ink.svg]]

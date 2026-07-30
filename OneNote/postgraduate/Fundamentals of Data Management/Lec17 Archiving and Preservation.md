下面为你整理成清晰、结构化、可直接作为课堂笔记使用的版本。整体保持中文解释 + 必要英文术语原文，符合教学笔记风格，无特殊符号。
 
**Archiving and Preservation**  
Chris Wood  
Fundamentals of Data Management  
0000-0001-6258-6447  
**Lecture Outline**  
本节课主要讨论：

- “Archiving and Preservation（归档与保存）”的含义
- 为什么不能简单理解为 “cp files.zip /archive”￼（提示：答案是否定的）
- 课程目标：
    - 解释 preservation 在数据生命周期中的位置
    - 理解长期数字保存（long-term digital preservation）的难点
    - 理解 OAIS（Open Archival Information System）模型的基础概念
 
**1. Preservation in the Data Lifecycle****（在数据生命周期中的保存）**  
传统误解：

- Preservation 常被认为是“最后一步”，数据生成后才开始考虑保存。

更正确的理解：

- Preservation 是数据生命周期的一部分，应该在规划阶段就被纳入。
- 数据在 preservation 之后依然会继续“活着”，仍然需要被访问、理解和再利用。

数据生命周期包括：  
Plan → Create → Assure → Describe → Preserve → Discover → Combine → Process

![Exported image](Exported%20image%2020260730192803-0.png)

**￼****2. Why Preserve Data?****（为什么保存数据？）**  
根据生命周期模型：

- 因为未来可能有人需要再次使用这些数据（re-use）
- 数据是科学记录的一部分，也是出版记录的一部分
- 因此需要以以下方式保存：
    - Safe（安全）
    - Discoverable（可被发现）
    - Accessible（可访问）
    - Comprehensible（可理解）
- 这正是数字保存系统与机构的目标
 
**3. Preservation Media****（保存介质对比）****￼**

![Exported image](Exported%20image%2020260730192805-1.png)

对不同存储介质，从写入速度、读取速度、容量、耐久性、风险等维度比较如下：

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|**Medium**|**Write**|**Read**|**Capacity**|**Durability**|**Risks**|
|Stone|very slow|slow|low|high|几乎没有，但信息密度极低|
|Wax|very slow|slow|low|high|Fire, fragility|
|Paper|slow|slow|low|high|Fire, water|
|Magnetic tape|fast|fast/slow|high|moderate|Fire, mechanical failure, EM fields|
|Optical (光盘)|fast|fast|moderate|high|Fire|
|Magnetic disk (HDD)|very fast|very fast|high|moderate|Fire, mechanical failure, EM fields|
|Solid state (SSD/Flash)|very fast+|very fast+|high|moderate|Fire, EM fields, quantum effects|
|DNA|very slow|very slow|high|high|Fire|

课堂提示：避免火灾。
 
**4. DNA as Storage****（用** **DNA** **存储数据）**  
基本原理：

- DNA 由 4 个碱基（T, G, A, C）组成，可用来编码二进制
    - 示例：T,G 代表 1；A,C 代表 0
    - 例如：161 = 10100001 = TAGCAACT
- Encoded by synthesising人工 DNA（非常慢而且昂贵）
- Decoded by sequencing（测序，成本较低但仍旧昂贵）

性能：

- 编码费用约 10,000 美元 / MB
- 解码费用约 100 美元 / MB
- 存储密度和耐久性极强

关键事实：

- 1 克 DNA 可存储约 700 TB 数据，理论保存 100,000 年
- 相比之下，3 TB 硬盘需要 233 块，总重约 151 kg，寿命约 20 年

研究来源：

- Church, Gao, Kosuri（Harvard Medical School）
- Birney, Goldman（European Bioinformatics Institute）
 
**5. Global Digital Storage Capacity****（全球技术存储能力）**  
（Hilbert & López, Science 2011）

- 强调数字化时代存储需求指数级增长
- 2019 年更新的研究进一步支持“所有信息都正在数字化”
 
**6****. Digital Preservation Strategies****（数字保存策略）**  
现实情况：

- ==当前所有数据基本都储存在== ==HDD====、====Tape== ==或== ==Solid State==
- ==HDD== ==的可靠性并不高==
    - 一项 CMU（2007）研究：Annual Replacement Rate（ARR）从 1% 到 13% 不等
    - 现状与当年几乎相同
    - 若你有 100 个 1 TB HDD，每年至少损坏 1 个
    - 你应当预计每年都会损失 TB 级数据

引用：  
Schroeder & Gibson, Disk failures in the real world, USENIX FAST
 
**7.** **Replication, replication, replication****（保存策略核心：复制）**  
关键观点：

- Tape 和 Flash 也并非完美
- 某些数字介质必然会失败，这是可预期的
- 因此必须通过 replication（多副本保存）进行风险控制

要点：

- 多副本（multiple copies）
- 多地点（multiple locations）
- 副本数量取决于数据价值和所能接受的风险水平

下面为你整理成清晰、课程笔记风格的中文与关键英文词汇保留版本，结构化、易复习，且不加入任何特殊符号。
 
**A Systematic Approach to Archiving****（系统化的归档方法）**  
**1. OAIS: Open Archival Information System Reference Model**  
OAIS 是一个数字存档的参考模型，具有标准化、通用化和长期保存导向的特征。  
背景

- 由 CCSDS（Consultative Committee on Space Data Systems）在 2000 年代提出
- 标准化为 ISO 14721:2003，并在 ISO 16363 中进一步扩展
- 全球多家机构采用，包括：
    - US National Archives and Records Administration
    - US Library of Congress
    - British Library
    - Bibliothèque nationale de France
    - National Library of the Netherlands
    - UK Digital Curation Centre
    - OCLC
    - JSTOR
    - 多数大学图书馆系统

总结：OAIS 是全球数字保存领域的标准框架。
 
**2. The OAIS Reference Model****（****OAIS** **模型概览）****￼**

![Exported image](Exported%20image%2020260730192806-2.png)

核心组成模块：

- Ingest
- Archival Storage
- Data Management
- Access
- Administration
- Preservation Planning
- 伴随的信息结构：SIP、AIP、DIP 和 Descriptive Information

模型结构作用：

- Producer 提交 SIP
- 经过 Ingest 处理成为 AIP 存入存档系统
- Consumer 查询后获得 DIP
 
**3. OAIS** **的关键概念（****Not just for digital libraries****）**  
OAIS 的概念不仅用于图书馆，也适用于任何长期数字保存系统。  
**3.1 Long Term**

- 时间跨度足够长，以至于底层技术可能发生重大变化￼例如：从 HDD 转向 DNA Storage

**3.2 Designated Community**

- 被保存对象的目标用户群
- 决定需要多少背景知识假设
- 决定需要附加多少 Representation Information

**3.3 Representation Information****（****RI****）**  
用于确保数字对象可理解，包括：

- 元数据
- 文件格式说明
- 解码软件及其文档
- 相关技术文档
- 确保数字对象在未来仍可被解释和使用
 
**4. OAIS Information Packages****（****OAIS** **信息包）****￼**

![Exported image](Exported%20image%2020260730192807-3.png)

**SIP**  
Submission Information Package

- 由 Producer 提交给存档系统
- 包含数字对象和必要的 Representation Information

**AIP**  
Archival Information Package

- 存档系统内部的保存形式
- 是封装结构，绑定：
    - 数据对象
    - 表征信息
    - 其他元数据（时间戳、复制策略等）

**DIP**  
Dissemination Information Package

- 提供给 Consumer 的形式
- 通常从 AIP 派生，按需求重新包装

**Descriptive Information**

- 用于支持检索、编目、查询
- 不是信息包本身，但用于帮助定位和描述数字对象
 
**5. OAIS** **工作流程各模块详解**  
以下模块在模型图中多次重复出现，这里统一整理说明：

![Exported image](Exported%20image%2020260730192809-4.png)  

**5.1 Ingest****（接受入库）**  
功能：

- 接收新的数字对象（DOs）
- 收集尽可能多的 Representation Information
- 将 DO 和 RI 打包成 AIP
- 是进入存档系统的唯一入口

特点：

- 非常劳动力密集
- 强调数据的完整描述和可理解性
 
**5.2 Archival Storage****（存档存储）**  
功能：

- 存储 AIP 的“比特层面内容”
- 维护副本、选择存储介质
- 根据 Preservation Planning 执行存储策略

特点：

- 会管理复制、校验、介质迁移等长期保存任务
 
**5.3 Data Management****（数据管理）**  
功能：

- 维护所有 Descriptive Information
- 管理目录、索引、引用、元数据
- 为 Access 提供可查询的信息

特点：

- 数字图书馆中的编目系统在 OAIS 中属于此部分
 
**5.4 Access****（访问服务）**  
功能：

- 接收来自 Consumer 的查询
- 通过 Data Management 查询索引
- 通过 Archival Storage 获取 AIP 中的数据
- 将 DO 打包成 DIP 提供给用户

特点：

- 关注可访问性与用户交互
 
**5.5 Administration****（管理）**  
功能：

- 监控系统运行情况
- 管理 Ingest 流量、查询情况、数据增长等
- 包含人工和自动化操作
 
**5.6 Preservation Planning****（保存计划）**  
功能：

- 制定整个存档系统的策略
- 由专业保存人员（人类 curator）负责
- 涉及技术演化监控、格式迁移策略、复制策略等

下面为你继续整理课堂笔记，结构清晰、中文为主、关键英文术语保留，风格与前几部分保持一致。
 
**Using OAIS in Data Management****（在数据管理中应用** **OAIS****）****￼**

![Exported image](Exported%20image%2020260730192809-5.png)

OAIS 模型不仅适用于大型机构，对研究项目、实验室甚至个人研究者的日常数据管理也非常有帮助。  
关键思考框架：  
**Designated Community**  
未来会是谁来查看我的数据

- 是本领域专家、项目成员、学生、未来研究者
- 不同用户对背景知识的假设不同，需要的补充信息量也不同

**Representation Information****（****RI****）**  
我需要随数据一起保存哪些额外信息

- 元数据（metadata）
- 文件格式说明
- 使用的软件与版本
- 解码或显示数据所需的依赖
- 文档说明

**AIP****（****Archive Information Package****）**  
我应该如何存储我的数据

- 使用哪种格式
- 如何保证安全
- 如何把数据及其元数据绑定为一个整体进行保存

**DIP****（****Dissemination Information Package****）**  
如果有人要访问我的数据，应如何分享

- 提供什么格式
- 是否需要转换或重新打包

**Long Term**  
若我以现在的方式保存数据，几年后还能安全存在吗

- 是否需迁移格式
- 存储介质是否可靠

课堂引用：Remember the panda（数据格式与软件依赖的重要性）  
视频链接： [http://www.youtube.com/watch?v=N2zK3sAtr-4](http://www.youtube.com/watch?v=N2zK3sAtr-4)
 
**Repository Technologies****（典型数字存储与存档平台）****￼**

![Exported image](Exported%20image%2020260730192813-6.png)

**1. DSpace**  
DSpace 是全球使用最广的 institutional repository 平台之一（约 3000 个实例）。  
用途

- 存储文本、视频、音频、数据等多种数字对象
- 提供 Web 分发
- 提供检索功能
- 适用于数字归档与长期保存

三大核心功能

- 支持材料与元数据的采集和 Ingest
- 提供便捷的列表浏览与搜索
- 支持内容的长期保存

案例

- University of Edinburgh 的 DataShare 基于 DSpace￼[http://datashare.ed.ac.uk/](http://datashare.ed.ac.uk/)

代码源  
[https://github.com/DSpace/DSpace](https://github.com/DSpace/DSpace)
 
**2. Fedora Repository****￼**

![Exported image](Exported%20image%2020260730192814-7.png)

全称：Flexible Extensible Digital Object Repository Architecture  
特点

- 高度可扩展的数字对象存储框架
- 提供抽象数字对象模型
- Content Model Architecture 支持：
    - 联邦式与分布式 repository
    - 对象版本管理
    - 对象间关系
    - 事件历史（访问审计）
- 支持 RESTful 与 SOAP/WSDL API
- 主要用于图书馆和大型研究机构

官网  
[https://duraspace.org/fedora/](https://duraspace.org/fedora/)
 
**3. CKAN****（较偏** **metadata repository****）****￼**

![Exported image](Exported%20image%2020260730192916-8.png)

CKAN 主要用于 metadata catalogue，而不仅仅是数据本体。  
特点

- 开源
- 用 Python + Flask 构建
- 搜索基于 Apache Solr
- 可存储 structured data（data resources）
- 提供数据切片与探索工具
- 支持二进制文件上传
- 丰富的插件体系：地图搜索、S3 存储、文件解析、认证扩展等
- 易于自定义
- 支持 metadata federation（联合）与 harvesting（收割）
- 提供全面的 RPC 风格 API

用途：开放数据平台常用工具（如政府开放数据网站）
 
**When OAIS Isn’t Enough****（当** **OAIS** **不再完全适用）****￼**

![Exported image](Exported%20image%2020260730192917-9.png)

OAIS 模型假设存档是集中式、受控的、长期的，但现代 Web 世界变得更加分布式、动态化。  
挑战

- 数据越来越分散
- Web 的发布模式无法完全套用 OAIS 的集中式思路
- 大规模复制分散在不同区域

一种替代方向：完全采用 keep copies 模式，发展分布式保护体系。  
**LOCKSS****（****Lots of Copies Keeps Stuff Safe****）**

- 强管理的分布式保存平台
- 为科学期刊等“dark copies”而设计
- 强调对攻击与损坏的鲁棒性
- 多个 LOCKSS 节点通过投票机制验证内容完整性
 
**Distributed & Unmanaged Archiving****（分布式且无集中管理的存档）****￼**

![Exported image](Exported%20image%2020260730192918-10.png)

**1. World-Wide Web**  
优点

- 高度分布式
- 使用开放协议，鲁棒、可扩展
- 发布简单

缺点  
– 搜索与索引依赖搜索引擎（Google 等）  
– 缺乏 provenance、quality、versioning  
– 无自动复制机制（需使用 ResourceSync 等技术）  
– URL 稳定性差（404 问题）  
**2. Peer-to-peer Sharing****（如** **BitTorrent****）**  
优点

- 自然分布式
- 多副本自动形成

缺点  
– 搜索依赖第三方索引（如 Pirate Bay）  
– 缺乏 provenance、quality、versioning  
– 不适合作为科研级长期归档
 
**Summary****（总结）**

- Archiving and Preservation 是数字对象生命周期的重要组成部分
- Preservation 的目的是 **future re-use**
- 若值得保存，就意味着它未来会被再次使用
- 硬盘存储仍占主导，但并不可靠
- 传统 repository（DSpace、Fedora）通常遵循 OAIS 模型
- 但 Web 时代的数据分布性让传统保存方式面临新挑战
 
如你需要，我可以继续为你制作：

- 一页式 OAIS cheat sheet（非常适合考试复习）
- OAIS 信息包流程图（中文或中英双语）
- Repository 技术对比表格￼只需要告诉我你是否需要继续整理哪一部分。
 \> 来自 \<[https://chatgpt.com/c/6932ec39-7f6c-8330-8ab0-bfc2b384606a](https://chatgpt.com/c/6932ec39-7f6c-8330-8ab0-bfc2b384606a)\>

![[Lec17 Archiving and Preservation - Ink.svg]]

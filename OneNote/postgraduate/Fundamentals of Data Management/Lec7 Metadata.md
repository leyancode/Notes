下面为你整理成结构清晰、课堂笔记风格的 **中英双语笔记**，关键术语全部保留英文原文，且无任何特殊符号。你可以直接复制到自己的笔记中继续扩展。
 
**Fundamentals of Data Management**  
**Lecture Notes: Metadata, Semantics, Ontologies**  
Chris Wood
 
**1 Metadata** **元数据**  
**1.1 Overview** **概览**  
本节课程介绍以下内容：

- Metadata 的概念
- Metadata 的来源
- Metadata 使用的原因
- 引入 Semantics 与 Ontologies，为下一周 RDF 与 Semantic Data 做铺垫

**1.2 What is Metadata** **元数据是什么****￼**

![Exported image](Exported%20image%2020260730191159-0.png)

Metadata 指：

- Data about data
- 一种“关于数据的数据”
- One person’s metadata is another person’s data￼对某些人来说是元数据，对另一些人来说是核心数据
- ==Information that makes data useful==￼使数据可被解释、可被理解的信息
- A description of what the data is￼数据是什么、如何使用、未来忘记时如何理解
- Used increasingly by automated systems￼自动化系统依赖 metadata 解释数据
- ==Metadata helps us understand data and possibly reproduce data====￼====帮助理解数据语义，甚至复现实验或观测方式==

总==结：====Metadata== ==是使数据可解释、可共享、可重现的关键信息。==
 
**2 Kinds of Metadata** **元数据的类型****￼**

![Exported image](Exported%20image%2020260730191200-1.png)

**2.1 System Metadata / Structural Metadata**  
==系统元数据== ==/== ==结构元数据==  
描述数据的存储与组织方式，例如：

- ==File ownership== ==文件所有权==
- ==Modification date== ==修改时间==
- ==Packaging format== ==数据如何被封装==

**2.2 Content Metadata / Descriptive Metadata**  
==内容元数据== ==/== ==描述性元数据==  
描述数据“关于什么”，包括：

- ==What== ==数据与什么相关==
- ==Where== ==数据与什么地点相关==
- ==When== ==数据与什么时间相关==
- ==Who== ==数据与哪些人相关==
- ==How== ==数据如何被收集或创建==
- ==Why== ==数据为何被收集或创建==
- ==Who collected / created== ==谁创建了数据==
- ==When collected== ==何时采集==
- ==Where collected== ==在何地采集==

这类 metadata 用于理解数据本身的语义背景。
 
**3 Metadata Categorisation** **元数据分类方法****￼**

![Exported image](Exported%20image%2020260730191204-2.png)

不同机构与作者有不同分类方式。  
**3.1 Structural/Control Metadata and Guide Metadata**  
（Bretherton & Singley, 1994）  
doi:10.1109/SSDM.1994.336950  
**3.2 Technical, Business and Process Metadata**  
（Ralph Kimball）  
urn:isbn:978-0-470-14977-5  
**3.3 Descriptive, Structural and Administrative Metadata**  
（National Information Standards Organisation）  
urn:isbn:1-880124-62-9  
这些分类反映组织对 metadata 的不同需求与关注点。
 
**4 Where is Metadata Stored** **元数据存储位置**  
Metadata 通常存储在两个地方之一：￼

![Exported image](Exported%20image%2020260730191205-3.png)

**4.1 Embedded Metadata** **内嵌元数据**  
例如 JPEG, MPEG, HDF5

- Metadata 与 data 存放在同一个文件
- 优点：不容易丢失
- 缺点：对 header 大小有限制，不够灵活

**4.2 External Metadata** **外部元数据文件**  
例如 data dictionary, metadata repository

- 更灵活、可扩展
- 但管理成本更高
 
**5 Semantics** **语义**

![Exported image](Exported%20image%2020260730191206-4.png)

Semantics 指数据的意义，包括如何在 data 与 metadata 中表达这种意义。  
一个简单例子：文件中的 “date” 可能表示：

- The date that the data describes 数据所指的时间
- The date stored 数据写入时间
- A time during the day 数据属于当天某一时刻
- A daily average 日平均
- The first data point’s timestamp 数据集中第一个时间点

此外，语义应明确约定术语。例如：

- “rain” 是否包括 “sleet” 或 “hail”￼严谨语义定义可避免歧义。
 
**6 Vocabulary** **与** **Ontology**

![Exported image](Exported%20image%2020260730191207-5.octet-stream)

**6.1 Vocabulary** **词汇表（受控词汇）**  
Vocabulary 是：

- Controlled vocabulary
- 用于表达语义的定义集合
- Precise definitions for a set of terms
- 可用于 metadata 或 data 本身

与此对比的是 Folksonomy（标签体系）：

- 由用户自由打标签
- 动态、响应性强
- 但不受控、缺乏一致性￼
- ![Exported image](Exported%20image%2020260730191208-6.png)

**6.2 Ontology** **本体**  
W3C 的定义概要：

- Vocabulary 与 Ontology 并无绝对界限
- Ontology 越复杂，结构越正式化
- Ontology 常用于支持 inference（推理、关系、层级）
- Vocabulary 更像定义列表，但两者有大量重叠

解释：

- Vocabulary: terms + definitions
- Ontology: terms + definitions + relationships + inference

下面为你继续按照课堂笔记风格，使用 **中英双语、结构清晰、保留英文关键术语、不加入任何特殊符号** 进行整理。整体格式与你上一段整理保持一致，可直接加入你的课程笔记中。
 
**7 Examples of Vocabularies** **词汇表示例****￼**

![Exported image](Exported%20image%2020260730191209-7.png)

**7.1 FOAF (Friend of a Friend)**  
用途：描述人与人之间的关系、基本个人信息。  
示例 RDF：  
\<==rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#=="￼xmlns:foaf="http://xmlns.com/foaf/0.1/"\>￼ \<foaf:Person\>￼ \<foaf:name\>Chris Wood\</foaf:name\>￼ \<foaf:homepage rdf:resource="https://www.ed.ac.uk/profile/dr-chris-wood" /\>￼ \</foaf:Person\>￼\</rdf:RDF\>  
**7.2 More Vocabulary Examples** **更多词汇表示例****￼**

![Exported image](Exported%20image%2020260730191210-8.png)

- schema.org￼为网页提供结构化信息标注。
- SKOS (Simple Knowledge Organization System)￼用于表示其他 controlled vocabularies（受控词汇表）。
- DOAP (Description of a Project)￼用于描述软件项目的信息。
- Dublin Core￼常用于 metadata 标准化，将在后续课程详细讨论。

**7.3 Domain-specific Ontologies** **专用领域本体**  
这些本体为特定行业或科学领域定义结构化语义：

- Human Disease Ontology
- UK Ordnance Survey ontology（英国行政区边界）
- CHMO: Chemical Methods Ontology
- NERC Vocabulary Service（环境科学领域的受控术语）

**7.4 Non-specific Vocabularies** **非特定领域词汇**

- FAST vocabulary￼基于 Library of Congress headings，约170万词条，分为8个主题。
 
**8 Why Should You Use Metadata** **为什么要使用元数据****￼**

![Exported image](Exported%20image%2020260730191213-9.png)

Metadata 的价值主要体现在以下方面：  
**8.1 Discoverability** **可发现性**

- Metadata 让数据更容易被检索
- 搜索系统和用户均可基于 metadata 进行查找

**8.2 Reusability** **可复用性**

- Metadata 让数据“可理解”，因此可被复用
- 可用于相同目的，如验证实验
- 也可用于新的研究问题
- 有助于发现相关数据

**8.3 Reproducibility** **可重现性**

- 当 metadata 包含 how / why / where 的信息时￼他人可以复现实验、验证结论

总结：Metadata 是 FAIR 数据原则（Findable, Accessible, Interoperable, Reusable）的核心。

![Exported image](Exported%20image%2020260730191214-10.png)

**9 What Makes Good Metadata** **什么是好的** **Metadata**  
高质量的 metadata 需满足以下特征：￼

![Exported image](Exported%20image%2020260730191216-11.png)

**9.1 Core Qualities** **核心特性**

- Complete 完整
- Accurate 准确
- Precise 精确
- Conforming to standards 遵循标准
- Semantic clarity（语义清晰）￼所有术语必须有明确含义

**9.2 Formatting and Syntax** **格式与语法要求**

- 正确的格式（如日期时间格式）
- 遵守 schema 或 vocabulary 的规则
- 哪些字段是 mandatory（必填）
- 哪些字段可选

**9.3 Accessibility** **可访问性**

- Metadata 应可在线获取
- 可被引用、链接（addressable）
- 可被程序自动 harvest（收集）
 
**10 What is Metadata: Provider vs Receiver** **视角差异****￼**

![Exported image](Exported%20image%2020260730191218-12.png)

当你提供数据 vs 接收数据时，关心的 metadata 内容不一样。  
**10.1 Providing Data** **提供数据时应包含的信息****￼**

![Exported image](Exported%20image%2020260730191219-13.png)

- Why were the data created? 数据为何被创建
- What limitations do the data have? 数据有哪些局限
- What do the data mean? 数据含义
- How to cite the data? 如何引用
- Who collected / created 数据由谁产生
- What methods were used 数据如何生成

**10.2 Receiving Data** **接收数据时想了解的内容**

- What are the data gaps? 有何缺失
- What processes were used to create the data?
- Are there fees associated with the data? 是否收费
- At what scale were the data created? 数据尺度或分辨率
- Meaning of table values 表中每个值的含义
- Required software 打开数据的工具需求
- Projection used 数据使用的地理投影
- Redistribution: can I pass the data on? 能否共享给他人
 
**11 Who Should Create Metadata** **谁来创建** **Metadata****￼**

![Exported image](Exported%20image%2020260730191219-14.png)

**11.1 Ideal Author** **理想情况**

- 最适合创建 metadata 的人，是创建数据的人
- 因为他们最了解数据的内容、背景与限制

**11.2 Additional Contributors** **额外贡献者**

- 数据分发者或 curator 也可能添加 metadata
- 特别是 structural metadata（结构元数据）

Metadata 可以存储在：

- 数据库内部
- 文件内部
- 单独的数据字典（data dictionary）
 
**12 Structural Metadata** **结构元数据****￼**

![Exported image](Exported%20image%2020260730191221-15.png)

**12.1** **内容描述类结构元数据**  
通常由领域专家编写，描述数据结构：

- Column / variable names 列名、变量名
- Data types 数据类型（int, float, date 等）
- Limitations（如 string 长度）
- Valid ranges 合法取值范围
- Dataset dimensions 数据维度
- Codelists 编码列表
- Relationships 关系
- Definitions 定义
- 使用哪些 vocabularies

也适用于模拟内容（例如书籍）：

- 页码
- 章节顺序

**12.2** **数据库中的** **Structural Metadata****￼**

![Exported image](Exported%20image%2020260730191225-16.png)

数据库系统尤其依赖结构化 metadata，包括：

- Table names 表名
- Owners / permissions 权限
- Update histories 更新记录
- Table size 表大小
- External data sources 外部自动填充数据的来源
- Referential integrity constraints 参照完整性约束
- Column constraints（unique key 等）
- Foreign key cardinality 外键的关联数量

在 PostgreSQL 中，这些 metadata 存储于以 pg_ 开头的系统表中，统称 Definition Schema。
   

**13 Important Metadata Standards** **元数据标准概览****￼**

![Exported image](Exported%20image%2020260730191226-17.png)

**13.1 Why are Metadata Standards Needed** **为什么需要元数据标准**

- 现有的 metadata 标准数量众多，每个标准都有特定侧重点
- 虽然关注点不同，但通常都会要求记录类似的信息
- 选择合适的标准通常取决于：
    - Your field of practice 所属领域
    - Your motivation for using metadata 使用 metadata 的目的

然而，选择哪一个标准依然不容易，因此如 RDA Metadata Standards Directory Working Group 等机构正在编制可查询目录，帮助用户找到最适合的数据标准。  
参考： [https://rd-alliance.org/group/metadata-standards-directory-working-group.html](https://rd-alliance.org/group/metadata-standards-directory-working-group.html)

![Exported image](Exported%20image%2020260730191226-18.png)

**14 Metadata Standards: Examples** **元数据标准示例**  
下面整理课程中提到的主要 metadata 标准及其应用领域。
 
**14.1 Dublin Core Metadata Initiative (DCMI)****￼**

![Exported image](Exported%20image%2020260730191227-19.png)

官网与规范：

- Dublin Core Metadata Terms
- Dublin Core Metadata Element Set (DCMES)
- [http://dublincore.org/documents/dces/](http://dublincore.org/documents/dces/)

**14.1.1 Focus** **适用场景**

- 强调 Web 资源、出版物、数字对象
- 已被标准化为
    - ISO 15836:2009
    - ANSI/NISO Z39.85-2012

**14.1.2 Core Elements** **核心元素**  
常见的 Dublin Core 元素包括：  
Contributor  
Coverage  
Creator  
Date  
Description  
Format  
Identifier  
Language  
Publisher  
Relation  
Rights  
Source  
Subject  
Title  
Type  
这些元素帮助对资源进行一致而结构化的描述。
 
**14.2 FGDC CSDGM**

![Exported image](Exported%20image%2020260730191229-20.png)

Full name: FGDC Content Standard for Digital Geospatial Metadata  
重点：地理空间数据  
机构：The Federal Geographic Data Committee（美国政府跨机构委员会）  
官网： [http://www.fgdc.gov/metadata/geospatial-metadata-standards](http://www.fgdc.gov/metadata/geospatial-metadata-standards)  
**Profiles and Extensions** **扩展与子集**

- Biological Data Profile (BDP)￼支持生态、生物、且含地理空间属性的数据
- 其他针对领域的扩展

￼￼

![Exported image](Exported%20image%2020260730191229-21.png)

**14.3 ISO 19115 / ISO 19139**  
标准名称：Geographic Information: Metadata  
重点：地理空间信息和服务  
相关页面：  
[http://www.fgdc.gov/metadata/geospatial-metadata-standards#fgdcendorsedisostandards](http://www.fgdc.gov/metadata/geospatial-metadata-standards#fgdcendorsedisostandards)  
特点：

- 全球通用的地理信息元数据标准
- ISO 19139 为 XML 实现规范
 
**14.4 Ecological Metadata Language (EML)**  
重点：生态学领域数据  
官网： [http://knb.ecoinformatics.org/eml_metadata_guide.html](http://knb.ecoinformatics.org/eml_metadata_guide.html)  
特点：

- 针对生态研究的数据结构、方法、采样过程等提供详细描述
- 在生态信息学中使用极为广泛
 
**14.5 Darwin Core**  
重点：生物标本与自然历史记录（如博物馆标本）  
官网： [http://rs.tdwg.org/dwc/index.htm](http://rs.tdwg.org/dwc/index.htm)  
用途：

- 标准化描述物种、分类信息、标本采集记录等
- 被自然历史博物馆、野外观察站等广泛采用
 
**14.6 Geography Markup Language (GML)****￼**

![Exported image](Exported%20image%2020260730191230-22.png)

重点：表示地理特征，如道路、桥梁、行政区域  
官网： [http://www.opengeospatial.org/standards/gml](http://www.opengeospatial.org/standards/gml)  
特点：

- XML 编码
- 用于交换和建模地理实体
- 是 OGC（Open Geospatial Consortium）标准
 
**14.7 Protein Data Bank Exchange Dictionary (PDBx)**  
以及 Macromolecular Crystallographic Information Framework (mmCIF)  
重点：三维蛋白质与核酸结构  
官网： [http://mmcif.wwpdb.org/](http://mmcif.wwpdb.org/)  
特点：

- PDBx 定义用于结构生物学中 PDB 数据提交、注释、存档所需的 metadata
- 全球结构生物学数据库体系的重要组成部分
 
**14.8 Other Metadata Standards** **其他元数据标准**  
**DDI: Data Documentation Initiative**  
官网： [http://www.ddialliance.org/](http://www.ddialliance.org/)  
重点：社会科学数据、调查数据、问卷数据的结构化描述  
**CDWA: Categories for the Description of Works of Art**  
官网： [http://www.getty.edu/research/publications/electronic_publications/cdwa/index.html](http://www.getty.edu/research/publications/electronic_publications/cdwa/index.html)  
重点：艺术品、文化遗产对象（如雕塑、建筑、绘画）的描述与分类  
下面继续为你整理课堂内容，保持与之前完全一致的 **中英双语、结构清晰、关键术语保留英文、不加入特殊符号** 的笔记格式，可直接拼入你的课程笔记。
 
**15 Semantic Annotation** **语义标注****￼**

![Exported image](Exported%20image%2020260730191234-23.png)

**15.1 What is Semantic Annotation** **什么是语义标注**  
Semantic Annotation 指在已有数据上增加语义信息，通常用于以下场景：

- Annotating existing data 对现有数据进行补充标注
- 特别适用于由他人创建的 derived data 或 long-tail data￼这些数据往往存在格式不一致、信息缺失或错误

**15.2 Why Semantic Annotation is Needed** **为什么需要语义标注**  
数据的后续用户通常希望：

- 标注其中的错误
- 引用 accepted ontologies（公认的本体）
- 指向外部的最新数据资源

这使得数据更加：

- 可理解
- 可验证
- 可链接到更广的知识体系

**15.3 How Semantic Annotation Works** **如何执行语义标注**

- Semantic Web 中的许多技术也可用于此场景￼例如 RDF、RDFS、OWL、SPARQL
- 同样的 ontologies 可用于原始数据与标注数据
- 通过 semantic annotation，可以将数据转换为具有机器可解释意义的“语义增强数据”

**15.4 Community and Initiatives** **社区与相关组织**

- EUDAT 有专门的 Semantic Annotation Working Group￼关注如何为科学数据提供语义标注、链接数据、基于语义的发现与访问
 
**16 Further Reading on Metadata and Semantics** **拓展阅读**  
以下资源提供更深入的资料，涵盖 metadata、ontologies、语义数据建模等主题。  
**16.1 Metadata** **基础入门**

- Understanding Metadata￼[http://www.niso.org/publications/press/UnderstandingMetadata.pdf](http://www.niso.org/publications/press/UnderstandingMetadata.pdf)

**16.2 DataONE** **教学模块**

- Lessons 7 and 8（Metadata 与 Data Management）￼[https://www.dataone.org/education-modules](https://www.dataone.org/education-modules)

**16.3 Metadata Standards Directory**

- RDA Metadata Standards Directory Working Group￼[https://rd-alliance.org/working-groups/metadata-standards-directory-working-group.html](https://rd-alliance.org/working-groups/metadata-standards-directory-working-group.html)

**16.4** **语义技术与** **EUDAT**

- Semantics at the Second EUDAT Conference￼[http://www.eudat.eu/system/files/Semantics%20at%20the%20Second%20EUDAT%20Conference.pdf](http://www.eudat.eu/system/files/Semantics%20at%20the%20Second%20EUDAT%20Conference.pdf)
- B2FIND User Documentation￼[http://www.eudat.eu/User%20Documentation-%20B2FIND.html](http://www.eudat.eu/User%20Documentation-%20B2FIND.html)

这些材料涵盖元数据标准、语义工具、学术与产业最常用的 metadata 框架。
 
**17 Metadata: Summary** **元数据总结**  
Metadata 的关键作用如下：  
**17.1 What is Metadata** **元数据是什么**

- Metadata is “data about data”
- Metadata is documentation of data
- 是数据可理解、可发现、可访问、可复用的基础

**17.2 Why Metadata Matters** **为什么重要**  
Metadata 让数据：

- Discoverable 可被发现
- Accessible 可被访问
- Understood 可被理解
- Re-used 可被复用

**17.3 Metadata Standards** **元数据标准的作用**

- 提供结构化与一致性
- 不同领域、组织会选择不同标准
- 选择时应基于明确标准，如：
    - 数据类型
    - 组织规范
    - 可用资源

**17.4 Semantics Adds Value**

- 当 metadata 标准包含 semantic definitions 时，附加价值更高
- 支持机器可理解
- 支持跨系统 interoperability
- 支持基于 ontology 的自动推理
 
**18 Acknowledgements and Re-Use** **致谢与版权说明**  
以下内容来自课程幻灯片说明的整理：

- 本课程内容由 University of Edinburgh 和相关作者制作
- 授权方式：CC-BY 4.0
- 最初由 Adam Carter（EPCC）编写，用于 Fundamentals of Data Management 课程
- Chris Wood 对 2018-19、2019-20、2022-23、2025-26 学年作了轻微更新
- Slide 13 使用 NERC Vocabulary Server（CC-BY 4.0）内容
- Slides 24 和 25 使用 DCMI（CC-BY 3.0）内容
- 部分示例元数据标准源自 Shaun de Witt（STFC）的“Metadata and EUDAT”幻灯片
- DataONE 相关幻灯片来自 [https://www.dataone.org/education-modules](https://www.dataone.org/education-modules%EF%BC%88CC0%EF%BC%89)（CC0）
- PID slides 内容来自 EUDAT 项目（[www.eudat.eu](http://www.eudat.eu\)/)）
 
如需，我可以为你把整个 Metadata 单元：

1. 汇总为一页考试 cheat sheet
2. 整理成可打印的 LaTeX 讲义格式
3. 按主题制作思维导图
4. 提供 RDF、Ontology、Vocabulary 的可视化关系图
5. 为你写一个“Metadata 总复习大纲”

你希望下一步做哪一个？
 \> 来自 \<[https://chatgpt.com/c/69322758-4b90-832d-9d17-6e5cb2579c2b](https://chatgpt.com/c/69322758-4b90-832d-9d17-6e5cb2579c2b)\>

![[Lec7 Metadata - Ink.svg]]

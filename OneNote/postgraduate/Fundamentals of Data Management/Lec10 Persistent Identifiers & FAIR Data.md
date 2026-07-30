下面继续按照课堂笔记的格式整理本部分内容，保持中文讲解、英文关键术语保留、结构清晰、便于复习。
 
**Lecture X: Persistent Identifiers & FAIR Data**  
（持久标识符与 FAIR 数据）
 
**1. Overview****（概览）**  
本讲分为两个部分：  
**Part 1****：****Persistent Identifiers****（****PIDs****）**

- 什么是 PID
- 为什么需要 PID
- PID 如何帮助数据管理

**Part 2****：****FAIR Data**

- FAIR 原则如何改进数据管理流程
- FAIR 与科研数据共享的关系
 
**2.** **Persistent Identifiers****（持久标识符）**  
**2.1 Why PIDs?****（为什么需要** **PIDs****？）****￼**

![Exported image](Exported%20image%2020260730191326-0.png)

随着数字时代的数据规模和数据类型不断增长：

- 数据分散在**不同地点**、由**不同组织**控制
- 数字对象数量迅速增长
- 数据生成越来越便宜、快速
- 数据正在成为一种“科研基础设施”
- 科学研究更强调跨领域、跨团队的数据共享

为了保证数据可被长期且唯一地引用，需要使用 **Persistent Identifiers****（持久标识符）**。  
**使用** **PIDs** **的好处：**

- 数据可以被可靠、唯一地引用（globally addressable）
- 数据可以被找到 → 更便于你本人、他人、以及机器搜索
- 数据可以被引用（citation）
- 数据可以被链接（linking）
- 支持数据的长期可用性与可重用性

**结论**：PIDs 是开放科学与现代数据管理的核心基础。
 
**2.2 What are PIDs?****（什么是** **PID****？）**  
==Persistent Identifier== ==是一种== **唯一、永久指向特定对象的字符串标识符**。  
它们具有 **不随位置变化而改变的特性**。  
数据位置可以改，但 PID 不会变。  
**可能的** **PID** **格式包括：**

- 10876/abc123
- 10.1594/WDCC/CMIP5.NCCNMpc
- ark:/13030/tf5p30086k
- [http://purl.org/dc/elements/1.1](http://purl.org/dc/elements/1.1)
- urn:lsid:ubio.org:namebank:11815

PID 本质上是持久的 ID，用来标识：

- 数据集（datasets）
- 论文（publications）
- 软件（software artifacts）
- 设备（instruments）
- 实验对象（samples）￼等。
 
**2.3 PID Systems****（****PID** **系统）**  
目前常见的 PID 基础设施包括：

|   |   |
|---|---|
|**PID** **类型**|**描述**|
|Handle System|最通用的 PID 架构|
|DOI（Digital Object Identifier）|最常见，用于论文、数据集|
|ARK（Archival Resource Key）|用于长期保存的数字对象|
|PURL（Persistent URL）|永久链接 URI|
|LSID（Life Science Identifier）|生物学数据库特定 ID|
|URN（Uniform Resource Name）|永久命名方案，如 ISBN|
 
**2.4 Examples of PIDs****（****PID** **示例）**  
**1. Handle System**  
[http://hdl.handle.net/1234/56](http://hdl.handle.net/1234/56)  
**2. DOI**  
[http://doi.org/10.1002/prot.9999](http://doi.org/10.1002/prot.9999)  
**3. ARK**  
[http://www.nmah.org/ark:/13030/tf5p30086k](http://www.nmah.org/ark:/13030/tf5p30086k)  
**4. PURL**  
[http://purl.oclc.org/keith/home](http://purl.oclc.org/keith/home)  
**5. LSID**  
urn:lsid:\<Authority\>:\<Namespace\>:\<ObjectID\>[:\<Version\>]  
**6. URN**  
urn:isbn:0451450523  
下面继续将这一部分内容整理成结构清晰、适合课堂笔记的格式，采用中文说明 + 英文关键词保留，强调逻辑结构与可复习性。
 
**2.5 What PIDs Identify****（****PID** **可以识别什么？）**  
PIDs 用于标识 **resources****（资源）**。==资源本身被视为一个== _black box_==（黑盒子）==，可能包含：

- 数据（data）
- 元数据（metadata）
- 文档（document）
- 软件代码（software code）
- 其他数字对象

此外，PIDs 还可以标识 **真实世界的实体（****things****）**，例如：

- 物种（在 LSID 中常见）
- 作者 / 机构（ORCID, ROR）
- 实验样本、设备

重要：  
**PID** **通常指向它所标识的事物**，无论是数字对象还是概念实体。
 
**2.6 PIDs Are Globally Unique****（****PID** **的全局唯一性）**  
==PID== ==的一个核心优势是== **global uniqueness****（全局唯一）**==。==  
这意味着：  
**优点：**

- 对使用者来说，能够唯一且稳定地找到对应资源。
- 防止歧义、冲突，特别在国际化数据共享环境中尤为重要。

**挑战：**

- 如何保证新分配的 PID 不会与他人的冲突？
- 如何让别人发现（discover）你的 PID？
- 谁负责维护 PID 注册表？

因此，通常由 **专门的机构或基础设施**负责 PID 分配：

- DOI → DataCite / Crossref
- Handle → CNRI
- ARK → California Digital Library
- ORCID → ORCID Registry
 
**2.7 PIDs Are Persistent Over Time****（持久性）**  
持久性（==persistence==）是 PID 最核心的特征之一：

- PID **永远不应改变**
- 数据即使迁移，也不能改变 PID
- 必须始终保证 PID 能正确解析到资源

**责任与机制：**

1. **数据控制者（****data controller****）**
    - 保证数据迁移后仍可通过 PID 找到最终位置
    - 数据存在不同服务器、云平台等都不影响 PID
2. **PID** **发行机构（****PID authority****）**
    - 长期维护 PID 解析服务
    - 确保唯一性（federation）
    - 保证查询（resolution）始终可用

例如 Handle 系统通过 **分离标识符与存储位置（****decoupling identifier and location****）** 的方式，使得数据迁移不会破坏 PID。
 
**2.8** **PIDs Establish a Redirection Layer**  
（PID 通过“重定向层”实现稳定性）  
引用经典计算机科学语录：  
“All problems in computer science can be solved by another level of indirection.”  
– David Wheeler  
**工作原理：**  
==PID → URL →== ==数字对象（====digital object====）==  
例如：  
doi:10.xxx/yyy → [https://server-A/data/123](https://server-A/data/123)  
若数据迁移到另一服务器：  
[https://server-B/dataset/new_location](https://server-B/dataset/new_location)  
只需更新 PID 指向的新 URL：  
doi:10.xxx/yyy → [https://server-B/dataset/new_location](https://server-B/dataset/new_location)  
==无需改变== ==PID== ==本身==。  
**好处：**

- 数据迁移不影响引用
- 组织变更不影响 PID
- 文件路径变更不影响 PID

结论：  
**PID** **是数据位置变化与长期可访问性之间的****“****缓冲层****”****（****indirection layer****）**==。==

![Exported image](Exported%20image%2020260730191328-1.png)

**2.9 Handle System Architecture****（****Handle** **系统架构思想）****￼**

![Exported image](Exported%20image%2020260730191329-2.png)

讲义提供的一张概念架构图体现：

- PID 管理位于网络层的上一层
- 支持 Value Added Services（如元数据、类型信息、引文分析）
- PID resolution 系统为应用、客户端提供统一访问入口
- PID 可指向本地、云端、数据库、文件系统等任意存储中的数字对象

（无需特殊符号，因此略述图像内容）
 
**2.10 Minimal Metadata****（****PID** **最小元数据）****￼**

![Exported image](Exported%20image%2020260730191333-3.png)

PID 通常至少存储 minimal metadata，确保解析与验证：

- **Checksum**（校验和） → 用于验证对象完整性
- **Creation timestamp**（PID 创建时间）
- **Graph structure / links**（链接关系）
- **Collection membership**（所属集合）

某些 metadata 是静态的，某些则可以动态更新。
 
**2.11 Advantages of PIDs****（****PID** **的优势）**  
总结 PID 的关键优势：

![Exported image](Exported%20image%2020260730191334-4.png)

**1. Persistent Identity via Indirection**  
通过“间接层”保证持久身份，即使资源移动也不会改变标识符。  
**2. Static Identity**  
PID 稳定不变，适合作为引用对象的“正式 ID”。  
**3.** **承受数据变化**  
即使发生变化，PID 仍保持有效：

- 数据位置变化
- 所有权变化
- 数据格式变化

**4. Embedded IDs**  
PID 可以嵌入在数据本体中用于追踪：

- 当前状态
- 更新历史
- 相关实体（related entities）

**5. Networks of Persistent Links**  
PID 形成可追溯的链接网络：

- 数据与元数据之间
- Provenance chains（数据来源链）
- 实体继承与引用结构

PID 网络对于：

- 重现性（reproducibility）
- 数据治理（data governance）
- 长期存档（long-term preservation）

至关重要。  
下面继续按照课堂笔记格式，整理本次内容，确保结构清晰、术语规范、适合复习使用。
 
**2.12 PIDs: Disadvantages****（****PID** **的劣势与挑战）****￼**

![Exported image](Exported%20image%2020260730191335-5.png)

尽管 PIDs 在数据共享中非常关键，但其部署与维护也带来成本：  
**1.** **创建成本与额外工作**

- 需要决定 **识别粒度（****granularity****）**：标识整个数据集？单个文件？版本？
- 跨组织协调（coordination）成本较高。

**2.** **需要维护解析系统（****resolution system****）**

- PID 系统必须长期稳定运行。
- 持久性（persistence）不是自动保证的，需要组织投入。

**3.** **组织层面的自律（****organisational discipline****）**

- 技术必要但不足够，需要管理制度配合。

**4.** **成本****—****效益分析**

- 并非所有数据都值得分配 PID。
- 必须评估：**Is your data worth it?**

结论：  
PID 有价值，但应在**合理的场景**下启用，例如重要数据、公共数据、科研产出等。
 
**2.13 Requirements for a “Good” Identifier**  
（一个“好的标识符”需要满足什么要求？）  
**核心要求：**

1. **不是基于可变属性（****not based on changeable attributes****）****￼**不应依赖：
    - 数据存放位置（location）
    - 所有权（ownership）
    - 会随时间变化的属性
2. **Opaque****（不透明），最好是****“dumb number”**
    - 含义明确的编号容易引发误解、语言问题
    - 富语义 ID 可能引发知识产权争议（例如包含地名、单位名）
3. **Unique****（唯一性）**
    - 避免冲突（collisions）
    - 确保全球唯一

**Nice to have****（额外便利特性）****：**

- **Human-readable**（人可读性）
- **Copy-and-paste friendly**（易复制）
- **符合** **URI** **规范**

这些特性共同提高标识符的 **persistence****（持久性）**。
 
**2.14 How to Use a PID****（如何使用** **PID****）**  
**若** **PID** **属于你的数据：**

- 应在相关场景明确写出 PID，例如：
    - 论文
    - 数据集说明
    - 网页
    - 其他 Linked Data 中

**若引用他人的** **PID****：**

- 使用 PID 获取资源
- 或在引用中使用 PID 作为权威标识符

示例：  
[http://hdl.handle.net/1234/56](http://hdl.handle.net/1234/56)  
访问后系统会自动将 PID 解析到对应数据对象。
 
**2.15 How to Get a PID****（如何获得** **PID****）****￼**

![Exported image](Exported%20image%2020260730191336-6.png)

最常见方式：  
**将数据上传到提供** **PID** **的服务或仓库（****repository****）**  
包括：

- **EUDAT**
- **机构仓库（****institutional repository****）**
- **学科仓库（****subject repository****）**
- **FigShare****、****Zenodo**（都会自动分配 DOI）

**服务端如何获得** **PID****？**  
以 EUDAT 为例：

- 通过 EPIC 服务（由 SARA 提供）
- 遵循 RESTful EPIC API
- 参考网站： [http://www.pidconsortium.eu/](http://www.pidconsortium.eu/)
- EUDAT 成员还可使用 Python 脚本包装该服务
 
**2.16 IDs for People****（为****“****人****”****分配的** **PID****）**  
在研究数据管理中，常常需要给“人”分配一个稳定的、唯一标识符。  
用途：

- 表明贡献者（creator/contributor）
- 避免重名问题（如同一机构内有多个叫 Chris Wood 的人）
- 增强信用（credit）与可追踪性

**常见的个人** **PID****：**

- **ORCID**
- **International Standard Name Identifier****（****ISNI****）**
 
**2.17 PIDs: Summary****（****PID** **小结）**

- PID 用于唯一识别并定位数字对象
- 作用包括：**citation****（引用）**、**linking****（链接）**、**unambiguous reference****（无歧义引用）**
- 存在多种 PID 系统，但目标趋同
- 持久性需要努力维持，但多数工作由服务方完成
- 获取 PID 的方法包括：
    - EUDAT
    - 学科仓库
    - 机构仓库
    - FigShare / Zenodo
 
**Part 3: FAIR Data Principles****（****FAIR** **数据原则）**  
**3.1 Background****（背景）**  
FAIR 原则的核心思想：

- 良好的数据管理是为了提升科学知识，而不是目的本身
- 数据对象需要被长期保存（long-term care）
- ==关键目标：增强数据的== **可重用性（****reusability****）**
- 为机器与人提供清晰的数据结构与标准
- 数据采集成本高，因此需要妥善维护
- 因数据管理标准不统一，FAIR 提供统一指导理论

FAIR 不是标准（standard），而是 **一套指南（****guiding principles****）**。  
**定义来源（****2016****）**  
Wilkinson, M., Dumontier, M., Aalbersberg, I. et al.  
_The FAIR Guiding Principles for scientific data management and stewardship._  
Sci Data 3, 160018 (2016).  
[https://doi.org/10.1038/sdata.2016.18](https://doi.org/10.1038/sdata.2016.18)  
**3****.2 FAIR** **的四项原则**

- **Findable****（可查找）**
- **Accessible****（可获取）**
- **Interoperable****（可互操作）**
- **Reusable****（可复用）****￼**
![Exported image](Exported%20image%2020260730191337-7.png)  

**3.3 FAIR Principles****（****FAIR** **原则详细条款）**  
FAIR（Findable, Accessible, Interoperable, Reusable）是一套用于提升科研数据可发现性、可获取性、可互操作性与可复用性的指导原则。
 
**F: Findable****（可查找）**  
目标：让用户与机器能够轻松找到数据与元数据。

- **F1. (meta)data are assigned a globally unique and persistent identifier****￼**数据与元数据必须具有全球唯一且持久的 PID。
- **F2. data are described with rich metadata (defined by R1)****￼**数据必须具有丰富的元数据描述。
- **F3. metadata clearly and explicitly include the identifier of the data it describes****￼**元数据中必须明确包含其对应数据的标识符（例如 DOI）。
- **F4. (meta)data are registered or indexed in a searchable resource****￼**数据与元数据必须注册在可搜索的系统中，如 catalogue、index、repository。
 
**A: Accessible****（可获取）**  
目标：用户与机器能够通过标准通信协议检索数据。

- **A1. (meta)data are retrievable by their identifier using a standardized communications protocol****￼**使用标准协议（如 HTTP/HTTPS）可通过 PID 访问数据或元数据。
- **A1.1 the protocol is open, free, and universally implementable****￼**协议必须开放、免费并可广泛实现。
- **A1.2 the protocol allows for an authentication and authorization procedure, where necessary****￼**协议在必要时必须支持身份验证与授权（例如隐私数据）。
- **A2. metadata are accessible, even when the data are no longer available****￼**即使数据本身不再可用，元数据仍必须保持可访问。
 
**I: Interoperable****（可互操作）**  
目标：数据能够与其他数据、应用、系统进行集成。

- **I1. (meta)data use a formal, accessible, shared, and broadly applicable language for knowledge representation****￼**使用正式、可访问、共享且通用的知识表示语言（如 RDF、JSON-LD）。
- **I2. (meta)data use vocabularies that follow FAIR principles****￼**使用符合 FAIR 原则的词汇表（如 RDFS、OWL、SKOS、DC）。
- **I3. (meta)data include qualified references to other (meta)data****￼**数据与元数据应包含对其他（元）数据的明确引用（qualified references），例如使用 URI。
 
**R: Reusable****（可复用）**  
目标：使数据能够被他人理解、引用、复用再研究。

- **R1. meta(data) are richly described with a plurality of accurate and relevant attributes****￼**数据与元数据必须具备充分、准确且相关的描述。
- **R1.1. (meta)data are released with a clear and accessible data usage license****￼**必须提供明确且可访问的许可协议（如 CC-BY、CC0）。
- **R1.2. (meta)data are associated with detailed provenance****￼**必须提供详细的数据来源（provenance）信息，例如创建者、生成方式、时间等。
- **R1.3. (meta)data meet domain-relevant community standards****￼**数据必须符合领域内普遍认可的标准，如基因组、气候建模等社区标准。
 
**3.4 Practicalities****（****FAIR** **实践要点）****￼**

- 数据 catalogues 应便于实现 F（Findable）。
- ==应使用唯一== ==PIDs====（多为== ==HTTP identifiers====）。==
- 即使数据受限，元数据也应始终公开。
    - 受限原因可为隐私（与 TRE lecture 有关）、知识产权等。
- 存储库（repository）可包含：
    - metadata
    - data objects
    - documentation（补充文件）
- ==可使用== ==content negotiation== ==提供不同格式的表示==
    - 有利于 F（查找）与 A（获取）。
 
**3.5 Additional Guidelines****（额外指导原则）****￼**

- 满足最低 FAIR 要求的门槛应保持较低，方便社区采用。
- FAIR 定义了现代数据资源、工具、词汇表与基础设施应具备的特性，以助第三方发现与再利用数据。
- FAIR 是敏捷的（agile）：可随元数据完善、工具改进而更新。
- 应优先支持开放技术，而不是专有解析器或特定领域的封闭工具。
- FAIR 数据几乎总是需要机器可读的 metadata。
- GO-FAIR 国际机构为研究者提供培训与支持。
- 根据 Scheffler 等研究，FAIR 原则对数据挖掘与人工智能至关重要（doi:10.1038/s41586-022-04501-x）。
 
**3.6 Examples****（****FAIR** **原则实例）****￼**

![Exported image](Exported%20image%2020260730191338-8.png)

- PIDs（HTTP identifiers、Handle）
- metadata
- federated search tools / catalogues

- 支持多格式访问
- machine-aware 与 human-aware 区分
- canonical URLs

- controlled vocabularies
- machine-readable formats
- 丰富的 metadata

- 可下载的数据与补充资源
- 指向标准（standards）的链接
- 清晰 licence 信息
 
**3.7 Acknowledgements****（署名与复用说明）**

- © 2014–2025 The University of Edinburgh and contributors
- 采用 CC-BY 4.0 授权，可以复用
- 原内容由 Adam Carter（EPCC）制作，Chris Wood 在 2022/23 做重大更新
- PID 相关内容来自 EUDAT 项目，贡献者包括 Tobias Weigel（DKNZ）、Larry Lannom（CNRI）
 
- 一页纸 Cheat Sheet
- 总结性期末复习笔记
- 知识点思维结构图
- 中英双语精简版

告诉我即可继续。
 \> 来自 \<[https://chatgpt.com/c/69322968-6328-8330-b9c3-ea29e7ec29e5](https://chatgpt.com/c/69322968-6328-8330-b9c3-ea29e7ec29e5)\>      

**原则：**
 
**原则：**
 
**原则：**
 
**原则：**
 ![Exported image](Exported%20image%2020260730191339-9.png)  
![Exported image](Exported%20image%2020260730191343-10.png)  

**Findable****：**  
**Accessible****：**  
**Interoperable****：**  
**Reusable****：**
 
课程材料版权：
 
如果你希望，我可以继续为 FAIR + PID 整理成：
 \> 来自 \<[https://chatgpt.com/c/69322968-6328-8330-b9c3-ea29e7ec29e5](https://chatgpt.com/c/69322968-6328-8330-b9c3-ea29e7ec29e5)\>

![[Lec10 Persistent Identifiers & FAIR Data - Ink.svg]]

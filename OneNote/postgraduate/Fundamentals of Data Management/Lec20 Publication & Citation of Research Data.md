**Publication & Citation of Research Data**  
研究数据的发布与引用  
Fundamentals of Data Management  
Lecturer: Chris Wood
 
**1. Data Publication** **数据发布**  
**1.1** **什么是发布（****publish****）数据？**  
发布数据通常意味着：  
• 让数据对他人可用（available for others to use）  
• 通常公开开放，但也可能设置访问控制（access controls）  
• 将数据长期托管在第三方平台（institutional or domain repository）  
• 数据不可变（immutable）  
• 数据可引用（citable），通常使用 Persistent Identifier（持久标识符，如 DOI）  
• 数据可能会经过同行评审，但不像期刊论文那样常见  
**要点：数据发布的核心目标是确保可获取性、长期保存性、可引用性、可重用性。**
 
**2. Where Can I Publish Data?** **数据可以发布在哪里？**  
数据发布平台大致分为四类：  
**2.1 Institutional Repository****（机构级数据仓库）**  
由大学或研究机构运营，适合存放本机构研究者产生的数据。  
**2.2 Interdisciplinary Repository****（跨学科仓库）**  
面向广泛领域的数据，适用于多学科研究项目。
 
**2.3 Alongside a Standard Journal Publication**  
作为期刊论文的 supplementary material（补充材料）  
• 常见于科学论文，需要提供额外数据文件供验证与复用  
• 但补充材料的位置可能并非长期稳定  
**2.4 Data Journals****（数据期刊）**  
专门发表数据集与数据描述文章（Data Descriptor）。  
例如：  
• Scientific Data（Nature 出版）  
• Geoscience Data Journal  
这些期刊通常要求数据：  
• 存放于认证的数据中心  
• 符合 FAIR 原则  
• 有明确的元数据与数据描述文档
 
**2.5 Subject Area Repositories****（学科领域数据仓库）**  
面向特定研究领域，通常具备更严格的标准与长期维护能力。  
例如 NERC 体系的数据中心：  
• British Oceanographic Data Centre (BODC)  
• Centre for Environmental Data Analysis (CEDA/BADC)  
• NERC Earth Observation Data Centre  
• UK Solar System Data Centre  
• Environmental Information Data Centre  
• National Geoscience Data Centre  
• Polar Data Centre  
此外还有与 ERICs（European Research Infrastructure Consortium）相关的各类仓库。  
这些仓库提供领域特定的标准、元数据结构与长期保存链路。
 
**3. Geoscience Data Journal: Approved Data Centres**  
地球科学数据期刊认证的数据中心（部分列表）  
以下是被 Geoscience Data Journal 认可的数据仓库（能够托管可发表的数据集）：  
• 3TU.Datacentrum  
• British Atmospheric Data Centre (BADC/CEDA)  
• British Oceanographic Data Centre (BODC)  
• CISL Research Data Archive  
• CSIRO Data Access Portal  
• EIDC（Environmental Information Data Centre）  
• Figshare  
• IEDA: EarthChem  
• IEDA: MGDS  
• NCAR（USA）  
• EOL（Earth Observing Lab）  
• RDA（Research Data Archive for weather/climate）  
• NGDC（National Geoscience Data Centre）  
• NEODC（Earth Observation Data Centre）  
• NOAA NCDC / NODC / NGDC  
• PANGAEA  
• Zenodo  
• Polar Data Centre（PDC）  
（此列表仅为部分，具体以期刊官方要求为准）
 
**4. Common Repository Software** **常见数据仓库软件**  
这些软件支持机构搭建自己的数据发布平台：  
**DSpace**  
[http://www.dspace.org](http://www.dspace.org/)  
• 全球使用最广的机构仓库系统之一  
**Fedora**  
[http://www.fedora-commons.org](http://www.fedora-commons.org/)  
• 灵活的数字内容管理框架  
**Invenio**  
[http://invenio-software.org](http://invenio-software.org/)  
• 最初用于文档管理，后发展为通用数字仓储平台  
• CERN 等机构广泛采用
 
下面继续为你整理这部分课堂笔记，保持与你前面整理内容完全一致的格式：中文为主、中英双语、结构清晰、无特殊符号、适合复习和考试。
 
**Common Repository Software** **常见数据仓库软件（提醒）**  
这些软件框架用于构建机构级或学科级数据仓库：  
**1. DSpace**  
[http://www.dspace.org](http://www.dspace.org/)  
• 全球应用最广泛的机构存储系统之一  
• 支持长期保存与公开发布  
**2. Fedora**  
[http://www.fedora-commons.org](http://www.fedora-commons.org/)  
• 强大的数字内容管理框架  
• 灵活，可构建复杂的数字资源库  
**3. Invenio**  
[http://invenio-software.org](http://invenio-software.org/)  
• CERN 开发，功能完善  
• 最初专注文档管理，现已发展为综合性仓储平台
 
**Mandatory Publication** **强制性数据发布要求（部分示例）**  
不同研究资助机构对数据发布有不同政策：  
**数据提交时限**  
• ESRC（经济与社会研究委员会）  
AHRC（艺术与人文研究委员会，用于考古数据）  
→ 要求在项目结束后 3 个月内提供数据（offer of data）  
• NERC（自然环境研究委员会）  
→ 要求数据在采集后尽快存储（甚至早于项目结束）  
**数据保存期限（****preservation periods****）**  
• AHRC：3 年  
• BBSRC、MRC、Wellcome Trust：10 年  
• EPSRC：自最后一次数据请求起计 10 年  
• STFC：至少 10 年，无法重新测量的数据应尽可能无限期保存  
来源：[www.dcc.ac.uk](http://www.dcc.ac.uk/)
 
**FAIR Data** **原则（再次提醒）**  
FAIR 已成为数据发布的主流标准：  
参考 [www.go-fair.org](http://www.go-fair.org/)  
数据应满足：  
**F — Findable****（可发现）**  
• 使用持久标识符  
• 有丰富的元数据  
**A — Accessible****（可获取）**  
• 使用标准化协议  
• 需要权限的情况下仍保证元数据可访问  
**I — Interoperable****（可互操作）**  
• 使用可共享、标准化的知识表示语言  
• 使用受 FAIR 原则约束的词汇表  
**R — Reusable****（可复用）**  
• 富元数据描述  
• 明确许可
 
**Data Citation** **数据引用**  
**1. Data in the Life Cycle** **数据生命周期中的引用位置**  
数据生命周期通常包括：  
Collect → Assure → Describe → Preserve → Discover → Integrate → Analyze  
数据引用贯穿整个生命周期，尤其是 Discover 与 Analyze 阶段。
 
**Definitions** **概念与定义**  
**1. Data Citation** **数据引用**  
• “The practice of providing a reference to data in the same way as researchers cite printed resources.”  
即：像引用论文一样引用数据。  
• 数据引用是将数据视为正式研究成果（primary research output）而非副产品的关键步骤。  
**2. Data Author** **数据作者**  
• “Individuals generating digital data that are later deposited in a data collection.”  
任何创建并提交数据集的研究者即为数据作者。  
**3. Persistent Identifier****（****PI****）持久标识符**  
例如 DOI。  
• 唯一、可解析、长期稳定  
• 可指向最新版本，或指向可获取指定版本的元数据页面
 
**Benefits of Data Citation** **数据引用的好处**  
**1. Short-term Benefits** **短期益处**  
• 更容易发现数据与论文之间的关系  
• 便于验证与复用已有研究  
• 为数据作者带来学术信用（credit）  
• 使用数据的研究者可追踪方法与上下文  
• 可以统计数据集的影响力（citation count）  
• 有助于发现使用同一数据集的其他研究成果  
**2. Long-term Benefits** **长期益处**  
• 利用出版基础设施使数据更长期可用  
• 引用数据可减少窃取或未署名使用的风险  
• 提高数据发现效率  
• 可通过数据引用衡量数据贡献者的影响力  
• 鼓励更多研究者发布高质量数据  
• 提升科学透明性  
• 加快科研进程
 
**Data Citation Principles** **引用原则（****Force11****）**  
参考： [https://www.force11.org/datacitation](https://www.force11.org/datacitation)  
基本原则包括：  
• Importance（重要性）  
• Credit and Attribution（署名与信用）  
• Evidence（作为研究证据）  
• Unique Identification（唯一标识）  
• Access（可获取性）  
• Persistence（持久性）  
• Specificity and Verifiability（特定性与可验证性）  
• Interoperability and Flexibility（互操作性与灵活性）  
这些原则定义了引用的目的、功能与属性。
 
**Collaborative Effort** **数据引用的协作生态**  
数据引用需要多个角色共同协作：  
• Journal publishers（期刊出版社）  
• Data publishers / repositories（数据仓储）  
• Data authors（数据作者）  
• Data managers（数据管理者）  
• Data users（数据使用者）  
• Professional organizations（专业组织）
 
**Collaborating with Publishers** **与出版方合作**  
数据作者与数据管理者应当：

1. 选择合适的数据仓库发布数据
2. 使用通用的标准与元数据格式
3. 将数据整理到可发布质量
4. 从数据仓库获取正式 citation 信息，并在论文中正确引用
5. 在论文中引用所有使用过的数据集
6. 通知数据仓库相关论文已经发表

这一流程确保数据与论文在学术生态中同步标准化管理。  
下面继续为你整理本节内容，保持与你前面笔记相同的格式：中文为主、中英双语、条理清晰、无特殊符号，适合复习或纳入总结笔记。
 
**Collaborating with Publishers**  
与出版方协作（数据中心、仓储、期刊的职责）  
**1. Role of Data Centers / Distributors**  
数据中心或数据发布机构应当：  
**1.1** **保持数据与元数据的长期稳定**  
• 数据与元数据应保持静态、不随时间变化  
• 制定数字保存策略（Digital preservation policy）  
• 制定版本策略（Versioning strategy）以确保可追踪性  
**1.2** **提供持久标识符（****Persistent and Unique Identifiers****）**  
• 例如 DOI、Handle、ARK  
• 便于发现（discoverability）与引用（citation）  
**1.3** **确保已发布数据长期可访问（****Accessibility****）**  
• 对敏感（sensitive）、商业（commercial）、或过时（obsolete）数据可实行限制访问（restricted access）  
**1.4** **提供引用信息**  
• 为作者提供正式 citation 字段（author、title、year、identifier 等）  
• 将与数据集相关的出版物（papers）链接加入元数据
 
**2. Role of Journal Publishers**  
期刊出版社应当：  
**2.1** **提供清晰的数据引用指南**  
• 告诉作者应如何、在哪里引用数据  
• 指定推荐仓库时，应尊重资助机构的要求（funders’ requirements）  
**2.2** **在论文发表时通知数据仓库**  
• 若论文引用某个数据集，期刊应通知数据仓储机构，以保证数据与文章的关联性
 
**How to Cite Data**  
如何引用数据  
数据引用与引用论文或书籍类似，需要提供足够信息以识别与定位数据。  
**目前尚无统一的国际数据引用格式**  
• 应遵从：

- 所投稿期刊的标准
- 使用的数据仓库的引用格式（如 DataCite、Dryad）
- 学科专业组织的引用规范
 
**Information Needed in a Data Citation**  
数据引用中通常需要的信息  
以下字段根据仓库与学科要求可能有所不同。  
**1.** **基本引用信息**  
• Author / Principal Investigator / Data Creator  
• Release Date 或 Publication Year（完整数据集的发布年份）  
• Title of Data Source（数据集正式标题）  
• Version / Edition Number（使用的数据版本）  
• Format of the Data（数据格式，例如 NetCDF、CSV、TIFF）  
**2.** **数据来源与仓储信息**  
• 3rd Party Data Producer（若数据来自第三方仓库）  
• Archive / Distributor（托管该数据的仓库名称与组织）  
**3.** **标识、访问与上下文信息**  
• Locator or Identifier（如 DOI、Handle、ARK）  
• Access Date and Time（在线访问日期）  
• Subset of Data Used（若只使用部分数据需注明）  
• Editor / Contributor（整理或增值处理数据的人）  
• Publication Place（数据发布机构的地点）  
• Data within a Larger Work（若数据是出版物的附录或数据补充）
 
**Examples of Data Citation Formats**  
常用引用格式示例  
**1. DataCite** **格式**  
Creator (Publication Year): Title. Publisher. Identifier  
示例：  
Smith J. (2020): UK Coastal Climate Observations 1990–2020. British Oceanographic Data Centre. [https://doi.org/10.xxxx/yyyyy](https://doi.org/10.xxxx/yyyyy)  
**2. Dryad** **格式**  
Author (Date of Article Publication). Data from: Article name. Dryad Digital Repository. doi:DOI number  
示例：  
Li L. (2023). Data from: Population structure of migratory birds. Dryad Digital Repository. [https://doi.org/10.xxxx/yyyy](https://doi.org/10.xxxx/yyyy)
 
**Conclusions** **总结**  
• 可发布数据的平台越来越多，包括机构库、跨学科库、学科专属库、数据期刊等  
• 遵循所在学科的良好实践（good practice）至关重要  
• 原则：将数据放在你自己也会去寻找它的地方  
• 数据发布越来越成为强制要求  
• 数据引用对数据生产者与使用者都带来直接与长期收益  
• 数据作为正式研究产出，引用是关键的一步
 
**Acknowledgements & References**  
（课堂引用来源）  
主要内容取自 DataONE Education Module Lecture 9: Data Citation  
[http://www.dataone.org/sites/all/documents/L09_DataCitation.pptx](http://www.dataone.org/sites/all/documents/L09_DataCitation.pptx)  
其他重要参考：

1. ANDS, Data Citation Awareness Guide
2. National Science Board (2005), Long-Lived Digital Data Collections
 
如果你愿意，我可以为你将整套“数据发布与数据引用”内容整理成：  
• 一页 A4 考试速记版  
• 结构图、思维导图、流程图  
• 可放进 coursework 的正式学术笔记  
• 中英双语概览（快速复习）  
告诉我你希望的版本即可。
 \> 来自 \<[https://chatgpt.com/c/6932ee6c-9fa4-8330-954c-05cfe560b5e5](https://chatgpt.com/c/6932ee6c-9fa4-8330-954c-05cfe560b5e5)\>
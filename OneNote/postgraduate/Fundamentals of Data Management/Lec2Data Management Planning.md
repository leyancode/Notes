**Data Lifecycle and Data Management Planning (DMP)**  
**课程主题**：数据生命周期与数据管理计划  
**讲师**：Mr. Kostas Kavoussanakis, Group Manager, EPCC  
**参考资料**：DataONE Education Module: Data Management Planning (2025)  
[https://dataoneorg.github.io/Education/lessons/03_planning/slides.html](https://dataoneorg.github.io/Education/lessons/03_planning/slides.html)
 
**Lecture Outline** **（课程提纲）**

- 什么是 **data lifecycle**（数据生命周期）？
- ![Exported image](Exported%20image%2020260730190903-0.png)
- 为什么这是一个有用的概念？
- 什么是 **data management planning (DMP)**？
- 它的用途是什么？
- 它如何融入数据生命周期？
- 学完本课，你应该能够：
    - 描述数据生命周期及其在研究与商业中的应用
    - 理解准备 **DMP** 的重要性
    - 识别 DMP 的关键组成部分
 
**What do we mean by Data Lifecycle?** **（什么是数据生命周期）**

![Exported image](Exported%20image%2020260730190904-1.png)

- 一种思维方式：关注 **数据、数字对象、文件** 从 **创建到存储（或删除）** 的不同阶段
- 在不同阶段：
    - 数据的使用方式或被看待的方式可能不同
    - 需要相应的管理策略
- 某些阶段涉及 **责任转移**：
    - 与更广泛的群体共享数据集
    - 向数据存储库交付“最终”数据
    - 将数据集作为论文的补充材料发表
- **数据管理** 已成为研究、制造、服务型企业的常见环节
 
**Research Data Management** **（科研数据管理）**

- 数据生命周期与典型的研究/实验流程高度契合：
    - **Create / Measure**（创建或测量数据）
    - **Analyse / Study**（分析与研究）
    - **File / Store**（存档，供下次使用）
- 有计划且有效的 DM 已成为现代科研的特征
    - **Funding agencies** 要求 DMP
    - **Open Data movement**：公共资金支持的数据应公开可用
    - 因此 → 必须提前规划！
 
**Product Data Management** **（产品数据管理）**

![Exported image](Exported%20image%2020260730190906-2.png)

- 在制造业/生产中，大部分产品信息以 **数字形式** 生成：
    - CAD 二维/三维设计图
    - 软件规格说明
    - 相关文档
- 这些信息适合：
    - **Version control****（版本控制）**
    - **Configuration management****（配置管理）**
- **安全关键行业**（如航空、航海）有法规要求，需保存图纸与文档数十年
 
**Test-Data Management** **（测试数据管理）**

- 在数据驱动的服务行业（如 **零售分析** **Retail Analytics**）：
    - 用于训练或开发分类器的 **训练数据****/****采样数据** 也需要管理
- **重要性在于数据溯源** **(Provenance)**：
    - 为什么做出某个决策？
    - 基于什么数据？在哪里？能否证明？
- **案例：英国** **COVID-19** **数据错误** **(2020)**
    - 因 **Excel** **文件达到最大容量** → 近 16,000 个确诊病例未被纳入追踪系统
    - 结果：决策需要重新考虑
    - 来源： [https://www.theverge.com/2020/10/5/21502141/uk-missing-coronavirus-cases-excel-spreadsheet-error](https://www.theverge.com/2020/10/5/21502141/uk-missing-coronavirus-cases-excel-spreadsheet-error)
 
**Data Lifecycle Models** **（数据生命周期模型）**  
**1. Digital Curation Centre (DCC)** **模型**

![Exported image](Exported%20image%2020260730190907-3.png)

- Plan
- Collect
- Assure
- Describe
- Preserve
- Discover
- Integrate
- Analyze

**2. DataONE** **模型（****Hybrid****）**

![Exported image](Exported%20image%2020260730190907-4.png)

- Plan
- Acquire / Collect
- Assure
- Describe
- Preserve
- Discover
- Combine
- Process
- Integrate
- Analyze

我帮你把这一部分的 **Data Lifecycle Components & Data Management Planning (DMP)** 笔记整理成清晰的结构，保留英文关键词，并加上中英文解释，方便复习。
 
**Data Lifecycle Components** **（数据生命周期组成部分）**  
**1. Acquire****（获取****/****采集）**

- 来源：创建 (create)、观测 (observe)、测量 (measure)、仿真生成 (simulation)、写作、复用 (re-use) 现有数据（数据库/Internet）。
- 属于：**Raw Data****（原始数据）**，相当于“实验室记录本”或“核心知识产权 (core IP)”
- 关键点：从一开始就要有序组织 → **选择标准格式**
 
**2. Assure****（保证****/****验证）**

- 验证 (validate)、校准 (calibrate)、测试 (test)
- 检查生成数据的方法是否正确
- 仿真场景 → 正确测试代码
- 检查子集选择方法或合成数据生成器是否正确
- 记录所用的校准方法
- 如果原始数据需校正仪器偏差，必须记录校正步骤
 
**3. Describe****（描述）**

- 示例：避免使用 **SAM1, SAM2**，而是用有意义的变量名
- 记录单位（米、毫米、秒差距？）
- 记录数据解释所需信息 → 1年、10年、100年后仍可理解
- **使用** **metadata standards****（元数据标准）！**
 
**4. Preserve****（长期保存）**

- 当数据进入此阶段 → 成为 **科学记录** **(scientific record)** 或 **企业审计轨迹** **(audit trail)** 的一部分
- 存储时需考虑：
    - 完整性 (integrity)
    - 备份 (backup)
    - 冗余复制 (replication)
    - 未来可访问性（10年后还能读？软件工具会不会过时？）
    - 数据与元数据需保持在一起
 
**5. Discover****（发现）**

- 如果数据不可发现，相当于不存在
- 关键：**可描述性** **(description)** 与 **可访问性** **(accessibility)**
- 问题：
    - 如何让他人发现并使用我的数据？
    - 如何找到其他研究者的数据？
 
**6. Combine****（结合****/****整合）**

- 整合 (integrating)、合并 (merging) 数据 → 产生新的洞见
- 依赖：良好的元数据 + 合适的工具
- 注意：**数据使用许可** **(licensing conditions)**
 
**7. Process****（处理）**

- 使用计算能力从旧数据生成新数据
- 示例：
    - Data Analytics
    - Digital sensor data analysis
    - Simulation input
    - Re-analysis of integrated third-party data
 
**Data Management Planning (DMP)**

![Exported image](Exported%20image%2020260730190908-5.png)

**1.** **定义**

- DMP = 一个正式文档，记录整个数据生命周期中的管理方法
- 包含：
    - 我会生成什么数据？
    - 我如何描述它们？
    - 我如何存储？
    - 我是否会发表/共享？如果不，共享障碍是什么？
    - 他人如何发现它们？
 
**2.** **作用**

- 规划整个生命周期的起点
- 确保数据在研究过程中与研究完成后都**安全**
 
**3. Why prepare a DMP?****（为什么准备** **DMP****？）**

- **节省时间** → 避免后期重组
- **提升科研效率** → 数据更容易保存
- 可能带来**新发现**（未预料到的）
- 确保未来你和他人都能理解与使用数据
- 避免重复工作
- **强制性要求**：研究资助机构通常要求在申请中提供 DMP
 
📌 总结：  
数据生命周期 = **Plan → Acquire → Assure → Describe → Preserve → Discover → Combine → Process**  
而 **DMP** **是起点**，规定了在整个生命周期中如何管理数据。  
很好，这一部分讲的是 **General DMP (Data Management Plan)** **的组成部分**。我帮你整理成清晰的分点笔记，并保持关键英文原文。
 
**Components of a General DMP** **（通用数据管理计划的组成部分）**  
**主要组成**：

1. Information about data & data formats（数据与数据格式信息）
2. Metadata content and format（元数据内容与格式）
3. Policies for access, sharing and re-use（访问、共享与再利用的政策）
4. Long-term storage and data management（长期存储与管理）
5. Budget（预算）
 
**1. Information about Data & Data Formats**

![Exported image](Exported%20image%2020260730190909-6.png)

**1.1 Description of data to be produced** **（将产生的数据描述）**

- 类型：Experimental, Observational, Simulated?
- 原始 (raw) 还是衍生 (derived)?
- 实体收藏 (physical collections) 或纯数字 (digital)?
- 数据形式：模型及其输出、仿真结果、文档、电子表格、软件、图像等

![Exported image](Exported%20image%2020260730190913-7.png)

**1.2 How data will be created or acquired** **（数据如何创建****/****获取）**

- 何时？（When）
- 何地？（Where）
- 通过何种手段？（By what means: instruments, acquisition, simulation）
 
**1.3 How data will be processed** **（数据如何处理）**

- 使用的软件、算法、工作流
- 能否在 **5****年后复现** 分析或再现数据产品？
- 科学复现性 (Scientific reproducibility)
- 商业法规要求 (Business regulation)

![Exported image](Exported%20image%2020260730190914-8.png)

**1.4 File or database formats** **（文件或数据库格式）**

- 格式选择与理由
- 文件命名规则 (naming conventions)
- 数据库 schema 的选择
 
**1.5 Quality assurance & control** **（质量保证与控制）**

- 数据清洗方法
- 缺失值/部分记录处理
- 采样与子集选择策略
- 是否涉及合成数据 (synthetic data)

![Exported image](Exported%20image%2020260730190915-9.png)

**1.6 Existing data** **（现有数据）**

- 来源 (origins)？
- 是否与新数据结合？
- 新数据与已有数据的关系？
 
**1.7 Short-term data management** **（短期管理）**

- 配置管理与版本控制（provenance & reproducibility）
- 数据备份 (backup)
- 数据安全与保护 (security & protection)
- 是否包含敏感数据 (sensitive data)？
- 谁负责？（责任人）
 
**2. Metadata Content & Format** **（元数据内容与格式）**

![Exported image](Exported%20image%2020260730190916-10.png)

**定义**：Metadata = Data about data

- 用于文档化 (documentation) 与数据报告
- 提供数据集的上下文 (context)
- 描述信息：时间与空间、仪器、参数、单位、文件等

**2.1 What metadata are needed**

- 任何能使数据有意义的细节

**2.2 How metadata will be created/captured**

- 实验记录本 (lab notebooks, electronic or otherwise)
- 仿真参数
- 仪器自动保存

**2.3 Metadata format**

- 使用的社区标准 (community standards)
- 格式选择理由
 
**3. Policies for Access, Sharing & Re-use**

![Exported image](Exported%20image%2020260730190917-11.png)

**3.1 Obligations** **（义务）**

- Funding agency 要求
- Institution / Business 政策
- 法律或监管要求

**3.2 Data sharing details**

- 多久共享？（How long?）
- 何时共享？（When?）
- 如何访问？（How?）
- 数据创建者/收集者的权利

**3.3 Ethical & Privacy Issues**

- 伦理与隐私问题

![Exported image](Exported%20image%2020260730190918-12.png)

**3.4 Intellectual Property & Copyright** **（知识产权与版权）**

- 谁拥有版权？
- 机构政策、资助机构政策
- 政治或商业原因下的 embargo（延迟公开）

**3.5 Intended future uses/users** **（预期的未来用途****/****用户）**  
**3.6 Citation** **（引用）**

- 数据应如何引用？
- 是否需要持久化引用 (persistent citation)?
 
**4. Long-Term Storage & Data Management**

![Exported image](Exported%20image%2020260730190919-13.png)

**4.1 Preservation** **（保存哪些数据）**  
**4.2 Archiving** **（存档位置）**

- 最合适的存档位置
- 是否符合社区标准

**4.3 Transformations & Formats** **（数据转化与格式要求）**

- 符合存档政策

**4.4 Responsibility** **（责任人）**

- 存档的联系人
 
**5. Budget** **（预算）**

![Exported image](Exported%20image%2020260730190923-14.png)

**5.1 Anticipated costs**

- 数据准备与文档化的时间
- 所需硬件/软件
- 人员成本
- 存档费用

**5.2 Cost coverage**

- 一次性支付 (up front)?
- 持续性支付 (over time)?
 
📌 总结：  
一个合格的 DMP 涉及 **数据本身（****formats, QA, provenance****）****→** **元数据（****metadata standards****）****→** **政策（共享、版权、伦理）****→** **存储（短期****+****长期）****→** **预算** 五大方面。  
好的，这一部分是 **DMP** **实例** **+** **总结**，我帮你整理成清晰的双语笔记，保留关键英文术语，方便直接作为学习/复习材料。
 
**Example 1: Retail Analytics** **（零售分析）**  
**问题**：  
“Using the customer database for the last 5 years, can we predict which customers redeem their mortgage early?”  
（利用过去 5 年的客户数据库，能否预测哪些客户会提前还清房贷？）

![Exported image](Exported%20image%2020260730190924-15.png)

**1. Information about data & formats** **（数据与格式）**

- 使用的 “customer database”：是单一数据库？实时数据库？子集？合成数据 (synthetic)?
- 如何采样、训练、测试分类器？
- 如何保证 **repeatable / reproducible**？
- 输出如何记录？报告？模型？

**2. Metadata** **（元数据）**

- 我们是否理解所有 schema 字段的含义？
- 如果有多个来源，字段是否匹配？

**3. Policies** **（访问****/****共享****/****再利用政策）**

- 是否有法规或审计要求？

**4. Long-term storage** **（长期存储）**

- 是否需要保存样本或合成数据集？保存在哪里？安全性如何？

**5. Budget** **（预算）**

- 人员时间？计算需求？本地？外部云？
 
**Example 2: Wing Airflow Simulation** **（机翼气流仿真）**

![Exported image](Exported%20image%2020260730190925-16.png)

**问题**：  
“Explore optimal design parameters of a proposed new wing using our in-house simulation code Flite2016”  
（使用内部仿真代码 Flite2016 探索新机翼的最优设计参数）  
**1. Information about data & formats**

- 输入：CAD 图纸 → FE 网格生成 → 网格作为仿真输入
- 格式：CAD formats? mesh formats? partitioning formats?
- 输出：Simulation outputs → NetCDF? HDF5? 字段内容？

**2. Metadata**

- 如何记录仿真参数以保证可复现？
- 是否将元数据存储在输出文件中？包括单位？

**3. Policies**

- 是否为迭代过程？中间文件如何存储与共享？
- 是否需要与其他部门/学术伙伴共享？
- 是否需要可视化/进一步分析？

**4. Long-term storage**

- 哪些内容需要为 **regulatory approval****（法规审批）** 保存？
    - CAD 图纸、参数元数据、软件、实际数据集？

**5. Budget**

- 人员时间、计算资源、存储空间
 
**Tools for Creating DMP** **（****DMP** **工具）**

![Exported image](Exported%20image%2020260730190926-17.png)

- **DMPTool**: [dmptool.org](https://dmptool.org/)
- **DMPonline**: [dmponline.dcc.ac.uk](https://dmponline.dcc.ac.uk/)
 
**Summary** **（总结）**

- **Data Lifecycle** 是一种有用的思维方式：数据从创建 → 使用 → 存档（或销毁）
- 适用于 **科研数据** **&** **商业数据**
- 提前规划 DMP 可以避免后期问题，提升效率
- **DMP** **不必复杂**：
    - 只需 2–10 页的实用文档
    - 工具（如 DMPonline）会一步步指导
- 关键是 **实施**：简单、实用才有价值
- 数据管理与 DMP 的核心目标：**支持** **21** **世纪的可复现** **(reproducible)** **分析与研究**
 
**Acknowledgements**

- 部分内容来源于：
    - DataONE Education Module: _Data Management Planning_ (Retrieved Sep 2025)
    - [https://dataoneorg.github.io/Education/lessons/03_planning/slides.html](https://dataoneorg.github.io/Education/lessons/03_planning/slides.html)
 
要不要我帮你做一个 **对比表格：****Retail Analytics vs Wing Simulation** **的** **DMP** **设计**（每一列是 _Data, Metadata, Policy, Storage, Budget_），这样可以清晰展示两个不同领域的应用差异？
 \> 来自 \<[https://chatgpt.com/c/68cf18f9-fc74-832a-99a4-edc55f45bb22](https://chatgpt.com/c/68cf18f9-fc74-832a-99a4-edc55f45bb22)\>

![[Lec2Data Management Planning  - Ink.svg]]

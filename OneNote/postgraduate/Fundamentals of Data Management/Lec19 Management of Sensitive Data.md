下面为你将课堂内容整理成清晰、结构化的课堂笔记格式（中文为主，中英双语保留关键术语），便于复习和引用。未加入任何特殊符号。
 
**Management of Sensitive Data**  
Fundamentals of Data Management  
Lecturer: Kostas Kavoussanakis（Group Manager, EPCC）  
Email: [k.kavoussanakis@epcc.ed.ac.uk](mailto:k.kavoussanakis@epcc.ed.ac.uk)
 
**1. Lecture Outline** **课程概要**  
本节课主要回答以下问题：  
• 处理敏感数据需要了解什么  
• 什么是 information governance（信息治理）  
• 什么是 safe haven（安全隔离环境）  
• 为什么作为科学家也必须关心法律问题  
完成本课程后你应能够：  
• 理解敏感数据（尤其是 personal data 个人数据）在研究中的风险与处理要求  
• 理解 Five Safes 模型  
• 理解如何构建安全的数据访问与分析环境
 
**2. Sensitive Data** **敏感数据的类别**  
敏感数据（Sensitive data）可能具有多种性质：

1. **Ethically sensitive****（伦理敏感）****￼**例如：敏感位置数据。
2. **Commercially sensitive****（商业敏感）****￼**例如：市场价格数据、商业机密、知识产权。
3. **Personally sensitive****（个人敏感）****￼**例如：医疗记录、健康信息。

这些数据可能受到额外的法律与监管要求，例如金融数据受金融监管。  
本课程重点关注 **personal data****（个人数据）**。
 
**3. Personal Data** **个人数据**  
定义（UK GDPR Article 4）：  
**Personal data means any information relating to an identified or identifiable natural person.**  
即：数据不仅有创造者，还有数据主体（data subject）。  
Special categories of personal data（敏感类别）包括：  
• 种族  
• 宗教  
• 健康状况  
• 政治观点  
• 性生活  
• 犯罪记录  
研究中的典型个人数据：  
• 医疗记录、医学影像、访谈内容、语音样本、姓名、住址等。
 
**4. Confidentiality in the UK** **英国的保密义务**  
在英国，personal data 首先受到 **duty of confidentiality****（保密义务）** 保护。  
• 英格兰和威尔士：由普通法（Common Law）规定  
• 苏格兰、北爱有不同解释方式  
举例：  
患者向医生提供数据用于医疗，如果医生未经必要理由分享这些数据，即违反保密义务。  
核心问题：**Would the data subject be surprised?**  
如果是，那么数据使用违反保密性。
 
**5. The GDPR** **通用数据保护条例**  
欧洲范围：GDPR（General Data Protection Regulation, 2016）。  
英国：  
• GDPR 被写入 _Data Protection Act 2018_  
• 脱欧后形成 UK GDPR（内容高度相似）  
GDPR 赋予数据主体的核心权利包括：  
• Right to be forgotten（被遗忘权, Art.17）  
• 控制谁可以处理其私人数据（Art.7）  
• 数据可携带性（Art.20）  
• 信息透明（Art.12–14）  
• 数据泄露通知权（Art.33–34）  
• 限制自动化 profiling（Art.21）  
• 儿童数据的特别保护（Art.8）  
• Privacy by design & by default（隐私内建、默认保护）
 
**6. GDPR Principles GDPR** **的处理原则**  
所有个人数据处理活动（processing，包括存储）必须遵守以下原则：  
• Lawfulness, fairness, transparency（合法、公平、透明）  
• Purpose limitation（目的限制）  
• Data minimisation（数据最小化）  
• Accuracy（准确性）  
• Storage limitation（存储时间限制）  
• Integrity and confidentiality（完整性与安全性）  
此外：  
• Data controllers（数据控制者）必须承担直接责任  
• Data processors（数据处理者）需在正式协议下处理数据
 
**7. GDPR Lawfulness** **合法性要求**  
• 数据处理必须具有 **lawful basis****（合法基础）**  
• 如果基于 consent（同意），必须是：  
informed（知情）、specific（具体）、freely given（自由意愿）  
数据主体必须知道：  
• 其权利  
• 数据如何使用  
• 若发生 personal-data breach（数据泄露）必须报告  
GDPR 定义了 special categories of personal data（敏感数据），通常即我们研究中说的 sensitive data。
 
**8. Working with Personal Data** **使用个人数据的关键问题**

1. **Consent****（同意）****￼**• 获得、记录、管理都可能是挑战￼• 若未来研究用途不可预知，是否还能依赖 consent？
2. **Alternative lawful basis****（其他合法基础）****￼**在英国（尤其苏格兰）：￼**Public task****（公共任务）** 可作为研究的数据处理合法基础。
 
**9. Working with Personal Data (2)** **去标识化与安全环境**  
• De-identification（去标识化）、pseudonymisation（假名化）、confidentialisation（保密化）可用于数据再利用  
但：  
• 医学影像是否容易去标识化？  
• 跨数据源 mashup 是否易重新识别？  
因此需要保护的不仅是数据本身，还包括 **数据处理的环境与上下文**：  
• Trusted Research Environments (TREs) 可信研究环境  
• Safe havens 安全隔离区  
任何在英国处理 personal data 的人都受 _Data Protection Act 2018_ 管辖：  
• 身份可能是 Data Controller 或 Data Processor  
• 必须了解责任  
• ignorantia juris non excusat（不知法不免责）。  
下面根据课堂内容为你整理成清晰、结构化的课堂笔记格式（中文为主，中英双语，保留关键英文术语，逻辑清晰、适合考试复习）。  
无任何特殊符号。
 
**Information Governance** **信息治理**  
**1.** **背景与定义**  
数据科学推动了社会科学和医学科学的发展，但数据的利用必须与个人隐私权进行平衡，尤其是在多个数据源进行数据链接（data linking）时，隐私风险显著增加。  
**Information governance（信息治理）**的目标是：  
有效管理组织的信息资产，使其价值最大化，同时将信息相关风险降到最低。  
其中所谓的资产（asset），多指个人数据（personal data）。  
**2.** **信息治理的分层结构（****separation of concerns****）**  
利用职责分离的方式降低风险，使各参与方只看到其职责范围内的数据。  
角色划分包括：  
**Data Controllers****（数据控制者）**  
• 只能看到自己负责的数据  
• 不直接进行跨数据集链接  
**Indexer****（索引者，可信第三方）**  
• 接收各数据控制者的数据集  
• 将所有个人可识别信息（PII）替换为匿名的 ID  
• 创建 Master Index File，用于指示不同数据集中哪些匿名 ID 属于同一人（但不显示真实身份）  
**Linker****（链接者，可信第三方）**  
• 根据 Indexer 提供的 Master Index File，将各去标识化数据进行链接  
• 与 Indexer 和 Data Controllers 相互独立  
**Researchers****（研究人员）**  
• 在安全环境中（如 TRE）使用已经去标识化、链接好的数据集  
• 无权访问任何真实身份信息
 
**Information Governance (2)** **信息治理的风险与挑战**  
**1. Re-identification risk** **再识别风险**  
即使 PII 被移除，当研究数据与公开数据（如 social media）结合后，仍可能推断出某个人的身份。  
典型风险包括：  
• 小样本群体（small cohorts）  
• 复杂或多次查询导致返回结果集中仅有一人  
• 数据 mashup 可能使匿名化失效  
因此在研究环境中，数据一般称为 **de-identified****（去标识化）** 而非 anonymised（匿名化），因为完全匿名通常无法保证。  
**核心假设：**  
• 在合适的组合下  
• 使用合适的工具  
任何人都有可能被重新识别。
 
**The Five Safes Model** **五重安全模型**  
五重安全模型由英国国家统计局（UK Office for National Statistics）于 2003 年提出，用于评估和管理个人数据研究中的风险。  
官网参考： [http://www.fivesafes.org/](http://www.fivesafes.org/)  
Five Safes 包括五个维度：  
**1. Safe Projects**  
数据使用是否适当、合法、伦理且合理  
Is this use of the data appropriate, lawful, ethical and sensible?  
**2. Safe People**  
用户是否可信，是否具备正确的资质和使用规范  
Can the users in question be trusted to use it properly?  
**3. Safe Data**  
数据是否经过适当处理，在满足研究需求的同时不会泄露隐私  
Does the dataset avoid unnecessary disclosure risk?  
**4. Safe Settings**  
使用环境是否安全可靠，是否限制未经授权的访问与使用  
Does the environment limit unauthorised use or mistakes?  
**5. Safe Outputs**  
研究输出是否经过检查，确保不会泄露个人隐私  
Is confidentiality maintained in the released outputs?  
任何研究解决方案都需要在五个维度间平衡风险。
 
**Trusted Research Environments (TREs)** **可信研究环境**  
TREs（亦称 Safe Havens、Secure Data Environments）是安全受控的计算环境，研究人员可在其中访问个人数据，用于公共利益研究。  
定义（SAIL Databank）：  
安全环境，研究者可通过严格审批流程访问高价值数据，从而促进公共利益研究。  
EPCC 运营的 TRE 示例：  
• Scottish National Safe Haven（苏格兰国家安全港）  
• Smart Data Foundry Data Safe Haven  
• 多个私人 Safe Havens  
EPCC 提供的是 **safe setting**，包括部分 safe people（例如系统管理员）。  
其他 “safes” 则由各 Safe Haven 的控制者负责，以保持职责分离。
 
**Example: Information Governance & Data Linking** **信息治理与数据链接示例**  
以下示例展示了 Indexer（索引者）、Linker（链接者）和 Researchers（研究者）之间的数据流：

1. 数据控制者 A/B/C 将含有真实身份信息的数据发送给 Indexer
2. Indexer 删除 PII，用匿名 ID 替换
3. Indexer 生成 Master Index File
4. 控制者 A/B/C 将去标识化数据发送给 Linker
5. Linker 根据 Master Index File 链接数据
6. Researchers 获取最终去标识化、已链接的数据

示例最终结果：  
z1234 对应某一个人，其多来源数据（健康信息、收入、家庭状况等）被整合在同一行中。  
注意：这些数据仍然属于 personal data，因为每一行对应一个真实个体，只是去除了 PII。
 
**Common Features of TREs TRE** **的共同特征**  
TRE 通常采用严格的安全结构：  
**1.** **网络隔离（****network zoning****）**  
常见的 zone 包括：  
• DMZ（demilitarised zone）网络边界管理区  
• Analytics zone（研究分析区，研究人员工作区）  
• Secure data management zone（数据管理与治理区）  
• 可能包含 HPC zone（高性能计算区）  
**2.** **研究人员访问方式**  
• 使用虚拟桌面（virtual desktop）访问  
• 安装标准分析软件、可能包含数据科学环境  
• 无互联网访问  
• 禁止复制、粘贴  
• 禁止数据上传与下载（包括代码与文档）  
数据的进入与结果的输出（ingress & egress）由信息治理团队负责，而不是研究人员。  
**3.** **数据管理人员**  
• 被信任处理数据移动  
• 可能维护数据库、API 等  
• 跨 TRE 数据交换必须受控
 
**Examples of TREs in the UK** **英国典型** **TRE**  
• **Scottish National Safe Haven**  
Public Health Scotland 管理，EPCC 运营  
• **SAIL Databank**  
Public Health Wales 管理，Swansea University 运营  
• **NHS England Secure Data Environment**  
使用公共云部署  
• **OpenSafely**  
由 University of Oxford 开发，通过 TPP 和 EMIS 的 TRE 运行  
• **ONS Secure Research Service**  
Office for National Statistics 管理，部署在公共云  
下面继续为你整理课堂笔记内容，保持统一格式（中文为主，中英双语、保留关键词），结构清晰，适合直接用于复习或整理课程讲义。
 
**Data Science in TREs: Software Safety**  
TRE 环境中的数据科学与软件安全  
**1.** **传统** **TRE** **的软件工具**  
“Traditional” TREs 通常提供标准的第三方统计工具，例如：  
• SAS  
• Stata  
• SPSS  
• R Studio  
这些工具功能有限、可控性强，因此风险较低。  
**2.** **现代** **TRE** **的软件需求变化**  
“Modern” TREs 越来越倾向于支持开放源代码生态，提供：  
• Python  
• Scala  
• Julia  
• 访问大型开源包仓库（runtime access to Internet package repositories）  
这是为了满足现代数据科学和机器学习研究的需求。  
**3.** **新的安全挑战（****Security Challenges****）**  
**3.1** **语言能力差异导致风险增加**  
• Python 是强大的系统级语言，功能远超 R  
• 比喻：Python 是瑞士军刀，而 R 像茶匙  
意味着 Python 有更高的系统访问能力，因此 TRE 需加强控制。  
**3.2** **网络访问限制**  
• TRE 内通常默认禁用互联网  
• 是否可能只开放特定仓库，如 PyPI 或 CRAN？  
• 面临的问题：

- repo poisoning（仓库投毒）
- GitHub 访问是否可控
- 开源包可能包含可执行系统调用导致安全隐患

**3.3 Five Safes** **的强化**  
为了支持更强大的工具，可能需要加强 Five Safes 的其他部分：  
• stronger Safe People（更严格的用户资质审核）  
• stricter Safe Settings（更严格隔离的运行环境）  
• more rigorous Safe Outputs（加强输出审查）  
总体原则仍然是：**风险平衡（****balancing risk****）**。
 
**Multiple Safe Havens** **多个安全港的协同工作**  
**1.** **分散存储（****siloed****）的问题**  
例如：  
• 苏格兰数据存储在 NSH（Edinburgh）  
• 威尔士数据在 SAIL（Swansea）  
• 英格兰数据在 NHS SDE  
• 北爱尔兰可能在各自系统  
如果研究目标需要整个 UK 人群的数据，则必须进行 **safe haven federation****（安全港联邦式协作）**。  
**2. Federation** **思路：****Send Compute to the Data**  
这是 TRE 领域的经典原则：**把计算发送到数据处，而不是把数据发送出来**。  
**简单情况：**  
• 聚合查询（sums, averages）可轻松联邦化  
例如：在各自 TRE 内计算局部平均值，再合并结果。  
**复杂情况：**  
• 机器学习训练  
• 高维查询  
• 随时间演化的模型  
这些情况复杂度急剧增加，需要更多工程与安全策略：  
**关键挑战：**  
• 是否将脚本发送到各 TRE？  
• 是否使用 containerised application（容器化应用）？  
• 信任问题：你会在敏感数据上运行未知容器吗？  
• 各 TRE 中的变量名、数据含义能保证一致吗？  
因此需要更先进的隐私增强技术（Privacy-enhancing technologies, PETs）：  
包括：  
• 安全多方计算（SMPC）  
• 联邦学习（Federated Learning）  
• 同态加密（Homomorphic Encryption）  
这是当前活跃的研究方向。
 
**In Summary** **总结**  
**1.** **研究数据时代与敏感数据处理**  
• 开放研究数据时代已经到来  
• 你很可能会接触个人数据，甚至 special category data  
这些数据受到严格的法律监管，包括：  
• UK Data Protection Act (2018)  
• UK GDPR / GDPR (2016)  
**2.** **你可能参与构建数据安全** **IT** **环境**  
• TRE、Safe Haven、Secure Data Environment 是核心设施  
• 实际工作大部分属于 Information Governance（信息治理）  
信息治理的本质是平衡：  
• 公共利益中的研究收益  
• 与潜在隐私风险  
**3. Five Safes** **是** **TRE** **管理的基础框架**  
作为 TRE 建设和运营的标准起点，Five Safes 帮助从多角度评估与管理风险。
 
如果你需要，我可以进一步：  
• 把全套敏感数据课程整理成总结版（重点+例子+英文术语）  
• 制作一页 A4 Cheat Sheet（考试前速记）  
• 整理为更学术、可放入 coursework 或报告的版本  
• 生成思维导图结构或 TRE 风险模型结构图  
告诉我你希望的格式即可。
 \> 来自 \<[https://chatgpt.com/c/6932ee6c-9fa4-8330-954c-05cfe560b5e5](https://chatgpt.com/c/6932ee6c-9fa4-8330-954c-05cfe560b5e5)\>
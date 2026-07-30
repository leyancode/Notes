以下为按照课堂笔记风格整理的内容，采用清晰分层、重点突出、关键英文术语保留原文，可直接用作你的课程复习资料。
 
Introduction to Blockchain and Hyperledger Fabric  
Fundamentals of Data Management  
Lecture Notes（整理版）
 
1. Blockchain 简介
 
• 最早被提出于论文  
“Bitcoin: A Peer-to-Peer Electronic Cash System”（2008）  
作者：Satoshi Nakamoto  
• Bitcoin Blockchain 是目前最成熟、运行最久的区块链系统  
– 主要用于比特币交易（含 mining）  
• Hyperledger 对区块链的定义：  
– “An operating system for marketplaces, data-sharing networks, micro-currencies, and decentralized digital communities.”  
• 本课程关注 **区块链的商业应用（****blockchain for business****）**  
– 不关注 cryptocurrency￼

1. What is Blockchain?
 
• ==一个== **distributed ledger****（分布式账本）**  
– 作为业务的 system of record  
– 存储 transactions 列表  
– 在网络节点之间共享（shared with network peers）  
• 为什么叫 Blockchain？  
– 数据存储在 blocks 中  
– blocks 之间通过哈希连接形成 chain￼

1. Blockchain for Business
 
• **Shared ledger**  
– 全网共享的 add-only system of record  
– 交易记录不可篡改（immutable）  
• **Permissioned****（权限控制** **/** **隐私）**  
– 控制身份访问与可见性  
– 交易可认证、可验证  
• **Smart Contracts****（智能合约）**  
– 商业逻辑嵌入链上  
– 与交易一起自动执行  
• **Consensus****（共识** **/ Trust****）**  
– 所有参与者认可经过网络验证的交易  
– 不需要 proof-of-work（避免大量算力消耗）￼

1. 如何判断一个好的 Blockchain Use Case
 
一个好的区块链应用场景需要具备：  
• 明确的 business problem  
• 替代技术存在明显不足  
• 可识别的 business network  
– Participants、Assets、Transactions  
• 对 Trust 的需求  
– 例如：Consensus、Immutability、Provenance（可溯源性）

1. Use Case 示例
 
5.1 Open Trusted Supply Chain  
• What?  
– 消费者要求供应链透明度  
– 欧盟加强企业供应链信息披露法规  
• How?  
– 使用 blockchain 安全传输供应链资产信息  
• Benefits:  
– 数据 verifiable、不可被篡改  
– 提升透明度和效率  
– 政府获得可靠数据  
– 消费者可做出知情决策￼  
5.2 Aircraft Maintenance（航空器维护）  
• What?  
– 复杂系统中组件 provenance（来源信息）难以追踪  
• How?  
– 使用 blockchain 记录组件生产与来源  
– 多方可访问：制造商、航空公司、维护单位、监管机构  
• Benefit:  
– 信任提升（无人垄断 provenance）  
– 可进行精确召回（specific recall）￼  
5.3 Other Use Cases  
• 构建新型 distributed internet  
– 分布式 DNS、分布式 CA 等  
– 使用现有网络协议（TCP/IP、UDP/IP）  
• Decentralized Digital Identities（去中心化数字身份）  
– 用户自己拥有身份信息，而非 Facebook/Google  
– 一个身份可连接教育、医疗、驾驶证等记录  
• 示例：Ethereum Name Service￼

1. Things that have gone wrong
 
• Smart contracts 编写错误（programmer errors）  
• Cryptocurrency exchange 被黑  
– MtGox (2014)：全球最大比特币交易所之一  
• 可疑商业实践  
– Quadriga (2019)：£135m 资金丢失事件￼

1. Blockchain Frameworks and Platforms
 
7.1 Public Blockchain  
• Permissionless（无需权限）  
• 任何人可加入或退出  
• 通常使用 proof-of-work、proof-of-stake  
• 常见平台：  
– Ethereum、Cardano、Solana、Polkadot  
• 特定用途：  
– Bitcoin Blockchain（作为 store of value）  
7.2 Private Blockchain  
• 企业使用场景，强调 privacy  
• Permissioned（需授权）  
• 常见实现：  
– Hyperledger Fabric  
– Ethereum（可部署为 private blockchain）  
下面为你把三张幻灯片与后续 Ethereum、Hyperledger 内容按课堂笔记格式整理成流畅、结构清晰、适合复习的版本。重点英文术语全部保留，整体保持专业、简洁、可用于期末复习。
 
**Hyperledger Fabric: A Technical Overview****（技术概览）**

1. Fabric 网络基础结构
 
• **Each organisation adds one or more peer nodes**  
每个组织可以部署一个或多个 peer 节点。  
• **Each peer has its own ledger**  
每个 peer 都维护自己的 ledger（不可篡改 transaction 记录）。  
• 下图示例展示一个简单 Fabric Blockchain 网络  
– 每个组织一个 peer  
– 每个 peer 节点包含：  
S：Smart Contract（Chaincode）  
L：Ledger  
P：Peer Node  
（图示说明略，为课堂 PPT 结构）
 
1. 交易执行流程（Application → Peer → Orderer）
 
• **Application** **通过** **chaincode** **与** **peer** **节点交互**  
– 应用程序负责向 chaincode 发起 query 或 update 请求。  
• **Orderer** **将交易打包为** **blocks** **并广播给所有** **peers**  
– 负责 ordering  
– 确保所有 peers 的 ledger 状态一致（up-to-date）  
典型流程：

1. Application 连接到 peer
2. 调用 chaincode（发送 proposal）
3. Peer 执行 chaincode，返回 proposal response
4. Application 将交易提交给 Orderer
5. Orderer 将交易打包成 block
6. Peers 接收 block 并更新 ledger
 
1. Private Channels（私有通道）
 
• Peer nodes、Orderer nodes、Application 可以加入指定 channel  
• 同一个 channel 内：  
– Ledger 是独立的  
– 交易与通信仅对 channel 内的成员可见（隐私与隔离性）  
• 作用：  
– 在商业网络中，即使有竞争关系，也能在同一 Fabric 中安全协作  
– 通常用于保密交易、监管场景、企业间合作
 
**Ethereum****（以太坊）**

1. 基本介绍
 
• 开源 blockchain platform（2015 发布）  
• 任何组织或个人都可构建 decentralized applications（dapps）  
• 全球最大、最活跃的区块链开发社区之一  
• 具有 **programmable blockchain** 特性  
– 与 Bitcoin 仅支持预定义操作不同  
– Ethereum 支持灵活的 Smart Contracts￼

1. 核心技术：Ethereum Virtual Machine（EVM）
 
• 所有 Ethereum 节点都运行 EVM  
– 执行相同指令以维持全网共识  
• Smart Contracts 主要用 **Solidity** 编写  
• dapps 的前端常用 Python 或 JavaScript  
• Ethereum 使用 peer-to-peer network protocol  
• 当前使用 Proof-of-Work（PoW）  
– 即将切换至 Proof-of-Stake（PoS）
 
**Blockchain Frameworks and Platforms****（区块链平台）**

1. Hyperledger （Linux Foundation 主导）
 
• 面向企业级应用的开源区块链技术协作组织  
• 主要 Framework：  
– **Hyperledger Iroha**（面向移动开发）  
– **Hyperledger Sawtooth**（Intel 提出 PoET 共识）  
– **Hyperledger Burrow**（基于 EVM 的模块化平台）  
– **Hyperledger Indy**（去中心化身份管理）  
– **Hyperledger Fabric**（IBM 主导开发）
 
**Hyperledger Fabric****（核心内容）**

1. Fabric 的特点
 
• 企业级 blockchain implementation  
• 不支持 Bitcoin 或其他 cryptocurrency  
• 使用 Docker 进行模块化服务部署（modular、pluggable）  
• 支持 verified identities、private & confidential transactions  
• 是 permissioned blockchain 的典型代表  
• 无需 mining，无需 Proof-of-Work  
• 支持性能扩展、审计（auditability）、身份管理、安全性与隐私

1. Fabric 满足 Blockchain for Business 的四大特征
 
• **Permissioned network**  
– 参与方共同定义 membership 与 access control  
• **Confidential transactions**  
– 仅授权成员可查看特定交易  
• **No cryptocurrencies / No mining**  
– 不依赖 PoW  
• **Programmable Smart Contracts**（Chaincode）  
– 用于自动化业务流程

1. Fabric 的应用者（Users）
 
• Walmart：食品安全与可追溯性  
• OpenIDL：保险业监管报告  
• ScanTrust：供应链管理  
• NIIT：航空客票流程优化  
• JD.com：零售区块链平台  
• Oracle Blockchain Platform：基于 Fabric 的企业 PaaS
 
**Fabric: Technical Architecture****（技术架构）**

1. 资产（Assets）
 
• Fabric 通过 transactions 处理资产（具有货币价值的对象）  
• 资产以 key-value pairs 表示  
• 资产由 Smart Contracts（Chaincode）创建/更新  
• 所有成员必须执行相同版本的 chaincode

1. 身份管理（Membership Service Provider, MSP）
 
• MSP 管理 ID 与认证  
• 一个 Fabric 网络可由多个 MSP 管理  
• 允许不同组织定义不同的认证标准和结构
 
**Hyperledger Fabric** **网络结构总结**

1. 网络组件
 
• Peer  
• Orderer  
• Chaincode  
• Ledger  
• Channel  
• MSP  
• Application

1. 核心机制
 
• Endorsement（背书）  
• Ordering（排序）  
• Validation（验证）  
• Ledger update（账本更新）  
下面为你继续整理课堂笔记，整体保持与前面一致的格式、用语和结构。关键英文词汇全部保留，不加入特殊符号。你可以直接将此内容作为复习讲义使用。
 
**Fabric: Identity Management****（身份管理）**

1. 核心概念
 
• **Every peer is assigned a digital certificate from a CA**  
– 每个 peer 都会从 Certificate Authority（CA）获取数字证书。  
– 证书用于身份识别、认证以及访问控制。  
• Fabric 使用 **Membership Service Provider****（****MSP****）** 来统一管理身份：  
– MSP 定义组织内的身份结构、验证规则  
– 一个 Fabric 网络可以拥有多个 MSP  
– 支持不同组织间的身份互操作

1. 身份结构示意（基于图示）
 
• Network（N）：Fabric 网络整体  
• Organization（Org1、Org2）：每个组织包含多个 peers  
• Peer（P1、P2、P3、P4）：具体节点  
• Identity（D5、D7、D6、D8）：节点对应的数字身份  
• CA（CA1、CA2）：为组织颁发证书  
• Channel（C）：组织之间进行通信的通道  
• Channel Policy（CP）：指定哪些 MSP/身份可加入 channel  
总结：  
Fabric 通过 **数字证书** **+ MSP + CA** 组合，实现细粒度、可审计的身份与权限管理，是企业级区块链的关键能力。
 
**Fabric: Application Developer Perspective****（开发者视角）**

1. Client Application 与 SDK
 
• 客户端应用程序通过 SDK 与 Fabric 网络交互：  
– **Node SDK****（****JavaScript****）**  
– **Java SDK**  
– Python、REST、Go SDK 正在开发中  
• 应用通过 SDK：  
– 提交交易（submit transaction）  
– 调用 Smart Contract  
– 查询数据（query）

1. Ordering Service 在开发过程中的角色
 
• Ordering service 管理 pluggable trust engine  
– 负责 trusted transactions 的排序  
– 生成 block 并广播给 peers  
– 保持 ledger 的一致性

1. 开发流程（基于图示）
 
• Blockchain developer 开发：  
– Smart Contract  
– Client Application  
• Client Application 调用 SDK  
• SDK 与 Ordering Service 和 Ledger 进行交互  
• World State 与 Blockchain 存储最终数据与事件
 
**Fabric: The Ledger****（账本结构）**

1. Ledger 由两部分组成
 
• **World State**  
– 持当前的数据值，以 key-value pairs 形式存储  
– 应用程序可以直接访问最新状态  
– 避免遍历 transaction log  
• **Blockchain**  
– 持所有导致当前 World State 的历史变更  
– 数据结构不可修改（immutable）  
– 由 blocks 组成，block 之间通过 hash 链接

1. 两者的关系
 
• Blockchain 是完整历史（what happened）  
• World State 是当前状态（latest values）  
• Ledger = Blockchain + World State
 
**Fabric: The Ledger – World State**

1. World State 的实现
 
• 世界状态由数据库实现  
– 常用：CouchDB 或 LevelDB  
• 使用 key-value pairs：  
– key：资源标识  
– value：资源的当前状态（可为简单类型或复合结构）

1. 示例（汽车账本状态）
 
• CAR1：value = Audi，version=0  
• CAR2：value = {type: BMW, color: red, owner: Jane}，version=0

1. Smart Contracts 在 World State 中的职责
 
• 应用程序通过 Smart Contracts 操作状态  
– get state  
– put state  
– delete state  
• 使用 Ledger API 与状态交互
 
**Fabric: The Ledger – Blockchain**

1. Blockchain 的功能
 
• 保存对象到达当前状态的完整历史  
• 由 Ordering Service 负责：  
– 排序 transactions  
– 生成 interlinked blocks

1. Block header 内容
 
• 当前 block 的 transactions hash  
• 前一个 block 的 hash  
→ 形成不可篡改的链式结构
 
**Fabric: Pros and Cons****（优缺点）**

1. Pros
 
• Private channels 支持保密交易  
• Peer、Orderer、Ledger、CA 组件分离清晰  
• 已广泛用于行业实际场景  
• Oracle、AWS 均基于 Fabric 提供 PaaS  
• Open source，由 Linux Foundation 维护

1. Cons
 
• 技术相对新（1.0 于 2017 发布）  
• Robustness、vulnerabilities 尚未完全成熟  
• 安装、升级、卸载流程较复杂  
• 学习曲线陡峭，开发文档需改进  
• 旧版本（如 1.4）稳定性差  
• 当前主版本为 2.2.4
 
**Summary****（总结）**

1. 区块链技术
 
• 在 cryptocurrency 领域表现成熟（Bitcoin 超过 10 年）  
• 主要分类：  
– Public blockchain（Bitcoin、Ethereum）  
– Private blockchain（如 Hyperledger Fabric）  
• 适用场景：  
– Asset trading（股票、债券、音乐、能源）  
– Sharing economy（出租房间、办公空间、车辆）  
– Supply chain management  
– Autonomous Smart Contracts  
– Asset provenance（溯源）

1. Hyperledger Fabric
 
• 面向企业网络  
• 支持 Smart Contracts 与 Chaincode  
• 保密交易（private channels）  
• 已有多家企业使用（Walmart、JD、Oracle、AWS）  
• 架构强大但部署成本高、运维复杂  
• 提供可扩展、安全、可审计的企业级区块链能力
 
如果你愿意，我可以将所有章节进一步整理成：  
• 超精简版考试速记笔记  
• 中英双语对照完整版  
• 思维导图结构（text-based）  
• 按主题重组的 revision pack（区块链概念 / Fabric 架构 / 身份管理 / Ledger / Use cases）  
你希望接下来如何整理？
 \> 来自 \<[https://chatgpt.com/c/6932e9eb-3324-8333-b65c-cc6f6dc173b0](https://chatgpt.com/c/6932e9eb-3324-8333-b65c-cc6f6dc173b0)\>
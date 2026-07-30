1. NoSQL 与现代数据库模型
2. 区块链
3. Metadata、FAIR、数据归档与生命周期
4. Big Data 与 Three Vs(lec1)
5. 机器学习中的数据处理（Batch vs Online、Streaming）
6. HDFS、Hadoop、MapReduce Pipeline、Readers
7. 文件系统（Lustre、Parallel FS、I/O strategies、HDF5、NetCDF）
8. Data Warehouse、Data Lake、Cassandra、Neo4j
9. 关系数据库（PK、FK、3NF、ERD、多对多设计）
10. ACID 事务
11. HPC 数据存储与访问
12. Messaging Systems、Kafka
13. 研究数据管理（Long-term preservation）
 
**1. NoSQL** **与现代数据库模型**  
**【****2021 Q1(a)****】**  
Why do we need NoSQL databases?￼

![Exported image](Exported%20image%2020260730193030-0.png)

￼￼==Because NoSQL databases provide== **high horizontal scalability**==, handle== **large structured/semi-structured/unstructured data**==, offer== **schema flexibility**==, avoid== **ORM**==, and support== **data models** ==(key-value, document, column, graph) that fit modern distributed applications.==￼  
We need NoSQL databases because they are designed for **distributed storage with high horizontal scalability**, and are suitable for **large structured, semi-structured, or unstructured data**. They allow applications to **store objects without using Object-Relational Mapping (ORM)**.  
Many NoSQL systems **don’t require schemas**, giving **flexibility in storing documents with different content**. They typically have **no join operations**, and some data models (e.g., graph databases) **naturally link related data**. They also **do not provide full ACID transactions**, trading strict consistency for scalability, with some systems offering **simpler transactional guarantees**.  
Overall, NoSQL databases support **high scalability**, **schema flexibility**, and **efficient handling of diverse data models** such as key-value, document, column, and graph.￼￼  
**【****2021 Q1(b)****】**  
Give an example of where you would use a key-value database. List two advantages and two disadvantages.￼

![Exported image](Exported%20image%2020260730193032-1.png)  

**Example:**  
==Key-value stores are used where applications need== **fast lookups of values by a simple key**==, such as== **session management** ==or== **caching user preferences**==.==

1. **Very fast** ==data access (low latency).==
2. **Flexible value type** ==(can store simple or large structured/unstructured data).==

1. **No schema or data validation** ==(handled by applications).==
2. **No joins or complex queries**==; limited functionality.==￼￼

**逻辑点版答案（按课件原意压缩）**  
**Example use case**  
Use a key-value database for **persistent storage of session data** or **fast retrieval in a caching system**, where data is accessed via a simple key and high performance is required.

1. **Low-latency access****￼**Key-value stores provide **single-digit millisecond latency**.
2. **Flexible values****￼**The value can be **anything**: simple numbers/strings, large documents, pictures, videos.

1. **No schema / validation****￼**Many don’t require schemas; **validations must be done by the application**.
2. **Limited querying****￼****No join operations** and limited querying capabilities.

￼￼  
**【****2021 Q1(c)****】****￼**

![Exported image](Exported%20image%2020260730193036-2.png)

Describe four other approaches to storing data besides relational/NoSQL.￼  
**超短版答案（直接背）**

1. **RDF databases** ==(Resource Description Framework databases).==
2. **XML databases** ==(XML-based storage).==
3. **Hybrid / object-relational databases** ==(e.g. PostgreSQL as object-relational).==
4. **File-based storage** ==(e.g. plain files such as CSV, JSON, XML documents).==
 
1. **RDF databases (Resource Description Framework databases)**
    - Store data as **triples** for representing **semantic relationships** on the web; used for **linked data** and knowledge representation.
2. **XML databases**
    - Store data in **XML documents**, suitable for **hierarchical, document-centric data**, and were **popular in the past** as an alternative to relational tables.
3. **Hybrid / object-relational databases**
    - **Extend relational databases** with **object-oriented features** (e.g. PostgreSQL → **object-relational**), allowing more complex types while still using tables and SQL.
4. **File-based storage (no DBMS)**
    - Data stored directly as **files** (e.g. CSV / JSON / XML) in a file system or data lake, without a full DBMS; simple but lacks queries, transactions, and integrity constraints.
 
**【****2023 Q1(c)****】**  
Briefly describe Cassandra and Neo4j DBMSs, including Data Model, Query Language, and use cases.￼

![Exported image](Exported%20image%2020260730193037-3.png)

- **Data model:** ==Column-oriented; tables with partitions; flexible schema.==
- **Query language:** ==CQL (similar to SQL).==
- **Use case:** ==High-velocity, high-volume data on a scalable, fault-tolerant distributed cluster.==
    - **Example:** ==Large-scale log or sensor data ingestion.==

- **Data model:** ==Property graph model (nodes + relationships with properties).==
- **Query language:** ==Cypher (declarative, graph-traversal-optimised).==
- **Use case:** ==Deeply linked relationship data and pattern discovery.==
    - **Example:** ==Real-time product recommendation or fraud detection.==
 
**【逻辑点版答案】（按课件内容压缩）**  
**Cassandra**

- A **column-oriented database**.
- Uses **keyspaces**, **tables**, **rows**, and **partitions** (base unit of data access).
- **Flexible schema** (rows may have different sets of columns).
- **Peer-to-peer architecture** with **no single point of failure**, highly scalable.

- **CQL (Cassandra Query Language)**, similar to SQL; used for creating keyspaces, tables, and CRUD operations.

- Suitable for **high velocity and high volume data**, **horizontal scalability**, and **fault-tolerant distributed clusters**.
- **Example:** Ingesting continuous streams of operational or sensor data across many nodes.

**￼****￼****【****2023 Q1(d) –** **部分涉及区块链】**  
Briefly explain when you may consider blockchain technology.￼

![Exported image](Exported%20image%2020260730193038-4.png)  

==Use blockchain when a business problem needs trust, an identifiable business network, and alternatives have weaknesses.==  
==It is appropriate because it provides consensus, immutability, and provenance.==  
==A unique feature: a shared, add-only, immutable ledger.==  
==Use public for open, permissionless networks; use private for business privacy.==  
==Choose Hyperledger Fabric for permissioned, confidential transactions and no proof-of-work; choose Ethereum for public programmable dapps.==  
############################################  
逻辑点版（贴近课件原文、层次清晰）  
############################################  
**1. When to consider blockchain**  
• When there is a business problem to be solved and alternative technologies pose significant weaknesses.  
• When an identifiable business network exists with participants, assets, and transactions.  
• When a need for trust arises: consensus, immutability, provenance.  
**2. Why blockchain is appropriate**  
• Provides a shared ledger that is add-only and immutable.  
• Ensures secured, authenticated, verifiable transactions.  
• Smart contracts embed business terms and execute automatically across the network.  
**3. Unique feature**  
• The immutable, shared ledger forming a chain of blocks that prevents alteration and ensures provenance.  
**4. Public vs private**  
• Public: permissionless, anyone can join; uses PoW/PoS; suitable for open networks (e.g., Ethereum).  
• Private: permissioned, provides privacy and controlled visibility; suitable for business blockchains (e.g., Hyperledger Fabric).  
**5. Implementation choice**  
• Hyperledger Fabric: chosen for enterprise use; supports permissioned networks, confidential transactions, no cryptocurrencies or proof-of-work; modular and scalable.  
• Ethereum: chosen if a public, programmable blockchain is required for open decentralized applications.
 
**【****2024 Q2(d)****】**  
List the 4 data models used to categorise NoSQL databases.￼

1. **Key-Value**
2. **Document**
3. **Column**
4. **Graph**
 
**【****2024 Q2(e)****】**  
Explain why many NoSQL databases do not support ACID transactions.

![Exported image](Exported%20image%2020260730193039-5.png)

**超短背诵版**  
==They drop ACID to achieve== **high scalability**==,== **distributed performance**==, and== **flexible schemas**==, often providing== **simpler, non-ACID guarantees** ==instead.==  
**逻辑点版（来自课件原文特性）**

- They are designed for **distributed storage with high horizontal scalability**.
- ACID constraints restrict performance in distributed systems.
- NoSQL systems often favour **eventual consistency** and **simple operations** for scalability.
- Many provide **no transactions**, or only **simpler forms** (e.g., atomicity + isolation + durability in Cassandra).
 
**2.** **区块链**  
**【****2021 Q1(d)****】**  
Describe what a blockchain is.  
Explain public vs private blockchain.  
When to use blockchain?  
Discuss when to use Hyperledger Fabric instead of Ethereum.￼

![Exported image](Exported%20image%2020260730193040-6.png)  

**What is blockchain:**  
==A distributed ledger storing an immutable, add-only list of transactions in blocks linked as a chain.====￼==  
**Public vs private:**  
==Public = permissionless, anyone can join, uses PoW/PoS.==  
==Private = permissioned, suitable for business, provides privacy.====￼==  
**When to use blockchain:**  
==When there is a business problem with trust needs, an identifiable business network, and alternatives show weaknesses; requires consensus, immutability, provenance.====￼==  
**Why Hyperledger Fabric instead of Ethereum:**  
==Use Fabric for permissioned, private, confidential business networks without cryptocurrencies or proof-of-work; modular, enterprise-focused; supports private channels and verified identities.==￼￼  
**1. What is Blockchain**  
• A distributed ledger, the system of record for a business.  
• Stores a list of transactions within blocks linked to form a chain.  
• Shared with network peers; add-only and immutable.  
**2. Public vs Private Blockchain**  
• Public: permissionless; anyone can join/leave; uses PoW/PoS; examples include Ethereum, Cardano; Bitcoin chain used as store of value.  
• Private: permissioned; suitable for business; provides privacy; examples include Hyperledger Fabric and private Ethereum deployments.￼  
**3. When to Use Blockchain**  
• When solving a business problem where alternative technologies show significant weaknesses.  
• Requires an identifiable business network (participants, assets, transactions).  
• A need for trust: consensus, immutability, provenance.  
**4. When to Use Hyperledger Fabric Instead of Ethereum**  
• Fabric is a permissioned, enterprise blockchain supporting private and confidential transactions.  
• No cryptocurrencies and no proof-of-work; reduced computational cost.  
• Modular, pluggable services with verified identities and private channels.  
• Suitable for business applications such as supply chain, asset trading, regulatory reporting.
 
**【****2023 Q1(d)****】**  
Explain when you may consider using Blockchain.  
Include whether to choose public/private and which implementation.￼

![Exported image](Exported%20image%2020260730193041-7.png)  

==Use blockchain when there is a business problem with trust needs, an identifiable business network, and alternatives show weaknesses.==  
==Choose public for permissionless open networks; choose private for business privacy and permissioned access.==  
==Use Hyperledger Fabric for enterprise, permissioned, confidential transactions; use Ethereum for public, programmable, open dapps.==  
￼￼  
**When to consider blockchain**  
• There must be a business problem where alternative technologies pose significant weakness.  
• There must be an identifiable business network with participants, assets, and transactions.  
• A need for trust: consensus, immutability, provenance.  
**Public vs private**  
• Public blockchain: permissionless; anyone can join; uses PoW/PoS; suits open networks (e.g., Ethereum).  
• Private blockchain: permissioned; suitable for businesses needing privacy and controlled visibility (e.g., Hyperledger Fabric).  
**Which implementation**  
• Use Ethereum for open, public, programmable blockchain environments.  
• Use Hyperledger Fabric when requiring permissioned access, confidential transactions, no cryptocurrencies, and enterprise-focused modular architecture.
 
**3. Metadata****、****FAIR****、数据归档与****life cycle**  
**【****2022 Q1(a)****】**  
Describe the difference between descriptive and structural metadata.￼

![Exported image](Exported%20image%2020260730193042-8.png)

- ==Descriptive metadata: content/meaning (what, who, where, when, why, how).==
- ==Structural metadata: technical/structural details (how stored, organised, constrained).==￼￼

- Descriptive / “content” metadata:
    - Describes what the data relates to: what / where / when / who / how / why it was collected or created, who collected it, where and when collected.
    - Helps others understand the subject of the data and reuse/interpret it.
- Structural (system/control) metadata:
    - Describes how the data are stored and organised: file ownership, modification date, how it is packaged.
    - In files / databases: table and column names, data types, formats, limits, valid ranges, dimensions, relationships, keys, permissions, update history, external sources.
    - Provides the technical structure needed to manage and access the data.

**￼****￼****￼****【****2022 Q1(b)****】**  
Explain the benefits of following FAIR guidelines.￼

![Exported image](Exported%20image%2020260730193045-9.png)  

- ==FAIR makes data Findable, Accessible, Interoperable and Reusable, especially for machines, improving discovery and re-use.==￼

- Purpose of FAIR:
    - “Guidelines to enhance data reusability” for machines and humans; support long-term care of digital assets.
- Findable (F):
    - (Meta)data have globally unique, persistent identifiers; rich metadata; registered/indexed in searchable catalogues.
    - PIDs and catalogues make data easier to discover.
- Accessible (A):
    - (Meta)data retrievable by standardised, open, universally implementable protocols; metadata stay accessible even if data are not.
    - Supports controlled access (authentication/authorisation) when needed.
- Interoperable (I):
    - Use formal, shared, broadly applicable knowledge representation languages and vocabularies that follow FAIR principles.
    - Enables integration and machine processing across systems and domains.
- Reusable (R):
    - Rich descriptions, clear and accessible data usage licence, detailed provenance, and domain-relevant community standards.
    - Maximises re-use, including for data mining and artificial intelligence.

**￼****￼****【****2022 Q1(c)****】**  
What archival strategies ensure research dataset usability in future?￼

![Exported image](Exported%20image%2020260730193046-10.png)  

- ==Plan preservation in the data lifecycle, store in OAIS-style trusted repositories with rich Representation Information, and use replication on reliable media.====￼====￼==

- Treat preservation as part of the data lifecycle:
    - Preservation is not just “cp files.zip /archive” but a lifecycle step to keep data safe, discoverable, accessible, and comprehensible for re-use.
- Use reliable digital preservation strategies:
    - Expect media to fail; plan for HDD/tape/flash failures and technology change.
    - Use “replication, replication, replication”: multiple copies, multiple locations, according to data value and acceptable risk.
- Follow OAIS-based archival practice:
    - Use the OAIS Information Packages:
        - SIP (Submission Information Package) at ingest.
        - AIP (Archival Information Package) for long-term storage.
        - DIP (Dissemination Information Package) for users.
    - Capture sufficient Representation Information (file formats, software needed, documentation, metadata) so the Designated Community can understand and use the data in the long term.
    - Support functions of Ingest, Archival Storage, Data Management, Access, Administration, and Preservation Planning.
- Use trusted repository technologies:
    - Institutional or subject repositories (e.g. DSpace, Fedora-based systems) that index, preserve, and provide search/access over time.
- Overall goal:
    - Preserve data “for re-use”, ensuring that in future they remain safe, findable, accessible, and understandable to their Designated Community.

**￼****￼****【****2023 Q3(c)****】**  
Hadoop would most commonly be used in one key phase of the data lifecycle. State which phase this is. Explain how features of CSV, XML and JSON could be beneficial at other stages in the lifecycle.￼

![Exported image](Exported%20image%2020260730193047-11.png)  
![Exported image](Exported%20image%2020260730193048-12.png)

**Hadoop would most commonly be used in one key phase of the data lifecycle. State which phase this is. Explain how features of CSV, XML and JSON could be beneficial at other stages in the lifecycle.****￼****￼**

- ==Hadoop phase:== **Process**==.==
- ==CSV: simple for== **Acquire / Assure**==.==
- ==XML: structured, self-describing for== **Describe / Share**==.==
- ==JSON: flexible for== **Combine / Integrate**==.==
 
**Logic-point version****（基于课件内容逐句提取）**

- Hadoop is used for large-scale **data processing**, specifically **Map → Sort → Combine → Partition → Reduce** on HDFS.
- This corresponds to the **Process** phase in the data lifecycle model (Analyze / Process / Combine in DataONE hybrid model).

- CSV is **simple**, **line-based**, easy for Hadoop TextInputFormat to read.
- Beneficial in **Acquire** and **Assure**:
    - Easy to generate from instruments or simulations.
    - Easy to validate or clean during assurance steps.
- Supports early data handling before Hadoop processing.

- XML is **hierarchical** and **self-describing**.
- Beneficial in the **Describe** stage:
    - Carries structure and metadata together.
    - Helps record information that is needed to interpret data in 1, 10, or 100 years (per Describe-stage slides).
- Useful for **sharing** data when structure must be preserved.

- JSON is structured text suited to key–value formats similar to Hadoop Map output.
- Beneficial in the **Combine / Integrate** stage:
    - Flexible for merging or reusing data from different systems.
    - Widely used for exchanging data across services.
- Supports later lifecycle phases where external data sources are integrated before processing.

**￼****￼****【****2024 Q1(b)****】**  
What does FAIR stand for? Give one principle for each.￼

![Exported image](Exported%20image%2020260730193049-13.png)

**Ultra-short version**  
==FAIR === **Findable, Accessible, Interoperable, Reusable**==.==  
**Logic-point version**

- **Findable**
    - F1: (Meta)data have a **globally unique and persistent identifier**.
- **Accessible**
    - A1: (Meta)data are retrievable via a **standardised, open protocol**.
- **Interoperable**
    - I1: (Meta)data use a **formal, shared, broadly applicable language**.
- **Reusable**
    - R1: (Meta)data are **richly described with accurate, relevant attributes**.

**￼****￼****【****2024 Q1(c)****】**  
Steps to make metadata both human readable and machine readable.￼

![Exported image](Exported%20image%2020260730193150-14.png)

- ==Use== **standard formats**==,== **controlled vocabularies**==,== **clear structure**==, and== **machine-readable representations**==.==

**Logic-point version**

- Use **rich, complete, accurate, precise** metadata with clear semantics so humans can understand it.
- Use **standards**: conform to established metadata standards (e.g., Dublin Core, domain standards).
- Use **controlled vocabularies / formal languages** so machines can interpret terms unambiguously.
- Ensure **correct formatting and syntax** (e.g., valid date/time formats).
- Make metadata **accessible online**, **addressable**, and **harvestable** for automated systems.
- Provide **persistent identifiers (PIDs)** to support discovery by humans and machines.
- Keep metadata **public even when data are restricted**, ensuring machine access to descriptions.

**￼****￼****￼****【****2024 Q3(b)****】**  
Briefly discuss the data lifecycle and its importance in data management planning.￼

![Exported image](Exported%20image%2020260730193151-15.png)

**Briefly discuss the data lifecycle and its importance in data management planning.**

- ==The data lifecycle describes how data move from creation to use and preservation; it guides planning so data remain understandable, accessible, and reusable.==
 
**Logic-point version**

- **Data lifecycle meaning**
    - A way of thinking about the different stages through which data pass **from creation to storage or deletion**.
    - At each stage, data are used or regarded differently, requiring different management actions.
    - Stages involve transitions of responsibility (e.g., sharing with groups, depositing to a repository, publishing).
- **Why it is useful**
    - Helps researchers and businesses think systematically about how to **create, assure, describe, preserve, discover, integrate and analyse** data.
    - Maps naturally onto research workflows: **create/measure → analyse → store for reuse**.
    - Increasingly important due to funder requirements, open data expectations, regulation, provenance, and reproducibility needs.
- **Importance for data management planning (DMP)**
    - A DMP is created at the **Plan** stage and lays out decisions for all later stages:
        - what data will be created,
        - how they will be described,
        - how they will be processed, stored, shared, preserved, and found.
    - Good planning **saves time**, increases **research efficiency**, supports **reproducibility**, prevents data loss, and avoids duplicated effort.
    - Ensures that data are **safe for the present and the future** and that others can use and understand them.
- **Overall importance**
    - The lifecycle provides a structured framework for managing data responsibly and efficiently over its entire existence.
    - It underpins modern research and business practice by supporting **reliable, transparent, and reusable** data.

**￼****￼****￼****【****2021 Q3****】**  
It is widely agreed that there is value in archiving research data so that it can be reused in future analysis. D==escribe what can be done, from the moment that data is collected or created, to try to ensure that data can be re-used by researchers in twenty years’ time.== Your answer should include what needs to be done and how such steps could help future researchers.￼

![Exported image](Exported%20image%2020260730193155-16.png)  

==【====2021 Q3====】==

- ==Use good metadata, standards, representation information, suitable formats, replication, and OAIS-style preservation planning from the start to ensure long-term understanding, accessibility, and reuse.==

**Logic-point version**

- **Create rich metadata from the moment data is collected:**
    - Record why, how, where, when, and by whom the data were created.
    - Include limitations, processes used, formats, scales, and meanings of values.
    - Helps future researchers understand and correctly interpret the data.
- **Capture sufficient Representation Information (RI):**
    - Document file formats, software needed to read them, and any dependencies.
    - Ensures the Designated Community can still decode and understand the data in twenty years.
- **Use standard, open, well-documented formats and vocabularies:**
    - Avoid obscure or proprietary formats.
    - Supports long-term readability and machine processing.
- **Plan preservation early as part of the lifecycle:**
    - Treat preservation as a continuous activity (not “copy to archive at the end”).
    - Ensures that data remain safe, discoverable, accessible, and comprehensible.
- **Use trusted repository systems (OAIS model):**
    - Ingest data as SIPs with rich metadata.
    - Store data as AIPs with preservation metadata.
    - Provide DIPs for future users.
    - Supports durable management of data, metadata, and access.
- **Ensure safe long-term storage:**
    - Expect media to fail; use replication, multiple copies, multiple locations.
    - Reduces risk of data loss as storage technologies change.
- **Assign persistent identifiers (PIDs):**
    - Makes datasets globally findable and citable long into the future.
- **Ensure metadata remains accessible even if data are restricted:**
    - Allows discovery and reuse planning even when access conditions change.

Overall effect:  
These steps ensure that future researchers can **find**, **access**, **decode**, **understand**, and **re-use** the data even as technologies, formats, and institutions change.
 
**4. Big Data** **与** **Three Vs**  
**【****2024 Q1(a)****】**

![Exported image](Exported%20image%2020260730193156-17.png)

(a) Define the “Three Vs” of big data. For each, state a suitable storage method, giving a reason for that method. You may wish to consider aspects including file format, technology, or hardware in your answers.￼￼

- ==Three Vs:== **Volume, Variety, Velocity**==.==
- **Volume:** _Large files (“digital objects”) in bytes; large numbers of files/digital objects_ → ==store in a== **distributed file system** for scalable storage of large digital objects.
- **Velocity:** _Rapidly changing datasets (tweets, data streams from scientific instruments)_ → ==store as== **data streams** ==(streaming storage/ingest)== to handle continuously arriving updates.
- **Variety:** _Complex data structures; semi-structured (XML, JSON, Web) and unstructured (email, PDF, images, video, audio)_ → ==store using== **formats suited to semi-/unstructured data** (e.g., XML/JSON for semi-structured; binary objects for unstructured) to preserve structure/content.
 
1. **Three Vs of Big Data**
    - ==Volume – very large data sizes.==
    - ==Variety – many different data types and structures.==
    - ==Velocity – high-rate data streams arriving continuously.==
2. **Volume – storage method and reason**
    - Use **distributed / parallel file systems** such as **HDFS**, **Lustre**, **GPFS**.
    - HDFS: “cluster of commodity nodes”, “data redundancy”, “designed to withstand failure”, “good for processing massive amounts of data that is written once and processed often”.
    - Lustre/GPFS: striping over many disks, high bandwidth, high performance on HPC systems.
3. **Variety – storage method and reason**
    - Use **self-describing scientific file formats** such as **HDF5** or **netCDF**.
    - HDF5: “versatile data model”, “can represent very complex data objects and a wide variety of metadata”, “self describing”, portable.
    - netCDF: “software library and self-describing data format”, “portable, machine independent data”.
    - Rich metadata and flexible data model support heterogeneous, structured scientific data.
4. **Velocity – storage / handling method and reason**
    - Use **data streaming frameworks** with in-memory processing, e.g. **Data Streaming + Spark Streaming / Structured Streaming / Apache Storm**.
    - Streaming: “process as data is produced”, “no persistence – data is not stored”, “access to a sliding window on the data”, “short response times”.
    - Spark/Storm: distributed, fault-tolerant processing of streams of tuples / batches, suitable for high-velocity Big Data such as sensor streams or tweets.

￼￼￼  
**5.** **机器学习中的数据处理：****Batch, Online, Streaming****￼**

![Exported image](Exported%20image%2020260730193158-18.png)

**【****2021 Q2(a)(i)****】**  
Which scenario most likely uses online learning? Why?  
**【****2021 Q2(a)(ii)****】**  
What factors are important in a data streaming algorithm such as online learning?  
**【****2021 Q2(a)(iii)****】**  
Which scenario suits batch learning? Suitability of multi-pass vs single-pass algorithms?
 
**6. HDFS****、****Hadoop****、****MapReduce Pipeline****、****Readers** **（****lec3, lec14, lec16****）**

![Exported image](Exported%20image%2020260730193159-19.png)  
![Exported image](Exported%20image%2020260730193159-20.png)

**￼****【****2021 Q2(b)(i)****】**  
Explain how HDFS stores files larger than the largest disk in the cluster.  
**【****2021 Q2(b)(ii)****】**  
Describe two MapReduce techniques to identify similar papers, including:  
Reader, Mapper input/output, shuffle, Reducer input/output.￼  
**Ultra-short (memorise)**

- ==HDFS is== **distributed**==: a file is== **split and stored across Data Nodes** ==in a== **cluster of commodity nodes**==, with== **data redundancy**==, so it can store files== **larger than any single disk** ==and== **withstand failure**==.==

1. **Frequently cited together**==: papers are similar if they== **cite the same DOIs together**==; Map emits== **pairs of cited DOIs**==, Reduce== **counts co-occurrence**==.==
2. **Frequently citing same papers**==: papers are similar if they are== **co-citers of the same cited DOI**==; Map emits== **pairs of citing DOIs**==, Reduce== **counts co-occurrence**==.==
 
**Logical points (compressed)**

- **Distributed file system (HDFS)**:
    - Computation runs on **Data Nodes**
    - **Distributed** storage across nodes in the cluster
    - **Data redundancy**
    - **Designed to withstand failure**
- Therefore a very large file can be stored by distributing its contents across multiple nodes (rather than requiring one disk to hold it all), with redundancy for resilience.
 
**(ii) Two MapReduce techniques to identify similar papers**  
Below, use **citations.csv** records of the form: \<CITING_DOI, CITED_DOI\> (each paper has a unique DOI).
 
**Technique 1: “papers frequently cited together”**  
Strategy: Two papers are similar if they **cite the same DOIs together** (co-cited references within the same citing paper).

- **Reader (InputFormat):**
    - Reads citations.csv and presents each record as a key/value pair for the Mapper.
- **Map input:**
    - \<CITING_DOI : CITED_DOI\>
- **Map function (description):**
    - Output citing DOI as key, cited DOI as value.
- **Map output:**
    - [ \<CITING_DOI, CITED_DOI\> ]
- **Shuffle phase:**
    - Framework **groups by key** (CITING_DOI) before calling reducers.
- **Reduce input:**
    - \<CITING_DOI : List(CITED_DOI)\>
- **Reduce function (description):**
    - Concatenate/assemble the list of cited DOIs for that citing paper.
- **Reduce output:**
    - \<CITING_DOI : "cited1, cited2, cited3, ..."\>

- **Reader:**
    - Reads the output of Step A as key/value pairs.
- **Map input:**
    - \<CITING_DOI : "cited1, cited2, ..."\>
- **Map function (description):**
    - Generate all unordered pairs (cited_i, cited_j) from the list; emit each pair with value 1.
- **Map output:**
    - [ \<"cited_i+cited_j", 1\>, ... ]
- **Shuffle phase:**
    - Group by pair key ("cited_i+cited_j").
- **Reduce input:**
    - \<"cited_i+cited_j" : List(1)\>
- **Reduce function (description):**
    - Sum the list to count how often the pair is co-cited.
- **Reduce output:**
    - \<"cited_i+cited_j" : count\>￼Interpretation: High counts indicate references frequently cited together, implying similarity in citation behaviour.
 
**Technique 2: “papers frequently citing the same papers”**  
Strategy: Two papers are similar if they **both cite the same cited DOI** (co-citers).

- **Reader (InputFormat):**
    - Reads citations.csv and presents each record as key/value to the Mapper.
- **Map input:**
    - \<CITING_DOI : CITED_DOI\>
- **Map function (description):**
    - Emit CITED_DOI as key and CITING_DOI as value (inversion).
- **Map output:**
    - [ \<CITED_DOI, CITING_DOI\> ]
- **Shuffle phase:**
    - Group by key (CITED_DOI).
- **Reduce input:**
    - \<CITED_DOI : List(CITING_DOI)\>
- **Reduce function (description):**
    - Concatenate/assemble list of papers that cite this cited DOI.
- **Reduce output:**
    - \<CITED_DOI : "citing1, citing2, citing3, ..."\>

- **Reader:**
    - Reads the output of Step A.
- **Map input:**
    - \<CITED_DOI : "citing1, citing2, ..."\>
- **Map function (description):**
    - Generate all unordered pairs (citing_i, citing_j) from the list; emit each pair with value 1.
- **Map output:**
    - [ \<"citing_i+citing_j", 1\>, ... ]
- **Shuffle phase:**
    - Group by pair key ("citing_i+citing_j").
- **Reduce input:**
    - \<"citing_i+citing_j" : List(1)\>
- **Reduce function (description):**
    - Sum values to count how often two papers co-cite the same cited DOI.
- **Reduce output:**
    - \<"citing_i+citing_j" : count\>￼Interpretation: High counts indicate pairs of papers that frequently cite the same works, suggesting similarity.
    
￼￼  
**【****2023 Q3(a)****】**

![Exported image](Exported%20image%2020260730193201-21.png)

Explain the Hadoop pipeline stages: Read, Map, Combine, Reduce.￼

- **Read:** ==turns input into== **key/value pairs** ==for Mapper (e.g.,== **TextInputFormat: key=byte offset, value=line text**==).==
- **Map:** ==Apply same function to each input record;== **must output list of (key,value) pairs**==; runs== **on nodes where input data resides**==.==
- **Combine: Optimisation only**==; may run== **zero/one/more times**==;== **must not alter final result**==;== **keys must not be altered**==; no re-sort after combine.==
- **Reduce:** ==Framework== **groups by key**==;== **one reduce call per unique key**==; reduces to== **(key,value) pairs** ==(single value per key).====￼====￼==

**3(a) Purpose + what the framework does**

- Purpose: turn input files into records for MapReduce.
- Framework: uses **InputFormat interface** to produce key/value pairs, e.g.
    - **TextInputFormat: key = byte offset of line**, **value = line text**
    - **KeyValueTextInputFormat:** each line has **key/separator/value**
    - **SequenceFileInputFormat: compressed binary format**

- Purpose: apply same function to each piece of data in parallel.
- Framework:
    - Runs Map **on every node where the input data resides**
    - Enforces Map output type: **list of (key,value) pairs**

- Purpose: reduce data volume before Reduce (helper to reducer).
- Framework properties:
    - **Optimisation only**
    - May execute **zero, one or more times**
    - **Must not alter the final result**
    - **Keys must not be altered**
    - Hadoop **does not re-sort after the Combine stage**

- Purpose: combine/group mapped values into final per-key results.
- Framework:
    - **Groups by key** before calling reducers
    - **Only one reduce call for each unique key**
    - Reduce outputs **(key,value) pairs**, with **a single value for each input key**
 ![Exported image](Exported%20image%2020260730193202-22.png)

**【****2023 Q3(b)(i)****】**  
Identify three types of Hadoop reader.￼  
**【****2023 Q3(b)(ii)****】**  
Which file type (CSV/XML/JSON) is easiest to read using Hadoop Readers?
 ![Exported image](Exported%20image%2020260730193206-23.png)  

- **Text reader:** ==line-based (key offset, value line).==
- **Key/value line reader:** ==each line has== **key/separator/value**==.==
- **Compressed binary reader:** ==Hadoop== **SequenceFile** ==format.====￼====ii) Easiest:== **CSV** ==(line-based text).==￼￼

**3(b) Readers and easiest format**
 
**【****2023 Q3(c)****】**

![Exported image](Exported%20image%2020260730193207-24.png)

Which data lifecycle phase is Hadoop commonly used in?  
Explain how CSV/XML/JSON help in other lifecycle stages.￼￼

- ==Hadoop most commonly:== **Process** ==phase (distributed processing / MapReduce).==
- **CSV:** ==simple tabular exchange for== **Create/Store/Process**==.==
- **XML/JSON: semi-structured**==; can carry structure/schema-like fields for== **Describe/Combine/Discover**==.== 
**3(c) Hadoop in the data lifecycle + CSV/XML/JSON benefits elsewhere**

- **Process** (distributed data processing using MapReduce / Hadoop)

- **CSV**
    - Simple, row-based text: useful for straightforward **Store** / exchange and batch-style **Process**.
- **XML / JSON**
    - **Semi-structured data**: helpful when you need **complexity in data structures** (e.g., Web-style data), supporting **Describe** and **Combine** with richer structure than flat text.
 
￼￼  
**7.** **文件系统：****Lustre****、****Parallel FS****、****I/O strategies****、****HDF5****、****NetCDF**  
**【****2022 Q2(a)****】**

![Exported image](Exported%20image%2020260730193209-25.png)

Name the main components of the Lustre filesystem.￼￼

- **Object Storage Servers (OSS)** ==(store data on== **Object Storage Targets (OST)**==)==
- **Metadata Server (MDS)** ==(stores metadata on== **Metadata Target (MDT)**==)==
- **Clients** ==(standard== **POSIX** ==access)==

- **OSS + OST**: OSS store data on one or more OST; OST handles client data request ↔ physical storage; OSS operate in parallel.
- **MDS + MDT**: one MDT per filesystem; stores all metadata (filenames, directories, permissions, file layout).
- **Clients**: access Lustre via standard POSIX I/O.
 
**【****2022 Q2(b)****】**

![Exported image](Exported%20image%2020260730193210-26.png)

Discuss methods for improving performance on parallel filesystems like Lustre.￼￼

- ==Use== **striping**==: data== **striped across OSTs (round robin)** ==to enable== **simultaneous read/write**==.==
- ==Tune with== **lfs**==: set/query== **stripe count** ==and== **stripe size** ==(directory-level configuration).==
- ==Prefer== **big reads/writes**==; avoid patterns that create== **contention/serialisation**==.==

- **Striping (key method)**
    - Data **striped across OSTs (round robin)**; file split into units; enables **simultaneous read/write** to different units.
    - Benefit: improves **bandwidth**, overall available performance, and **maximum file size**.
    - Cost: **communication overhead** and **contention**, including **serialisation** if multiple processes access the same units.
- **Practical tuning via Lustre tools**
    - Use **lfs df -h** to query number of OSTs; **lfs getstripe** to query stripe settings; **lfs setstripe** to configure stripe count/size (e.g., policies for “large file” vs “lots of files”).
- **Access-pattern guidance (from I/O/MPI-I/O slides)**
    - Best performance for **big reads/writes**; parallel I/O behaviour is complicated and user-managed; poor patterns (e.g., many processes hitting same units / many opens) can overload limits and reduce performance.
 
**【****2022 Q2(c)****】**

![Exported image](Exported%20image%2020260730193311-27.png)

Name three I/O strategies/methods in parallel applications.￼￼

![Exported image](Exported%20image%2020260730194414-28.png)  
![Exported image](Exported%20image%2020260730194415-29.png)

- **Master I/O**
- **Individual files (one file per process)**
- **I/O libraries (e.g. MPI-I/O / HDF5 / NetCDF)**

- **Master I/O**: one process performs I/O; others communicate data to/from it.
- **Individual files**: each process writes its own file (shared FS or local scratch).
- **I/O libraries**: use parallel I/O libraries to manage access and layout.
 
**【****2022 Q2(d)****】**

![Exported image](Exported%20image%2020260730194419-30.png)

Benefits and drawbacks of higher level I/O libraries (HDF5, NetCDF) vs POSIX or MPI I/O.  
**Ultra-short (memorise)**

- **Structured, self-describing data + metadata**
- **Portability** ==and== **machine independence**
- **Parallel-ready**==,== **high-level APIs**==, tool support==

- **Overhead / complexity**
- **Less raw control** ==than low-level I/O==

**Logic-point**

- **Structure & metadata**: turn bytes into information; self-describing formats (groups, datasets, attributes).
- **Portability**: portable file formats; handle types and endianness.
- **Parallel-ready & performance features**: built on **MPI-IO / Parallel HDF5**; optimisations available.
- **Ease of use & tools**: high-level APIs; wide language and tool support; interoperability.

- **Overhead**: additional software layers may add cost.
- **Reduced low-level control**: compared with direct **POSIX I/O / MPI-I/O** access patterns.
      

**【****2022 Q2(e)****】**

![Exported image](Exported%20image%2020260730194420-31.png)

Design I/O approach for reading many small files and writing one large file.  
Discuss filesystem choice, configuration, software approaches.￼￼

- **Parallel filesystem**==:== **Lustre**
- **Config**==: tune== **striping** ==(many OSTs, suitable stripe size)==
- **Read (many small files)**==:== **individual files per process**
- **Write (one large shared file)**==:== **MPI-I/O or Parallel HDF5/NetCDF**

**Logic-point**

- Use a **parallel filesystem (Lustre)**.
- Configure **striping across multiple OSTs** to increase bandwidth for the large output file.
- Choose **appropriate stripe count and stripe size** (via lfs) to enable simultaneous access.

- **Input (many small files)**
    - Use **individual files**, each worker reads its own file.
    - Avoid contention and excessive communication.
- **Output (one very large shared file)**
    - Use **MPI-I/O** or **higher-level parallel I/O libraries (Parallel HDF5 / NetCDF)**.
    - Allows all processes to contribute to a **single file image**, avoiding post-processing and scalability issues.
 
**【****2024 Q3(a)****】****￼**

![Exported image](Exported%20image%2020260730194422-32.png)

Discuss features important for efficient HPC data storage and access.
 
- **Parallel filesystems** ==with== **striping**
- **High-bandwidth hardware** ==(many disks, fast networks)==
- **Parallel I/O libraries**
- **Metadata and structure**
- **Portability and scalability**

**Logic-point**

- **Many disks** (HDD/SSD/NVMe) with **striping** for bandwidth.
- **High-performance networks** connecting compute and I/O nodes.
- Dedicated **I/O servers and adapters** to scale access.

- **Parallel filesystems** (e.g. Lustre) to allow concurrent access.
- **Parallel I/O libraries** (MPI-I/O, HDF5, NetCDF) for scalable read/write.
- **Optimised access patterns**: large, contiguous I/O preferred.

- **Metadata and structure** to make data understandable and reusable.
- **Portable, self-describing formats** to ensure long-term access.
- Balance **performance**, **resilience**, and **ease of use**.
 
**8. Data Warehouse****、****Data Lake****、****Cassandra****、****Neo4j**  
**【****2023 Q1(a)****】**  
Describe what a Data Warehouse is and its four unique characteristics.￼

![Exported image](Exported%20image%2020260730194423-33.png)

- ==A data warehouse is a store of large historical data used for analysis.==
- ==Characteristics: subject-oriented, integrated, non-volatile, time-variant.====￼==

- **Definition**
    - Store of large historical data using a single DBMS
    - Consolidates data from many data sources
    - Data is transformed and ready for analysis
- **Four characteristics**
    - **Subject-oriented**: analysis by subject (e.g. sales)
    - **Integrated**: consistency across different data sources
    - **Non-volatile**: ingested data does not change
    - **Time-variant**: analysis focuses on change over time

**￼****【****2023 Q1(b)****】**  
List two advantages and two disadvantages of using a Data Lake instead of a Data Warehouse.￼

![Exported image](Exported%20image%2020260730194424-34.png)

·  
**Two advantages and two disadvantages of using a Data Lake instead of a Data Warehouse.**

- ==Advantages: stores raw data as-is; no schema defined in advance.==
- ==Disadvantages: data must be filtered and structured by analyst; data is unlinked and unfiltered.==

- **Advantages**
    - Stores structured and unstructured raw data in original form
    - No need to design schema or queries in advance
- **Disadvantages**
    - Analyst must filter, format, structure and link data
    - Data is stored unfiltered and unlinked

**￼****【****2023 Q1(c)****】**  
Cassandra & Neo4j:  
Data Model, Query Language, use cases.￼

![Exported image](Exported%20image%2020260730194425-35.png)  

**Cassandra**

- ==Data model: NoSQL distributed database.==
- ==Query language: CQL.==
- ==Use cases: large-scale distributed data storage.==

- **Data model**
    - NoSQL Database Management System
    - Distributed storage
- **Query language**
    - CQL (Cassandra Query Language)
- **Use cases**
    - Managing large-scale, distributed data
 
**Neo4j**

- ==Data model: graph database.==
- ==Query language: Cypher.==
- ==Use cases: graph data and relationship analysis.==

- **Data model**
    - NoSQL graph database
- **Query language**
    - Cypher (declarative, SQL-like)
- **Use cases**
    - Data with relationships
    - Graph traversal and linked data analysis
 
**9.** **关系数据库：****PK****、****FK****、****3NF****、****ERD****、多对多设计****￼**

![Exported image](Exported%20image%2020260730194425-36.png)

**【****2023 Q2(a)****】**  
Define primary key.￼  
**超短版**  
==A Primary Key is a field that== **holds a unique value in every entry** ==of a table and is== **required to identify records**==.==

- A Primary Key is **a field guaranteed to hold a unique value in every entry**.
- It is **required to identify records within a table**.
- The DBMS may **automatically generate** the unique key.￼

**【****2023 Q2(b)(i)****】**  
State the foreign key in the student tables.￼  
**超短版**  
==The== **Student ID** ==column in the second table is a Foreign Key.==

- ==A Foreign Key is a field that== **refers to the Primary Key of another table**==.==
- In this case, **Student ID** links the Module table to the Student table.￼￼

**【****2023 Q2(b)(ii)****】**  
Explain how to reduce redundancy in the database.￼  
**超短版**  
==Remove repeated data by== **normalising**==: place repeated info (e.g., module organiser, address) into== **separate tables** ==and reference them using keys.==

- Redundancy occurs when data appears repeatedly across rows.
- To reduce redundancy, apply normalisation:
    - Move repeating attributes (e.g., **Address**, **Module organiser**) into **separate tables**.
    - Use **Primary Key / Foreign Key** links instead of storing repeated text.
- This follows the design goal to **minimise data redundancy**.
 
**【****2023 Q2(b)(iii)****】**  
Example of removing a many-to-many relationship.  
**超短版**  
==Introduce an== **intermediate table** ==between Students and Modules.==

- Many-to-many relationships are removed by introducing an **intermediate table**.
- Exactly as shown in the lecture slide example (Borrower–Book → Loan table).
- Here:￼￼Student ---\< StudentModule \>--- Module
- Each record associates **one student** with **one module**.￼￼

**【****2023 Q2(b)(iv)****】**  
What is 3rd Normal Form?  
How to change the database structure to meet 3NF?  
**超短版**  
==3NF requires that== **non-primary key columns depend only on the primary key**==.==  
==Redesign by== **separating data that depends on other attributes** ==into new tables (e.g., address, halls, module organiser).==  
**逻辑点版（课件原句压缩）**

- 3NF = **All conditions of 1NF and 2NF**, plus:
- **Non-primary key columns must depend only on the primary key**.

- Identify fields that do **not** depend solely on Student ID or on (StudentID, ModuleName).￼Examples:
    - **Address** depends on the hall, not on Student ID.
    - **Module organiser** depends on the **Module**, not on Student ID.
- Create separate tables:
    - **Halls** table (Hall → Address).
    - **Modules** table (Module Name → Module organiser).
- The student table keeps only Student-dependent attributes; module table keeps module-dependent attributes.
- This removes **transitive dependencies** and achieves 3NF.￼￼
- ![Exported image](Exported%20image%2020260730194429-37.png)

- 每个表都满足 1NF：每个单元格只有一个值。
- 复合主键只出现在 ENROLLMENT 中，非主键属性 ModuleScore 依赖整个主键 (StudentID, ModuleID) → 满足 2NF。
- 所有非主键属性都 **只依赖该表的主键，不经由其他非主键传递依赖**：
    - 地址不再通过 “Hall 名称/学生” 传递依赖，而是直接依赖 HallID。
    - ModuleOrganiser 不再通过学生传递依赖，而是直接依赖 ModuleID。
- 因此所有表都在 3rd Normal Form

**￼****1. Student** **表（学生基本信息）**
 
- ==每个学生一行。==
- ==Hall== ==不直接存名字和地址，而是用== ==HallID== ==连接到== ==HALL== ==表。==
- ==非主键字段== ==FirstName, Surname, HallID== ==都只依赖== ==StudentID====。==
 
**2. Hall** **表（宿舍及地址）**
 
==HALL(====￼== ==HallID PK,====￼== ==HallName,====￼== ==Address====￼====)==

- ==(H1, 'Pollock', 'Holyrood Park Road, Edinburgh, EH2')==
- ==(H2, 'Buccluech House', 'Buccluech Place, Edinburgh, EH1')==
- ==Address== ==只依赖== ==HallID / HallName====，不依赖学生== ==→== ==消除== ==“====同一个== ==Hall== ==重复地址====”== ==的冗余。==
 
**3. Module** **表（课程信息）**
 
==MODULE(====￼== ==ModuleID PK,====￼== ==ModuleName,====￼== ==ModuleOrganiser====￼====)==

- ==(M1, 'Fundamentals of Data Management', 'Dr Brown')==
- ==(M2, 'Introduction to High Performance Computers','Professor Smith')==
- ==(M3, 'Introduction to Statistics', 'Dr Jones')==
- ==ModuleOrganiser== ==只依赖== ==ModuleID / ModuleName====，不依赖学生。==
 
**4. Enrollment / StudentModule** **表（选课** **+** **成绩，多对多中间表）**
 
==ENROLLMENT(====￼== ==StudentID PK/FK -\> STUDENT.StudentID,====￼== ==ModuleID PK/FK -\> MODULE.ModuleID,====￼== ==ModuleScore====￼====)==

- ==(S001, M1, 75)==
- ==(S002, M1, 80)==
- ==(S001, M2, 65)==
- ==(S003, M3, 55)==
- ==复合主键== ==(StudentID, ModuleID)====。==
- ==ModuleScore== ==依赖整个主键（某个学生在某门课上的成绩）。==
 
**10. ACID** **事务**  
**【****2023 Q2(c)****】**  
Explain what is meant by “Transactions are ACID”.￼  
==ACID === **Atomicity, Consistency, Isolation, Durability**==.==

- **Atomicity:** All commands of a transaction are performed, or none of them.
- **Consistency:** A transaction takes the database from one **consistent state** to another consistent state.
- **Isolation:** When transactions execute in parallel, **their effects must not be visible to each other** until committed.
- **Durability:** After commit, the changes are **permanent**, even if a system failure occurs.

￼￼  
**【****2024 Q2(e) – NoSQL** **不支持** **ACID** **的原因，也可归此类】**  
**超短背诵版**  
==They drop ACID to achieve== **high scalability**==,== **distributed performance**==, and== **flexible schemas**==, often providing== **simpler, non-ACID guarantees** ==instead.==  
**逻辑点版（来自课件原文特性）**

- They are designed for **distributed storage with high horizontal scalability**.
- ACID constraints restrict performance in distributed systems.
- NoSQL systems often favour **eventual consistency** and **simple operations** for scalability.
- Many provide **no transactions**, or only **simpler forms** (e.g., atomicity + isolation + durability in Cassandra).
 
**11. HPC** **数据存储与访问**  
**【****2024 Q3(a)****】**  
Features important for efficient HPC data storage and access.￼

![Exported image](Exported%20image%2020260730194430-38.png)

Discuss features important for efficient HPC data storage and access.
 
- **Parallel filesystems** ==with== **striping**
- **High-bandwidth hardware** ==(many disks, fast networks)==
- **Parallel I/O libraries**
- **Metadata and structure**
- **Portability and scalability**

**Logic-point**

- **Many disks** (HDD/SSD/NVMe) with **striping** for bandwidth.
- **High-performance networks** connecting compute and I/O nodes.
- Dedicated **I/O servers and adapters** to scale access.

- **Parallel filesystems** (e.g. Lustre) to allow concurrent access.
- **Parallel I/O libraries** (MPI-I/O, HDF5, NetCDF) for scalable read/write.
- **Optimised access patterns**: large, contiguous I/O preferred.

- **Metadata and structure** to make data understandable and reusable.
- **Portable, self-describing formats** to ensure long-term access.
- Balance **performance**, **resilience**, and **ease of use**.

**￼****￼****【****2024 Q3(c)(i)****】**

![Exported image](Exported%20image%2020260730194431-39.png)

File format recommendation for many small files on HPC.  
**【****2024 Q3(c)(ii)****】**  
Which parts of the data lifecycle will be on HPC system?  
**【****2024 Q3(c)(iii)****】**  
How to enable others to find the generated files?￼  
**(c)(i) What data format would you recommend and why?**

- **HDF5 / NetCDF**
- **Container format**==,== **self-describing**==, avoids== **many small files**

- Use **HDF5 or NetCDF** as a **container format**.
- Supports **structured data + metadata** in **fewer large files**.
- **Portable**, **self-describing**, and **parallel-ready**.
- Avoids performance problems caused by **very large numbers of small files** on parallel filesystems.
 
**(c)(ii) What parts of the data lifecycle are undertaken on the HPC system?**

- **Acquire**
- **Process / Analyse**
- **Assure**

- **Acquire**: data are **created/generated** (simulation, computation).
- **Process / Analyse**: applying computing power to create **new data from old**.
- **Assure**: **validate, test, check correctness** of generated data.
- HPC system mainly supports **compute-intensive stages** of the lifecycle.
 
**(c)(iii) How would you enable other scientists to find the generated files?**

- **Metadata**
- **Standard formats**
- **Persistent identifiers / repositories**

- **Describe** data using **metadata** (meaningful names, units, context).
- Use **standard, self-describing formats** (HDF5 / NetCDF).
- Publish or register data in a **repository** to support **Discover** stage.
- Ensure data are **discoverable and accessible** through description and

￼￼￼
 
**12. Messaging Systems****、****Kafka****￼**

![Exported image](Exported%20image%2020260730194432-40.png)

**【****2024 Q2(a)****】**  
Explain what a data messaging system is and when to use it.￼

- ==A messaging system stores data as messages for asynchronous service-to-service data streaming.==
- ==Used to handle high velocity or spiky data and decouple processing from data generation.==

- **What it is**
    - Stores data as messages
    - Asynchronous service-to-service data streaming
    - Acts as a data buffer
    - Messages stored in a log or queue until processed and deleted
- **When to use**
    - Decouple heavy data processing from data generation
    - Handle high velocity data
    - Handle spiky data

**【****2024 Q2(b)****】**  
What are the two main types of messaging systems?￼￼  
**What are the two main types of messaging systems?**

- ==Point-to-point message queues.==
- ==Publish/Subscribe messaging.==

- **Point-to-point message queues**
    - One message consumed by one application
    - Sender knows the receiver
- **Publish/Subscribe messaging**
    - Each message delivered to all interested applications
    - Producer is decoupled from consumers
 
**【****2024 Q2(c)****】**  
Describe Kafka.  
Include 5 design goals.￼  
**Describe Kafka. Include 5 design goals.**

- ==Kafka is a publish-subscribe messaging system.==
- ==Design goals: event-driven, decoupling, horizontal scalability, high throughput, fault tolerance.==

- **What Kafka is**
    - Publish-subscribe messaging system
    - Asynchronous message passing
    - Producer → Broker → Consumer
    - Messages stored in ordered topic logs
- **5 design goals**
    - Event-driven architecture
    - Completely decouple producers and consumers
    - Scale horizontally
    - Provide high throughput for producers and consumers
    - Provide fault tolerance via data replication
   

**13.** **研究数据管理（档案、可持续性、长期保存）**  
**【****2022 Q1(c)****】**

![Exported image](Exported%20image%2020260730194433-41.png)

Archival strategies for research dataset usability in the future.  
**【****2021 Q3****】****￼**

![Exported image](Exported%20image%2020260730194434-42.png)

End-to-end steps from data creation to long-term reuse (20 years).  
**【****2024 Q1(c)****】**

![Exported image](Exported%20image%2020260730194435-43.png)

Ensuring metadata remains both human-readable and machine-readable.  
**【****2024 Q3(b)****】**

![Exported image](Exported%20image%2020260730194438-44.png)

Importance of the data lifecycle.
 
1. **按知识点生成****“****高频考点总结** **+** **答案模板****”**
2. **按年份自动生成对应的模拟卷**
3. **为每个知识点写****“****极简速记** **+** **高频陷阱提示****”**
4. **为每题写满分答案（可长可短）供你背诵**

你希望我下一步做哪一种？
 \> 来自 \<[https://chatgpt.com/c/692f6ef6-f038-8328-88e1-92b9b4133597](https://chatgpt.com/c/692f6ef6-f038-8328-88e1-92b9b4133597)\>  

**总目录（按知识点分类）**
   

**Advantages:**  
**Disadvantages:**  
**Advantages****（直接来自课件特点）**  
**Disadvantages**  
==Four other approaches to storing data besides relational/NoSQL are:==
 
**逻辑点版答案（带一句话描述，够用就行）**
 
**Cassandra:**  
**Neo4j:**
 
**Data Model**  
**Query Language**  
**When to use / example**
   

==The four NoSQL data models are:==
 
Many NoSQL databases do not support ACID transactions because:
       
==Ultra-short version==  
Logic-point version
 
==Ultra-short version==  
Logic-point version
 
==Ultra-short version==  
Logic-point version
 
**Ultra-short version**
 
**1. Which phase of the data lifecycle uses Hadoop?**  
**2. How CSV can help in other lifecycle stages?**  
**3. How XML can help in other lifecycle stages?**  
**4. How JSON can help in other lifecycle stages?**  
State one principle for each (from the slide list):  
**Ultra-short version**  
Steps to ensure metadata is both human-readable and machine-readable (all from lecture content):  
**Ultra-short version**
 
(完全基于课件内容，不加入外部知识)
 
**Ultra-short version**  
(Strictly based on the Archiving & Preservation and Metadata lectures)
 
**Ultra-short version (for memorising)**
 
**Logical-point version**
   

**(i) HDFS storage**  
**(ii) Two MapReduce techniques for “similar papers”**
 
**(i) How HDFS stores files larger than the largest disk**
   

Step A: Gather all citations made by each paper  
Step B: Count all pairs that are cited together
 
Step A: Gather all papers that cite each cited DOI (invert citation dataset)  
Step B: Count all pairs of papers that cite the same cited DOI
    
**3(a) Stages**  
**Read**  
**Map**  
**Combine**  
**Reduce**
    
==i) Readers (3 examples):==  
**(i) Three reader types (what they do)**

- **Line-based text reader** (byte-offset key, line-text value)
- **Key/value per line reader** (key + separator + value)
- **Compressed binary reader** (SequenceFile)

**(ii) Easiest format to read with provided readers**

- **CSV** (fits line-based text readers most directly)￼￼￼
   

**Hadoop: most common lifecycle phase**  
**How CSV / XML / JSON help at other lifecycle stages**
 
**Ultra-short (memorise)**  
**Logic-point**
 
**Ultra-short (memorise)**  
**Logic-point**
   

**￼****Ultra-short (memorise)**  
**Logic-point**
 
**Benefits**  
**Drawbacks**  
**Benefits**  
**Drawbacks**
      

**Ultra-short (memorise)**  
**Parallel filesystem & configuration**  
**Software approaches**
   

**Ultra-short (memorise)**  
**Hardware considerations**  
**Software considerations**  
**Data model and usability**
 
**超短版（背诵）**  
**逻辑点版**  
**超短版（背诵）**  
**逻****辑点版**
 
**超短版（背诵）**  
**逻辑点版**
 
**超短版（背诵）**  
**逻辑点版**
 
**逻辑点版**  
**逻辑点版**  
**逻辑点版（严格贴课件思想）**
 
**逻辑点版**  
**What is 3rd Normal Form?**  
**How to change the structure to satisfy 3NF**  
**为什么这是** **3NF****？**
 
==STUDENT(====￼== ==StudentID PK,====￼== ==FirstName,====￼== ==Surname,====￼== ==HallID FK -\> HALL.HallID====￼====)==
   

==例子数据：==
   

==例子数据：==
   

==例子数据（由原始第二张表转换）：==
 
**逻辑点版（完全使用课件原文）**  
Many NoSQL databases do not support ACID transactions because:
   

**Ultra-short (memorise)**  
**Hardware considerations**  
**Software considerations**  
**Data model and usability**  
**Ultra-short (memorise)**  
**Logic-point**
 
**Ultra-short (memorise)**  
**Logic-point**
 
**Ultra-short (memorise)**  
**Logic-point**
 
**超短版（背诵）**  
**逻辑点版**  
**超短版（背诵）**  
**逻辑点版**
 
**超短版（背诵）**  
**逻辑点版**
    
如果你愿意，我可以进一步为你：
 \> 来自 \<[https://chatgpt.com/c/692f6ef6-f038-8328-88e1-92b9b4133597](https://chatgpt.com/c/692f6ef6-f038-8328-88e1-92b9b4133597)\>

![[Final_exam - Ink.svg]]

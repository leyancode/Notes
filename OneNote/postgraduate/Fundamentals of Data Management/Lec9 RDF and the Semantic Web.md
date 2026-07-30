下面为你整理成结构清晰、课程笔记风格的 Lecture 9 内容，保持中文讲解 + 关键概念英文原文，并完全保留重要术语的英文写法。无特殊符号。
 
**Lecture 9: RDF and the Semantic Web**  
（RDF 与语义网）  
**1. Semantic Data vs The Semantic Web**  
（语义数据 vs 语义网）

- 两者几乎是同义词（almost synonymous）。
- 在元数据（metadata）课程中提到：语义技术（semantic technologies）不仅可以用于 Web，也可以用于本地平台，例如文件元数据。
- 但语义技术最典型、最强大的应用场景仍然是在 Web（online）环境中。
 
**2. Linked Open Data****（****LOD****）**  
来自 Tim Berners-Lee 的 Linked Data 原则  
原文链接： [http://www.w3.org/DesignIssues/LinkedData.html](http://www.w3.org/DesignIssues/LinkedData.html)  
四条基本原则：

1. **Use URIs as names for things****￼**使用 URI 作为“事物”的唯一标识符。
2. **Use HTTP URIs so that people can look up those names****￼**URI 使用 HTTP 协议，这样用户和程序都可以访问并查询它。
3. **When someone looks up a URI, provide useful information using standards (RDF, SPARQL)****￼**当有人访问 URI 时，提供有用的、标准化的数据（如 RDF、SPARQL 返回的数据）。
4. **Include links to other URIs so that they can discover more things****￼**提供指向其他 URI 的链接，使数据可以被进一步探索。
 
**3. What is RDF?**  
（什么是 RDF？Resource Description Framework）  
RDF 是一种 **数据模型**，用于表达结构化信息。其核心是**Triple（三元组）**结构：  
**RDF Triple**  
**SUBJECT – PREDICATE – OBJECT**  
例子：  
The cat sat on the mat → SUBJECT: cat, PREDICATE: satOn, OBJECT: mat  
**各部分的类型要求：**

- **Subject****（主语）** 可以是：
    - URI 资源
    - blank node（空白节点）
- **Predicate****（谓语）** 必须是：
    - URI 资源
- **Object****（宾语）** 可以是：
    - URI 资源
    - blank node
    - literal（字面量，如字符串、数字）

**RDF** **作为图表示**  
RDF 图（RDF Graph）中：

- SUBJECT 和 OBJECT 是节点（nodes）
- PREDICATE 是边（edge）

图结构天然支持推理（inference）。  
**图示例**  
示意（讲座中的例子）：  
[http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburghhttp://dbpedia.org/resource/Scotlandhttp://dbpedia.org/property/capitalhttp://dbpedia.org/resource/University_of_Edinburghhttp://dbpedia.org/ontology/city)￼http://dbpedia.org/resource/Scotland￼http://dbpedia.org/property/capital￼http://dbpedia.org/resource/University_of_Edinburgh￼http://dbpedia.org/ontology/city￼  
RDF 图能够推理出：  
University of Edinburgh 位于 Scotland（因为 Edinburgh 是 Scotland 的 capital 或 city）。
 
**4. RDF ≠ RDF/XML**  
RDF 是数据模型，而 **RDF/XML** 只是 RDF 的一种 **序列化格式**（serialisation）。  
示例：  
\<rdf:Description rdf:about="http://dbpedia.org/resource/Scotland"\>￼ \<dbpedia-owl:capital rdf:resource="http://dbpedia.org/resource/Edinburgh" /\>￼\</rdf:Description\>￼  
注意：

- RDF/XML 是表达 RDF 的一种方式
- 但并不是 RDF 本身
 
**5. RDF Serialisation Formats**  
（RDF 序列化格式）  
常见序列化方式包括：

- Turtle
- N-Triples
- N-Quads
- JSON-LD
- N3
- RDF/XML

不同序列化表示同样的 RDF 数据。
 
**6. Turtle** **示例**  
Turtle 是一种更简洁的人类可读的 RDF 表示方式。  
示例：  
\<[http://dbpedia.org/resource/Scotland](http://dbpedia.org/resource/Scotland)\>￼ \<[http://dbpedia.org/ontology/capital](http://dbpedia.org/ontology/capital)\>￼ \<[http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburgh)\> .￼  
\<[http://dbpedia.org/resource/University_of_Edinburgh](http://dbpedia.org/resource/University_of_Edinburgh)\>￼ \<[http://dbpedia.org/ontology/city](http://dbpedia.org/ontology/city)\>￼ \<[http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburgh)\> .￼  
Turtle 具有以下特点：

- URI 可简写为 prefixes（讲座未涉及，但真实使用最常见）
- 更适合人工阅读和编写
- 常用于 Linked Data 发布与 SPARQL 存储接口

下面继续以课堂笔记的统一格式整理本部分内容，保持中英双文说明，并保留关键英文术语。
 
**7. Turtle****（续）**  
在 Turtle 中可以使用 **prefix****（前缀）** 简化 URI，使 RDF 表示更加简洁可读。  
示例：  
@prefix dbpRes \<[http://dbpedia.org/resource/](http://dbpedia.org/resource/)\> .￼@prefix dbpOnt \<[http://dbpedia.org/ontology/](http://dbpedia.org/ontology/)\> .￼  
dbpRes:Scotland dbpOnt:capital dbpRes:Edinburgh .￼dbpRes:University_of_Edinburgh dbpOnt:city dbpRes:Edinburgh .￼  
说明：

- @prefix 定义可复用的简写。
- dbpRes:Scotland 等价于完整 URI [http://dbpedia.org/resource/Scotland](http://dbpedia.org/resource/Scotland)。
- Turtle 允许换行、缩进等人类友好的格式。
- 更多 Turtle 规范可见： [http://www.w3.org/TR/turtle/](http://www.w3.org/TR/turtle/)
 
**8. Common Prefixes****（常用** **RDF** **前缀）**  
RDF 数据常使用一些标准命名空间前缀。下列是常见的几个：

|   |   |
|---|---|
|**Prefix**|**Namespace URI**|
|rdf|[http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)|
|rdfs|[http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)|
|owl|[http://www.w3.org/2002/07/owl#](http://www.w3.org/2002/07/owl#)|
|xsd|[http://www.w3.org/2001/XMLSchema#](http://www.w3.org/2001/XMLSchema#)|
|dc|[http://purl.org/dc/elements/1.1/](http://purl.org/dc/elements/1.1/)|
|foaf|[http://xmlns.com/foaf/0.1/](http://xmlns.com/foaf/0.1/)|

更多前缀查询工具：  
[http://prefix.cc](http://prefix.cc/)
 
**9. N-Triples**  
N-Triples 是 **Turtle** **的子集（****subset****）**，更加严格和简单：  
特点：

- **Line-based**：每个 triple 必须单独一行，不允许换行。
- 不允许使用 CURIE（Compact URI，如 dbpRes:Scotland），必须使用完整 URI。
- 语法比 Turtle 更冗长，但易于机器处理。

例如，以下写法（使用 CURIE）在 N-Triples 中是不合法的：  
dbpRes:Scotland dbpOnt:capital dbpRes:Edinburgh .￼  
必须写成完整 URI。
 
**10. RDF/XML**  
RDF/XML 是 RDF 的一种 **序列化格式**，使用 XML 来表达三元组。  
示例：  
\<?xml version="1.0"?\>￼\<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"￼ xmlns:dbpedia-owl="http://dbpedia.org/ontology/"\>￼  
\<rdf:Description rdf:about="http://dbpedia.org/resource/Scotland"\>￼ \<dbpedia-owl:capital￼ rdf:resource="http://dbpedia.org/resource/Edinburgh" /\>￼ \</rdf:Description\>￼  
\</rdf:RDF\>￼  
说明：

- 一个 RDF/XML 文档本质上就是一个 **XML** **文档**。
- 文档内容是 RDF，并使用 RDF 规范： [http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)
- 一个 triple 对应一个 \<rdf:Description\> 块：
    - rdf:about 表示 Subject
    - 子标签表示 Predicate
    - rdf:resource 或标签体表示 Object

在 RDF/XML 中也可以使用 prefix，例如 rdf、dbpedia-owl。
 
**11. ASIDE: Using curl to explore the Semantic Web**  
（使用 curl 探索语义网）  
命令行可以直接请求 RDF 数据：  
curl [http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)￼curl -i [http://www.w3.org/1999/02/22-rdf-syntax-ns#](http://www.w3.org/1999/02/22-rdf-syntax-ns#)￼curl -H "Accept: application/rdf+xml" [http://www.example.com/resource](http://www.example.com/resource)￼  
说明：

- -i 展示 HTTP 头信息。
- -H "Accept: ..." 可以指定希望服务器返回 RDF/XML、Turtle、JSON-LD 等格式。
 
**12. JSON-LD**  
（JSON for Linking Data）  
JSON-LD 是另一种 RDF 序列化方式，采用 JSON 语法，易于 Web 应用使用。  
示例：  
[{￼ "@context": {￼ "capital": "[http://dbpedia.org/ontology/capital](http://dbpedia.org/ontology/capital)"￼ },￼ "@id": "[http://dbpedia.org/resource/Scotland](http://dbpedia.org/resource/Scotland)",￼ "capital": "[http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburgh)"￼},{￼ "@context": {￼ "city": "[http://dbpedia.org/ontology/city](http://dbpedia.org/ontology/city)"￼ },￼ "@id": "[http://dbpedia.org/resource/University_of_Edinburgh](http://dbpedia.org/resource/University_of_Edinburgh)",￼ "city": "[http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburgh)"￼}]￼  
特点：

- 使用 @context 定义前缀。
- @id 表示 Subject。
- 属性名即 Predicate，其值可为 URI 或 Literal。
- 与现代 Web API、高层应用结合度高，是 RDF 最流行的格式之一。
 
**13. RDF in Action****（实际应用案例）**  
RDF 技术与 Linked Data 已广泛用于许多开放数据或知识库项目：

- DBpedia
- MusicBrainz
- BBC 数据平台
- data.gov.uk
- Freebase（Google Knowledge Graph 的前身）
- FOAF（Friend of a Friend）
- US Census data
- CiteSeer
- GeoNames
- YAGO
- CIA Factbook
- 其他大量开放知识库
 
**14. BBC Example****（****BBC** **使用** **RDF** **的示例）**  
BBC 为其内容构建了 RDF 数据，并提供可访问的 URI。  
示例资源页面：  
[https://www.bbc.co.uk/things/68f083a5-ac60-4035-9d85-353e67a643b4](https://www.bbc.co.uk/things/68f083a5-ac60-4035-9d85-353e67a643b4)  
请求其 Turtle 表示：  
curl [https://www.bbc.co.uk/things/68f083a5-ac60-4035-9d85-353e67a64.ttl](https://www.bbc.co.uk/things/68f083a5-ac60-4035-9d85-353e67a64.ttl)￼  
BBC 的本体库：  
[www.bbc.co.uk/ontologies](http://www.bbc.co.uk/ontologies)  
URI 设计指南：  
[https://www.w3.org/Provider/Style/URI](https://www.w3.org/Provider/Style/URI)  
下面继续以课堂笔记的格式整理本部分内容，保持中文讲解、英文关键术语不翻译，并保持整体清晰、结构化。
 
**15. RDF Vocabularies****（****RDF** **词汇表** **/** **词汇体系）**  
RDF Vocabulary 指用于 RDF 图中的一组 **IRIs** **集合**，用于定义可复用的术语（例如类、属性）。  
官方定义（来自 _RDF 1.1 Concepts and Abstract Syntax_）：  
“An RDF vocabulary is a collection of IRIs intended for use in RDF graphs.  
For example, the IRIs documented in [RDF11-SCHEMA] are the RDF Schema vocabulary. RDF Schema can itself be used to define and document additional RDF vocabularies.”  
参考：  
[http://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/](http://www.w3.org/TR/2014/REC-rdf11-concepts-20140225/)  
常见 RDF vocabularies：

- **RDF Schema (RDFS)**
- **OWL**
- **FOAF**
- **SKOS**
- **Dublin Core (DC)****￼**等。

RDF vocabularies 用于：

- 定义类（classes）
- 定义属性（properties）
- 定义资源之间的语义关系
 
**16. OWL: The Web Ontology Language**  
OWL 是语义网最核心的本体语言（ontology language）。  
OWL 的作用：

- **定义术语（****terminology****）**：用于 RDF 文档中的类、属性、关系。
- **定义类别层次（****class taxonomies****）**：例如继承关系、分类结构。
- **定义属性层次（****property taxonomies****）**。
- OWL 文档本质上也是 **RDF** **文档**（用 RDF 方式序列化）。

OWL 提供更强的表达能力，例如：

- 类之间的逻辑关系（equivalentClass, disjointWith）
- 属性约束（domain, range, functionalProperty）
- 更丰富的推理能力（reasoning）
 
**17. Storage of Semantic Data****（语义数据的存储方式）**  
语义网数据通常有两种存储方式：  
**1.** **文件存储（****File-based Storage****）**  
使用 RDF 的序列化格式，例如：

- RDF/XML
- Turtle
- JSON-LD
- N-Triples

适合静态数据或文件级分发。  
**2. Triplestore**  
一种专门用于存储 RDF 的 **图数据库（****graph database****）**。  
特点：

- 三元组存储
- 支持 SPARQL 查询
- 常用于大规模语义数据（例如 DBpedia）

**分布式特性**  
语义网强调“链接”：

- 一个 RDF 声明（triple）可以跨三个不同的服务器
- 数据不需要集中在一个地方
- 多个机构共同构成 Linked Data 网络
 
**18. Accessing Semantic Data****（访问语义数据）**  
主要方式包括：  
**1. Bulk RDF Download**  
一次性下载全量 RDF 文件（如 .ttl、.rdf、.jsonld）。  
**2. Web Query****（****HTTP GET****）**  
通过 HTTP 协议请求 RDF：  
GET Accept: application/xml+rdf￼  
使用 cURL 示例：  
curl -H "Accept: application/rdf+xml" [http://example.com/resource](http://example.com/resource)￼  
**3. SPARQL**  
一个专为 RDF 设计的查询语言，用于访问 triplestore。
 
**19. Requesting with HTTP GET****（****HTTP GET** **请求** **RDF** **数据）**  
语义网中：

- URI 代表资源（resource），而不是网页。
- URI 通常指代一个“真实的对象”（例如城镇），而非具体文档。

示例：  
访问：  
[http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburgh)￼  
你访问的不是“网页”，而是“爱丁堡这座城市”的标识。  
浏览器会获得一个 **303 See Other** 重定向，跳到：  
[http://dbpedia.org/page/Edinburgh](http://dbpedia.org/page/Edinburgh)￼  
说明：

- /resource/ 用于标识资源
- /page/ 用于返回 HTML 页面

使用 cURL 可直接看到 HTTP 响应头及 303：  
curl -I [http://dbpedia.org/resource/Edinburgh](http://dbpedia.org/resource/Edinburgh)￼
 
**20. Requesting with HTTP GET (2): Fragment Identifiers****（使用** **Fragment** **区分资源）**  
语义网中常使用 fragment 标识符（#）来区分“资源本身”和“该资源的文档”。  
例如：  
[http://www.w3.org/2000/01/rdf-schema#Literal](http://www.w3.org/2000/01/rdf-schema#Literalhttp://www.w3.org/2000/01/rdf-schema#Resource)￼http://www.w3.org/2000/01/rdf-schema#Resource￼  
HTTP 协议规定：

- fragment（#后部分）不会发送给服务器
- 实际请求的是：

[http://www.w3.org/2000/01/rdf-schema](http://www.w3.org/2000/01/rdf-schema)￼  
意思是：

- 服务器返回包含 RDF 内容的“文档”
- 客户端根据 fragment 找到对应的资源表示

更多说明：  
[http://www.w3.org/TR/cooluris/](http://www.w3.org/TR/cooluris/)
 
**21. Content Negotiation****（内容协商）**  
Content Negotiation 允许使用同一个 URI ，返回不同格式的表示（representations）。  
例如访问：  
[https://www.foo.com/rdf](https://www.foo.com/rdf)￼  
根据请求头不同：

- cURL → 可能获得 Turtle
- Web 浏览器 → 获得 HTML
- Python API → 获得 JSON 或 JSON-LD

依赖 HTTP Header：  
Accept: text/turtle￼Accept: application/rdf+xml￼Accept: application/ld+json￼  
作用：

- 避免出现多个 URI 指向同一个资源
- 统一标识、提升互操作性（interoperability）
- 是标准 Web 技术

但缺点：

- 不容易发现所有可用格式
- 不同服务器的支持程度不同
 
**22. SPARQL****（****RDF** **查询语言）**  
SPARQL 对 RDF 的作用类似 SQL 对关系型数据库。  
**SPARQL** **查询类型：**

1. **SELECT Query****￼**返回表格（tabular）形式的结果。￼类似 SQL 的 SELECT。
2. **CONSTRUCT Query****￼**生成一个新的 RDF graph。￼常用于数据转换、清洗、构建新本体。
3. **DESCRIBE Query****￼**返回一个资源的相关 RDF 声明（关于该资源的所有 triples）。
4. **ASK Query****￼**返回 true/false￼常用于验证某种模式是否存在。

下面继续按课堂笔记格式，将 SPARQL、语义推理（Semantic Reasoning）与课程总结整理清晰，采用中文讲解 + 英文关键术语保留的方式。
 
**23. SPARQL Example****（****SPARQL** **查询示例）**  
给定 RDF 数据：  
:t1 foaf:name "Chris Wood" .￼:t1 foaf:based_near :Edinburgh .￼  
:t2 foaf:name "Barack Obama" .￼:t2 foaf:based_near :WashingtonDC .￼  
示例查询：  
SELECT ?name￼WHERE {￼ ?x foaf:name ?name￼}￼  
返回结果：

- Chris Wood
- Barack Obama

**关于前缀** **“:”** **的说明**

- 以 “:” 开头的资源名（如 :t1、:Edinburgh），代表其全 URI **由当前文档的** **base URI** **进行补全**。
- 这些资源（如 :Edinburgh）在文档其他部分应当被完整定义。
 
**24. Format of a SPARQL Query**  
（SPARQL 查询格式）  
一个完整的 SPARQL 查询通常包含以下部分：  
PREFIX ns1: \<...\>￼PREFIX ns2: \<...\>￼...￼  
SELECT ...￼FROM \<...\>￼FROM NAMED \<...\>￼WHERE {￼ ...￼}￼  
GROUP BY ...￼HAVING ...￼ORDER BY ...￼LIMIT ...￼OFFSET ...￼VALUES ...￼  
常见功能说明：

- **PREFIX**：声明命名空间前缀
- **FROM / FROM NAMED**：可用于定义查询的数据集
- **WHERE**：核心 pattern matching 匹配区域
- **GROUP BY / HAVING**：分组与约束
- **ORDER BY**：排序
- **LIMIT / OFFSET**：分页
- **VALUES**：临时绑定变量值

提示：在多数 SPARQL Endpoint 上，数据集由系统预先定义，因此 FROM 部分往往省略。
 
**25. SPARQL over HTTP**  
（SPARQL 作为协议）  
SPARQL 不只是查询语言，同时也是一种 **协议**（protocol），正式名称为：  
**SPARQL over HTTP**  
它定义了如何通过 HTTP 发送查询。  
标准描述（来自 W3C）：  
“SparqlQuery is the protocol's only interface. It contains one operation, query,  
which is used to convey a SPARQL query string and, optionally,  
an RDF dataset description.”  
参见：  
[http://www.w3.org/TR/rdf-sparql-protocol/](http://www.w3.org/TR/rdf-sparql-protocol/)  
查询通常通过 URL 编码（URL encoding）发送，如：  
[http://example.com/sparql?query=SELECT+*+WHERE+%7B+...+%7D](http://example.com/sparql?query=SELECT+*+WHERE+%7B+...+%7D)￼  
也可以使用 POST 方式发送复杂查询。
 
**26. Semantic Reasoning****（语义推理）**  
当事实是机器可读并具有标准化语义时，可以进行自动推理（inference）。  
示例：

- Chris Wood lives in Edinburgh
- Edinburgh is in Scotland
- 推理：Chris Wood lives in Scotland

推理过程需要 **Inference Engine****（推理引擎）**，例如 OWL reasoner：

- Pellet
- Hermit
- Fact++

**推理规则示例（****RDFS** **风格）**  
如果：  
?A rdfs:subClassOf ?B .￼?x rdf:type ?A .￼  
则可推断：  
?x rdf:type ?B .￼  
这类规则允许构建层级结构（hierarchies），并自动扩展事实。
 
**27. Conclusions****（总结）**  
本讲的主要内容如下：

- RDF 是一种语义数据模型，用三元组表示资源之间的关系。
- 每个资源（thing）应具有唯一的 URI。
- RDF 可以通过多种序列化格式（Turtle、RDF/XML、JSON-LD 等）存储。
- 内容协商（content negotiation）与 URI 重定向机制允许浏览器、脚本、API 根据请求获得适合的格式（HTML、JSON-LD、Turtle 等）。
- RDF vocabularies（如 RDFS、OWL）用于定义术语和资源的语义。
- OWL 可表达更复杂的逻辑结构与本体。
- SPARQL 是 RDF 的查询语言，类似于 SQL 对关系数据库的作用。
- 通过本体（ontology）与推理（reasoning），可从已有事实中自动得出新结论。
 
如果你希望，我可以继续整理成：

- 一页 A4 的 RDF & SPARQL 速查表（Cheat Sheet）
- RDF vs RDFS vs OWL 的对比表
- SPARQL 常用查询模式合集（如 OPTIONAL、FILTER、UNION、GRAPH）

告诉我你需要哪一种即可。
 \> 来自 \<[https://chatgpt.com/c/69322968-6328-8330-b9c3-ea29e7ec29e5](https://chatgpt.com/c/69322968-6328-8330-b9c3-ea29e7ec29e5)\>
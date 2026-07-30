下面为你将 MapReduce 这一整段课堂内容按课程笔记风格整理成**结构化中文笔记（保留关键英文原文）**，内容清晰、分层明确，可直接加入你的学习文档。
 
**MapReduce**  
**Fundamentals of Data Management**  
授课者：Adam Carter
 
**1. What is MapReduce?**  
**什么是** **MapReduce****？****￼**

![Exported image](Exported%20image%2020260730192153-0.png)

• MapReduce 是一种适用于 **distributed systems** 的并行化模式。  
• 它是一种 **programming paradigm****（编程范式）**，是一种思考问题的方式。  
• 程序员只需要提供 **map** 和 **reduce** 函数，其余如调度和数据移动由框架自动处理。  
• 问题的并行化拆分方式保持稳定，但实际数据流动取决于数据特性。  
• 最初由 Google 设计用于解决特定问题，但适用范围极广。
 
**2. Map** **阶段**  
**Map** **的作用与形式****￼**

![Exported image](Exported%20image%2020260730192154-1.png)

• Map 阶段对所有输入数据运行同一个函数，即将函数 “map” 到每一份数据上。  
例：  
若 f(x) = x * x，  
map(f, [1,2,3,4,5]) = [1,4,9,16,25]  
• MapReduce 中的 Map 函数必须返回：  
**List of (key, value) pairs**  
key/value 通常从输入数据中提取。  
• 大规模 MapReduce 中，Map 函数会在存储输入数据的每个节点上执行（数据本地化原则）。
 
**3. Reduce** **阶段**  
**Reduce** **的作用****￼**

![Exported image](Exported%20image%2020260730192156-2.png)

• Reduce 将 Map 输出的数据进行汇总（summary）。  
• 单纯 reduce 示例：  
reduce(+, [1,2,3]) = 6  
• MapReduce 的 reduce 不是返回单个值，而是：  
对每个 **key** 生成一个单一的 value → 输出仍是 (key, value) pairs。
 
**4. MapReduce Pattern****（模式）****￼**

![Exported image](Exported%20image%2020260730192157-3.png)

完整 MapReduce 模式要求：  
Map \<Key1 : Value1\> → List(\<Key2 : Value2\>)￼Reduce \<Key2 : List(Value2)\> → List(\<Key3 : Value3\>)  
• Map 与 Reduce 都必须是 **stateless**（无状态）。  
• 框架确保所有相同 Key2 的数据会被自动分组，然后才调用 Reducer。  
• 对每一个唯一的 Key2，只会触发一次 Reduce。  
**示例（****Counting words****）**  
输入句子：  
Key1 = 行号  
Value1 = 文本  
Map 输出每个单词：  
\<Key2 : Word, Value2 : 1\>  
Reduce 汇总得到每个单词出现次数：  
\<Key3 : Word, Value3 : count\>  
示例：  
Map 输入：  
\<223, "shop at my shop"\>  
Map 输出：  
[\<shop,1\>, \<at,1\>, \<my,1\>, \<shop,1\>]  
Reduce 输入：  
\<shop, [1,1]\>  
Reduce 输出：  
\<shop,2\>￼￼
   

**5.** **例题：****Counting Words with MapReduce**  
**使用** **MapReduce** **统计单词出现次数****￼**

![Exported image](Exported%20image%2020260730192201-4.png)

**Map Inputs → Map Outputs**  
\<0 : "A boy drove a car"\> ￼→ [\<a,1\>, \<boy,1\>, \<drove,1\>, \<a,1\>, \<car,1\>]  
\<1 : "A car drove at a bus"\> ￼→ [\<a,1\>, \<car,1\>, \<drove,1\>, \<at,1\>, \<a,1\>, \<bus,1\>]  
\<2 : "Can a boy drive a car?"\> ￼→ [\<can,1\>, \<a,1\>, \<boy,1\>, \<drive,1\>, \<a,1\>, \<car,1\>]  
\<3 : "A danger – a banana!"\> ￼→ [\<a,1\>, \<danger,1\>, \<a,1\>, \<banana,1\>]  
**Reduce Inputs → Reduce Outputs**  
\<a, [1,1,1,1,1,1,1,1]\> → \<a, 8\>￼\<at, [1]\> → \<at,1\>￼\<banana, [1]\> → \<banana,1\>￼\<boy, [1,1]\> → \<boy,2\>￼...￼\<car, [1,1,1]\> → \<car,3\>￼...
 
**6. MapReduce Exercise 1****￼**

![Exported image](Exported%20image%2020260730192202-5.png)

**练习** **1****：统计每个专利被引用次数**  
数据来自 US National Bureau of Economic Research：  
[http://www.nber.org/patents/](http://www.nber.org/patents/)  
格式：  
CITING, CITED￼3858241, 956203￼3858241, 1324234￼3858242, 1515701￼3858244, 956203  
**任务**  
统计每个 patent 被引用（CITED）多少次。  
不需输出那些从未被引用的专利。  
**期望输出：**  
956203, 2￼1515701, 1￼1324234, 1
 
**6.1 Exercise 1: Solution****（解答）****￼**

![Exported image](Exported%20image%2020260730192203-6.png)

**Reader**  
(key, value) 均为 Integer。  
**Map**  
类型：\<Integer,Integer\> → List(\<Integer,Integer\>)  
从输入 \<citing, cited\> 提取 cited，输出 \<cited,1\>。  
**Reduce**  
类型：\<Integer,List(Integer)\> → List(\<Integer,Integer\>)  
对 value 列表求和。  
**示例：**  
Map 输入：  
\<3858241, 956203\>  
输出：  
[\<956203,1\>]  
Reduce 输入：  
\<956203, [1,1,1,1]\>  
输出：  
\<956203, 4\>
 
**7. MapReduce Exercise 2****￼**

![Exported image](Exported%20image%2020260730192204-7.png)

**练习** **2****：反转引用关系**  
任务：  
对于每个专利，输出引用它的所有专利列表。  
**期望输出**  
956203, 3858241, 3858244￼1515701, 3858242￼1324234, 3858241
 
**7.1 Exercise 2: Solution****（解答）****￼**

![Exported image](Exported%20image%2020260730192205-8.png)

**Reader**  
key 和 value 依然为 Integer。  
**Map**  
\<Integer,Integer\> → List(\<Integer,Integer\>)  
输出：  
\<cited, citing\>  
**Reduce**  
\<Integer,List(Integer)\> → List(\<Integer,String\>)  
将 citing 列表拼接为字符串。  
**示例：**  
Map 输入：  
\<3858241, 956203\>  
输出：  
[\<956203,3858241\>]  
Reduce 输入：  
\<956203, [3858241, 3858244]\>  
输出：  
[\<956203, "3858241, 3858244"\>]
 
下面为你将“Finding similar patents（寻找相似专利）”这一部分整理成**清晰的课堂笔记格式**，包括两种 MapReduce 策略、输入输出示例，以及 “MapReduce at scale” 的关键说明。全程保持中英双文、保留技术关键词，方便你直接使用在课堂笔记中。
 
**Finding Similar Patents**  
**查找相似专利（基于引用关系）****￼**

![Exported image](Exported%20image%2020260730192206-9.png)

**1. Patent Data & Citation Records**  
专利数据与引用记录  
给定专利引用记录格式为：  
“CITING”, “CITED”  
3858241, 956203￼3858241, 1324234￼3858242, 151570￼3858244, 956203  
**问题：如何判断哪些专利是相似的？**  
一种常见思想：如果两个专利经常被同一批专利引用，或者它们都引用相同的专利，那么它们在技术内容上很可能相关。
 
**2. Strategy 1: Patents Frequently Cited Together**  
**策略** **1****：经常被共同引用的专利（****cited together****）**  
此策略使用 MapReduce 找出**在被引用时常常成对出现**的专利。￼

![Exported image](Exported%20image%2020260730192308-10.png)

**Step 1: Gather all citations made by each patent**  
收集每个专利引用的所有其他专利。  
Map 输入与输出：  
Map Input:￼\<“1111”, “9999”\>  
Map Output:￼[\<“1111”, “9999”\>]  
Reduce 输入与输出：  
Reduce Input:￼\<“1111”, [“9999”, “2222”, “7777”]\>  
Reduce Output:￼[\<“1111”, “9999, 2222, 7777”\>]  
即：对每个 CITING 专利整理它引用的所有 CITED 列表。
 
**Step 2: Count all pairs that are cited together**  
对每一条引用列表，生成所有被共同引用的 pair 并计数。  
示例（Map 阶段）：  
Map Input:￼\<“1111”, “9999, 2222, 7777”\>  
Map Output:￼[￼ \<“2222+9999”, 1\>,￼ \<“2222+7777”, 1\>,￼ \<“7777+9999”, 1\>￼]  
（组合逻辑：从 {9999, 2222, 7777} 中生成所有 pair）  
Reduce 阶段汇总 pair 出现次数：  
Reduce Input:￼\<“2222+9999”, [1,1,1,1]\>  
Reduce Output:￼[\<“2222+9999”, 4\>]  
含义：专利 2222 与 9999 共被引用了 4 次，因此可能技术相关。
 
**3. Strategy 2: Patents Frequently Citing Same Patents****￼**

![Exported image](Exported%20image%2020260730192312-11.png)

**策略** **2****：经常引用同一批专利的专利（****citing the same patents****）**  
逻辑与策略 1 类似，但**方向反转**：  
不是看谁被一起引用，而是看谁一起引用别人。  
**Step 1: Gather all patents that cite each patent**  
整理“被引用专利 → 引用它的所有专利”。  
Map 输入与输出：  
\<“1111”, “9999”\> → [\<“9999”, “1111”\>]  
Reduce 输入与输出：  
\<“9999”, [“1111”, “3333”, “8888”]\>￼→ [\<“9999”, “1111, 3333, 8888”\>]  
即：9999 这个被引用专利被 1111、3333、8888 所引用。
 
**Step 2: Count all pairs of patents that cite the same item**  
对引用同一个专利的 CITING 列表进行 pair 组合与计数：  
Map 阶段：  
\<“9999”, “1111, 3333, 8888”\>￼→ [￼ \<“1111+3333”, 1\>,￼ \<“1111+8888”, 1\>,￼ \<“3333+8888”, 1\>￼ ]  
Reduce 阶段：  
\<“1111+3333”, [1,1,1,1]\>￼→ \<“1111+3333”, 4\>  
含义：1111 与 3333 多次共同引用 9999，因此它们可能相似。
 
**4. Comparison of Two Strategies**  
**两种策略对比总结**

|   |   |   |
|---|---|---|
|**Strategy**|**核心思想**|**发现的相似性类型**|
|Frequently Cited Together|被同样的专利共同引用|技术内容可能属于相同问题域|
|Frequently Citing Same Patents|引用同一批专利|技术来源或基础类似|

两者结合可以构建 patent similarity graph 或推荐系统。
 
**5. MapReduce at Scale**  
**大规模下的** **MapReduce** **特点****￼**

![Exported image](Exported%20image%2020260730192314-12.png)

• Map 与 Reduce 函数为 **stateless**，便于 massive parallelisation（海量并行）。  
• Input data 被并行送入多个 Map 任务执行。  
• Map 完成后，系统必须执行 **shuffle + group by key**，即：  
– 根据 key 对所有 Map 输出重新分区与分组  
– 这是 MapReduce 中最昂贵的阶段（网络传输量巨大）  
图示说明要点：  
Input Data → 多个 Map → Shuffle / Group → 多个 Reduce → 输出结果。  
**MapReduce at Scale**  
**大规模** **MapReduce** **执行特点**  
**1. Stateless Functions**  
• Map 与 Reduce 函数必须是 **stateless**（无状态）。  
• 无状态意味着它们可以被自由分配到许多节点执行，从而支持 **massive parallelisation**（大规模并行）。  
**2. Shuffle Phase is Expensive**  
• 在 Map 阶段与 Reduce 阶段之间有一个关键步骤：  
**grouping and moving data**（分组与数据移动）。  
• 该过程通常称为 **shuffle** 或 **shuffle and sort**。  
• Shuffle 会跨节点移动大量 key-value 数据，是 MapReduce 中最昂贵的阶段。  
示意图概念：  
Input Data → 多个 Map → Shuffle（key-based routing）→ 多个 Reduce → Output  
。
 
**Joining Multiple Data Sets**  
**多数据集连接（****Inner Join / MapReduce Join Strategies****）**  
本节主要讲解如何在分布式环境（如 Hadoop MapReduce）中进行**join** **操作**，尤其是当数据不在同一节点时。

![Exported image](Exported%20image%2020260730192315-13.png)

**1. Inner Join****（内连接）**  
示例数据：  
Customers（客户表）  
1, Stephanie Leung, 555-555-555 ￼2, Edward Kim, 123-456-7890 ￼3, Jose Madriz, 281-330-8004 ￼4, David Stork, 408-555-000  
Orders（订单表）  
3, A, 12.95, 02-Jun-2008 ￼1, B, 88.25, 20-May-2008 ￼2, C, 32.00, 30-Nov-2007 ￼3, D, 25.02, 22-Jun-2009  
**Inner Join** **结果**  
基于第一列 customer ID，输出：  
1, Stephanie Leung, 555-555-555, B, 88.25, 20-May-2008 ￼2, Edward Kim, 123-456-7890, C, 32.00, 30-Nov-2007 ￼3, Jose Madriz, 281-330-8004, A, 12.95, 02-Jun-2008 ￼3, Jose Madriz, 281-330-8004, D, 25.02, 22-Jun-2009
 
**2. Reduce-Side Join****（在** **Reduce** **阶段完成** **Join****）**  
适用于：**两边数据都很大**、无法在 Map 阶段完成 join。￼

![Exported image](Exported%20image%2020260730192316-14.png)

核心思想：

1. 在 Map 阶段为每条记录添加**tag**，标明它来自哪张表（Customers 或 Orders）。
2. 按 join key（例如 Customer ID）作为 Map 输出 key。
3. Shuffle 阶段会将相同 key 的记录发送到同一个 Reducer。
4. Reducer 对来自不同数据集的记录进行 join。
 
**2.1 Reduce-side join: Repartitioned Join 1****（第一阶段）**  
**Step 1. Map** **阶段添加** **tag**  
例如：  
Customers：  
1 → \<1, ("Customers", Stephanie Leung, 555-555-555)\>￼2 → \<2, ("Customers", Edward Kim, 123-456-7890)\>  
Orders：  
3 → \<3, ("Orders", A, 12.95, 02-Jun-2008)\>￼1 → \<1, ("Orders", B, 88.25, 20-May-2008)\>  
**Step 2. Shuffle + Reduce**  
所有 key=1 的记录被发送到 Reducer 1：  
Customers, 1, Stephanie Leung, 555-555-555 ￼Orders, 1, B, 88.25, 20-May-2008  
Reducer 输出 join 结果。

![Exported image](Exported%20image%2020260730192318-15.png)

**2.2 Reduce-side join: Repartitioned Join 2****（第二阶段）**  
Reduce 会产生**cross-product****（笛卡尔积）**：  
即对于相同 key 的 Customers 与 Orders 的所有匹配组合全部输出。  
示例 key=3：  
Customers:  
3, Jose, 281-330-8004  
Orders:  
3, D, 25.02, 22-Jun-2009 ￼3, A, 12.95, 02-Jun-2008  
Reduce 输出两行：  
3, Jose, 281-330-8004, D, 25.02, 22-Jun-2009 ￼3, Jose, 281-330-8004, A, 12.95, 02-Jun-2008  
然后可由第二次 Map 实现不同的 join 类型（inner / outer）。  
Hadoop 提供支持这些模式的专用类。
 
**3. Map-Side Join****（在** **Map** **阶段完成** **Join****）****￼**

![Exported image](Exported%20image%2020260730192319-16.png)

适用于：  
• **一个大数据集** **+** **一个小数据集**  
• 小数据集可以复制到所有节点（Distributed Cache）  
**原理**

1. 将小数据集（例如 Customers）放入 Hadoop Distributed Cache，复制到所有 Mapper 节点。
2. Map 读取大数据集（例如 Orders），并在本地查找 join key。
3. 在 Map 阶段直接输出 join 结果；无需 Shuffle、无需 Reduce。

**优点**  
• 避免昂贵的 shuffle data movement  
• 性能极高
 
**4. Alternatives When Replication Join is Not Possible****￼**

![Exported image](Exported%20image%2020260730192320-17.png)

**当小表仍然太大无法复制时的替代方案**  
**方案** **1****：****Map-side filtering**  
• 在 Map 端过滤数据，只保留需要 join 的记录  
例：只保留“住在 Scotland 的 customers”。  
这样能减少参与 join 的数据量。
 
**方案** **2****：****Replicate only join keys**  
• 不复制完整记录，只复制 join keys 列表  
• 只有真正需要的记录才会参与数据传输（降低网络成本）
 
**方案** **3****：使用** **Bloom Filter****（近似过滤结构）**  
适用于 join keys 仍然太大时。  
Bloom filter 可以快速判断一个 key 是否可能存在：  
BloomFilter.contains(x):  
• 如果返回 false → x 一定不在集合中  
• 如果返回 true → x “可能在” 集合中（但有可能误报 false positive）  
Bloom filter 的优势：  
• 占用空间小  
• 可用于大规模数据过滤，减少 Shuffle 阶段的数据量  
• 可将数据集缩小到可以执行 replicated join 的范围
 
**5. Bloom Filter****（布隆过滤器）****￼**

![Exported image](Exported%20image%2020260730192324-18.png)

布隆过滤器是一种**概率型**数据结构，用位数组 + 多个 hash 函数实现。  
原理简述：

1. 向 filter 插入元素时，用多种 hash 函数计算多个位置并置为 1
2. 查询元素时，只需检查同样的 hash 函数对应的位
3. 如果任意一个位为 0 → 该元素一定不存在
4. 如果所有位均为 1 → 元素可能存在（存在 false positive）

用途：  
• 在 MapReduce join 前过滤掉不必要的数据  
• 在分布式系统中减少数据传输成本
 
**完整小结：****Join Strategies in MapReduce**

|   |   |   |   |
|---|---|---|---|
|**Join Strategy**|**适用场景**|**优点**|**缺点**|
|Reduce-side Join|两边数据大|通用、无需数据预先分布|Shuffle 成本高|
|Map-side Join|大表 + 小表|无需 Shuffle，效率极高|小表必须能复制到所有节点|
|Filtering + Join|数据量过大|降低 join 输入量|增加预处理逻辑|
|Bloom Filter Join|join key 大|进一步减少数据传输|有 false positive|

下面为你将本批内容整理为**课堂笔记格式（中英双文，结构清晰、无特殊符号）**。  
内容涵盖：MapReduce at scale、Inner Join、Finding Similar Patents 两种策略、MapReduce 数据流特点。  
你可以直接复制到你的课程笔记中，与前面整理的章节保持一致。
   

**Joining Multiple Data Sets: Inner Join**  
**多数据集连接：****Inner Join** **的逻辑**  
假设两张表：  
**Customers**  
1, Stephanie Leung, 555-555-555 ￼2, Edward Kim, 123-456-7890 ￼3, Jose Madriz, 281-330-8004 ￼4, David Stork, 408-555-000  
**Orders**  
3, A, 12.95, 02-Jun-2008 ￼1, B, 88.25, 20-May-2008 ￼2, C, 32.00, 30-Nov-2007 ￼3, D, 25.02, 22-Jun-2009  
Inner Join based on customer ID：  
**Join Output**  
1, Stephanie Leung, 555-555-555, B, 88.25, 20-May-2008 ￼2, Edward Kim, 123-456-7890, C, 32.00, 30-Nov-2007 ￼3, Jose Madriz, 281-330-8004, A, 12.95, 02-Jun-2008 ￼3, Jose Madriz, 281-330-8004, D, 25.02, 22-Jun-2009  
概念总结：  
• Inner Join 只保留两边数据都有相同 key 的记录。  
• 多对一、多对多均能生成多条输出记录。
 
**Finding Similar Patents**  
**使用** **MapReduce** **查找相似专利**  
给定 citation 数据格式（CITING, CITED）：  
3858241, 956203 ￼3858241, 1324234 ￼3858242, 151570 ￼3858244, 956203  
问题：怎样识别“相似专利”？  
常见策略有两类：
 
**Strategy 1: Patents Frequently Cited Together**  
**策略** **1****：经常被一起引用的专利（****frequently cited together****）**  
**Step 1:** **为每个** **citing** **专利收集它引用的所有专利**  
Map 输入 → 输出  
\<“1111”, “9999”\> → [\<“1111”, “9999”\>]  
Reduce 输入 → 输出  
\<“1111”, [“9999”, “2222”, “7777”]\> ￼→ [\<“1111”, “9999, 2222, 7777”\>]  
即：整理 citing → list of cited。
 
**Step 2:** **从** **citation list** **中构造所有** **co-cited pairs**  
Map 输入 → 输出  
\<“1111”, “9999, 2222, 7777”\>￼→ [￼ \<“2222+9999”, 1\>,￼ \<“2222+7777”, 1\>,￼ \<“7777+9999”, 1\>￼ ]  
Reduce 输入 → 输出  
\<“2222+9999”, [1,1,1,1]\> → \<“2222+9999”, 4\>  
含义：  
2222 与 9999 共同被引用了 4 次 → 可能技术相关。
 
**Strategy 2: Patents Frequently Citing Same Patents**  
**策略** **2****：共同引用相同目标专利的专利（****frequently citing same patents****）**  
**Step 1:** **收集每个被引用专利的** **citing** **列表**  
Map 输入 → 输出  
\<“1111”, “9999”\> → [\<“9999”, “1111”\>]  
Reduce 输入 → 输出  
\<“9999”, [“1111”, “3333”, “8888”]\>￼→ [\<“9999”, “1111, 3333, 8888”\>]  
即：将 CITED → list of CITING。
 
**Step 2:** **生成** **pairs of patents that cite the same one**  
Map 输入 → 输出  
\<“9999”, “1111, 3333, 8888”\>￼→ [￼ \<“1111+3333”, 1\>,￼ \<“1111+8888”, 1\>,￼ \<“3333+8888”, 1\>￼ ]  
Reduce 输入 → 输出  
\<“1111+3333”, [1,1,1,1]\> → \<“1111+3333”, 4\>  
含义：  
1111 与 3333 多次共同引用 9999 → 技术特征可能类似。
 
**Key Insight**  
**两种相似专利识别思路总结**

|   |   |   |
|---|---|---|
|**Strategy**|**特点**|**衡量的相似性**|
|Cited together|被同一批引用方共同引用|技术被他人使用方式类似|
|Citing same patents|引用了同一批基础专利|技术来源或依赖关系相似|

这两种方法常用于专利推荐系统、专利聚类、技术主题分析等。  
下面为你将本段内容整理成与之前一致的**课堂笔记格式（中英双文、结构清晰、可直接加入你的** **lecture notes****）**。  
重点包括：Reduce-side Join（两种 repartition 模式）、Map-side Join、Replicated Join、Alternatives、Bloom Filter 等内容。
 
**Reduce-side Join: Repartitioned Join 1**  
**Reduce** **端** **Join****（模式** **1****）：加入** **Tag + Shuffle + Join**  
**1.** **背景**  
当两个数据集分布在不同节点上时，MapReduce 无法直接在 Map 阶段完成 Join，需要在 Reduce 阶段完成。  
**2.** **关键思想**  
**在** **Map** **阶段为每条记录添加** **tag****，标记数据来源（****Customers** **或** **Orders****）。**  
之后按照 join key（如 Customer ID）进行 shuffle，让所有相同 key 的记录聚集到同一个 Reducer。
 
**Customers** **数据**  
1,Stephanie Leung,555-555-555 ￼2,Edward Kim,123-456-7890 ￼3,Jose Madriz,281-330-8004 ￼4,David Stork,408-555-000  
**Orders** **数据**  
3,A,12.95,02-Jun-2008 ￼1,B,88.25,20-May-2008 ￼2,C,32.00,30-Nov-2007 ￼3,D,25.02,22-Jun-2009
 
**Step 1. Map** **阶段（添加** **tag****）**  
Customers Map 输出形式：  
1 → \<1, ("Customers", Stephanie Leung, 555-555-555)\> ￼2 → \<2, ("Customers", Edward Kim, 123-456-7890)\>￼…  
Orders Map 输出形式：  
3 → \<3, ("Orders", A,12.95,02-Jun-2008)\> ￼1 → \<1, ("Orders", B,88.25,20-May-2008)\>￼…
 
**Step 2. Shuffle** **阶段**  
按 key 进行重新分区（Repartition）和数据移动。  
结果类似：  
Reducer for key=1：  
Customers: 1,Stephanie Leung,555-555-555 ￼Orders: 1,B,88.25,20-May-2008  
Reducer for key=3：  
Customers: 3,Jose Madriz,281-330-8004 ￼Orders: 3,A,12.95,02-Jun-2008 ￼Orders: 3,D,25.02,22-Jun-2009
 
**Step 3. Reduce** **阶段**  
Reducer 将来自不同 tag 的记录进行 join，输出对应行。
 
**Reduce-side Join: Repartitioned Join 2**  
**Reduce** **端** **Join****（模式** **2****）：****Cross-product + Second Map**  
**核心思想**  
• Reducer 对同一 key 的输入记录进行 **cross-product****（笛卡尔积）**。  
• 对每个 “Customers record × Orders record” 组合生成一条输出。  
• 第二次 Map 可以进一步决定 join 类型（inner、left outer、right outer）。  
**示例（****key=3****）**  
Reducer 输入：  
Customers: 3,Jose,281-330-8004 ￼Orders: 3,D,25.02,22-Jun-2009 ￼Orders: 3,A,12.95,02-Jun-2008  
Reducer 输出 cross-product：  
Customers × Orders1 ￼Customers × Orders2  
第二次 Map（Map2）生成最终格式：  
3,Jose,281-330-8004,D,25.02,22-Jun-2009 ￼3,Jose,281-330-8004,A,12.95,02-Jun-2008  
**Hadoop** **支持**  
Hadoop 提供现成支持此 join 模式的类与 API。
 
**Map-side Join: Replicated Join**  
**Map** **端** **Join****（复制式** **Join****）**  
**特点**  
• 当 **一个大数据集** **+** **一个非常小的数据集** 时最适用。  
• 将小数据集复制（replicate）到所有 Mapper 节点（用 Distributed Cache）。  
• Join 在 Map 阶段完成。  
• 不需要 Shuffle，也不需要 Reduce → 性能显著提升。  
**优势**  
• 避免昂贵的 Shuffle 阶段。  
• 可线性扩展 Map 并行度。
 
**Alternatives to Replicated Join**  
**当小数据集仍然太大时的替代方案**  
**1. Map-side Filtering****（****Map** **端过滤）**  
• 在 Map 阶段过滤掉不需要 join 的记录。  
• 例如：只选择 Customers 中“住在 Scotland 的用户”。  
• 过滤后数据量大幅减少，可能变得足够小从而可以使用 replicated join。
 
**2. Replicate Only Join Keys**  
• 不复制整张小表，只复制 join key 列表。  
• Map 端检查 key 是否参与 join，仅传输实际需要 join 的记录。
 
**3. Use a Bloom Filter****（布隆过滤器）**  
当 join key 列表仍然过大时，可构建布隆过滤器进行近似过滤。  
Bloom Filter 语义：  
• contains(x) = true → x **可能存在**（可能误报 false positive）  
• contains(x) = false → x **一定不存在**  
优势：  
• 占用空间小  
• 在 Map 阶段高效过滤无效记录  
• 显著减少 Shuffle 阶段的数据传输量  
误报率（false positives）与 Bloom Filter 的大小成反比。
 
**核心总结（****Join Strategies in MapReduce****）**

|   |   |   |   |
|---|---|---|---|
|**Join Type**|**适用场景**|**优点**|**缺点**|
|Reduce-side Join|两个数据集都很大|通用、无需预分布数据|Shuffle 阶段代价高|
|Reduce-side Join 2|多对多 join|支持 cross-product 与二次 Map|输出量大时昂贵|
|Map-side Replicated Join|大表 + 小表|无 Shuffle，高效|小表必须能复制到所有 Mapper|
|Map-side Filtering|数据太大时的预处理|显著减少数据量|需提前知道过滤条件|
|Bloom Filter Join|join key 仍然过大|高效过滤，减少数据传输|存在 false positives|

![[Lec14 MapReduce - Ink.svg]]

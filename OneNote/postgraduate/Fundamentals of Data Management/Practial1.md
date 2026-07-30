==sed====, awk, and grep:==  
==gnu/UNIX tools for processing data==  
==Fundamentals of Data Management==  
==Dr Chris Wood==
   

Aim of this talk  
• A very brief introduction to three powerful command-line tools  
• Make sure you’re aware of their existence  
• Provide you with the knowledge you need for the upcoming practical exercise  
NOTE  
Other scripting languages exist (Perl, Bash, Python, R)  
We just don’t have time to cover all of them here...
 
**What do they do?（它们的作用）**

- **grep****￼**→ Find lines matching a regular expression￼→ 查找匹配 **正则表达式** 的行￼✅ 适合用来 **搜索**
- **sed****￼**→ Stream editor. Make (small) changes to text as it’s being processed￼→ **流编辑器**，在文本流被处理时进行（小规模的）修改￼✅ 适合用来 **替换 / 删除 / 插入**
- **awk****￼**→ Scanning & processing language. Useful for printing out certain columns of formatted text data￼→ **扫描和处理语言**，特别适合从结构化文本（如表格）中提取和处理特定列￼✅ 适合用来 **字段提取 / 计算 / 格式化输出**
- 它们都是 **filter programs（过滤器程序）****￼**→ 接收输入（stdin），处理数据，输出结果（stdout）￼→ 更多例子可见: [Filter (Unix) - Wikipedia](http://en.wikipedia.org/wiki/Filter_\(Unix\))
 
**对比表格：grep / sed / awk**

|   |   |   |   |
|---|---|---|---|
|**工具**|**主要功能**|**常用场景**|**示例**|
|**grep**|搜索匹配行（支持正则表达式）|在日志中查找关键词|grep "error" logfile.txt|
|**sed**|流编辑器：替换、删除、插入文本|批量替换文件中的单词|sed 's/foo/bar/g' file.txt|
|**awk**|文本处理语言：按列提取、计算、格式化|打印数据文件的特定列或计算值|awk '{print $1, $3}' data.txt|
 
**Command Line Tools（命令行工具）**

- These tools are **gnu/UNIX command line tools****￼**→ 这些工具是 **gnu/UNIX 命令行工具**
- **Available out-of-the-box** on Linux and UNIX-based systems (including OS X)￼→ 在 **Linux** 和 **UNIX 系统（包括 OS X）** 上开箱即用
- **Available on Windows** through **cygwin** 或 **Windows Subsystem for Linux (WSL)****￼**→ 在 Windows 上可通过 **cygwin** 或 **WSL** 使用
 
**The UNIX Philosophy（UNIX 哲学）**

- **Programs do one thing, and do it well****￼**→ 每个程序只做一件事，并且做好
- **The power comes from combining the programs****￼**→ 强大的地方在于可以组合多个程序
- 常用组合方式：
    - **Redirection 重定向**：\<（输入），\>（输出）
    - **Pipe 管道**：|
 
**stdin & stdout（标准输入与输出）**

- **stdin**（standard input） → 标准输入
- **stdout**（standard output） → 标准输出

例子：

![Exported image](Exported%20image%2020260730190929-0.png)

$ input_file \> program \> output_file

![Exported image](Exported%20image%2020260730190929-1.png)  
![Exported image](Exported%20image%2020260730190930-2.png)

解释：

1. input_file → 作为 **stdin** 输入
2. program → 读取输入并处理
3. output_file ← 处理结果通过 **stdout** 写入

另一个例子：
   

$ cat hello.txt \> output.txt￼  
→ 把 hello.txt 的内容写入 output.txt
 
**The UNIX Pipeline（UNIX 管道）**

- **基本形式**：
 
$ program1 | program2￼

![Exported image](Exported%20image%2020260730190934-3.png)

- 含义：program1 的 **stdout** → 传递给 program2 的 **stdin**

例子：
 
$ cat hello.txt | grep "world"￼  
→ 从 hello.txt 中筛选包含 “world” 的行

- **多个程序链式处理**：

![Exported image](Exported%20image%2020260730190935-4.png)  

$ program1 | program2 | program3￼  
例子：
 
$ cat access.log | grep "404" | wc -l￼  
→ 在日志 access.log 中查找 **404 错误** 并统计数量
 
**grep（文本搜索工具）**  
**grep****（文本搜索工具）**  
**语法**：
 
grep [options] PATTERN [FILE...]￼

- **PATTERN**：要匹配的模式（pattern）
- **FILE...**：可指定多个文件，若省略则默认使用 **stdin**

常用选项：

- -i → 忽略大小写 (**ignore case**)
- -n → 显示匹配行的行号 (**line number**)
- -r → 递归搜索目录 (**recursive**)
- -v → 反向匹配（显示不匹配的行）
 
**1.** **基本搜索**
 
==grep energy myfile.txt==  
→ 返回 myfile.txt 中所有包含字符串 **“energy”** 的行。  
⚠️ 注意：不会匹配 “Energy” 或 “ENERGY”，除非它们也包含 **小写** **energy**。
 
**2.** **使用正则表达式中的** **“.”****（任意单字符）**
 
==grep A.a. myfile.txt==  
→ 返回所有包含匹配模式 **A.a.** 的行。

- "." 表示任意单个字符
- 例如会匹配：Adam、Alan
- 不会匹配：Adrian（因为多了字符）
 
**3.** **匹配行首（****^****）**
 
==grep ^A.a. myfile.txt==  
→ 返回所有 **以** **A.a.** **开头** 的行。

- 例如会匹配：Adam、Alan
- 不会匹配：My name is Adam
 
**4.** **忽略大小写（****-i****）**
 
grep A.a. myfile.txt￼  
→ 不会匹配 adam
 
==grep -i A.a. myfile.txt==  
→ 可以匹配 adam、Adam、aDaM 等
 
**💡** **补充例子**
 
grep -n "error" logfile.txt￼  
→ 显示所有包含 **error** 的行及其 **行号**
 
grep -v "debug" logfile.txt￼  
→ 显示所有 **不包含** **debug** 的行
      

**sed****（流编辑器）**  
**基本语法**：
 
sed [options] 'command' [FILE...]￼

- **sed** 用于逐行处理文本，支持替换、删除、插入等操作
- 若省略 **FILE**，则默认从 **stdin** 读取
- 可以同时处理多个文件
 
**1.** **字符串替换（****substitute****）**
 
sed =='s/adam/harry/g'== myfile.txt  
→ 将文件中所有 adam 替换为 harry。

- ==s = substitute====（替换）==
- ==g = global====（全局替换，而不是只替换第一处）==
 
**💡** **补充例子**

1. **替换每行第一个匹配项（不加** **g****）**
 
sed 's/error/warning/' logfile.txt￼  
→ 仅替换每行第一个 **error** 为 **warning**

1. **删除包含某模式的行**
 
sed '/debug/d' logfile.txt￼  
→ 删除所有包含 **debug** 的行

1. **在每行开头添加前缀**
 
sed 's/^/[INFO] /' logfile.txt￼  
→ 在每行开头添加 [INFO]
 
**Important Note**

- **sed is non-destructive** → sed 默认**不会修改原始文件**，它的输出只是“处理后的文本流”。
- 如果想直接修改文件，要用 **-i (in-place)** 选项，但一般不推荐。

例子：
 
$ sed 's/Adam/Bob/g' afile￼  
→ 输出替换后的结果，但 **afile** 文件内容不会改变。

![Exported image](Exported%20image%2020260730190936-5.png)

**1.** **基本替换**
 
$ sed 's/Adam/Bob/g' afile￼  
输出：
 
Bob is the first word on this line.￼The first word on this line is not Bob.￼
 
**2.** **指定行替换**  
只在第 1 行替换：
 
$ sed '1s/Adam/Bob/g' afile￼  
只在第 2 行替换：
 
$ sed '2s/Adam/Bob/g' afile￼

![Exported image](Exported%20image%2020260730190937-6.png)

**3.** **指定匹配的出现次数**  
只替换每行的第 2 个匹配项：
 
$ sed 's/Adam/Bob/2' afile￼
 
**4.** **使用正则表达式**  
匹配以 Adam 开头的行：
 
$ sed 's/^Adam/Bob/g' afile￼
 ![Exported image](Exported%20image%2020260730190938-7.png)  
![Exported image](Exported%20image%2020260730190939-8.png)

**￼****￼****5.** **引用匹配内容** **&**
 
$ sed 's/A.a./Bob&/g' afile2￼  
→ 在匹配到的内容前加上 Bob  
输出示例：
 
BobAdam appears on this line...￼Here is a line that contains both BobAdam and BobAlan.￼
 
💡 **补充例子**：

- 删除第 3 行：
 
sed '3d' afile￼

- 在每行末尾添加字符串：
 
sed 's/$/ [END]/' afile￼
 
**awk****（文本处理工具）**  
**基本语法**：
 
awk [options] ['program'] [FILE...]￼

- awk 是一个**逐行处理**工具，可以对每一行执行指定的“程序”。
- 特别适合处理 **space-separated****（空格分隔）**、**comma-separated****（****CSV****）** 或 **tab-separated****（****TSV****）** 文件。
- **$n** → 表示第 n 个字段（column）
- **$0** → 表示整行
 
**1.** **基本例子**

![Exported image](Exported%20image%2020260730190940-9.png)

数据文件 tsvfile：
 
6.346 9.874 12.196￼16.482 12.001 1.094￼6.972 8.231￼  
命令：
 
awk '{print $1, $3}' tsvfile￼  
输出：
 
6.346 12.196￼16.482 1.094￼6.972￼
 
**2.** **指定分隔符** **-F**

![Exported image](Exported%20image%2020260730190944-10.png)  

awk -F. '{print $2}' afile2￼  
→ 输出两个 . 之间的内容（分隔符不会出现在结果中）。
 
💡 **补充例子**：

1. 打印第 2 列和第 3 列的和：
 
awk '{print $2 + $3}' tsvfile￼

1. 打印所有行号和内容：
 
awk '{print NR, $0}' tsvfile￼

- NR → 当前行号（Number of Record）
- 过滤数值大于 10 的行：
 
awk '$1 \> 10 {print $0}' tsvfile￼
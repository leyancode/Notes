**Introduction**  
**Advanced Message-Passing Programming (EPCC11012)**  
课程名称  
Advanced Message-Passing Programming (EPCC11012)  
授课教师  
Alexei Borissov  
EPCC  
授课时间  
14 January 2026  
版本信息  
v1.1.0  
EPCC – Alexei Borissov ([a.borissov@epcc.ed.ac.uk](mailto:a.borissov@epcc.ed.ac.uk))
 
**Acknowledgements**  
**教学材料说明**  
本课程的教学幻灯片使用 **LaTeX Beamer** 制作。  
LaTeX Beamer 是一个用于学术演示文稿的 LaTeX 文档类。  
相关链接  
[https://ctan.org/pkg/beamer](https://ctan.org/pkg/beamer)
 
**License**  
**课程资料许可协议**  
本课程材料采用 **Creative Commons BY-NC-SA 4.0 License**。  
许可条款说明：  
Non-Commercial  
非商业用途。你不得将本材料用于任何商业目的。  
Share-Alike  
相同方式共享。如果你对材料进行了 remix、transform 或 build upon，你必须在相同许可协议下发布你的成果。  
Attribution  
署名。你必须给出适当的署名，提供许可协议链接，并说明是否对原始材料进行了修改。  
署名方式应合理，但不得暗示原作者认可你的使用方式。  
完整许可协议文档  
[https://creativecommons.org/licenses/by-nc-sa/4.0/](https://creativecommons.org/licenses/by-nc-sa/4.0/)
 
**What is AMPP?**  
**什么是** **Advanced Message-Passing Programming**  
Advanced Message-Passing Programming（AMPP）是 **Message-Passing Programming** 课程的直接进阶版本。  
课程目标与特点：

- 在已有 Message-Passing Programming 知识基础上继续深入
- 强调对已有知识的扩展与深化
- 引入新的技术与方法，以进一步挖掘并行程序的性能潜力
- 聚焦于 performance optimisation（性能优化）
 
**Structure**  
**课程结构概览**  
Table of Contents：

1. Structure
2. Weekly structure
3. Assessment
4. Feedback

本部分主要介绍课程整体组织方式与学习安排。
 
**Exploiting what you already know**  
**基于已有知识的高级内容扩展**  
本课程重点在于**充分利用你已经掌握的** **MPI** **基础知识**，并在此之上引入更高级的特性：  
Advanced derived datatypes  
高级派生数据类型，用于更高效地描述复杂内存布局。  
Advanced collective operations  
高级集体通信操作，用于提升通信性能与可扩展性。  
User-defined reduction operators  
用户自定义归约算子，允许根据应用需求定义新的 reduction 行为。  
Advanced virtual topologies  
高级虚拟拓扑，用于更合理地映射通信模式与硬件结构。  
下面是**继续按课堂笔记格式整理后的内容**，与前一部分保持**结构一致、中文为主，中英对照辅助**，保留关键技术术语的英文原文，不引入任何特殊符号。
 
**Exploring new areas**  
**探索新的高级主题**  
在巩固已有 MPI 基础与高级用法的同时，本课程还将引入一系列新的方向与扩展主题：  
Neighbourhood collective operations  
邻域集体通信操作。基于虚拟拓扑中“邻居关系”的 collective communication，更贴近真实通信模式。  
Persistent communication requests  
持久化通信请求。通过 persistent requests 减少重复通信初始化开销，适用于重复通信场景。  
Remote memory accesses  
远程内存访问（RMA）。支持 one-sided communication，使进程可以直接访问其他进程的内存空间。  
MPI-3 shared memory  
MPI-3 共享内存模型。允许同一节点上的 MPI 进程通过 shared memory window 直接共享数据。  
Hybrid programming  
混合并行编程。结合 MPI 与 OpenMP、threads 或 accelerator programming 的混合并行模式。  
MPI Parallel IO  
MPI 并行输入输出。用于高性能并行文件读写，避免串行 I/O 瓶颈。  
MPI Internals  
MPI 内部实现机制。理解 MPI library 的设计与实现原理，有助于性能调优与调试。  
MPI tools  
MPI 工具链。包括 profiling、tracing、debugging 等工具，用于分析与优化并行程序。
 
**Weekly structure**  
**每周教学结构**  
Table of Contents：

1. Structure
2. Weekly structure
3. Assessment
4. Feedback

本部分重点介绍课程的**周安排与教学节奏**。
 
**Weekly structure – Timetable**  
**时间安排**  
Lecture  
每周五，2 小时  
11:10 – 13:00  
Practical  
每周一，2 小时  
10:00 – 12:00  
Assessment  
每周三，1 小时  
12:10 – 13:00
 
**教学节奏设计理念**  
通过上述时间安排，课程形成一个完整的学习闭环：  
On Friday  
学习新的理论内容与概念。  
Weekend  
利用周末时间消化新知识，思考疑问，并提前查看 practical 内容。  
On Monday  
通过实践巩固知识，同时解答在理解过程中产生的额外问题。  
On Wednesday  
通过 assessment 检查自己对本周新主题的掌握程度。
 
**Assessment**  
**课程考核结构**  
Table of Contents：

1. Structure
2. Weekly structure
3. Assessment
4. Feedback

本部分介绍课程的考核方式与设计目的。
 
**Assessments**  
**Weekly quizzes** **考核模式**  
课程采用 **每周一次、****40** **分钟的** **quiz** 作为主要考核方式。  
该考核模式的特点与优势包括：

- 提供更广泛且更深入的内容覆盖
- 覆盖更多主题，而不是集中在少数考点
- 每个主题都有对应的独立 quiz
- 工作量分布均匀，避免期末集中压力
- 最差的一次 quiz 成绩将被 discard
- 更准确地反映学生对知识的整体掌握情况
 
**Quiz** **工具说明**  
对于从未使用过 Learn quizzes 的同学：

- 课程将提供一次 mockup quiz
- 目的是帮助学生熟悉 quiz 工具本身
- mockup quiz 不计入最终成绩

下面是**Feedback** **部分按课堂笔记格式整理后的版本**，与前文保持一致，**中文为主，中英双文辅助**，保留关键平台与术语的英文原文，不使用任何特殊符号。
 
**Feedback**  
**反馈机制概览**  
Table of Contents：

1. Structure
2. Weekly structure
3. Assessment
4. Feedback

本部分介绍课程的反馈渠道及其在课程改进中的作用。
 
**Feedback – General**  
**反馈方式说明**  
课程提供正式的反馈渠道，具体如下：  
Feedback form  
反馈表单通过 **Wooclap** 平台提供。  
Anonymous  
反馈为匿名提交，教师无法识别提交者身份。  
Can submit anytime  
反馈可在学期内任意时间提交。  
Can submit as many times as you want  
允许多次提交反馈，不限次数。  
Positive and negative feedback equally welcome  
无论是正面还是负面的反馈，均同样受到欢迎。
 
**Feedback – Based on previous feedback**  
**以往反馈对课程的实际影响**  
根据以往学生的反馈，Advanced Message-Passing Programming 已经做出了多项改进，包括：  
Renewing the slides and practicals  
更新课程幻灯片与实践内容。  
Redesigning the assessment format  
重新设计 assessment 形式，使其更加合理。  
Additional illustrations  
增加更多图示说明，例如 graph virtual topologies。  
Typos and clarifications  
修正拼写错误并补充必要的澄清说明。
 
**Feedback – Looking forward**  
**上一学期反馈对本课程的进一步改进方向**  
基于上一学期收集到的反馈，课程将在以下方面继续改进：  
Clarifying which practical exercises are optional  
明确指出哪些 practical exercises 是 optional。  
Provide PDFs without page breaks  
提供无分页中断的 PDF 版本，便于阅读与打印。  
Getting more examples  
增加更多示例内容，例如 live coding sessions。
 
如果你需要，我可以进一步帮你：

- 将 **Structure + Weekly structure + Assessment + Feedback** 合并成一页完整课程概览
- 压缩为“开学第一节课速读笔记”版本
- 按“考试不会考 / 可能考 / 必须知道”的角度做重点区分
 \> 来自 \<[https://chatgpt.com/c/696a1d68-8dcc-832c-8f45-3c1a3cca5531](https://chatgpt.com/c/696a1d68-8dcc-832c-8f45-3c1a3cca5531)\>
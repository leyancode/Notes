**MPI tools**  
**Advanced Message-Passing Programming (EPCC11012)**  
课程模块  
MPI tools  
授课教师  
Alexei Borissov  
EPCC  
授课时间  
16 January 2026  
版本信息  
v1.1.0  
EPCC – Alexei Borissov ([a.borissov@epcc.ed.ac.uk](mailto:a.borissov@epcc.ed.ac.uk))
 
**Acknowledgements**  
**Individuals**  
本讲内容受到以下研究人员工作的启发：  
David Henty
 
**Acknowledgements**  
**Contributors**  
教学材料说明：  
本讲幻灯片使用 **LaTeX Beamer** 制作。  
LaTeX Beamer 是用于学术演示文稿的 LaTeX 文档类。  
相关链接  
[https://ctan.org/pkg/beamer](https://ctan.org/pkg/beamer)
 
**License**  
**课程资料许可协议**  
本课程材料采用 **Creative Commons BY-NC-SA 4.0 License**。  
许可条款要点：  
Non-Commercial  
材料不得用于任何商业用途。  
Share-Alike  
如对材料进行 remix、transform 或 build upon，必须在相同许可协议下发布。  
Attribution  
使用材料时必须进行署名，提供许可协议链接，并说明是否对原始内容进行了修改。  
署名方式应合理，但不得暗示原作者对你的使用方式进行背书。  
完整许可协议文档  
[https://creativecommons.org/licenses/by-nc-sa/4.0/](https://creativecommons.org/licenses/by-nc-sa/4.0/)
 
**Profilers**  
**内容结构**  
Table of Contents：

1. Profilers
2. Debuggers
3. Trackers

本节重点介绍 **MPI profiling tools**，用于分析并行程序的性能行为。
 
**Profilers – Approaches available**  
**性能分析的两种主要方法**  
Statistical sampling  
统计采样。  
通过每隔固定时间 X 暂停程序执行，检查程序当前的执行状态与进展情况。  
Tracing  
事件追踪。  
在函数或 MPI 调用周围插入 trackers，对调用行为进行记录与分析。
 
**Profilers – Available tools**  
**常见** **MPI** **性能分析工具**  
以下是常用的 MPI profilers：  
Scalasca  
perftools / Cray Performance Analysis Tool (CrayPAT)  
Tuning and Analysis Utilities (TAU)  
属于 PARATOOLS 工具集的一部分。  
Linaro MAP  
属于 Linaro Forge 工具集的一部分。  
etc.  
此外还存在其他性能分析工具。  
补充说明：

- 可查阅 **Cirrus documentation** 以了解在 Cirrus 系统上可用的 profilers 及其使用方式
- 用户也可以通过 **PMPI interface** 自行实现诊断与分析工具
 
**Profilers – Introduction (CrayPAT)**  
**Cray Performance Analysis Tool** **简介**  
Documentation  
官方文档可参考以下链接：  
[https://support.hpe.com/hpesc/public/docDisplay?docId=a00114942en_us&page=index.html](https://support.hpe.com/hpesc/public/docDisplay?docId=a00114942en_us&page=index.html)  
Developed by HPE Cray  
该工具由 HPE Cray 开发，用于分析其系统上运行程序的性能。  
System requirements  
需要访问 HPE Cray 系统。  
在 Cirrus 和 ARCHER2 上均可使用。  
Profiling levels  
通过不同模块提供多层次性能分析：

- perftools-base
- perftools-lite
- perftools modules

不同模块对应不同的分析粒度与开销。  
下面是 **MPI tools – Profilers****：****How it works** 这一部分的**课堂笔记整理版**。格式与前文保持一致，中文为主，中英对照辅助，**关键命令与术语保留英文原文**，不引入任何特殊符号，便于直接并入你的总笔记。
 
**Profilers – How it works**  
**工作流程概览**  
MPI profiler 的基本使用流程如下：  
Documentation  
参考官方文档：  
[https://docs.cirrus.ac.uk/software-tools/perftools/](https://docs.cirrus.ac.uk/software-tools/perftools/)  
Load appropriate modules  
在运行前加载合适的 perftools 相关 modules。  
(Re)Compile your application  
以允许插入 trackers 的方式重新编译应用程序。  
Run your application  
运行带有 profiling 的程序。  
Interpret the data generated  
对生成的数据进行分析与解读，可以使用 command line 工具或 GUI 工具。
 
**How it works – Instrumentation****￼**

![Exported image](Exported%20image%2020260730160559-0.png)

**插桩（****instrumentation****）阶段**  
Instrumentation 的目的是在程序中自动插入性能跟踪代码，以便后续分析。  
Classic compilation  
使用 Cray 编译器编译 MPI 程序的经典方式：  
cc -o main main.c  
This is a standard compilation without profiling.
 
**Compilation with perftools-base or perftools-lite**  
在加载 perftools-base 或 perftools-lite 模块后，使用相同的编译命令即可自动插入 instrumentation：  
cc -o main main.c  
此时，编译器会在后台完成 profiler 所需的插桩。
 
**Compilation with full perftools instrumentation**  
对于更高级或更细粒度的分析，可以采用分步编译方式：  
cc -c main.c￼cc -o main main.o  
该方式允许 perftools 在编译和链接阶段插入更完整的 profiling 支持。
 
**How it works – Analysis**  
**分析阶段**  
在程序运行和数据分析阶段，需要注意以下步骤与工具：  
Running the code  
运行程序时，在可执行文件前加上 pat run：  
pat run ./main  
Problem size selection  
选择能够反映程序真实行为、但规模不过大的问题规模，避免分析数据过于庞杂。
 
**Viewing results with pat report**

![Exported image](Exported%20image%2020260730160600-1.png)

使用 pat report 对结果进行分析，例如：  
View call tree  
查看函数调用树：  
==pat report -O calltree==  
View callers and source code  
查看耗时最长的函数及对应的源代码行号：  
==pat report -O callers+src==  
View load balance  
查看负载均衡情况：  
pat report -O load balance
 
**Documentation and GUI**  
Documentation  
可通过以下命令查看完整文档：  
man pat report  
GUI  
也可以使用图形界面进行分析，具体方法参考 Cirrus documentation。
 
**How it works – Example**  
**示例说明**  
perftools-base 生成的 sample profile 通常包括：

- 各函数的执行时间分布
- MPI 调用所占比例
- 计算与通信时间对比
- 潜在的负载不均衡问题

该示例用于说明 profiler 输出结果的基本结构与可解读信息。

![Exported image](Exported%20image%2020260730160601-2.png)   
**Profiling interface (PMPI)**  
**PMPI** **接口的基本思想****￼**

![Exported image](Exported%20image%2020260730160602-3.png)

Behind the scene  
在 MPI 的底层实现中，每一个 MPI routine 都有一个对应的 **PMPI equivalent**。  
Example  
例如：

- MPI_Send 对应 PMPI_Send
 
**PMPI** **的作用机制**  
PMPI 接口允许用户：

- 捕获所有对 MPI routines 的调用
- 在调用真实 MPI 实现之前或之后插入 tracking mechanism
- 再调用底层对应的 PMPI routine

这一机制使得开发者可以在不修改 MPI 库源码的前提下，对 MPI 调用进行拦截和扩展。
 
**PMPI** **的用途**  
Profiling  
==PMPI== ==中的== ==P== ==表示== ==Profiling====，用于性能分析。==  
Debugging  
==可用于调试== ==MPI== ==程序的行为==。  
Tracking  
==可用于跟踪== ==MPI== ==调用顺序、频率和参数。==
 
**Profilers – PMPI Example (Barrier)**  
**示例一：拦截** **MPI_Barrier**

![Exported image](Exported%20image%2020260730160604-4.png)

Step 1: 自定义头文件 my_mpi.h  
在自定义头文件中声明需要拦截的 MPI 接口：  
#include \<mpi.h\>  
int MPI_Barrier(int comm);
 
**Step 2:** **在实现文件中加入** **tracker**

![Exported image](Exported%20image%2020260730160605-5.png)

在 my_mpi.c 中实现该接口，并加入自定义行为：  
#include "my_mpi.h"  
int MPI_Barrier(int comm)￼{￼ int my_rank;￼ MPI_Comm_rank(comm, &my_rank);￼ printf("MPI process %d reached the barrier.\n", my_rank);￼ PMPI_Barrier(comm);￼}  
Explanation  
该实现会在每次调用 MPI_Barrier 时打印当前进程的信息，然后再调用真正的 PMPI_Barrier。
 
**Profilers – PMPI Example (Send)**  
**示例二：修改** **MPI_Send** **的底层行为**

![Exported image](Exported%20image%2020260730160606-6.png)

该示例展示如何在不修改应用代码的情况下，将所有 MPI_Send 转换为同步发送。  
#include "my_mpi.h"  
int MPI_Send(const void* buffer,￼ int count,￼ MPI_Datatype datatype,￼ int recipient,￼ int tag,￼ MPI_Comm communicator)￼{￼ PMPI_Ssend(buffer, count, datatype, recipient, tag, communicator);￼}  
Explanation  
所有对 MPI_Send 的调用都会被重定向为 PMPI_Ssend，从而强制使用 synchronous send。
 
**Debuggers**  
**内容结构**

![Exported image](Exported%20image%2020260730160607-7.png)

Table of Contents：

1. Profilers
2. Debuggers
3. Trackers

本节介绍用于 MPI 程序调试的工具与方法。
 
**Debuggers – Available tools**  
**常见** **MPI** **调试工具**  
Linaro DDT  
属于 Linaro Forge 工具集，支持并行程序的可视化调试。  
TotalView  
属于 Perforce 软件套件，是经典的并行调试器。  
Classic debuggers  
如 GDB 等传统调试工具。
 
**使用传统** **debugger** **调试** **MPI** **的方式**  
方式一：Multiple debugger instances  
为每个 MPI process 启动一个 debugger 实例，并通过多个终端进行管理。  
方式二：Single debugger instance  
使用单个 debugger 实例，动态 attach 到或 detach 自某一个 MPI process。  
该方式通常较为复杂，操作上略显 hacky。  
下面是 **MPI tools – Trackers****（****MPI monitor****）部分的课堂笔记整理版**。整体结构、语言风格与前文完全一致，中文为主，中英对照辅助，保留关键工具名称与术语的英文原文，不使用任何特殊符号。
 
**Trackers**  
**内容结构**  
Table of Contents：

1. Profilers
2. Debuggers
3. Trackers

本节介绍 **tracker** **工具**，用于实时观察 MPI 程序的执行状态。
 
**MPI monitor**  
**基本信息**  
MPI monitor 是一个用于 MPI 程序的轻量级 tracking 工具。  
License  
Free and open-source，采用 AGPLv3 license。  
Availability  
项目地址：  
[https://github.com/rookiehpc/MPI_monitor](https://github.com/rookiehpc/MPI_monitor)
 
**功能定位**  
MPI monitor 的主要功能是：

- 告诉你每一个 MPI process 当前在程序中的位置
- 提供程序运行过程中的实时状态信息

它的目标是**可视化程序****“****正在做什么****”**，而不是分析性能或调试错误。
 
**What MPI monitor is not**  
**工具能力边界**  
MPI monitor 不是 debugger

- 如果程序发生 segmentation fault，只会直接崩溃
- 不提供 interactive debugging session

MPI monitor 不是 profiler

- 不会告诉你程序在某个函数上花费了多少运行时间
- 不会生成 runtime breakdown 或 performance report
 
**MPI monitor – Example**  
**使用示例说明**

![Exported image](Exported%20image%2020260730160611-8.png)

MPI monitor 可以在终端中实时输出程序状态，例如：

- 各个 MPI process 当前所处的函数或代码区域
- 程序是否在通信、同步或计算阶段

该输出是 live terminal output，可用于快速定位程序是否卡住或出现异常行为。
 
**MPI monitor – Practical advantages**  
**实际使用中的优势**  
MPI monitor 具有以下实用特点：  
No dependencies on external software  
不依赖任何额外的软件环境。  
No GUI  
无需图形界面，不需要 X11 forwarding 设置。  
No change to the compiler  
无需更换或特殊配置编译器。  
No change to the execution command  
程序运行方式保持不变。  
Minimal integration  
只需在项目中加入一个 header file (.h) 和一个 source file (.c)。
 
**使用场景总结**  
MPI monitor 适用于：

- 快速查看 MPI 程序是否在正常推进
- 判断是否存在进程卡在 barrier 或通信上的情况
- 作为 profiler 和 debugger 之外的轻量级补充工具
 
如果你需要，我可以帮你：

- 把 **Profilers vs Debuggers vs Trackers** 总结成一张对照表
- 提炼成“考试一句话定义版”
- 将 MPI tools 整讲内容整理成一页完整复习笔记
 \> 来自 \<[https://chatgpt.com/c/696a1d68-8dcc-832c-8f45-3c1a3cca5531](https://chatgpt.com/c/696a1d68-8dcc-832c-8f45-3c1a3cca5531)\>
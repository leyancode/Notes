好的，我帮你把课堂笔记整理成中英对照的结构化版本，关键术语保留英文原文。如下：
 
**Lecture Outline** **课程大纲**

- What do we mean by **“data formats”** **数据格式**?
- What’s the difference between **“encoding** **编码****”** and **“format** **格式****”**?
- Files and file systems 文件与文件系统
- Some commonly-used scientific file formats 一些常用的科学文件格式
- After completing this lesson, you should be able to:
    - Grasp the basics of how digital data are stored 掌握数字数据存储的基础
    - Name the most common standard file formats in use in HPC 能够列举高性能计算 (HPC) 中常见的标准文件格式
    - Understand why using standard file formats is **A Good Thing** 理解为什么使用标准文件格式是一件“好事”
    - Understand a little about **data recovery** 了解一些数据恢复的知识
 
**What does digital data look like?** **数字数据是什么样子的？**  
数字数据本质上是一串二进制比特：  
1001010100101010010101010101010001011101010110...￼
 
**Meaning from bits** **从比特到意义**  
数字数据的含义至少体现在三个层次：

1. **Encoding** **编码**：如何将数字和字符记录为一组比特。
2. **File format** **文件格式**：如何解释这些数字或字符的排列方式。
3. **Files, records** **文件与记录**：如何将比特集合成有意义的分组。
 
**Meaning from bits: encoding** **从比特到意义：编码**

- **Encoding = interpreting groups of bits** 编码 = 对比特组进行解释
- 标准编码格式（固定比特长度）
    - 例如：8 bits = 1 byte = 1 ASCII character
- 自定义二进制格式 (custom-specified binary formats)
    - 例如：“接下来的 8192 个比特表示 128 个 64-bit IEEE 浮点数”
- 编码通常发生在字符或数值层面
- 计算机系统以 **byte** **字节** 为基本单元
    - 例如：“4-byte integer” 而不是 “32-bit integer”
- 传统术语：
    - 2 bytes = 1 word
    - 4 bytes = 1 long-word
 
**Endianness** **字节序**

- **Endianness** 指的是字(word)的字节存储顺序：
    - **Little-endian**：最低有效字节 (least significant byte) 先存储
    - **Big-endian**：最高有效字节 (most significant byte) 先存储
    - “first” 表示在最小的内存地址处

**示例**  
十进制整数：8,762,359

- 十六进制：85B3F7 → 00 85 B3 F7（4 个字节）
- 内存布局：
    - Big-endian: 00 85 B3 F7
    - Little-endian: F7 B3 85 00

Address: 101 102 103 104￼Big-endian: 00 85 B3 F7￼Little-endian: F7 B3 85 00￼
 
**Endianness: further notes** **字节序补充说明**

- **Big-endian** 更直观
    - 类似十进制：千位-百位-十位-个位
- **Little-endian** 是 Intel x86/x64 微处理器架构的主流
- 大多数网络协议（尤其是 IPv6）使用 **Big-endian**
- **问题**：字节序会影响二进制文件在不同架构间的可移植性
    - 例如：Intel x86 生成的文件在 IBM z/series 主机上的问题
- **解决方法**：
    - 现代编译器提供了良好支持
    - 标准 I/O 库在处理标准文件格式时会自动解决

好的，我来继续帮你把这一部分也整理成中英对照的课堂笔记，保持和之前一样的风格，关键字保留 **英文原文**。
 
**Floating point formats** **浮点数格式**

- 计算机存储实数的方式与科学计数法类似：
    - (sign a) × 0.(mantissa m) × 10^(exponent e)
    - 不同之处：使用 **binary** **二进制** 表示而非十进制
    - 并且在 **m** **与** **e** **的符号** 上存在一些细节问题
- **IEEE 754 floating point standard**（最常用标准）：
    - **32-bit single precision** **单精度**
        - 1 bit sign
        - 8 bit exponent
        - 23 bit mantissa
    - **64-bit double precision** **双精度**
        - 1 bit sign
        - 11 bit exponent
        - 52 bit mantissa
 
**More complex encodings** **更复杂的编码**

- **EBCDIC** (Extended Binary Coded Decimal Interchange Code)
    - 一种 8-bit 字符编码，主要用于 IBM 主机操作系统
    - 与 **ASCII/UTF-8** 之间没有良好的对应关系
- **Compression formats** **压缩格式**（依赖算法）：
    - zip, jpeg, mpeg …
    - 通常是 **file format** **与** **encoding** **的组合**
- **Encryption tools and formats** **加密工具与格式**（依赖算法）：
    - mcrypt, BitLocker, TrueCrypt, AES, Blowfish …
 
**Meaning from bits: format** **从比特到意义：格式**

- **Format** **格式** **= interpreting decoded bit-groups (bytes, characters)**
    - 对解码后的比特组（字节、字符）进行解释
- **Using standard file formats** **使用标准文件格式**：
    - netCDF, XML, HDF5, PDF, CSV, JSON, GRIB, 以及一些 proprietary formats
- **Custom-specified file formats** **自定义文件格式**：
    - 例如：“文件头包含 8 个 4-byte 整数，其余部分为 64-bit 浮点数”
- 格式通常可以看作是 **文件或数据库层面** 的定义
 
**Files & file systems** **文件与文件系统**

- 在大多数计算机系统中，**文件是数据组织的基本单元**
- 文件包含：
    - 一个指向比特序列的 **pointer** **指针**
    - 一小部分描述信息（**metadata** **元数据**）
- **文件系统** **file system** 决定了 pointer 与 metadata 的具体形式
    - 是一个管理文件的框架：创建、操作、删除与追踪文件
    - 在 **POSIX-compliant file systems** 中，{pointer + metadata} = **inode**
    - 文件系统覆盖在实际存储介质（磁盘/磁带/闪存/CD 等）之上
    - 提供逻辑视角，通常依赖一个巨大的 inode 查找表
- 对比：
    - **Block device** 或 **object store**
 
**Files vs objects** **文件** **vs** **对象存储**

- **Object storage** **对象存储** 越来越普遍，尤其在 **big data** 场景
- 与 **HTTP/REST** 模型高度契合
- 例子：Amazon S3, Openstack/Swift, CDMI
- 区别主要在 **metadata** **存储方式**：
    - **Files** **文件系统**
        - 元数据集中存储，采用固定 schema
        - 查找（findability）简单（读 inode 表）
        - 可扩展性差，inode 表可能成为瓶颈
    - **Objects** **对象存储**
        - 元数据与内容捆绑，schema 灵活
        - 查找难度更大（cf. ls？）
        - 可扩展性更好
 
**Common native file systems** **常见本地文件系统**

- **Desktop** **桌面环境**
    - Windows: FAT, NTFS
    - MacOS: APFS, HFS, HFS+
    - Linux: ext2, ext3, ext4
- **Network / distributed** **网络或分布式**
    - Linux/Unix: NFS
    - Windows: SMB/CIFS
    - MacOS: AFP
- **HPC** **高性能计算环境**
    - Linux 系统：ext3, ext4 + NFS
    - GPFS: IBM 的专有高性能并行文件系统
    - Lustre: 开源高性能并行文件系统
    - HDFS: Google 分布式文件系统的开源克隆
- **现代计算机系统**：
    - 支持多种本地与外部文件系统
    - FAT 支持广泛（如 USB 设备）
    - NFS 与 SMB 同样广泛应用
 
好的 👍 我继续帮你把 **FORTRAN** **文件警告** **+** **文件格式实践** **+** **常见科学文件格式（****CSV, HDF5, CDF, netCDF****）** 这一部分整理成双语笔记：
 
**FORTRAN binary files: a warning!**  
**FORTRAN** **二进制文件：警告！**

- FORTRAN 生成的二进制文件是“特殊”的
- **FORTRAN I/O** **是基于** **record****（记录）** 的
    - 一个写语句 write(1,*) array 写入文件时的结构：￼\<number of bytes in array\>\<array\>\<number of bytes in array\>￼
    - 即：数组的二进制数据被作为一条记录写入，两边用 **字节大小** 包裹
    - 数字大小本身可能是 **4–12** **字节**，依赖于具体实现
- 因此：FORTRAN 二进制文件 **不可移植**
- 结论：应尽量避免使用！
 
**File format good practice**  
**文件格式最佳实践**

- 一般情况下：**不要自己发明新格式！**
- 注意研究领域内常用的标准
- 优先使用通用、开放的标准
- 使用标准文件格式与标准 I/O 库
- 如果必须自定义，至少采用已有的“框架”：
    - **CSV + UTF-8**
    - **XML**（后续课程会讲）
    - **Key-value pairs** **键值对**（cf. JSON, YAML）
 
**CSV (Comma Separated Values)**

- 最简单的 “标准” 文件格式！
- 一种写入 **表格数据（行****-****列）** 的简单方式：
    - 行 → 换行符分隔
    - 列 → 逗号分隔
- 注意：字符串中包含逗号时必须转义 → 否则容易导致 bug
- 依赖于字符编码（如 ASCII 或 UTF-8）
- 本身不是严格标准，但在 **RFC 4180** 中有描述
    - [http://tools.ietf.org/html/rfc4180](http://tools.ietf.org/html/rfc4180)
 
**HDF5 (Hierarchical Data Format v5)**

- 定义：一个 **数据模型、库和文件格式**，用于存储和管理数据
- 起源：美国伊利诺伊大学 NCSA 开发
- **数据模型**：基于 **groups****（组）** 和 **datasets****（数据集）**
    - groups 类似目录/文件夹
    - datasets 类似文件
    - 两者都可以有用户自定义的 **attributes** **属性**
- 特点：
    - 文件是二进制但可移植
    - 自动处理数据类型、endianness 等问题
    - 提供丰富的 **C API**，并有 Fortran、Java 绑定
    - 提供大量示例代码和数据片段
- 官网： [http://www.hdfgroup.org/HDF5/](http://www.hdfgroup.org/HDF5/)
 
**CDF (NASA Common Data Format)**

- 定义：NASA 提出的 **多维数据集存储抽象模型**
- 开发：NASA Goddard Space Flight Center
- **数据模型**：类似 HDF5
    - 都有 datasets 和 attributes
    - 但 CDF **没有** **groups**
- 特点：
    - 文件为二进制，可移植
    - 提供 C、Java 和 Fortran API
- 官网： [http://cdf.gsfc.nasa.gov/](http://cdf.gsfc.nasa.gov/)
 
**netCDF (Network Common Data Format)**

- 定义：“一种与机器无关的科学数据表示格式”
- 开发：Unidata（UCAR，美国大气研究大学联盟）
- ⚠️ 完全 **不兼容** **CDF**
- 数据模型：
    - **Classic**（大多数现有文件采用）
    - **Enhanced / netCDF-4**（基于 HDF5 构建）
- 特点：
    - 文件为二进制，可移植
    - 提供丰富的库：C、Fortran、Java
- 官网： [http://www.unidata.ucar.edu/software/netcdf/](http://www.unidata.ucar.edu/software/netcdf/)
 
好的 👍 我继续帮你把 **netCDF →** **学科专用格式** **→ Digital forensics → forensic** **工具** 这一部分整理成结构化中英双语笔记：
 
**netCDF (Network Common Data Format)**

- **定义**：一种与机器无关的科学数据表示格式
- **开发**：Unidata（UCAR，美国大气研究大学联盟）
- **兼容性**：与 CDF 完全不兼容
- **数据模型**：
    - **Classic**：大多数现有 netCDF 文件使用
    - **Enhanced / netCDF-4**：基于 HDF5 构建
- **特点**：
    - 可移植的二进制格式
    - 提供丰富的库：C, FORTRAN, Java
- 官网： [http://www.unidata.ucar.edu/software/netcdf/](http://www.unidata.ucar.edu/software/netcdf/)
 
**Discipline-specific formats** **学科专用格式**

- **GRIB (GRIdded Binary)**
    - 全球气象/天气数据的标准
- **FITS (Flexible Image Transport System)**
    - 天文学领域广泛使用的开放标准
    - 专为科学图像设计，包含丰富的 metadata
- **CFD GNS (CFD General Notation System)**
    - 计算流体力学模拟常用的开放标准
    - 被 CFX, Fluent, Star-CD 等支持，OpenFOAM 也有转换器
- **DICOM (Digital Imaging and Communications in Medicine)**
    - 医学成像半专有标准（主要由扫描仪厂商制定）
    - 不仅包含文件格式，还涉及通信协议等
    - 医学影像领域的通用标准

**共同点**：这些科学专用格式通常都配套有 **I/O library** 支持
 
**Digital forensics** **数字取证**

- **定义**：使用工具从文件或磁盘镜像中恢复数字数据
- **应用场景**：
    - 灾难恢复
    - 调查或执法用途
- **基本原理**：删除文件 ≠ 删除数据
    - “删除”通常只是从 inode 表移除链接（unlink inode）
    - 实际的比特序列仍保留在磁盘上，直到被覆盖
    - ⚠️ 已覆盖的数据不可恢复
- **目标**：从孤立的比特流中恢复文件的原始含义
- **限制**：加密数据无法通过数字取证恢复
 
**Simple forensic file tools** **简单取证工具**  
**1. file**

- 功能：读取文件头来判断文件类型
- 支持多种文本/二进制文件类型
- 不依赖文件扩展名
- 基于查找表（Unix-like 系统中默认为 /etc/magic）

示例：  
me@solaris$ file s_factor.o￼s_factor.o: ELF 32-bit MSB relocatable SPARC Version 1￼  
me@linux$ file factor.exe￼factor.exe: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV) dynamically linked...￼  
me@linux$ file s_factor.c￼s_factor.c: English text￼
 
**2. strings**

- 功能：从目标文件（object/binary）中提取可打印字符串
- 对应程序中的文本行
- 可用于嵌入 CVS/SVN 版本信息

示例：  
me@cygwin$ strings s_factor.o￼s_fact_allocate_vecs￼s_fact_allocate_vecs￼
 
**3. od (octal dump)**

- 功能：显示二进制文件的内容
- 可指定输出格式：字符、整型、浮点、双精度等
- 示例命令：￼$ od -c unknown-file￼￼输出示例（ASCII 解释，每 8-bit 一组）：￼0000000 377 330 377 340 \0 020 J F I F \0 001 001 \0 \0 001￼0000020 \0 001 \0 \0 377 333 \0 C \0 \b 006 006 \a 006 005 \b￼...￼
 
好的 👍 我来帮你把最后一部分 **od** **示例** **→ Emacs →** **高级取证工具** **→ Summary** **总结** **→ Acknowledgements** **致谢** 整理成笔记格式，中英双语：
 
**Example: od -b** **示例**  
命令：  
~ % cat input.txt￼100￼101￼102￼103￼104￼105￼  
~ % od -b input.txt￼0000000 061 060 060 012 061 060 061 012 061 060 062 012 061 060 063 012￼0000020 061 060 064 012 061 060 065 012￼0000030￼  
解释：

- od -b → 按 **octal** **八进制** 显示文件内容
- 061 060 060 → 对应 ASCII 编码 100
- 012 → 对应换行符 \n
 
**Simple forensic tools: Emacs**  
**简单取证工具：****Emacs**

- 经典的 Emacs 编辑器不仅是文本编辑器
- 也是一个非常强大的 **二进制文件查看****/****编辑器**
 
**More sophisticated forensics** **更复杂的取证工具**

- 市面上有很多数据恢复/取证公司：
    - 有些开发自己的工具
    - 大多数提供数据恢复服务
- **著名开源取证工具**：
    - **The Sleuth Kit** → [http://www.sleuthkit.org/](http://www.sleuthkit.org/)
        - 支持 Windows, Linux, MacOS, Unix
    - **Foremost** → [http://foremost.sourceforge.net/](http://foremost.sourceforge.net/)
        - 支持 Linux, Unix
 
**Summary** **总结**

- **数字数据存储形式**：
    - 存储在内存、磁盘、磁带中，都是最基本的 **bits****（二进制** **0** **和** **1****）**
- **描述性覆盖层** **(descriptive overlays)**：
    - 文件系统 (file system)
    - 字节编码 (byte encoding)
    - 文件格式 (file format)￼→ 它们赋予比特意义
- **挑战**：
    - 一旦丢失这些描述性 metadata，长期保存就会面临巨大问题
- **趋势**：
    - HPC 系统中的数据存储日益标准化
    - 使用标准格式 → 更好的可移植性 & 长久性
    - 更便于长期数据管理
 
**Acknowledgements** **致谢**

- 本讲义最初由 **Rob Baxter (EPCC, University of Edinburgh)** 编写
- 后续由 **Adam Carter (EPCC)** 与 **Chris Wood (EPCC)** 更新
- © 2015–2025 爱丁堡大学
- 允许为个人学习目的复制
 
📌 至此，你这节课的 **完整课堂笔记** 已经全部整理完毕：从 **数据格式与编码** **→** **字节序** **→** **浮点数** **→** **文件系统** **→** **科学文件格式** **→** **取证工具** **→** **总结**。  
要不要我帮你做一个 **整节课的****“****知识结构图****/****思维导图****”**（例如用树状结构：Data → Encoding/Format/File system → Scientific formats → Forensics → Summary），方便复习？
 \> 来自 \<[https://chatgpt.com/c/68daebe7-bb44-832a-b954-f16701ee982d](https://chatgpt.com/c/68daebe7-bb44-832a-b954-f16701ee982d)\>
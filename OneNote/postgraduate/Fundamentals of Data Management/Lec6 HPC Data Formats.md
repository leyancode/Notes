**HPC Data Formats**  
**Lecturer: Adrian Jackson**  
Email: _a.jackson@epcc.ed.ac.uk_  
Twitter: _@adrianjhpc_
 
**I/O****￼**

![Exported image](Exported%20image%2020260730191036-0.png)

- **I/O** **对所有应用****/****代码都是必不可少的**
    - 程序总需要 **读入** **(read in)** 或 **产生** **(produce)** 数据
    - 包括 **Instructions** 和 **Data**
- **基本层次结构** **(Basic hierarchy)**
    - CPU → Cache → Memory → Devices (包括 I/O)
- **在** **HPC** **系统中常常被****“****遗忘****”**
    - **Linpack** 测试并非 I/O bound
    - 不依赖 CPU 时钟频率或内存大小
- **在编程中也常被忽视**
    - 程序的开始和结束 I/O 往往被当作无关紧要的开销
 
**Challenges of I/O****￼**

![Exported image](Exported%20image%2020260730191037-1.png)

- I/O 超越了 **process–memory model**
    - 内存中的数据必须实际写入到外部设备上
- **文件** **(Files)** **的限制**
    - 文件格式往往与程序常见数据结构不匹配 (如 flat file / array)
    - 文件中常缺少数据的描述信息
- **系统相关性**
    - I/O libraries 或 options 依赖具体系统
    - 硬件在不同系统上差异显著
- **多种数据格式** **(formats)**
    - text, binary, big/little endian, Fortran unformatted …
    - 性能与可用性差异明显
- **磁盘系统复杂** **(Disk systems are complicated)**
    - RAID, 磁盘缓存 (disk caching), 内存缓存 (in-memory caching), I/O nodes, 网络等
 
**Data Formats****￼**

![Exported image](Exported%20image%2020260730191038-2.png)

- **数据存储和结构的描述方法**
- **POSIX I/O**
    - 文件的标准接口 (Standard interface to files)
    - Unix/Linux 常用方式
    - 假设系统有单一文件系统 (single filesystem)
    - 提供 open, close, write, read 等调用
    - **缺点**:
        - 不支持并行或 HPC I/O
        - 许多 NFS 出于性能原因并未完全实现 POSIX
        - 不规定文件中数据的组织方式
 
**Filesystems****￼**

![Exported image](Exported%20image%2020260730191039-3.png)

- **磁盘上存储数据的方式** **(Ways to store data on disk)**
- **文件系统的两个组成部分**:
    1. **Data storage**: 数据以 **blocks** 的形式存储
    2. **Indexing**: 通过索引 (如文件名) 关联数据块
- **文件可能包含多个** **blocks**
    - inodes → blocks 或 sectors
 
**Filesystems Examples****￼**

![Exported image](Exported%20image%2020260730191040-4.png)

- 不同文件系统定义了 **inodes → blocks** 的映射方式
    - 以及目录、文件名等的组织结构
- 还有一些替代方案 (如 log-structured filesystem)
- 常见文件系统：
    - **ext***: ext3, ext4
    - **xfs**
    - **zfs**
    - **btrfs**
- **文件系统选择的重要性**
    - 可能影响性能 (performance) 或功能需求
    - 但大多数情况下默认文件系统即可满足需求

好的 Leyan，我会继续帮你整理成课堂笔记风格，保持 **中英双文****+****关键术语英文原文**，方便复习。下面是整理好的版本：
 
**Files and Directories****￼**

![Exported image](Exported%20image%2020260730191041-5.png)

**Filesystems** **基础**

- **Filesystems** = 存储数据的结构化方式 (structured ways to store data)
- **Files**: 存放具体的数据 (specific data)
- **Directories**: 存放具体的文件 (specific files)
- **POSIX I/O** 并不规定文件中要存放什么内容
    - 对于文件系统来说，文件只是 **bytes of data**
    - 对于应用和用户来说，更有用的是结构和信息 (structure + information)
- **Metadata** 的作用
    - 让用户理解数据内容 (understand data)
    - 让应用独立处理数据 (process data independently)
 
**NetCDF**  
**简介**

![Exported image](Exported%20image%2020260730191042-6.png)

- **NetCDF = Network Common Data Format**
- 组成部分：
    - **Data model** (数据模型)
    - **File format** (文件格式)
    - **Application Programming Interface (API)**
    - **Library implementing the API** (提供 API 的库)
- 起源：
    - 由美国 **Unidata** 开发，主要用于 **Earth Science / Geoscience data**
    - 得到 **NSF (National Science Foundation)** 资助
- 特点：
    - **Software library + self-describing data format**
    - 可移植 (portable)，与机器无关 (machine independent)
    - NetCDF 4.0 之后可基于 **HDF5** 或原生 NetCDF 格式
        - HDF5 提供更大文件支持和无限数组维度 (unlimited array dimensions)
 
**NetCDF** **的定位** **(niche)**

![Exported image](Exported%20image%2020260730191046-7.png)

- **面向数组的科学数据** **(array-oriented scientific data)**
- **区别于其他数据管理方式**：
    - 单文件自描述 (self-describing file)，不同于数据库 (databases)
    - 高效直接访问 (efficient direct access)，不同于 XML
    - 提供多维数组抽象 (multidimensional array abstraction)，适合科学计算
    - 避免依赖外部表/注册表 (unlike GRIB, BUFR)
    - 注重简洁 (simplicity) 而非复杂功能 (unlike HDF5)
- **网络功能**: 内置支持客户端访问服务器上的结构化数据
- **用户社区**:
    - 支持众多第三方应用
    - 第三方 API (C, Fortran, Python, Matlab, Ruby …)
    - 社区约定 (如 **Climate and Forecast (CF) metadata conventions**)
 
**NetCDF** **的演变**

![Exported image](Exported%20image%2020260730191047-8.png)

- **Two Data Models**:
    1. **Classic model** (netCDF-1, netCDF-2, netCDF-3)
    2. **Enhanced model** (netCDF-4)
- **Two Formats (with variants)**:
    - Classic format + 64-bit offset variant (大文件支持)
    - netCDF-4 (HDF5-based) + Classic model variant
- **APIs**:
    - **C-based APIs**: C, C++, Fortran-77, Fortran-90, Perl, Python, Ruby, Matlab …
    - **Java interface**
- **兼容性**:
    - 新版本支持所有旧版本的数据模型、文件格式和 API
    - 不同语言 API 生成的文件可互相读取

![Exported image](Exported%20image%2020260730191048-9.png)   
**NetCDF Data Model (****数据模型组成****)**

![Exported image](Exported%20image%2020260730191049-10.png)

- **Variables**: N 维数组 (char, byte, short, int, float, double)
- **Dimensions**: 名称 + 长度
- **Attributes**: 注释/元数据 (annotations, metadata)
- **Groups**: 分层结构 (hierarchical, 类似 directories)
- **User-defined types**: 用户自定义类型
 
**Parallel Functionality (****并行功能****)**

- **Parallel HDF5**
- **NetCDF 4** 支持部分并行功能
- **Parallel NetCDF (PNetCDF)**
    - 独立库，可并行写 NetCDF 3 (及以下版本) 文件
    - 后续 NetCDF 4 版本中也整合了部分 PNetCDF 功能
 
👉 **复习重点提示** **(Review Notes)**

![Exported image](Exported%20image%2020260730191050-11.png)

- **NetCDF =** **数据模型** **+** **文件格式** **+ API +** **库**
- **Classic vs Enhanced model** 的区别
- **HDF5** **的优势**：大文件支持 + 无限维度
- **Metadata (****元数据****)** 在科学数据中的重要性
- **并行支持**: PNetCDF 与 Parallel HDF5 的差异

好的 Leyan 👍 我会把你提供的 **NetCDF** **文件示例** **+** **各种** **API** **示例** 整理成课堂笔记风格，保持 **中英双文**，并加上小结，方便你复习。
 
**NetCDF File** **示例**  
netcdf pres_temp_4D {￼dimensions: ￼ level = 2 ;￼ latitude = 6 ;￼ longitude = 12 ;￼ time = UNLIMITED ;  
variables: ￼ float latitude(latitude) ;￼ latitude:units = "degrees_north" ;￼ float longitude(longitude) ;￼ longitude:units = "degrees_east" ;￼ float pressure(time, level, latitude, longitude) ;￼ pressure:units = "hPa" ;￼ float temperature(time, level, latitude, longitude) ;￼ temperature:units = "celsius" ;  
data: ￼ latitude = 25, 30, 35, 40, 45, 50 ;￼ longitude = -125, -120, … ;￼ pressure = 900, 901, 902, … ;￼ temperature = 9, 10, 11, … ;￼}  
**解释**

- **dimensions**: 定义维度 (time, level, latitude, longitude)
- **variables**: 多维数组 (含 units 属性作为 metadata)
- **data**: 实际数据 (pressure, temperature 等)
- **time = UNLIMITED** 表示可追加时间序列
 
**NetCDF Programming Interfaces (APIs)**

- **支持语言**:
    - C, FORTRAN 77/90, C++, Perl, Java, Python, Ruby, NCL, Matlab, Objective C, Ada, R
    - 其中一些为 **第三方** **(third-party)**
- **核心实现**:
    - **C interface** 是除 Java 外所有接口的核心
 
**C API** **示例**

![Exported image](Exported%20image%2020260730191051-12.png)

#include \<netcdf.h\>￼…￼int ncid, x_dimid, y_dimid, varid;￼int dimids[NDIMS];￼int data_out[NX][NY];￼…￼if ((retval = nc_create(FILE_NAME, NC_CLOBBER, &ncid))) {￼ printf("Error: %s\n", nc_strerror(retval));￼ exit(1);￼}￼nc_def_dim(ncid, "x", NX, &x_dimid);￼nc_def_dim(ncid, "y", NY, &y_dimid);￼dimids[0] = x_dimid;￼dimids[1] = y_dimid;￼nc_def_var(ncid, "data", NC_INT, NDIMS, dimids, &varid);￼nc_enddef(ncid);￼nc_put_var_int(ncid, varid, &data_out[0][0]);￼nc_close(ncid);  
👉 **流程**:

1. nc_create 创建文件
2. nc_def_dim 定义维度
3. nc_def_var 定义变量
4. nc_enddef 结束定义模式
5. nc_put_var_* 写数据
6. nc_close 关闭文件
 
**Fortran 90 API** **示例**  
use netcdf￼integer :: ncid, varid, dimids(NDIMS)￼integer :: x_dimid, y_dimid  
call check( nf90_create(FILE_NAME, NF90_CLOBBER, ncid) )￼call check( nf90_def_dim(ncid, "x", NX, x_dimid) )￼call check( nf90_def_dim(ncid, "y", NY, y_dimid) )￼dimids = (/ y_dimid, x_dimid /)￼call check( nf90_def_var(ncid, "data", NF90_INT, dimids, varid) )￼call check( nf90_enddef(ncid) )￼call check( nf90_put_var(ncid, varid, data_out) )￼call check( nf90_close(ncid) )  
contains￼subroutine check(status)￼ integer, intent(in) :: status￼ if (status /= nf90_noerr) then￼ print *, trim(nf90_strerror(status))￼ stop "Stopped"￼ end if￼end subroutine check  
👉 Fortran 里常用 check 子程序检查错误状态。
 
**Java API** **示例**  
import ucar.ma2.*;￼import ucar.nc2.*;  
NetcdfFileWriter dataFile = null;￼try {￼ dataFile = NetcdfFileWriter.createNew(NetcdfFileWriter.Version.netcdf3, filename);￼ Dimension xDim = dataFile.addDimension(null, "x", NX);￼ Dimension yDim = dataFile.addDimension(null, "y", NY);￼ List\<Dimension\> dims = new ArrayList\<\>();￼ dims.add(xDim);￼ dims.add(yDim);￼ Variable dataVariable = dataFile.addVariable(null, "data", DataType.INT, dims);￼ dataFile.create();￼ dataFile.write(dataVariable, dataOut);￼} catch (IOException | InvalidRangeException e) {￼ e.printStackTrace();￼} finally {￼ if (null != dataFile)￼ try { dataFile.close(); } catch (IOException ioe) { ioe.printStackTrace(); }￼}  
👉 面向对象式接口，使用 Dimension 和 Variable 类。
 
**Python API** **示例**  
#from netCDF4_classic import Dataset￼#from numpy import arange, dtype  
nx = 6; ny = 12￼ncfile = Dataset('simple_xy.nc','w')  
data_out = arange(nx*ny) # 1D array￼data_out.shape = (nx,ny) # reshape to 2D array  
ncfile.createDimension('x', nx)￼ncfile.createDimension('y', ny)  
data = ncfile.createVariable('data', dtype('int32').char, ('x','y'))￼data[:] = data_out  
ncfile.close()￼print('*** SUCCESS writing example file simple_xy.nc!')  
👉 **Python** **使用最简洁**，配合 numpy 数组操作。
 
**Review Notes (****复习重点****)**

1. **NetCDF file** **结构** = dimensions + variables + attributes + data
2. **核心** **API** **流程** (create → define dims → define vars → enddef → write → close)
3. **C** **是核心接口**，Java 独立实现
4. **多语言支持**，Python 最简洁，Fortran 常与科学计算结合
5. **自描述** **(self-describing)** 是 NetCDF 的关键特点

好的 Leyan 👍 我会把你提供的这部分内容继续整理成课堂笔记风格，分成 **Python / C++ /** **高性能** **NetCDF / Parallel NetCDF /** **系统上的** **NetCDF** 五个部分，保持 **中英双文**，并附带复习小结。
 
**Python API** **示例**  
#from netCDF4_classic import Dataset￼#from numpy import arange, dtype  
nx = 6; ny = 12￼ncfile = Dataset('simple_xy.nc','w')  
data_out = arange(nx*ny) # 1D array￼data_out.shape = (nx,ny) # reshape to 2D array  
ncfile.createDimension('x', nx)￼ncfile.createDimension('y', ny)  
data = ncfile.createVariable('data', dtype('int32').char, ('x','y'))￼data[:] = data_out  
ncfile.close()￼print('*** SUCCESS writing example file simple_xy.nc!')  
👉 **Python** **特点**

- 依赖 netCDF4 库 + numpy
- 代码简洁，特别适合科研原型
 
**C++ API** **示例**  
#include \<netcdf\>￼using namespace netCDF;￼using namespace netCDF::exceptions;  
try {￼ NcFile dataFile("simple_xy.nc", NcFile::replace);￼ NcDim xDim = dataFile.addDim("x", NX);￼ NcDim yDim = dataFile.addDim("y", NY);￼ vector\<NcDim\> dims;￼ dims.push_back(xDim);￼ dims.push_back(yDim);￼ NcVar data = dataFile.addVar("data", ncInt, dims);￼ data.putVar(dataOut);￼ return 0;￼}￼catch(NcException& e) {￼ e.what();￼ return NC_ERR;￼}  
👉 **C++** **特点**

- 面向对象 API (NcFile, NcDim, NcVar)
- 与 C API 相比更现代化，但底层逻辑类似
 
**High-performance NetCDF**

![Exported image](Exported%20image%2020260730191052-13.png)

- **增强版** **NetCDF (version 4** **及以后****)**
    - 基于 **HDF5** 实现并行/高性能 I/O
    - 文件必须存为 **HDF5** **格式**

#include "netcdf.h"￼#include "hdf5.h"  
MPI_Comm comm = MPI_COMM_WORLD;￼MPI_Info info = MPI_INFO_NULL;￼int ncid, v1id, dimids[NDIMS];￼size_t start[NDIMS], count[NDIMS];  
res = nc_create_par(FILE, NC_NETCDF4 | NC_MPIIO, comm, info, &ncid);￼res = nc_def_dim(ncid, "d1", DIMSIZE, &dimids[0]);￼res = nc_def_dim(ncid, "d2", DIMSIZE, &dimids[1]);￼res = nc_def_var(ncid, "v1", NC_INT, NDIMS, dimids, &v1id);￼res = nc_enddef(ncid);  
start[0] = mpi_rank * DIMSIZE / mpi_size;￼start[1] = 0;￼count[0] = DIMSIZE / mpi_size;￼count[1] = DIMSIZE;  
res = nc_var_par_access(ncid, v1id, NC_INDEPENDENT);￼res = nc_put_vara_int(ncid, v1id, start, count, &data[mpi_rank * QTR_DATA]);￼res = nc_close(ncid);  
MPI_Finalize();  
👉 **要点**

- 使用 nc_create_par 创建并行文件
- NC_NETCDF4 | NC_MPIIO 打开 HDF5 并行模式
- 允许进程独立/协作写入数据
 
**Parallel NetCDF (PNetCDF)**

![Exported image](Exported%20image%2020260730191056-14.png)

- **PNetCDF** = 独立库，用于并行写入 **NetCDF** **格式** **(CDF-1, CDF-2, CDF-5)** 文件
- 唯一支持 **并行** **I/O** **的原生** **NetCDF** **格式库**
- 部分功能后续也被合并到 **NetCDF-4** 中

ret = ncmpi_create(MPI_COMM_WORLD, argv[1],￼ NC_CLOBBER | NC_64BIT_OFFSET, MPI_INFO_NULL, &ncfile);  
ret = ncmpi_def_dim(ncfile, "d1", nprocs, &dimid);￼ret = ncmpi_def_var(ncfile, "v1", NC_INT, ndims, &dimid, &varid1);￼ret = ncmpi_def_var(ncfile, "v2", NC_INT, ndims, &dimid, &varid2);￼ret = ncmpi_put_att_text(ncfile, NC_GLOBAL, "string", 13, buf);  
ret = ncmpi_enddef(ncfile);￼ret = ncmpi_put_vara_int_all(ncfile, varid1, &start, &count, &data);￼ret = ncmpi_put_vara_int_all(ncfile, varid2, &start, &count, &data);￼ret = ncmpi_close(ncfile);  
MPI_Finalize();  
👉 **要点**

- 与 NetCDF C API 类似，但函数前缀为 **ncmpi_**
- 支持 **collective I/O (*_all)**，保证数据一致性
 
**NetCDF on Systems (****以** **ARCHER2** **为例****)**

- 三种版本的 NetCDF 都可通过 **modules** 加载：
- **Serial NetCDF (v4)**
    - module: cray-netcdf
    - versions: 4.7.4.0, 4.7.4.2, 4.7.4.3
- **Parallel NetCDF with HDF5**
    - module: cray-netcdf-hdf5parallel
    - versions: 4.7.4.0, 4.7.4.2, 4.7.4.3
- **独立** **PNetCDF**
    - module: cray-parallel-netcdf
    - versions: 1.12.1.0, 1.12.1.2, 1.12.1.3
 
**Review Notes (****复习重点****)**

- **Python API**: 简洁，科研常用
- **C++ API**: 面向对象，封装度高
- **High-performance NetCDF (HDF5-based)**: 用于 **NetCDF-4** 并行 I/O
- **PNetCDF**: 独立库，支持 **NetCDF** **格式并行写入** **(CDF-1/2/5)**
- **系统模块** **(HPC** **上的** **NetCDF)**:
    - Serial NetCDF
    - Parallel NetCDF with HDF5
    - PNetCDF

好的 Leyan ✨ 我来帮你把这部分 **HDF5** 笔记整理为课堂风格，中英双文+保留关键术语。这样能方便你和前面 **NetCDF** 的笔记呼应。
 
**HDF5 (Hierarchical Data Format, version 5)**  
来源: [www.hdfgroup.org](https://www.hdfgroup.org/)  
HDF5 是一种独特的数据管理技术套件，能够处理 **极大规模和复杂的数据集合**。
 
**What is HDF5?**

![Exported image](Exported%20image%2020260730191056-15.png)  
![Exported image](Exported%20image%2020260730191058-16.png)

- **Versatile data model**
    - 能表示非常复杂的数据对象和各种 **metadata**
- **Portable file format**
    - 完全可移植的文件格式
    - 对数据对象数量或大小 **无限制** **(no limit)**
- **Software library**
    - 可运行在从 **laptops → massively parallel systems** 的各种计算平台上
    - 提供高级 API：**C, C++, Fortran 90, Java**
- **Performance features**
    - 内建性能优化：访问时间 (access time) 和存储空间 (storage space)
- **Tool support**
    - 提供工具和应用来 **管理、处理、查看、分析** 数据
 
**Data Model and Portability****￼**

![Exported image](Exported%20image%2020260730191058-17.png)

- **类比** **(Analogy)**
    - HDF = 文件中的目录结构 (directory/file hierarchy)
    - **Groups ≈ directories/folders**
    - **Datasets ≈ files**
- **Attributes**
    - groups 和 datasets 都可带有 **user-defined attributes**
- **Binary but portable**
    - HDF5 文件是二进制的，但平台无关
    - 处理数据类型、字节序 (endianness) 等问题由 HDF5 模型自动完成
 
**Why HDF5?**

![Exported image](Exported%20image%2020260730191059-18.png)

- **Structure** → 层次化、灵活的数据模型
- **Portability** → 可跨平台移植
- **Performance** → 高性能访问与存储优化
- **Self-Describing** → 文件包含自身的结构与描述信息
- **Tool Support** → 有广泛的工具生态
- **Pre-Optimised** → 针对存储和检索已优化
- **Parallel-Ready** → 原生支持并行功能
 
**HDF5 Data Model and File Structure**

![Exported image](Exported%20image%2020260730191100-19.png)  
![Exported image](Exported%20image%2020260730191101-20.png)

**Groups**

- 定义: **HDF5 group** = 一种分组结构 (grouping structure)，包含 **零个或多个** **groups** **或** **datasets**，以及相关 **metadata**
- **组成部分**:
    1. **Group header** → 包含 group 名称 & attributes
    2. **Group symbol table** → 列出 group 内容

**路径表示** **(Path Names)**

- 类似 **UNIX** **目录**
- / → root group
- /foo → root group 的成员，名为 foo
- /foo/zoo → foo group 的成员，名为 zoo

￼￼

![Exported image](Exported%20image%2020260730191105-21.png)  
![Exported image](Exported%20image%2020260730191106-22.png)

**￼****Review Notes (****复习重点****)**

1. **HDF5** **三要素**: Data Model + File Format + Software Library
2. **Groups ≈ Directories, Datasets ≈ Files**
3. **Attributes** **可附加到** **groups** **或** **datasets**
4. **Portable Binary Format** → 自带类型 & 字节序兼容
5. **优势** **(Why HDF5)**: Structure, Portability, Performance, Self-describing, Parallel-ready
6. **与** **NetCDF** **的关系**: NetCDF-4 基于 HDF5 构建

好的 Leyan 👍 我来帮你把这部分 **Performance** **与** **HPC I/O** **挑战** 整理成课堂笔记格式，中英双文，并保持你之前的复习风格。
 
**Performance (****性能****)**

![Exported image](Exported%20image%2020260730191107-23.png)

**Performance – HDF5 vs MPI-I/O**

![Exported image](Exported%20image%2020260730191108-24.png)

- **HDF5**: 提供 **高级抽象** **(high-level abstraction)**，带有数据模型与 metadata 支持，用户友好
- **MPI-I/O**: 提供 **底层接口** **(low-level interface)**，需要显式管理文件布局与访问模式，但能实现更细粒度的优化
- **对比**:
    - HDF5 适合科学应用（结构化、可移植、self-describing）
    - MPI-I/O 更接近硬件，性能更高但复杂度大
    - 实际 HPC 中，HDF5 通常调用 MPI-I/O 作为底层实现
 
**Challenges of I/O (I/O** **的挑战****)**

![Exported image](Exported%20image%2020260730191109-25.png)

**硬件层次** **(Hardware Layers)**

- **标准计算机硬件** **(Standard computer hardware)**
    - 多个磁盘 (multiple disks)
    - 接口: **PATA, SATA, SCSI (SAS)**
    - 优化: RAID (striping, replication), SSD (fast disks)
- **HPC / Server / SAN** **硬件**
    - 大量磁盘 (many disks)
    - 接口: **SCSI (SAS), Fibre Channel**
    - 优化: striping, 多适配器 & 网络接口

**网络文件系统** **(Network filesystems)**

- 提供多机、多用户共享访问 (access from many machines & users)
- 常见问题: **带宽瓶颈、锁管理复杂、并发冲突**
 
**Abstract vs Actual Hardware Hierarchy**

- **Abstract model (****抽象模型****)**
    - CPU → Memory → Disk (I/O devices, peripherals)
- **Actual hardware**
    - CPU, Memory, Graphics, Disk → 通过 **Northbridge / Southbridge controllers** 连接
- 说明：实际 I/O 通道比抽象模型复杂，涉及 **controller****、****bus****、****network** 等
 
**性能指标** **(Performance Interfaces & Bandwidth)**

![Exported image](Exported%20image%2020260730191110-26.png)  

|   |   |
|---|---|
|**Interface**|**Throughput Bandwidth (MB/s)**|
|SATA|600 MB/s|
|Serial Attached SCSI(SAS)|600 MB/s|
|NVMe|2000+ MB/s|
|Modern device peak|15 GB/s (单设备可达)|
 
**High Performance or Parallel I/O**

![Exported image](Exported%20image%2020260730191111-27.png)

- **多进程并行写入同一文件的问题**
    - 基础操作系统 **不支持**
    - 数据以磁盘块 (如 4K) 为单位缓存，**cache** **不一致** **(non-coherent)**
    - 即使写入不同区域，也可能出错
- **读取也存在挑战**
    - 例如：1024 个进程同时打开文件，可能超过文件系统文件句柄 (file handles) 限制
- **数据分布性** **(distributed data)**
    - 与处理器数量、任务划分方式强相关
- **并行文件系统** **(Parallel file systems)**
    - 支持多进程访问
    - 但实现复杂，用户使用难度大

￼Hierarchy • Hardware level • Disk drives, NVMe, SSDs • Networking and connected resources • Storage level • File system, object store, database • Semantics and guarantees • Software interface • I/O libraries and interfaces • How Storage and Hardware levels are exposed • Metadata and structure • Application and system level structure • Turning bytes into information/knowledge￼

![Exported image](Exported%20image%2020260730191114-28.png)

￼￼￼  
**Review Notes (****复习重点****)**

1. **HDF5 vs MPI-I/O**: 高层抽象 vs 底层高性能接口
2. **I/O** **硬件优化**: RAID, SSD, SAS, Fibre Channel, NVMe
3. **抽象层次** **vs** **实际硬件**: Northbridge/Southbridge 架构
4. **带宽差异**: NVMe ≫ SATA/SAS
5. **并行** **I/O** **挑战**: cache 不一致、文件句柄限制、数据分布复杂
6. **Parallel file systems**: 提供解决方案，但管理复杂

好的 Leyan 👍 我来帮你把这一部分 **MPI-I/O & HPC** **并行文件系统** **(Lustre)** 整理成课堂笔记风格，中英双文，并加上复习要点。
    
**MPI-I/O****（并行输入输出）**  
**基本目标与定位****￼**

![Exported image](Exported%20image%2020260730191115-29.png)

- **Aim**：提供对**single file** **的** **distributed access****￼**目标：允许多个 MPI 进程并行访问同一个文件
- **File shared****￼**文件是多个进程共享的
- **Control by programmer****￼**I/O 行为由程序员显式控制
- **Looks like a serial program has written the data****￼**从结果上看，文件内容就像是由一个串行程序写出的
- **Part of MPI-2 standard****￼**MPI-I/O 是 MPI-2 标准的一部分
- **Typically available in modern MPI libraries****￼**大多数现代 MPI 实现都支持 MPI-I/O
- **ROMIO****￼**若原生不支持，可使用 ROMIO（基于 MPI-1 实现的 MPI-IO 层）
 
**实现与性能特性**

- **Performance dependent on implementation****￼**实际性能强烈依赖于 MPI 库与底层文件系统的实现
- **Built on MPI collective operations****￼**MPI-I/O 内部大量使用 collective communication
- **Data structure defined by programmer****￼**文件视图与数据布局由程序员定义
- **Everything done with MPI calls****￼**所有 I/O 操作通过 MPI 接口完成
 
**数据模型与编程方式（****MPI-I/O cont.****）****￼**

![Exported image](Exported%20image%2020260730191116-30.png)

- **Array-based I/O****￼**MPI-I/O 天然适合数组型数据
- **Derived datatype****￼**每个进程使用 derived datatype 描述自己持有的数据子集
- **Each process creates description of subset it holds****￼**进程只描述“我负责哪一部分数据”
- **No checking of correctness****￼**MPI-I/O 不检查数据布局是否逻辑正确，责任在程序员
- **Library handles read and write to files****￼**实际的文件读写由 MPI-I/O 库完成
- **Don’t ever have all in memory****￼**不需要将全部数据集中到单一进程内存中
- **Scale as well as MPI communications****￼**I/O 扩展性通常与 MPI 通信的扩展性相当
- **Best performance for big reads/writes****￼**对大块、连续读写性能最佳
- **Info object****￼**通过 MPI_Info 向系统传递 implementation-specific 参数
- **Lots of optimisations and tuning****￼**支持大量底层优化与性能调优机制
 
**HPC / Parallel Systems** **中的** **I/O** **结构**  
**集群整体结构（概念层面）****￼**

![Exported image](Exported%20image%2020260730191117-31.png)

- **Basic cluster structure****￼**基本结构：
    - Individual nodes
    - Network
    - Network-attached filesystem
    - Local scratch disks
- **Multiple I/O systems****￼**通常存在多套 I/O 系统，例如：
    - Home filesystem
    - Work / Scratch filesystem
- **Different optimisation goals****￼**有的系统优化吞吐（production），有的优化交互访问（user access）
 
**I/O** **层次结构（****Hierarchy****）****￼**

![Exported image](Exported%20image%2020260730191118-32.png)

- **Compute Node****￼**计算节点，包含 Processor / Core
- **Local Disk / Scratch****￼**节点本地磁盘，延迟低但不可共享
- **Network****￼**连接计算节点与存储系统
- **Network Attached Filesystem****￼**共享并行文件系统（如 Lustre）
- **Filesystem servers and caching****￼**使用文件系统服务器、缓存等机制进行优化
 
**I/O** **与计算的分离架构**

- **I/O Compute Node****￼**专门负责 I/O 的节点
- **I/O Software / System****￼**独立的 I/O 软件栈
- **Adapters and Disks****￼**多磁盘、多适配器并行访问
- **Purpose****￼**将计算与 I/O 解耦，减少计算节点等待 I/O 的时间
   

￼￼￼￼  
**Lustre** **文件系统**  
**三大功能单元** **(Three functional units)****￼**

![Exported image](Exported%20image%2020260730191119-33.png)

1. **Object Storage Servers (OSS)**
    - 将数据存储在一个或多个 **Object Storage Targets (OSTs)**
    - OST 处理客户端请求与底层物理存储的交互
    - 一个 OSS 通常服务 2–8 个 OSTs
    - 文件系统容量 = 各个 OST 容量之和
    - OSS 独立并行运行
2. **Metadata Target (MDT)**
    - 每个文件系统有一个 MDT
    - 存储 **metadata**：文件名、目录、权限、文件布局
    - MDT 存储在 **Metadata Server (MDS)** 上
3. **Clients**
    - 提供 **标准** **POSIX** **访问**
 
**Lustre** **其他特点****￼**

![Exported image](Exported%20image%2020260730191120-34.png)  
![Exported image](Exported%20image%2020260730191124-35.png)

- **支持多种网络**: Infiniband, Ethernet, Myrinet, Quadrics
- **Striping (****条带化****)**
    - 数据分块 (file split into units)
    - 分布到不同的 OSTs (round robin)
    - 支持并行读写 (simultaneous read/write)
 
￼￼  
**Lustre Commands** **与** **Striping**

![Exported image](Exported%20image%2020260730191125-36.png)

**Striping****（条带化）的作用**

- **Improves bandwidth and overall performance****￼**提高 I/O 带宽、整体可用性能以及最大文件大小
- **Improves maximum file size****￼**单文件可扩展到多个 OST
- **Costs and overheads****￼**会引入通信开销和潜在的竞争
- **Serialisation risk****￼**多个进程访问同一条带单元时可能发生串行化
 
**lfs** **命令（****Lustre File System****）**

- **lfs df -h****￼**查询 Lustre 文件系统容量￼用途：间接了解 OST 数量与使用情况
- **lfs getstripe dirname****￼**查询目录的 striping 配置￼包括：
    - stripe count
    - stripe size
- **lfs setstripe dirname 0 -1 -1****￼**典型用途：大文件
    - stripe size: 0（默认）
    - start index: -1（自动选择）
    - stripe count: -1（使用所有 OST）
- **lfs setstripe dirname 0 -1 1****￼**典型用途：大量小文件
    - 单 OST，减少 metadata 和 contention
 
**GPFS****（****IBM Spectrum Scale****）**

![Exported image](Exported%20image%2020260730191126-37.png)

**基本特性**

- **IBM General Purpose File System**
- **Files broken into blocks, striped over disks****￼**文件被拆成 blocks 并条带化存储
- **Distributed metadata****￼**元数据分布式存储，包括 directory tree
- **Extended directory indexes****￼**支持大规模目录
- **Failure aware****￼**基于 partition 的故障感知
- **Fully POSIX compliant****￼**完全符合 POSIX 语义
 
**存储与策略**

- **Storage pools and policies****￼**使用存储池和策略管理数据
- **Groups disks****￼**磁盘分组
- **Tiered storage****￼**按性能、可靠性、本地性分层
- **Policies move and manage data****￼**策略驱动数据迁移与管理
- **Active data management****￼**主动管理数据位置
- **High performance**
 
**GPFS** **配置方式（****GPFS cont.****）**

![Exported image](Exported%20image%2020260730191127-38.png)

- **Shared disks****￼**通过 SAN 直接连接集群
- **Network Shared Disks (NSD)****￼**使用 NSD servers 提供共享磁盘
- **NSD across clusters****￼**可跨集群使用，性能优于 NFS
 
**I/O Strategies****（****I/O** **策略）**

![Exported image](Exported%20image%2020260730191128-39.png)

**单文件策略（****One file per program****）**

- **Basic approach****￼**程序只使用一个文件
- **Works well for serial programs**
- **Often first choice for many codes**
- **Works for shared memory parallelism**
 
**分布式内存下的问题**

- **Distributed memory****￼**数据不再位于单一地址空间
- **Master I/O strategy**
    - 一个进程负责所有 I/O
    - 其他进程通过通信发送数据
- **High overhead**
    - 通信成本高
    - 内存压力大
    - 无法有效利用并行 I/O 资源
 
**I/O Strategies****（续）**

![Exported image](Exported%20image%2020260730191129-40.png)

**Individual files****（每进程一个文件）**

- **Each process writes its own file****￼**文件可位于 shared filesystem 或 local scratch
- **Maximises I/O system usage**
- **File contents depend on CPU count and decomposition**
- **Pre / post-processing required****￼**改变进程数时需额外处理
- **Scalability issues**
    - 大规模进程时文件系统压力大
    - 文件数量或 file handles 成为瓶颈
- **Not a long-term solution**
- **Motivation for I/O libraries**
 
**What to use?****（如何选择）**

![Exported image](Exported%20image%2020260730191130-41.png)

- **Parallel computing context assumed**

**以性能为核心**

- **MPI-I/O**

**以数据格式和** **metadata** **为核心**

- **HDF5**

**面向地球科学生态**

- **NetCDF**
 
**Object Stores****（对象存储）**

![Exported image](Exported%20image%2020260730191134-42.png)

**基本特征**

- **Restricted interface**
- **Put**：创建新对象
- **Get**：获取对象
- **No POSIX-style requirements****￼**不需要完整的 POSIX I/O 语义
 
**设计理念**

- **Objects are traditionally immutable****￼**对象创建后不可修改
- **No locking for writes****￼**消除写入锁需求
- **Log-append style updates****￼**类似追加日志，拷贝并更新
- **Object ID generated on creation****￼**用于访问和定位

**xarray & zarr****￼**

![Exported image](Exported%20image%2020260730191135-43.png)

**zarr**

- **Python data format**
- 支持 **out-of-memory, chunked storage** （分块存储，支持超大数据集）
- 基于 **HDF5**，可与对象存储交互（如 **S3**）
- 多种存储后端 (**multiple backends**): Memory, Files, Object Stores
- 支持 **压缩存储** **(compressed storage)**

**xarray**

- **Python data format**
- 提供 **labelled arrays** 和 **datasets**
- 对 datasets 可直接进行操作（更高层抽象）
- 支持多种存储格式作为后端
 
**Summary****￼**

![Exported image](Exported%20image%2020260730191136-44.png)

- **硬件和软件** 都会影响 **性能** **(performance)** 和 **功能** **(functionality)**
- 性能并非唯一考虑因素：
    - **Security (****安全性****)**
    - **Resilience (****鲁棒性****/****容错性****)**
    - **Richness of data formats (****数据格式的丰富性****)**
- 实践中大多数情况下只需关注：
    - 数据是否有足够 **metadata** 以保证长期可理解性
    - 数据是否 **安全****/****鲁棒**
    - 是否能从所选编程语言中访问
- 如果性能成为瓶颈：
    - 需要检查 **hardware, software layers, parallel I/O**
    - **数据存储位置与访问方式** 会显著影响性能
 
**NetCDF** **细节** **(Details)****￼**

![Exported image](Exported%20image%2020260730191137-45.png)

**Dimensions (****维度****)**

- 定义变量形状、常用网格和坐标系统
- 每个 dimension 有 **名称** **(name)** 和 **长度** **(length)**
- 可被多个变量复用
- 可与 **coordinate variables** 关联，用于识别坐标轴
- **Classic NetCDF**: 最多只有一个 unlimited length (record dimension)
- **Enhanced NetCDF**: 多个维度都可设为 unlimited
 
**Variables (****变量****)****￼**

![Exported image](Exported%20image%2020260730191138-46.png)

- 定义存储数据的实体￼￼￼
- ![Exported image](Exported%20image%2020260730191139-47.png)
- **属性** **(Attributes)**:
    - 名称 (name)
    - 类型 (type)
    - 形状 (shape)
    - 可选属性 (attributes, e.g. units)
    - 值 (values)

**类型** **(Type)**

- **Classic NetCDF** **类型**:
    - char, byte (8-bit), short (16-bit), int (32-bit), float (32-bit), double (64-bit)
- **Enhanced NetCDF** **类型**:
    - 增加无符号类型: ubyte, ushort, uint, uint64
    - 增加 int64 (64-bit), string (可变长字符串)
    - 支持用户自定义类型 (user-defined types)

**形状** **(Shape)**

- 维度列表
- **无维度**: 标量 (scalar)
- **1** **维**: 向量 (1D)
- **2** **维**: 矩阵/网格 (2D grid)
- **多维**: 更复杂的数据结构

**属性** **(Attributes)**

- 指定数据属性，例如 **units**
 
**Review Notes (****复习重点****)**

1. **zarr**: Python 分块存储格式，适合大数据 + S3 对象存储
2. **xarray**: 高层 Python 数据模型，支持 labelled arrays/datasets
3. **总结**: 关注 metadata、安全性、语言可访问性，性能问题需考虑硬件和并行层
4. **NetCDF dimensions**: classic → 1 个 unlimited；enhanced → 多个 unlimited
5. **NetCDF variables**: name, type, shape, attributes, values
6. **类型**: Classic (char, int, float, double) vs Enhanced (unsigned types, int64, string, user-defined)

好的 Leyan 👍 我来帮你把 **CDL****、****NetCDF utilities****、****HDF5** **细节** 这部分整理成课堂笔记格式，中英双文，方便复习。  
￼￼

![Exported image](Exported%20image%2020260730191140-48.png)

**CDL (Common Data Language)**
 ![Exported image](Exported%20image%2020260730191144-49.png)

- **定义**:
    - **人类可读** **(human readable)** 的 NetCDF 数据集表示法
    - 可通过 **ncdump** 程序从 NetCDF 文件中获取

**示例****:**  
netcdf example { // example of CDL notation￼dimensions:￼ x = 3 ;￼ y = 8 ;  
variables:￼ float rh(x, y) ;￼ rh:units = "percent" ;￼ rh:long_name = "relative humidity" ;  
// global attributes￼ :title = "simple example, lacks some conventions" ;  
data:￼ rh =￼ 2, 3, 5, 7, 11, 13, 17, 19,￼ 23, 29, 31, 37, 41, 43, 47, 53,￼ 59, 61, 67, 71, 73, 79, 83, 89 ;￼}
 
**NetCDF Utilities**
 ![Exported image](Exported%20image%2020260730191145-50.png)

- **ncdump**
    - 将 NetCDF 文件转为 CDL 格式
    - 可选择输出全部、部分或仅 metadata
    - 可显示 **C / Fortran** **顺序索引**
- **ncgen**
    - 从 CDL 生成 NetCDF 文件
    - 还能生成 **C, Fortran, Java** **程序** 来创建对应的 NetCDF 文件
    - 与 ncdump 配合，可手动编辑或自动生成数据文件结构
- **nccopy**
    - 复制 NetCDF 文件到新文件
    - 支持 **压缩** 或 **格式转换** (e.g. classic → enhanced)
- **nc-config**
    - 生成编译/链接 NetCDF 程序所需的 flags
    - 示例:￼cc myapp.c -o myapp `nc-config --cflags --libs`￼f95 myapp.f -o myapp `nc-config --fflags --flibs`

**￼****￼****￼****HDF5 Details**  
**HDF5 Attributes****￼**

![Exported image](Exported%20image%2020260730191145-51.png)

**基本概念**

- **Attributes****￼**Attributes 是附加在 primary datasets、groups 或 named datatypes 上的**小型数据集**
- **Small named datasets****￼**属性本质上是带名字的小数据集
- **Name–value pairs****￼**以键值对形式存在
- **Multiple entries****￼**value 可以包含同一 datatype 的多个值
 
**API** **与性能注意事项****￼**

![Exported image](Exported%20image%2020260730191146-52.png)

- **Separate API****￼**Attribute 的读写使用独立的 API（H5A 系列）
- **Performance impact****￼**过大的 attribute 集合会影响性能
- **Design intent****￼**Attributes 主要用于 metadata，而非大规模数据存储
 
**The HDF5 API****（模块化接口）****￼**

![Exported image](Exported%20image%2020260730191147-53.png)

HDF5 使用模块化 API，不同前缀代表不同功能域。  
**File**

- **H5F**：File-level access routines￼示例：H5Fopen, H5Fcreate

**Group**

- **H5G**：Group 操作￼用于创建和管理对象集合￼示例：H5Gset

**Datatype**

- **H5T**：Datatype functions
    - 创建和操作 simple / compound datatypes
    - 定义数组中元素的类型

**Dataspace**

- **H5S**：Dataspace functions
    - 定义数据的维度和形状
    - 描述数据在文件中的逻辑空间

**Dataset**

- **H5D**：Dataset functions
    - 操作数据本身
    - 决定数据如何存储

**Property Lists**

- **H5P**：Property list functions
    - 控制对象创建和访问属性

**Attributes**

- **H5A**：Attribute 操作接口

**Compression**

- **H5Z**：Compression registration

**Error Handling**

- **H5E**：Error handling routines

**References**

- **H5R**：Reference routines

**Identifiers**

- **H5I**：Identifier routines
 
**Example: Create and Close a File****￼**

![Exported image](Exported%20image%2020260730191148-54.png)

**HDF5 Identifier**

- **hid_t****￼**HDF5 定义的类型，用于标识文件、dataset 等对象
 
hid_t file; /* identifier */  
**创建并关闭文件**
 
file = H5Fcreate(FILE, H5ACC_TRUNC, H5P_DEFAULT, H5P_DEFAULT);￼status = H5Fclose(file);

- **H5ACC_TRUNC****￼**若文件已存在，则覆盖
- **H5P_DEFAULT****￼**使用默认创建和访问属性
 
**Example: Creating a Dataset**  
**定义** **identifiers**

![Exported image](Exported%20image%2020260730191149-55.png)

hid_t dataset, datatype, dataspace;
 
**创建** **Dataspace**

- **Describe the size of the array**
 
dimsf[0] = NX;￼dimsf[1] = NY;￼dataspace = H5Screate_simple(RANK, dimsf, NULL);

- **RANK****￼**数据维度数量
- **dimsf****￼**各维度大小
 
**定义** **Datatype**
 
datatype = H5Tcopy(H5T_NATIVE_INT);￼status = H5Tset_order(datatype, H5T_ORDER_LE);

- **H5T_NATIVE_INT****￼**使用 C 本地 int 类型
- **H5T_ORDER_LE****￼**Little-endian 存储顺序
 
**创建** **Dataset**
 
dataset = H5Dcreate(file, DATASETNAME, datatype, dataspace, H5P_DEFAULT);

- 使用定义好的 dataspace 和 datatype
- 使用默认 dataset creation properties
 
**Example: Write Data to a File**

![Exported image](Exported%20image%2020260730191153-56.png)

status = H5Dwrite(dataset, H5T_NATIVE_INT, H5S_ALL,￼ H5P_DEFAULT, data);  
**关键点**

- **data****￼**C 中标准的 int 数组
- **H5S_ALL****￼**使用整个 dataspace
- **H5P_DEFAULT****￼**默认数据传输属性
 
**Example: Read Data from a File**

![Exported image](Exported%20image%2020260730191153-57.png)

status = H5Dread(dataset, H5T_NATIVE_INT, H5S_ALL,￼ H5P_DEFAULT, data);

- **Exactly analogous to write****￼**读写接口在语义上是对称的
 
**Hyperslabs****￼**

![Exported image](Exported%20image%2020260730191154-58.png)

**定义**

- **Hyperslabs****￼**数据集中的子区域
- 用于对 dataset 的局部读写

**参数**

- **start****￼**起始位置，例如 (0,1)
- **stride****￼**跨度，例如 (4,3)
- **count****￼**块的数量，例如 (2,4)
- **block****￼**每块大小，例如 (3,2)
 
**Parallel HDF5****￼**

![Exported image](Exported%20image%2020260730191155-59.png)

**基本设计目标**

- **Designed to work with MPI and MPI-IO**
- **Single file image****￼**所有进程看到同一个文件视图
 
**与串行** **HDF5** **的兼容性**

- **Parallel files compatible with serial HDF5**
- **Sharable across platforms****￼**串行与并行平台可互操作
 
**为什么不用** **one file per process**

- **Expensive post-processing**
- **Not usable by other processes**
- **Poor portability**
 
**核心设计原则****￼**

![Exported image](Exported%20image%2020260730191156-60.png)

- **Portable parallel I/O interface****￼**标准化接口，适用于不同平台
- **Single shared file model****￼**避免文件爆炸和后处理问题

![[Lec6 HPC Data Formats - Ink.svg]]

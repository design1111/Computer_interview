## linux目录

![image-20240922110245851](Linux驱动开发.assets/image-20240922110245851.png)





## 环境配置

https://www.linaro.org/downloads/

详细地址：https://releases.linaro.org/components/toolchain/binaries/4.9-2017.01/arm-linux-gnueabihf/

![image-20240610192250630](Linux驱动开发.assets/image-20240610192250630.png)

需要下载linaro当中的编译器，为了交叉编译



首先在Linux当中创建文件夹

![image-20240610191650201](Linux驱动开发.assets/image-20240610191650201.png)

创建一个linux_code文件夹，然后在文件夹中创建**nfs**和**tool**文件夹；再将下载好的编译器放到tool文件夹下面。

然后再创建一个文件夹，

在 **usr/local**当中创建一个**arm**的文件夹。

![image-20240610192121066](Linux驱动开发.assets/image-20240610192121066.png)



## 在vscode中配置Remote-SSH

首先安装remote-ssh插件，

<img src="Linux驱动开发.assets/image-20240610222339353.png" alt="image-20240610222339353" style="zoom:50%;" />

<img src="Linux驱动开发.assets/image-20240610222407638.png" alt="image-20240610222407638" style="zoom:50%;" />

<img src="Linux驱动开发.assets/image-20240610223158564.png" alt="image-20240610223158564" style="zoom:33%;" />

<img src="Linux驱动开发.assets/image-20240610223218107.png" alt="image-20240610223218107" style="zoom:33%;" />

<img src="Linux驱动开发.assets/image-20240610223302346.png" alt="image-20240610223302346" style="zoom:50%;" />

然后重启vscode就能打开了。但是每次打开都需要登录密码。

## makefile

Makefile 里面是由一系列的规则组成的，这些规则格式如下：

```
目标…... :  依赖文件集合…… 
 命令 1 
 命令 2 
 …… 
```



```
  main: main.o input.o calcu.o 
      gcc -o main  main.o input.o calcu.o 
  main.o: main.c 
      gcc -c main.c 
  input.o: input.c 
      gcc -c input.c 
  calcu.o: calcu.c 
      gcc -c calcu.c 
   
 clean: 
     rm *.o 
     rm main 
```

**上述代码中所有行首需要空出来的地方一定要使用“TAB”键！不要使用空格键！**

简洁版：

```
  objects = main.o input.o calcu.o
  main: $(objects)  
      gcc -o main $(objects) 
  
  .PHONY : clean 
  
  %.o : %.c 
      gcc -c $<  
   
  clean: 
	rm *.o 
    rm main 
```

```
  .PHONY : clean 
```

这句是**伪目标**。声明 clean 为伪目标以后不管当前目录下是否存在名为“clean”的文件，输入“make clean”的话规则后面的 rm 命令都会执行。

### 条件判断

其中条件关键字有 4 个：ifeq、ifneq、ifdef 和 ifndef，这四个关键字其实分为两对、ifeq 与ifneq、ifdef 与 ifndef，先来看一下 ifeq 和 ifneq，ifeq 用来判断是否相等，ifneq 就是判断是否不相等。

### Makefile函数使用

```
$(函数名 参数集合)
```

或

```
${函数名 参数集合}
```

参数集合是函数的多个参数，参数之间以逗号“,”隔开，函数名和参数之间以“空格”分隔开，函数的调用以“$”开头。



### 赋值符 =

```
1 name = zzk
2 curname = $(name)
3 name = zuozhongkai
4
5 print:
6 @echo curname: $(curname)
```

![image-20240922095947522](Linux驱动开发.assets/image-20240922095947522.png)

### 赋值符 :=

```
1 name = zzk
2 curname := $(name)
3 name = zuozhongkai
4
5 print:
6 @echo curname: $(curname)
```

![image-20240922100045520](Linux驱动开发.assets/image-20240922100045520.png)

### 赋值符 ?=

“?=”是一个很有用的赋值符，比如下面这行代码：

```
curname ?= zuozhongkai
```

上述代码的意思就是，如果变量 curname 前面没有被赋值，那么此变量就是“zuozhongkai”，如果前面已经赋过值了，那么就使用前面赋的值。

### 变量追加 +=

Makefile 中的变量是字符串，有时候我们需要给前面已经定义好的变量添加一些字符串进去，此时就要使用到符号“+=”，比如如下所示代码：

```
objects = main.o inpiut.o

objects += calcu.o
```

一开始变量 objects 的值为“main.o input.o”，后面我们给他追加了一个“calcu.o”，因此变量 objects 变成了“main.o input.o calcu.o”，这个就是变量的追加。

![image-20240922100454500](Linux驱动开发.assets/image-20240922100454500.png)



## shell指令

### 查看进程

可以通过 `ps` 命令查看进程相关属性和状态，这些信息包括进程所属用户，进程对应的程序，进程对 `cpu` 和内存的使用情况等信息。



## gdb安装



检测是否安装

```
rpm -qa | grep gdb
```



```
安装指令
sudo apt-get update
sudo apt-get install  gdb
```

```
检查是否安装成功
gdb -v
```



### 入门操作指令

```
命令               简写形式         说明
backtrace          bt、where       显示backtrace
break              b               设置断点
continue           c、cont         继续执行
delete             d               删除断点
finish                             运行到函数结束
info breakpoints                   显示断点信息
next               n               执行下一行
print              p               显示表达式
run                r               运行程序
step               s               一次执行一行，包括函数内部
x                                  显示内存内容
until              u               执行到指定行
其他命令
directory          dir             插入目录
disable            dis             禁用断点
down               do              在当前调用的栈帧中选择要显示的栈帧
edit               e               编辑文件或者函数
frame              f               选择要显示的栈帧
forward-search     fo              向前搜索
generate-core-file gcore           生成内核转存储
help                h              显示帮助一览
info                i              显示信息
list                l              显示函数或行
nexti               ni             执行下一行(以汇编代码为单位)
print-object        po             显示目标信息
sharelibrary        share          加载共享的符号
stepi               si             执行下一行
```

### (1）创建测试代码

```
gcc -g -o test.exe test.c
```

###  (2) 启动gdb

  在 Linux 操作系统中，当程序执行发生异常崩溃时，系统可以将发生崩溃时的内存数据、调用堆栈情况等信息自动记录下载，并存储到一个文件中，该文件通常称为 core 文件，Linux 系统所具备的这种功能又称为核心转储（core dump）。幸运的是，GDB 对 core 文件的分析和调试提供有非常强大的功能支持，当程序发生异常崩溃时，通过 GDB 调试产生的 core 文件，往往可以更快速的解决问题。

- 查看是否开启 core dump 这一功能

```
ulimit -a
```

如果 core file size（core 文件大小）对应的值为 0，表示当前系统未开启 core dump 功能

- 开启 core dump

```
ulimit -c unlimited
```

- 启动gdb

```
gdb test.exe
```



### (3) 设置断点

（1）根据行号设置断点

```
方法一：(gdb) b 28
方法二：(gdb) b main1.cpp : 29
```

（2）根据函数设置断点

```
(gdb) b main
```

（3）根据条件设置断点

```
(gdb) b test.c:10 if a == 1
```

（4）根据偏移量设置断点

```
(gdb) b +12
```

（5）根据地址设置断点

```
(gdb) b *0x40059b
```

（6）临时断点只生效一次

```
(gdb) tbreak test.c:12
```

（7）显示断点信息

```
(gdb) info break
```

（8）清除断点

```
清除某个断点 (gdb) delete 4
清除所有断点 (gdb) delete 
清除当前行断点  (gdb) clear
```

（9）追踪变量

```
display 变量
undisplay + 变量名编号  —— 取消对先前设置的那些变量的跟踪
```



### (4) 运行

```
运行run 				  			 				 r
继续单步调试next，不进入函数体（相当于F10） 		       n
继续执行到下一个断点(continue) 						 c
逐语句step（相当于F11）				 				  s
```

- 查看源码和行号(list)

```
l
```

###  (5) 打印变量的值

（1）打印变量

```
p a
```

（2）打印指针

```
p p
```

（3）打印main函数中的变量a

```
p 'main'::a
```

（4）打印指针指向的内容，@后面跟的是打印的长度

```
p *p@3
```

（5）设置变量打印

```
(gdb) set $index = 0
(gdb) p p[$index]
$6 = 1
```

（6）设置打印格式

```
x 按十六进制格式显示变量
d 按十进制格式显示变量
u 按十六进制格式显示无符号整型
o 按八进制格式显示变量
t 按二进制格式显示变量
a 按十六进制格式显示变量
c 按字符格式显示变量
f 按浮点数格式显示变量
(gdb) p/x a（按十六进制格式显示变量）
```

```
(gdb) p/x a
$7 = 0x1
```



### (6) 退出

```
q
```

### (7)额外补充

（1）结构体显示

（2）设置变量的值

```
set var —— 修改变量的值
```

（3）查看函数调用

```
bt —— 看到底层函数调用的过程【函数压栈】
```

（4）指定行号跳转

```
until + 行号 —— 进行指定位置跳转，执行完区间代码
```

（5）强制执行函数

```
finish —— 在一个函数内部，执行到当前函数返回，然后停下来等待命令
```

（6）显示所有warnings -Wall

```
gcc -g -Wall program.c -o program
g++ -g -Wall program1.c program2.c -o program 
```



参考资料：[【Linux】GDB调试教程（新手小白）_match: gdbm-devel-CSDN博客](https://blog.csdn.net/lovely_dzh/article/details/109160337)

[【Linux】GDB保姆级调试指南（什么是GDB？GDB如何使用？）_gdb教程-CSDN博客](https://blog.csdn.net/weixin_45031801/article/details/134399664)



## cmake

安装指令

```
sudo apt-get install cmake
```

安装成功检测

```
cmake --version
```

### 单个文件

（1）新建一个main.cpp文件，程序为cout<<"hello world"；

（2）新建一个CMakeLists.txt文件，内部内容为：

```
project(HELLO)
add_executable(hello ./main.cpp)
```

（3）执行cmake指令

```
cmake ./
```

![image-20240919211356849](Linux驱动开发.assets/image-20240919211356849.png)

执行完毕成功，生成以下文件

CMakeCache.txt  CMakeFiles  cmake_install.cmake  CMakeLists.txt  main.cpp  Makefile

（4）使用make指令

使用make工具编译我们的工程，生成可执行文件hello

![image-20240919211637146](Linux驱动开发.assets/image-20240919211637146.png)

对于指令的解释：

⚫**第一行 project(HELLO)**
project 是一个命令，命令的使用方式有点类似于 C 语言中的函数，因为命令后面需要提供一对括号，
并且通常需要我们提供参数，多个参数使用空格分隔而不是逗号“,”。
project 命令用于设置工程的名称，括号中的参数 HELLO 便是我们要设置的工程名称；设置工程名称并
不是强制性的，但是最好加上。
⚫ **第二行 add_executable(hello ./main.c)**
add_executable 同样也是一个命令，用于生成一个可执行文件，在本例中传入了两个参数，第一个参数
表示生成的可执行文件对应的文件名，第二个参数表示对应的源文件；所以 add_executable(hello ./main.c)表
示需要生成一个名为 hello 的可执行文件，所需源文件为当前目录下的 main.c。

### Out_of_Source方式创建

cmake 生成的文件以及最终的可执行文件 hello 与工程的源码文件 main.c 混在了一起，这使得工程看起来非常乱，当我们需要清理 cmake 产生的文件时将变得非常麻烦，这不是我们想看到的；我们需要将构建过程生成的文件与源文件分离开来，不让它们混杂在一起，也就是使用 out-of-source 方式构建。

将 cmake 编译生成的文件清理下，然后在工程目录下创建一个 build 目录，如下所示：

然后进入到 build 目录下执行 cmake：

```
cd build/
cmake ../
make
```

这样 cmake 生成的中间文件以及 make 编译生成的可执行文件就全部在 build 目录下了，如果要清理工程，直接删除 build 目录即可，这样就方便多了。

### 多个文件



指令查找链接：

[cmake-commands(7) — CMake 3.5.2 Documentation](https://cmake.org/cmake/help/v3.5/manual/cmake-commands.7.html)





## 静态库和动态库

在Windows下静态库的后缀为：.lib、动态库后缀为：.dll；

而在Linux下静态库的后缀为：.a或 .la、动态库的后缀为：.so。

###  （1）静态库

**<font color='red'>静态库生成</font>**

1. 写源文件，通过 `gcc -c xxx.c` 生成目标文件。
2. 用 `ar` 归档目标文件，生成静态库。
3. 配合静态库，写一个使用静态库中函数的头文件。
4. 使用静态库时，在源码中包含对应的头文件，链接时记得链接自己的库。



使用gcc，为这两个源文件生成目标文件：

```
g++ -c test.cpp test2.cpp
```

我们就得到了 test.o 和 test2.o。

**<font color='blue'>归档目标文件，得到静态库</font>**

我们使用 ar 将目标文件归档：

```undefined
ar crv libmylib.a test.o test2.o
```

我们就得到了libmylib.a，这就是我们需要的静态库。

上述命令中 crv 是 ar的命令选项：

- c 如果需要生成新的库文件，不要警告
- r 代替库中现有的文件或者插入新的文件
- v 输出详细信息

通过 `ar t libmylib.a` 可以查看 `libmylib.a` 中包含的目标文件。

可以通过 `ar --help` 查看更多帮助。

注意：我们要生成的库的文件名必须形如 `libxxx.a` ，这样我们在链接这个库时，就可以用 `-lxxx`。
反过来讲，当我们告诉编译器 `-lxxx`时，编译器就会在指定的目录中搜索 `libxxx.a` 或是 `libxxx.so`。

**<font color='blue'>生成对应的头文件</font>**

头文件定义了 libmylib.a 的接口，也就是告诉用户怎么使用 libmylib.a。

新建my_lib.h， 写入内容如下：



**<font color='red'>静态库使用</font>**



参考：[Linux静态库生成指南 - JollyWing - 博客园 (cnblogs.com)](https://www.cnblogs.com/jiqingwu/p/4325382.html)

[详解Linux下静态库/动态库的生成和使用（含代码示例和操作流程）&&动态库和静态库的区别_生成静态库的过程及作用-CSDN博客](https://blog.csdn.net/weixin_47826078/article/details/120474883)

###  （2）动态库

动态库生成



动态库使用



## 文件格式介绍

### elf文件

来源：[ELF文件格式的详解_.elf-CSDN博客](https://blog.csdn.net/pingxiaozhao/article/details/109239221)

#### 1.说明

ELF的英文全称是The Executable and Linking Format，最初是由UNIX系统实验室开发、发布的ABI(Application Binary Interface)接口的一部分，也是Linux的主要可执行文件格式。

从使用上来说，主要的ELF文件的种类主要有三类：

- 可执行文件（.out）：Executable File，包含代码和数据，是可以直接运行的程序。其代码和数据都有固定的地址 （或相对于基地址的偏移 ），系统可根据这些地址信息把程序加载到内存执行。
- 可重定位文件（.o文件）：Relocatable File，包含基础代码和数据，但它的代码及数据都没有指定绝对地址，因此它适合于与其他目标文件链接来创建可执行文件或者共享目标文件。
- 共享目标文件（.so）：Shared Object File，也称动态库文件，包含了代码和数据，这些数据是在链接时被链接器（ld）和运行时动态链接器（ld.so.l、libc.so.l、ld-linux.so.l）使用的。

本文主要从elf文件的组成构造的角度来进行分析，将elf文件的解析通过一步一步的分析得到里面的信息，同时通过python脚本解析，可以直观的看到文件的信息，通过本文的阅读，将对elf文件格式有着更加深刻的理解。

#### 2.elf文件的基本格式

elf文件是有一定的格式的，从文件的格式上来说，分为汇编器的链接视角与程序的执行视角两种去分析ELF文件。

![img](Linux驱动开发.assets/e90f0f98e086faa3189e6368e490aba5.png)

从程序执行视角来说，这就是Linux加载器加载的各种Segment的集合。比如只读代码段、数据的读写段、符号段等等。而从链接的视角上来看，elf又分为各种的sections。

注意Section Header Table和Program Header Table并不是一定要位于文件开头和结尾的，其位置由ELF Header指出，上图这么画只是为了清晰。

为了彻底的弄清楚elf文件的内容，可以先从ELF文件的头部开始分析。

![img](Linux驱动开发.assets/9c483f3f3a3f317efd8b7cde15324908.png)

根据readelf可以得到该文件的头部信息的情况。

![img](Linux驱动开发.assets/73d1be52b22c5de73db92ccbcd5ceb88.png)

根据定义，elf32的结构体定义，在Linux上可以在`/usr/include/elf.h`中找到

```
#define EI_NIDENT (16)
 
typedef struct
{
  unsigned char e_ident[EI_NIDENT];     /* Magic number and other info */
  Elf32_Half    e_type;                 /* Object file type */
  Elf32_Half    e_machine;              /* Architecture */
  Elf32_Word    e_version;              /* Object file version */
  Elf32_Addr    e_entry;                /* Entry point virtual address */
  Elf32_Off     e_phoff;                /* Program header table file offset */
  Elf32_Off     e_shoff;                /* Section header table file offset */
  Elf32_Word    e_flags;                /* Processor-specific flags */
  Elf32_Half    e_ehsize;               /* ELF header size in bytes */
  Elf32_Half    e_phentsize;            /* Program header table entry size */
  Elf32_Half    e_phnum;                /* Program header table entry count */
  Elf32_Half    e_shentsize;            /* Section header table entry size */
  Elf32_Half    e_shnum;                /* Section header table entry count */
  Elf32_Half    e_shstrndx;             /* Section header string table index */
} Elf32_Ehdr;
```

上述的`Elf32_Half`定义

```cobol
/* Type for a 16-bit quantity.  */



typedef uint16_t Elf32_Half;
```

其中`Elf32_Word`的定义

```cobol
/* Types for signed and unsigned 32-bit quantities.  */



typedef uint32_t Elf32_Word;
```

然后`Elf32_Addr`与`Elf32_Off`定义

```cobol
/* Type of addresses.  */



typedef uint32_t Elf32_Addr;



 



/* Type of file offsets.  */



typedef uint32_t Elf32_Off;
```

有了这些数据结构的信息，然后对应具体的数据细节如下：

- **e_ident[EI_NIDENT]**

文件的标识以及标识描述了elf如何编码等信息。

```cobol
Magic：   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
```

关于该结构体的索引可以看下面的表格：

| 名称       | 取值 | 目的           |
| :--------- | :--- | :------------- |
| EI_MAG0    | 0    | 文件标识(0x7f) |
| EI_MAG1    | 1    | 文件标识(E)    |
| EI_MAG2    | 2    | 文件标识(L)    |
| EI_MAG3    | 3    | 文件标识(F)    |
| EI_CLASS   | 4    | 文件类         |
| EI_DATA    | 5    | 数据编码       |
| EI_VERSION | 6    | 文件版本       |
| EI_PAD     | 7    | 补齐字节开始处 |
| EI_NIDENT  | 16   | e_ident[]大小  |

EI_CLASS的内容，当取值为0时，是非法类别，1是32位的目标，2是64位的目标。这里是1所以程序是32位的目标。

EI_DATA表示数据的编码，当为0时，表示非法数据编码，1表示高位在前，2表示低位在前。

EL_VERSION表示了elf的头部版本号码。

前面四个基本上确定的，内容第一个字符为7f，后面用ELF字符串表示该文件为ELF格式。

- **e_type**

该数据类型是uint16_t数据类型的，占两个字节。通过字段查看，可以看到这个值为`00 02`。表格定义如下：

| 名称      | 取值   | 含义               |
| :-------- | :----- | :----------------- |
| ET_NONE   | 0x0000 | 未知目标文件格式   |
| ET_ERL    | 0x0001 | 可重定位文件       |
| ET_EXEC   | 0x0002 | 可执行文件         |
| ET_DYN    | 0x0003 | 共享目标文件       |
| ET_CORE   | 0x0004 | Core文件(转储格式) |
| ET_LOPROC | 0xff00 | 特定处理器文件     |
| ET_HIPROC | 0xffff | 特定处理器文件     |

对应表格内容，可以看到类型为`EXEC`即可执行文件类型。

- **e_machine**

由字段可以看到为`00 28`，关于这个字段的解析，基本上就是表示该elf文件是针对哪个处理器架构的。

下面只列出几个常见的架构的序号

| 名称     | 取值 | 含义                       |
| :------- | :--- | :------------------------- |
| EM_NONE  | 0    | No machine                 |
| EM_SPARC | 2    | SPARC                      |
| EM_386   | 3    | Intel 80386                |
| EM_MIPS  | 8    | MIPS I Architecture        |
| EM_PPC   | 0x14 | PowerPC                    |
| EM_ARM   | 0x28 | Advanced RISC Machines ARM |

通过上述的表格，可以看到该架构是ARM处理器上运行的程序。

- **e_version**

该字段占四个字节，表示当前文件版本的信息。现在取值为`00 00 00 01`。从取值上来看

| 名称       | 取值 | 含义     |
| :--------- | :--- | :------- |
| EV_NONE    | 0    | 非法版本 |
| EV_CURRENT | 1    | 当前版本 |

- **e_entry**

这里表示程序的入口地址，目前为四字节，所以通过字段解析到的内容为`00 00 80 00`。得到可执行程序的入口地址为`0x8000`。

- **e_phoff**

该字段表示程序表头偏移。占四个字节，根据字段解析，可以查看当前的偏移量为`00 00 00 34`。也就是实际的偏移量为52个字节。这52个字节其实就是头部的信息数据结构体的大小。

- **e_shoff**

该区域比较重要，记录了section的偏移地址。为四字节，解析出来的字段为`0x00 04 24 5c`。所以得到地址为0x4245c。

![img](Linux驱动开发.assets/1f3d4258f29aec4604720539c9e097b5.png)

根据这个偏移得到section的内容：

![img](Linux驱动开发.assets/22c4dcee305e9ba41b5e3888e9dea325.png)

通过`readelf -t`也可以得到类似的结果。

![img](Linux驱动开发.assets/87e96734087d02f511be5b11291a0f90.gif)

关于节区如何解析。后面再进行描述。

- **e_flags**

特定处理器格式的标志，这里的字段解析为`05 00 02 00`。与特定的处理器相关。

- **e_ehsize**

elf文件的头部大小。该取值与头文件结构体的大小相关，目前为52字节，即`00 34`。

- **e_phentsize**

程序头部表项大小，当前取值为`00 20`，为32个字节，这里表示

![img](Linux驱动开发.assets/93cfa6df77825fa831b92c996a2e1c08.png)

关于程序表项的解析，后面再进行具体分析。

- **e_phnum**

目前取值为`00 01`，这里表示程序头的个数当前只有一个程序头，如果有多个程序头表，那么会在elf头文件之后，也就是52个字节之后，依次向下排列。因为这里是1，所以只有1个程序头。

- **e_shentsize**

表示节区头部表格大小，解析字段为`00 28`,也就是第一个节区的大小为40个字节的偏移处。根据`e_shoff`可以知道。

![img](Linux驱动开发.assets/ab86727ccf74cc67f5fb4ece37540a46.png)

将从`e_shoff`的区域向后面偏移40个字节，得到第一个节区的内容。

- **e_shnum**

节区的数量，由字段解析得到数据为`00 11`。此时得到节区的数量为17个。通过`readelf -t`也可以解析到节区的数量为17个。

```ruby
bigmagic@bigmagic:~/work/python_elf/elf$ readelf -t rtthread.elf 



There are 17 section headers, starting at offset 0x4245c:
```

- **e_shstrndx**

标记字符串节区的索引。当前的解析为`00 0e`。也就是14个节区为字符节区。

![img](Linux驱动开发.assets/3201993cfbd5aefb749a83f0ce63799e.gif)

到这里，头部信息的相关字段就解析完成了。

#### 4.elf文件的节区（Section）

elf文件中的节是从编译器链接角度来看文件的组成的。从链接器的角度上来看，包括指令、数据、符号以及重定位表等等。

**4.1 节区的作用**

在可从定位的可执行文件中，节区描述了文件的组成，节的位置等信息。通过`readelf -s`可以查看信息。

![img](Linux驱动开发.assets/e4f1bf1059ace38d7a0911d5bd8582a9.png)

这些节信息通过特定的地址偏移组成了一个elf文件的整体。

**4.2 节区的组成**

关于理解ELF中的Section。首先需要知道程序的链接视图，在编译器将一个一个.o文件链接成一个可以执行的elf文件的过程中，同时也生成了一个表。这个表记录了各个Section所处的区域。在程序中，程序的`section header`有多个，但是大小是一样。拿elf32文件来说

```cobol
typedef struct



{



  Elf32_Word sh_name;  /* Section name (string tbl index) */



  Elf32_Word sh_type;  /* Section type */



  Elf32_Word sh_flags;  /* Section flags */



  Elf32_Addr sh_addr;  /* Section virtual addr at execution */



  Elf32_Off sh_offset;  /* Section file offset */



  Elf32_Word sh_size;  /* Section size in bytes */



  Elf32_Word sh_link;  /* Link to another section */



  Elf32_Word sh_info;  /* Additional section information */



  Elf32_Word sh_addralign;  /* Section alignment */



  Elf32_Word sh_entsize;  /* Entry size if section holds table */



} Elf32_Shdr;
```

根据**e_shoff**可以找到section的地址，根据`e_shentsize`可以找到具体的第一个section的内容。

![img](Linux驱动开发.assets/29882d138abd1c2b629c6ee7d47d8feb.gif)

如果要找到每个段的具体细节，首先可以根据e_shstrndx找到节的字段。由于e_shstrndx=14。而且每个为40字节。那么一共是560字节的偏移。从`e_shoff`的地址`0x4245c`开始，首先偏移了`e_shentsize`也就是40个字节。然后向下得到40x14个Section表项。最后可以得到e_shstrndx对应的节区。

![img](Linux驱动开发.assets/3492a88e75e31cab656d9f501b7670ca.png)

为什么首先需要得到这个字符串节区，通过这个就可以得到节区的名字了。然后通过计算，节区字符串存在的区域：

![img](Linux驱动开发.assets/46a7f4845d32c4fd0aa18e029df922a2.png)

每个字符串以`\0`结尾。大小为`0000ab`也就是171个字节。接下来我们来举个具体的例子来解析Section。比如要读取.text的段。那么首先看一下细节。

![img](Linux驱动开发.assets/67f8ff9eb85c96fe1589a8b0deec1c8e.gif)

首先从字段结构体上进行分析：

- sh_name

表示从e_shstrndx的偏移地址开始，得到的字符字符串信息为该段的名字。目前解析到的为0x1b。最后算出得到实际的名称为.text。

![img](Linux驱动开发.assets/67f8ff9eb85c96fe1589a8b0deec1c8e.gif)

- sh_type

字段的类型为01，关于sh_type的类型，解析如下：

```cobol
/* Legal values for sh_type (section type).  */



 



#define SHT_NULL            0        /* Section header table entry unused */



#define SHT_PROGBITS        1        /* Program data */



#define SHT_SYMTAB          2        /* Symbol table */



#define SHT_STRTAB          3        /* String table */



#define SHT_RELA            4        /* Relocation entries with addends */



#define SHT_HASH            5        /* Symbol hash table */



#define SHT_DYNAMIC         6        /* Dynamic linking information */



#define SHT_NOTE            7        /* Notes */



#define SHT_NOBITS          8        /* Program space with no data (bss) */



#define SHT_REL             9        /* Relocation entries, no addends */



#define SHT_SHLIB          10        /* Reserved */



#define SHT_DYNSYM         11        /* Dynamic linker symbol table */



#define SHT_INIT_ARRAY     14        /* Array of constructors */



#define SHT_FINI_ARRAY     15        /* Array of destructors */



#define SHT_PREINIT_ARRAY  16        /* Array of pre-constructors */



#define SHT_GROUP          17        /* Section group */



#define SHT_SYMTAB_SHNDX   18        /* Extended section indeces */



#define    SHT_NUM         19        /* Number of defined types.  */



#define SHT_LOOS           0x60000000    /* Start OS-specific.  */



#define SHT_GNU_ATTRIBUTES 0x6ffffff5    /* Object attributes.  */



#define SHT_GNU_HASH       0x6ffffff6    /* GNU-style hash table.  */



#define SHT_GNU_LIBLIST    0x6ffffff7    /* Prelink library list */



#define SHT_CHECKSUM       0x6ffffff8    /* Checksum for DSO content.  */



#define SHT_LOSUNW         0x6ffffffa    /* Sun-specific low bound.  */



#define SHT_SUNW_move      0x6ffffffa



#define SHT_SUNW_COMDAT    0x6ffffffb



#define SHT_SUNW_syminfo   0x6ffffffc



#define SHT_GNU_verdef     0x6ffffffd    /* Version definition section.  */



#define SHT_GNU_verneed    0x6ffffffe    /* Version needs section.  */



#define SHT_GNU_versym     0x6fffffff    /* Version symbol table.  */



#define SHT_HISUNW         0x6fffffff    /* Sun-specific high bound.  */



#define SHT_HIOS           0x6fffffff    /* End OS-specific type */



#define SHT_LOPROC         0x70000000    /* Start of processor-specific */



#define SHT_HIPROC         0x7fffffff    /* End of processor-specific */



#define SHT_LOUSER         0x80000000    /* Start of application-specific */



#define SHT_HIUSER         0x8fffffff    /* End of application-specific */
```

当前为1，所以得到数据为程序数据。比如`.text .data .rodata`等等。

- sh_flags

表示段的标志，`A`表示分配的内存、AX表示分配可执行、WA表示分配内存并且可以修改。

- sh_addr

加载后程序段的虚拟地址

- **sh_offset**

表示段在文件中的偏移。

- sh_size

段的长度

- sh_addralign

段对齐

- sh_entsize

每项固定的大小

#### 5.elf文件的段(Segment)

关于`Linking View`与`Execution View`的具体含义，可以查看

```cobol
http://www.skyfree.org/linux/references/ELF_Format.pdf
```

这里有一张图值得研究一下：

![img](Linux驱动开发.assets/c03e4f56542a18f4609c01780c60b1b3.png)

对于链接视图，也就是我们前面分析的Section，可以理解目标代码文件的内容布局。而右边的ELF的执行视图，则可以理解为可执行的文件内容布局。链接视图由sections组成，而可执行的文件的内容由segment组成。

两者是有一些区别的，我们平时在进行程序构建的时候理解的.text、.bss、.data段，这些都是section,也就节区的概念。这些段通过`section header table`进行组织与重定位。

但是对于segment来说，程序代码段、数据段是Segment。代码段又可以分为.text，数据段又分为.data、.bss等。

通过`readelf -l`可以查看具体的可执行文件的细节。

![img](Linux驱动开发.assets/b951c9a9422f344ec01e47098aefbb68.png)

这里的信息和程序的加载直接相关。具体的elf文件加载过程这篇文章不会多说，后面会写文章专门叙述。本文的目的是elf文件格式的解析过程。

## 进程、线程

**线程是进程内部的一条执行序列或执行路径，一个进程可以包含多条线程。**

- 从资源分配的角度来看，进程是操作系统进行资源分配的基本单位。
- 从资源调度的角度来看，线程是资源调度的最小单位，是程序执行的最小单位

### 进程



### 线程



线程的实现方式

1. **内核级线程**（由内核直接创建和管理线程，虽然创建开销较大，但是可以利用多处理器的资源）
2. **用户级线程**（由线程库创建和管理多个线程，线程的实现都是在用户态，内核无法感知，创建开销较小，无法使用多处理器的资源）
3. **混合级线程**（结合以上两种方式实现，可以利用多处理器的资源，从而在用户空间中创建更多的线程，从而映射到内核空间的线程中，多对多，N：M（N>>M））

#### 线程库的使用

1、线程的创建

```
#include<phread.h>

int pthread_create(pthread_t *id , pthread_attr_t *attr, void(*fun)(void*), void *arg);

```

- id ：传递一个pthread_t类型的变量的地址，创建成功后，用来获取新创建的线程的TID
- attr：指定线程的属性 默认使用NULL
- fun：线程函数的地址
- arg：传递给线程函数的参数
- 返回值，成功返回0，失败返回错误码





1、查看进程号

```
ps -ef | grep rts
```

1.1 查看该进程下线程号

```
ps -T -p 进程号
```

PIDWie进程号，SPID为线程号，CMD为线程名称。

1.2  实时显示PID进程内的各个线程情况

```
top -H -p PID 
```

1.3 杀进程方法

传递signal给指定PID

```
kill -9 pid
```





# linux驱动八股文

1、驱动程序分为几类？
2、请解释一下Linux驱动程序的基本概念和原理
3、字符设备驱动需要实现的接口通常有哪些？
4、什么是设备树（Device Tree）？它在Linux驱动中的作用是什么？
5、如何编写一个字符设备驱动程序？
6、如何编写一个块设备驱动程序？
7、如何编写一个网络设备驱动程序？
8、主设备号与次设备号的作用
9、交叉编译器的作用
10、硬链接和软链接的区别
11、Linux内核的组成部分？
12、Linux内核有哪些同步方式？
13、如何在Linux系统中加载和卸载内核模块？
14、USB设备在Linux系统中如何进行驱动开发？
15、中断处理和中断控制器编程相关的知识有哪些？
16、用户空间和内核空间的通信方式有哪些？
17、BootLoader、Linux内核、根文件系统的关系？
18、linux内核中EXPORT_SYMBOL宏和EXPORT_SYMBOL_GPL宏的作用
19、DMA（Direct Memory Access）的工作原理是什么？在驱动开发中有哪些应用场景？
20、并行端口和GPIO编程在Linux驱动开发中的应用有哪些？
21、讲解一下时钟、定时器以及延时函数在驱动开发中的使用方法。
22、文件操作函数和IO操作函数在Linux驱动开发中的区别和使用方法是什么？
23、进程上下文和中断上下文有什么区别？在驱动开发过程中如何正确地使用它们？
24、请解释一下Linux字符设备文件系统的注册与管理机制。
25、container_of(ptr, type, member)的作用
26、kmalloc与vmalloc区别
27、内存管理单元MMU的作用？
28、简述MMU将VA转为PA的过程
29、操作系统的内存分配一般有哪几种方式，各有什么优缺点？
30、proc文件系统和sysfs文件系统分别用于什么目的？在驱动开发中如何使用它们？
31、Platform设备和ACPI（Advanced Configuration and Power Interface）之间有什么关系？在驱动开发中如何处理它们？
32、如何进行Linux驱动程序的性能调优和优化？请列举一些常用的技巧。
33、在虚拟化环境下，如何进行设备模拟和虚拟设备驱动开发？
34、设备电源管理及电源状态转换（Power Management）在Linux驱动中的应用方法是什么？
35、如何处理驱动程序中的错误，并进行调试？列举一些常用的内核调试器和跟踪工具。
36、在编写Linux驱动程序时，有哪些安全性与稳定性方面需要考虑的因素？
37、多线程编程和同步机制在Linux驱动开发中的应用有哪些？请举例说明。
38、Linux驱动程序应该考虑哪些可扩展性和可移植性问题？
39、如何解决不同内核版本兼容性问题，在不同版本的Linux系统上运行相同的驱动程序？
40、在嵌入式系统中，如何进行Linux驱动开发？有哪些特殊考虑点？
41、请讲解一下设备模型（Device Model）和总线（Bus）机制在Linux驱动开发中的应用。
42、如何编写文件系统相关的驱动程序，例如FAT、EXT4等？
43、在Linux驱动开发中，如何处理键盘、鼠标和触摸屏等输入设备？
44、视频显示设备驱动开发需要考虑哪些因素？请列举一些相关问题。
45、你了解哪些与Linux驱动开发相关的工具和调试技术？





# RTOS

## 邮箱

邮箱在操作系统中是一种常用的IPC通信方式，邮箱相比于信号量与消息队列来说，其开销更低，效率更高，所以常用来做==线程与线程==、==中断与线程间== 的通信。

通过邮箱，线程或中断服务函数可以将一个或多个邮件放入邮箱中。同样，一个或多个线程可以从邮箱中获得 邮件消息。当有多个邮件发送到邮箱时，通常应将先进入邮箱的邮件先传给线程，也就是说，线程先得到的是 最先进入邮箱的消息，即先进先出原则(FIFO)，同时RT-Thread中的邮箱支持优先级，也就是说在所有等待邮 件的线程中优先级最高的会先获得邮件。

- 邮件支持先进先出方式排队与优先级排队方式，支持异步读写工作方式。
- 发送与接收邮件均支持超时机制。
- 一个线程能够从任意一个消息队列接收和发送邮件。
- 多个线程能够向同一个邮箱发送邮件和从中接收邮件。
- **邮箱中的每一封邮件只能容纳固定的4字节内容**（可以存放地址）。
- 当队列使用结束后，需要通过删除邮箱以释放内存。

邮箱与消息队列很相似，消息队列中消息的长度是可以由用户配置的，但邮箱中邮件的大小却只能是固定容纳4 字节的内容，所以，使用邮箱的开销是很小的，因为传递的只能是4字节以内的内容，那么其效率会更高。


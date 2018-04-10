

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

* 【马哥私房菜】亲情推出《linux shell脚本攻略》视频教程

* 【马哥私房菜】亲情推出 git 视频教程


《Linux命令行与shell脚本编程大全》第三版 学习笔记
=============

第1部分 Part 1 Linux 命令行
-------------

 
# 第1 章 初识Linux shell

本章内容
-  什么是Linux
-  Linux内核的组成
-  探索Linux桌面
-  了解Linux发行版

## 什么是Linux

Linux可划分为以下四部分：

*  Linux内核
*  GNU工具
*  图形化桌面环境
*  应用软件

Linux系统的核心是内核。内核控制着计算机系统上的所有硬件和软件，在必要时分配硬件，
并根据需要执行软件。
内核主要负责以下四种功能：  
* 系统内存管理  
* 软件程序管理   也就是cpu和进程管理
* 硬件设备管理  
* 文件系统管理  

内核管理物理内存，还可以创建和管理虚拟内存。内核通过硬盘上的存储空间来实现虚拟内存，这块区域称为交换空间（swap space） 马哥私房菜 淘宝https://shop592330910.taobao.com/

```
ext                Linux扩展文件系统，最早的Linux文件系统  
ext2               第二扩展文件系统，在ext的基础上提供了更多的功能  
ext3               第三扩展文件系统，支持日志功能  
ext4               第四扩展文件系统，支持高级日志功能  
hpfs               OS/2高性能文件系统  
jfs                IBM日志文件系统  
iso9660            ISO 9660文件系统（CD-ROM）  
minix              MINIX文件系统   
msdos              微软的FAT16  
ncp                Netware文件系统  
nfs                网络文件系统   
ntfs               支持Microsoft NT文件系统  
proc               访问系统信息  
ReiserFS           高级Linux文件系统，能提供更好的性能和硬盘恢复功能  
smb                支持网络访问的Samba SMB文件系统  
sysv               较早期的Unix文件系统  
ufs                BSD文件系统  
umsdos             建立在msdos上的类Unix文件系统  
vfat               Windows 95文件系统（FAT32）  
XFS                高性能64位日志文件系统  
``` 

GNU/Linux shell是一种特殊的交互式工具。它为用户提供了启动程序、管理文件系统中的文
件以及运行在Linux系统上的进程的途径。shell的核心是命令行提示符。命令行提示符是shell负责
交互的部分。它允许你输入文本命令，然后解释命令，并在内核中执行。

默认shell是bash shell。（创建者bourne），称为bourne again shell。  
除了bash shell 还有其他的shell 马哥私房菜 淘宝https://shop592330910.taobao.com/ 
```
ash 一种运行在内存受限环境中简单的轻量级shell，但与bash shell完全兼容  
korn 一种与Bourne shell兼容的编程shell，但支持如关联数组和浮点运算等一些高级的编程特性  
tcsh 一种将C语言中的一些元素引入到shell脚本中的shell  
zsh 一种结合了bash、tcsh和korn的特性，同时提供高级编程特性、共享历史文件和主题化提示符的高级shell  
```

linux的桌面环境： X Window系统
```
KDE（K Desktop Environment，K桌面环境）  
GNOME桌面  
Unity桌面  是Ubuntu Linux发行版
Xfce      和KDE很像的一个桌面，但少了很多图像以适应低内存环境  
```

##  Linux 发行版

```
Slackware 最早的Linux发行版中的一员，在Linux极客中比较流行  
Red Hat 主要用于Internet服务器的商业发行版  
Fedora 从Red Hat分离出的家用发行版  
Gentoo 为高级Linux用户设计的发行版，仅包含Linux源代码  
openSUSE 用于商用和家用的发行版  
Debian 在Linux专家和商用Linux产品中流行的发行版  
```

```
CentOS 一款基于Red Hat企业版Linux源代码构建的免费发行版  
Ubuntu 一款用于学校和家庭的免费发行版  
PCLinuxOS 一款用于家庭和办公的免费发行版  
Mint 一款用于家庭娱乐的免费发行版   
dyne:bolic 一款用于音频和MIDI应用的免费发行版   
Puppy Linux 一款适用于老旧PC的小型免费发行版马哥私房菜 淘宝https://shop592330910.taobao.com/  
```


# 第2 章 走进shell

本章内容
- 访问命令行
- 通过Linux控制台终端访问CLI
- 通过图形化终端仿真器访问CLI
- 使用GNOME终端仿真器
- 使用Konsole终端仿真器
- 使用xterm终端仿真器

## 控制台终端
在图形化桌面出现之前，与Unix系统进行交互的唯一方式就是借助由shell所提供的文本命令
行界面（command line interface，CLI）。CLI只能接受文本输入，也只能显示出文本和基本的图形
输出。

按下Ctrl+Alt组合键，然后按功能键（F1~F7）进入要使用的虚拟控制台
tty代表电传打字机（teletypewriter）。这是一个古老的名词，指的是一台用于发送消息的机器。

## 图形化终端
图形化终端，除了虚拟化终端控制台，还可以使用Linux图形化桌面环境中的终端仿真包。
常见的图像模拟终端：

GNOME Terminal   
GNOME Terminal是GNOME桌面环境的默认终端仿真器。Ubuntu Unity，也采用。
常见的快捷键。 马哥私房菜 淘宝https://shop592330910.taobao.com/ 
  
  
Guake https://github.com/Guake/guake  
Konsole Terminal KDE桌     
Terminator https://launchpad.net/terminator  
tilda http://tilda.sourceforge.net/tildaabout.php  
xterm 最古老也是最基础的终端仿真软件包是xterm。

## 通过Linux 控制台终端访问CLI

## 通过图形化终端仿真访问CLI

## 使用GNOME Terminal 仿真器
GNOME Terminal是GNOME桌面环境的默认终端仿真器。很多发行版，如RHEL、Fedora和
CentOS，默认采用的都是GNOME桌面环境，因此GNOME Terminal自然也就是默认配备了。不
过其他一些桌面环境，比如Ubuntu Unity，也采用GNOME Terminal作为默认的终端仿真软件包。
它使用起来非常简单，是Linux新手的不错选择.马哥私房菜 淘宝https://shop592330910.taobao.com/


## 使用Konsole Terminal 仿真器
Konsole Terminal是KDE桌面环境的默认终端仿真器

## 使用xterm 终端仿真器

最古老也是最基础的终端仿真软件包是xterm。xterm软件包在X Window出现之前就有了，通常默认包含在发行版中。


# 第3 章 基本的bash shell 命令
本章内容
- 使用shell
- bash手册
- 浏览文件系统
- 文件和目录列表
- 管理文件和目录
- 查看文件内容

启动shell

shell 提示符

在Ubuntu Linux系统上，shell提示符看起来是这样的：
```bash
christine@server01:~$
```

在CentOS系统上是这样的：
```bash
[christine@server01 ~]$

```
bash 手册  man命令用来访问存储在Linux系统上的手册页面
```
Name              显示命令名和一段简短的描述    马哥私房菜 淘宝https://shop592330910.taobao.com/
Synopsis          命令的语法        
Confi guration    命令配置信息  
Description       命令的一般性描述  
Options           命令选项描述  
Exit Status       命令的退出状态指示  
Return Value      命令的返回值  
Errors            命令的错误消息  
Environment       描述所使用的环境变量  
Files             命令用到的文件  
Versions          命令的版本信息  
Conforming To     命名所遵从的标准  
Notes             其他有帮助的资料  
Bugs              提供提交bug的途径  
Example           展示命令的用法  
Authors           命令开发人员的信息  
Copyright         命令源代码的版权状况  
See Also          与该命令类型的其他命令  马哥私房菜 淘宝https://shop592330910.taobao.com/
```

如果不记得命令名怎么办？可以使用关键字搜索手册页。语法是：man -k 关键字。例如，要查找与终端相关的命令，可以输入man -k terminal。

除了对节按照惯例进行命名，手册页还有对应的内容区域。每个内容区域都分配了一个数字，从1开始，一直到9。
```
1 可执行程序或shell命令      马哥私房菜 淘宝https://shop592330910.taobao.com/
2 系统调用  
3 库调用  
4 特殊文件  
5 文件格式与约定  
6 游戏  
7 概览、约定及杂项  
8 超级用户和系统管理员命令  
9 内核例程               马哥私房菜 淘宝https://shop592330910.taobao.com/
```

## Linux 文件系统
s
你将会发现Linux使用正斜线（/）而不是反斜线（\）在文件路径中划分目录。在Linux中，反斜线用来标识转义字符，要是用在文件路径中的话会导致各种各样的问题

挂载点（mount point）

```
/ 虚拟目录的根目录。通常不会在这里存储文件  
/bin 二进制目录，存放许多用户级的GNU工具  
/boot 启动目录，存放启动文件  
/dev 设备目录，Linux在这里创建设备节点  
/etc 系统配置文件目录  
/home 主目录，Linux在这里创建用户目录  
/lib 库目录，存放系统和应用程序的库文件  
/media 媒体目录，可移动媒体设备的常用挂载点  
/mnt 挂载目录，另一个可移动媒体设备的常用挂载点  
/opt 可选目录，常用于存放第三方软件包和数据文件  
/proc 进程目录，存放现有硬件及当前进程的相关信息  
/root root用户的主目录  
/sbin 系统二进制目录，存放许多GNU管理员级工具  
/run 运行目录，存放系统运作时的运行时数据  
/srv 服务目录，存放本地服务的相关文件  
/sys 系统目录，存放系统硬件信息的相关文件  
/tmp 临时目录，可以在该目录中创建和删除临时工作文件  
/usr 用户二进制目录，大量用户级的GNU工具和数据文件都存储在这里  
/var 可变目录，用以存放经常变化的文件，比如日志文件      马哥私房菜 淘宝https://shop592330910.taobao.com/
```

常见的目录名均基于文件系统层级标准（filesystem hierarchy standard，FHS）。很多Linux发行版都遵循了FHS。

## 遍历目录

cd命令

绝对文件路径。绝对文件路径定义了在虚拟目录结构中该目录的确切位置，以虚拟目录的根目录开始，相当于目录的全名。

相对文件路径。相对文件路径允许用户指定一个基于当前位置的目标文件路径。相对文件路径不以代表根目  
录的正斜线（/）开头，而是以目录名（如果用户准备切换到当前工作目录下的一个目录）或是一个特殊字符开始。  

单点符（.），表示当前目录  
双点符（..），表示当前目录的父目录  

## 基本列表功能

ls命令  
-l参数会产生长列表格式的输出  
-F参数在目录名后加了正斜线（/）  
-a 以点号开头的隐藏文件现在都显示出来  
-R 了当前目录下包含的子目录中的文件  
选项并一定要像例子中那样分开输入：ls –F –R。它们可以进行如下合并：ls –FR。

## 显示长列表

```
  文件类型，比如目录（d）、文件（-）、字符型文件（c）或块设备（b）  
  文件的权限（参见第6章）  
  文件的硬链接总数  
  文件属主的用户名  
  文件属组的组名  
  文件的大小（以字节为单位）  
  文件的上次修改时间  
  文件名或目录名           马哥私房菜 淘宝https://shop592330910.taobao.com/
```

## 过滤输出列表

问号（?）代表一个字符。  
星号（*）代表零个或多个字符。 

在过滤器中使用星号和问号被称为文件扩展匹配（file globbing），指的是使用通配符进行模
式匹配的过程。通配符正式的名称叫作元字符通配符（metacharacter wildcards）


 
```bash
ls -l my_script

ls -l my_scr?pt

ls -l my*

ls -l my_s*t

ls -l my_scr[ai]pt   # 可能出现的2中字符，a，i。

ls -l f[a-i]ll    # 表示范围内的

ls -l f[!a]ll     #感叹号（!）将不需要的内容排除在外 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

## 创建文件
touch命令  
创建了你指定的新文件，并将你的用户名作为文件的属主。注意，文件的大小是零，因为touch命令只创建了一个空文件

## 复制文件
cp命令
```bash
cp source destination

```
-r或-R 选项表明递归操作  
-l 创建文件的硬链接而不是复制这个文件  
-s 创建文件的符号链接而不是复制这个文件
-L 跟随符号连接文件  
-P 不跟随符号连接文件  
 


## 制表键自动补全


## 链接文件  

符号链接 符号链接就是一个实实在在的文件，它指向存放在虚拟目录结构中某个地方的另一个文件. 
```bash
ln -s

``` 
硬链接  
硬链接会创建独立的虚拟文件，其中包含了原始文件的信息及位置。但是它们从根本上而言
是同一个文件。引用硬链接文件等同于引用了源文件。要创建硬链接，原始文件也必须事先存在.


## 重命名文件

mv命令  
注意，移动文件会将文件名从fall更改为fzll，但inode编号和时间戳保持不变。这是因为mv只影响文件名。

## 删除文件
rm命令  
在Linux中，删除（deleting）叫作移除（removing）


## 创建目录

mkdir命令  

```bash

mkdir New_Dir                       # 马哥私房菜 淘宝https://shop592330910.taobao.com/

mkdir -p New_Dir/Sub_Dir/Under_Dir  # 要想同时创建多个目录和子目录，需要加入-p参数：

```


## 删除目录
rmdir 命令  

rmdir只能删除空目录

## 查看文件类型

file命令  
决定文件是什么类型


## 查看整个文件

cat命令  
显示文本文件中所有数据  
-n参数会给所有的行加上行号  
-b参数 如果只想给有文本的行加上行号  
-T参数会用^I字符组合去替换文中的所有制表符  



more命令  
cat命令的主要缺陷是：一旦运行，你就无法控制后面的操作.  
more命令会显示文本文件的内容，但会在显示每页数据之后停下来,more命令是分页工具。  
按空格键或回车键以逐行向前的方式浏览文本文件。按q键退出。  
```
* 空格代表向下翻页  
* 回车带哦不下翻一行
* /字符串  代表在这个显示的内容中向下搜索字符串
*  ：f 立即显示出文件明以及当前行数。
*  q退出,离开
*  b或者ctrl + b  代表往前翻页，只对文件有效。对管道无用
```
less命令  
less和more类似 。但是比more更强大。支持往前查看文件内容 。  
```
* 空格代表向下翻一页
* pagedown向下翻一页
* pageup向上翻一页
* /字符串  代表向下搜索字符串
* ？字符串  代表向上搜索字符串
* n 代表重复前一个搜寻
* N反向重复前一个搜寻
* g到第一行
* G到最后一行
* q退出,离开
```

## 查看部分文件

tail命令  
tail命令会显示文件最后几行的内容（文件的“尾部”）。默认情况下，它会显示文件的末尾10行。  
-f参数允许你在其他进程使用该文件时查看文件的内容。
tail命令会保持活动状态，并不断显示添加到文件中的内容。这是实时监测系统日志的绝妙方式。     

head命令  
默认情况下，它会显示文件前10行的文本.head命令没有-f选项啊，因为文件开头是不会改变的。
-n参数，这样就可以指定输入想要显示的行数..  

# 第4 章 更多的bash shell 命令


本章内容
*   管理进程
*   获取磁盘统计信息
*   挂载新磁盘
*   排序数据
*   归档数据

## 探查进程

ps命令  
当程序运行在系统上时，我们称之为进程（process）。  
默认情况下，ps命令只会显示运行在当前控制台下的属于当前用户的进程。  
进程ID（Process ID，PID）  

Linux系统中使用的GNU ps命令支持3种不同类型的命令行参数：  
```
  Unix风格的参数，前面加单破折线；  
  BSD风格的参数，前面不加破折线；  
  GNU风格的长参数，前面加双破折线。  马哥私房菜 淘宝https://shop592330910.taobao.com/
```

Unix风格的ps命令参数：  
```
-A              显示所有进程  
-N              显示与指定参数不符的所有进程  
-a              显示除控制进程（session leader①）和无终端进程外的所有进程  
-d              显示除控制进程外的所有进程  
-e              显示所有进程，显示所有运行在系统上的进程。   
-C              cmdlist 显示包含在cmdlist列表中的进程  
-G              grplist 显示组ID在grplist列表中的进程  
-U              userlist 显示属主的用户ID在userlist列表中的进程  
-g              grplist 显示会话或组ID在grplist列表中的进程②  
-p              pidlist 显示PID在pidlist列表中的进程  
-s              sesslist 显示会话ID在sesslist列表中的进程  
-t              ttylist 显示终端ID在ttylist列表中的进程  
-u              userlist 显示有效用户ID在userlist列表中的进程  
-F              显示更多额外输出（相对-f参数而言）  
-O              format 显示默认的输出列以及format列表指定的特定列  
-M              显示进程的安全信息  
-c              显示进程的额外调度器信息  
-f              显示完整格式的输出，扩展了输出。  
-j              显示任务信息  
-l              显示长列表  
-o              format 仅显示由format指定的列  
-y              不要显示进程标记（process flag，表明进程状态的标记）  
-Z              显示安全标签（security context）①信息  
-H              用层级格式来显示进程（树状，用来显示父进程）  
-n              namelist 定义了WCHAN列显示的值  
-w              采用宽输出模式，不限宽度显示  
-L              显示进程中的线程  
-V              显示ps命令的版本号       马哥私房菜 淘宝https://shop592330910.taobao.com/
```

-f参数的扩展的列包含了有用的信息。  
```
  UID：         启动这些进程的用户。  
  PID：         进程的进程ID。  
  PPID：        父进程的进程号（如果该进程是由另一个进程启动的）。  
  C：           进程生命周期中的CPU利用率。  
  STIME：       进程启动时的系统时间。  
  TTY：         进程启动时的终端设备。  
  TIME：        运行进程需要的累计CPU时间。  
  CMD：         启动的程序名称。  
```

使用了-l参数之后多出的那些列。  
```
  F：          内核分配给进程的系统标记。  
  S：          进程的状态（O代表正在运行；S代表在休眠；R代表可运行，正等待运行；Z代表僵  化，进程已结束但父进程已不存在；T代表停止）。  
  PRI：        进程的优先级（越大的数字代表越低的优先级）。  
  NI：         谦让度值用来参与决定优先级。  
  ADDR：       进程的内存地址。  
  SZ：         假如进程被换出，所需交换空间的大致大小。  
  WCHAN：      进程休眠的内核函数的地址。 马哥私房菜 淘宝https://shop592330910.taobao.com/ 
```

BSD风格的参数：  
```
T               显示跟当前终端关联的所有进程  
a               显示跟任意终端关联的所有进程  
g               显示所有的进程，包括控制进程  
r               仅显示运行中的进程  
x               显示所有的进程，甚至包括未分配任何终端的进程
U               userlist 显示归userlist列表中某用户ID所有的进程
p               pidlist 显示PID在pidlist列表中的进程
t               ttylist 显示所关联的终端在ttylist列表中的进程
O               format 除了默认输出的列之外，还输出由format指定的列
X               按过去的Linux i386寄存器格式显示
Z               将安全信息添加到输出中
j               显示任务信息
l               采用长模式
o               format 仅显示由format指定的列
s               采用信号格式显示
u               采用基于用户的格式显示
v               采用虚拟内存格式显示
N               namelist 定义在WCHAN列中使用的值
O               order 定义显示信息列的顺序
S               将数值信息从子进程加到父进程上，比如CPU和内存的使用情况
c               显示真实的命令名称（用以启动进程的程序名称）
e               显示命令使用的环境变量
f               用分层格式来显示进程，表明哪些进程启动了哪些进程
h               不显示头信息
k               sort 指定用以将输出排序的列
n               和WCHAN信息一起显示出来，用数值来表示用户ID和组ID
w               为较宽屏幕显示宽输出
H               将线程按进程来显示
m               在进程后显示线程
L               列出所有格式指定符
V               显示ps命令的版本号       马哥私房菜 淘宝https://shop592330910.taobao.com/
```
大部分的输出列跟使用Unix风格参数时的输出是一样的，只有一小部分不同。  
```
  VSZ：        进程在内存中的大小，以千字节（KB）为单位。  
  RSS：        进程在未换出时占用的物理内存。  
  STAT：       代表当前进程状态的双字符状态码。  
```
BSD风格的l参数。它能输出更详细的进程状态码（STAT列）。双字符状态码能比Unix风格输出的单字符状态码更清楚地表示进程的当前状态。  
第一个字符采用了和Unix风格S列相同的值，表明进程是在休眠、运行还是等待。第二个参数进一步说明进程的状态。  
```
  <：该进程运行在高优先级上 。  
  N：该进程运行在低优先级上。  
  L：该进程有页面锁定在内存中。  
  s：该进程是控制进程。  
  l：该进程是多线程的。  
  +：该进程运行在前台 。   马哥私房菜 淘宝https://shop592330910.taobao.com/
```

GNU长参数风格：  
```
--deselect           显示所有进程，命令行中列出的进程  
--Group grplist      显示组ID在grplist列表中的进程  
--User userlist      显示用户ID在userlist列表中的进程  
--group grplist      显示有效组ID在grplist列表中的进程  
--pid pidlist        显示PID在pidlist列表中的进程  
--ppid pidlist       显示父PID在pidlist列表中的进程  
--sid sidlist        显示会话ID在sidlist列表中的进程  
--tty ttylist        显示终端设备号在ttylist列表中的进程  
--user userlist      显示有效用户ID在userlist列表中的进程  
--format format      仅显示由format指定的列  
--context            显示额外的安全信息  
--cols n             将屏幕宽度设置为n列  
--columns n          将屏幕宽度设置为n列  
--cumulative         包含已停止的子进程的信息  
--forest             用层级结构显示出进程和父进程之间的关系  
--headers            在每页输出中都显示列的头  
--no-headers         不显示列的头  
--lines n            将屏幕高度设为n行  
--rows n             将屏幕高度设为n排  
--sort order         指定将输出按哪列排序  
--width n            将屏幕宽度设为n列  
--help               显示帮助信息  
--info               显示调试信息  
--version            显示ps命令的版本号    马哥私房菜 淘宝https://shop592330910.taobao.com/
```


--forest参数。它会显示进程的层级信息，并用ASCII字符绘出可爱的图表 。  


## 实时监测进程
top命令  
跟ps命令相似，能够显示进程信息，但它是实时显示的 。  
第一行显示了当前时间、系统的运行时间、登录的用户数以及系统的平均负载。  
```
平均负载有3个值：最近1分钟的、最近5分钟的和最近15分钟的平均负载。值越大说明系统
的负载越高。由于进程短期的突发性活动，出现最近1分钟的高负载值也很常见，但如果近15分
钟内的平均负载都很高，就说明系统可能有问题。

通常，如果系统的负载值超过了2，就说明系统比较繁忙了。马哥私房菜 淘宝https://shop592330910.taobao.com/
```
第二行显示了进程概要信息——top命令的输出中将进程叫作任务（task）：有多少进程处在运行、休眠、停止或是僵化状态（僵化状态是指进程完成了，但父进程没有响应）。  
第仨行显示了CPU的概要信息。top根据进程的属主（用户还是系统）和进程的状态（运行、空闲还是等待）将CPU利用率分成几类输出。  
第四行说的是系统的物理内存：总共有多少内存，当前用了多少，还有多少空闲。  
第五行后一行说的是同样的信息，不过是针对系统交换空间（如果分配了的话）的状态而言的。  
最后一部分显示了当前运行中的进程的详细列表，有些列跟ps命令的输出类似。   
```
  PID：         进程的ID。                   马哥私房菜 淘宝https://shop592330910.taobao.com/
  USER：        进程属主的名字。
  PR：          进程的优先级。
  NI：          进程的谦让度值。
  VIRT：        进程占用的虚拟内存总量。
  RES：         进程占用的物理内存总量。
  SHR：         进程和其他进程共享的内存总量。
  S：           进程的状态（D代表可中断的休眠状态，R代表在运行状态，S代表休眠状态，T代表跟踪状态或停止状态，Z代表僵化状态）。
  %CPU：        进程使用的CPU时间比例。
  %MEM：        进程使用的内存占可用内存的比例。
  TIME+：       自进程启动到目前为止的CPU时间总量。
  COMMAND：     进程所对应的命令行名称，也就是启动的程序名。 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
键入f允许你选择对输出进行排序的字段  
键入d允许你修改轮询间隔  
键入q可以退出top  

## 结束进程

在Linux中，进程之间通过信号来通信。进程的信号就是预定义好的一个消息，进程能识别它并决定忽略还是作出反应。   
```
1       HUP             挂起
2       INT             中断
3       QUIT            结束运行
9       KILL            无条件终止
11      SEGV            段错误
15      TERM            尽可能终止
17      STOP            无条件停止运行，但不终止
18      TSTP            停止或暂停，但继续在后台运行
19      CONT            在STOP或TSTP之后恢复执行    马哥私房菜 淘宝https://shop592330910.taobao.com/
```

kill命令  
kill命令可通过进程ID（PID）给进程发信号。默认情况下，kill命令会向命令行中列出的  
全部PID发送一个TERM信号。遗憾的是，你只能用进程的PID而不能用命令名  

killall命令  
killall命令非常强大，它支持通过进程名而不是PID来结束进程。killall命令也支持通配符，这在系统因负载过大而变得很慢时很有用。  
```
# killall http*         # 马哥私房菜 淘宝https://shop592330910.taobao.com/
#
```
上例中的命令结束了所有以http开头的进程，比如Apache Web服务器的httpd服务。   


## 挂载存储媒体

mount命令  
默认情况下，mount命令会输出当前系统上挂载的 设备列表。  
```
$ mount      
/dev/sda2       on      / type ext4 (rw,errors=remount-ro)
proc            on      /proc type proc (rw,noexec,nosuid,nodev)
sysfs           on      /sys type sysfs (rw,noexec,nosuid,nodev)
none            on      /sys/fs/cgroup type tmpfs (rw)
none            on      /sys/fs/fuse/connections type fusectl (rw)
none            on      /sys/kernel/debug type debugfs (rw)
none            on      /sys/kernel/security type securityfs (rw)
udev            on      /dev type devtmpfs (rw,mode=0755)
devpts          on      /dev/pts type devpts (rw,noexec,nosuid,gid=5,mode=0620)
tmpfs           on      /run type tmpfs (rw,noexec,nosuid,size=10%,mode=0755)
none            on      /run/lock type tmpfs (rw,noexec,nosuid,nodev,size=5242880)
none            on      /run/shm type tmpfs (rw,nosuid,nodev)
none            on      /run/user type tmpfs (rw,noexec,nosuid,nodev,size=104857600,mode=0755)
none            on      /sys/fs/pstore type pstore (rw)
/dev/sda3       on      /home type ext4 (rw)
/dev/sdb1       on      /home/mamh/share type ext4 (rw)
binfmt_misc     on      /proc/sys/fs/binfmt_misc type binfmt_misc (rw,noexec,nosuid,nodev)
rpc_pipefs      on      /run/rpc_pipefs type rpc_pipefs (rw)
systemd         on      /sys/fs/cgroup/systemd type cgroup (rw,noexec,nosuid,nodev,none,name=systemd)
gvfsd-fuse      on      /run/user/1000/gvfs type fuse.gvfsd-fuse (rw,nosuid,nodev,user=mamh)
```
mount命令提供如下四部分信息：
```
  媒体的设备文件名
  媒体挂载到虚拟目录的挂载点
  文件系统类型
  已挂载媒体的访问状态      马哥私房菜 淘宝https://shop592330910.taobao.com/
```

手动挂载媒体设备的基本命令   
```
mount -t type device directory
```
type参数指定了磁盘被格式化的文件系统类型  

手动将U盘/dev/sdb1挂载到/media/disk
```
mount -t vfat /dev/sdb1 /media/disk
```

mount命令的参数
```
-a      挂载/etc/fstab文件中指定的所有文件系统      马哥私房菜 淘宝https://shop592330910.taobao.com/
-f      使mount命令模拟挂载设备，但并不真的挂载
-F      和-a参数一起使用时，会同时挂载所有文件系统
-v      详细模式，将会说明挂载设备的每一步
-I      不启用任何/sbin/mount.filesystem下的文件系统帮助文件
-l      给ext2、ext3或XFS文件系统自动添加文件系统标签
-n      挂载设备，但不注册到/etc/mtab已挂载设备文件中
-p      num 进行加密挂载时，从文件描述符num中获得密码短语
-s      忽略该文件系统不支持的挂载选项
-r      将设备挂载为只读的
-w      将设备挂载为可读写的（默认参数）
-L      label 将设备按指定的label挂载
-U      uuid 将设备按指定的uuid挂载
-O      和-a参数一起使用，限制命令只作用到特定的一组文件系统上
-o      给文件系统添加特定的选项                 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
-o参数允许在挂载文件系统时添加一些以逗号分隔的额外选项。以下为常用的选项。
```
  ro：以只读形式挂载。
  rw：以读写形式挂载。
  user：允许普通用户挂载文件系统。
  check=none：挂载文件系统时不进行完整性校验。
  loop：挂载一个文件。                  马哥私房菜 淘宝https://shop592330910.taobao.com/
```

mount命令  
从Linux系统上移除一个可移动设备时，不能直接从系统上移除，而应该先卸载。  
umount命令的格式非常简单：umount [directory | device ]   
umount命令支持通过设备文件或者是挂载点来指定要卸载的设备。如果有任何程序正在使用设备上的文件，系统就不会允许你卸载它。  


## 使用df 命令

查看所有已挂载磁盘的使用情况  
```
   设备的设备文件位置；
 能容纳多少个1024字节大小的块；
 已用了多少个1024字节大小的块；
 还有多少个1024字节大小的块可用；
 已用空间所占的比例；
 设备挂载到了哪个挂载点上。
```
-h。它会把输出中的磁盘空间按照用户易读的形式显示，通常用M来替代兆字节，用G替代吉字节  



## 使用du 命令
显示某个特定目录（默认情况下是当前目录）的磁盘使用情况  
-c：显示所有已列出文件总的大小。  
-h：按用户易读的格式输出大小，即用K替代千字节，用M替代兆字节，用G替代吉字节。  
-s：显示每个输出参数的总计。  


## 排序数据
sort命令  
```
-b --ignore-leading-blanks 排序时忽略起始的空白
-C --check=quiet 不排序，如果数据无序也不要报告
-c --check 不排序，但检查输入数据是不是已排序；未排序的话，报告
-d --dictionary-order 仅考虑空白和字母，不考虑特殊字符
-f --ignore-case 默认情况下，会将大写字母排在前面；这个参数会忽略大小写
-g --general-number-sort 按通用数值来排序（跟-n不同，把值当浮点数来排序，支持科学计数法表示的值）
-i --ignore-nonprinting 在排序时忽略不可打印字符
-k --key=POS1[,POS2] 排序从POS1位置开始；如果指定了POS2的话，到POS2位置结束
-M --month-sort 用三字符月份名按月份排序
-m --merge 将两个已排序数据文件合并
-n --numeric-sort 按字符串数值来排序（并不转换为浮点数）
-o --output=file 将排序结果写出到指定的文件中
-R --random-sort 按随机生成的散列表的键值排序
--random-source=FILE 指定-R参数用到的随机字节的源文件
-r --reverse 反序排序（升序变成降序）
-S --buffer-size=SIZE 指定使用的内存大小
-s --stable 禁用最后重排序比较
-T --temporary-directory=DIR 指定一个位置来存储临时工作文件
-t --field-separator=SEP 指定一个用来区分键位置的字符
-u --unique 和-c参数一起使用时，检查严格排序；不和-c参数一起用时，仅输出第一例相似的两行
-z --zero-terminated 用NULL字符作为行尾，而不是用换行符

```
-k和-t参数在对按字段分隔的数据进行排序时非常有用，例如/etc/passwd文件。可以用-t参数来指定字段分隔符，然后用-k参数来指定排序的字段 .  
```
sort -t ':' -k 3 -n /etc/passwd  # 根据用户ID进行数值排序


du -sh * | sort -nr # -r参数将结果按降序输出，这样就更容易看到目录下的哪些文件占用空间最多

```

## 搜索数据
grep命令  
```
grep three file1

grep t file1

grep -v t file1

grep -n t file1

grep -c t file1

grep -e t -e f file1

grep [tf] file1


```
反向搜索（输出不匹配该模式的行），可加-v参数  
显示匹配模式的行所在的行号，可加-n参数  
只要知道有多少行含有匹配的模式，可用-c参数.  
指定多个匹配模式，可用-e参数  

## 压缩数据
```
bzip2       .bz2    采用Burrows-Wheeler块排序文本压缩算法和霍夫曼编码
compress    .Z      最初的Unix文件压缩工具，已经快没人用了
gzip        .gz     GNU压缩工具，用Lempel-Ziv编码
zip         .zip    Windows上PKZIP工具的Unix实现
```
compress 和 uncompress命令来  

gzip：用来压缩文件  
gzcat：用来查看压缩过的文本文件的内容  
gunzip：用来解压文件  
```
gzip myprog  # gzip命令会压缩你在命令行指定的文件

gzip my*   # 在命令行指定多个文件名甚至用通配符来一次性批量压缩文件。
```

## 归档数据
tar命令  
```
-A --concatenate 将一个已有tar归档文件追加到另一个已有tar归档文件
-c --create 创建一个新的tar归档文件
-d --diff 检查归档文件和文件系统的不同之处
--delete 从已有tar归档文件中删除
-r --append 追加文件到已有tar归档文件末尾
-t --list 列出已有tar归档文件的内容
-u --update 将比tar归档文件中已有的同名文件新的文件追加到该tar归档文件中
-x --extract 从已有tar归档文件中提取文件
```

```
-C dir 切换到指定目录
-f file 输出结果到文件或设备file
-j 将输出重定向给bzip2命令来压缩内容
-p 保留所有文件权限
-v 在处理文件时显示文件
-z 将输出重定向给gzip命令来压缩内容
```

```
tar -cvf test.tar test/ test2/  # 创建了名为test.tar的归档文件，含有test和test2目录内容

tar -tf test.tar  # 列出tar文件test.tar的内容（但并不提取文件

tar -xvf test.tar  # 从tar文件test.tar中提取内容。如果tar文件是从一个目录结构创建的，那整个目录结构都会在当前目录下重新创建。



```



# 第5 章 理解shell
本章内容  
 探究shell的类型  
 理解shell的父/子关系  
 别出心裁的子shell用法  
 探究内建的shell命令  


## shell 的类型
/bin/bash  
/bin/sh  
/bin/dash--





## 进程列表

```

(pwd ; ls ; cd /etc ; pwd ; cd ; pwd ; ls)

```

```
{ pwd ; ls ; cd /etc ; pwd ; cd ; pwd ; ls ; echo $BASH_SUBSHELL; }
```
小括号进程列表会在一个子shell中执行。花括号的不会。

```
sleep 3000&
```
使用 & 把一个命令放到后台执行。通过jobs可以列出后台进程。  
fg和bg的使用  


## 内建命令/外部命令

```
which ps

type -a ps


```

命令历史 history  
使用 ！！ 快速执行上次的命令
使用 ！数字  快速执行history历史中对应标号的那个命令  



alias命令

```
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l='ls -CF'
alias la='ls -A'


```


# 第6 章 使用Linux 环境变量
本章内容  
 什么是环境变量  
 创建自己的局部变量  
 删除环境变量  
 默认shell环境变量  
 设置PATH环境变量  
 定位环境文件  
 数组变量  


## 什么是环境变量

bash shell用一个叫作环境变量（environment variable）的特性来存储有关shell会话和工作环  
境的信息（这也是它们被称作环境变量的原因）。这项特性允许你在内存中存储数据，以便程序  
或shell中运行的脚本能够轻松访问到它们。  

在bash shell中，环境变量分为两类：全局变量, 局部变量  

可以使用env或printenv命令
```
$ printenv
HOSTNAME=server01.class.edu
SELINUX_ROLE_REQUESTED=
TERM=xterm
SHELL=/bin/bash
HISTSIZE=1000
[...]
HOME=/home/Christine
LOGNAME=Christine
[...]
G_BROKEN_FILENAMES=1
_=/usr/bin/printenv
```


可以用printenv打印单独的某一个，env就不行。  
```
$ printenv HOME
/home/Christine
$
$ env HOME
env: HOME: No such file or directory
$

```
也可以使用echo显示变量的,在这种情况下引用某个环境变量的时候，必须在变量前面加上一个美元符（$）。  
```
$ echo $HOME
/home/Christine
$

```

局部环境变量只能在定义它们的进程中可见。尽管它们是局部的，但是和全局环境变量一样重要.  

set命令会显示为某个特定进程设置的所有环境变量，包括局部变量、全局变量以及用户定义变量.  

命令env、printenv和set之间的差异很细微。  
set命令会显示出全局变量、局部变量以  及用户定义变量。  
它还会按照字母顺序对结果进行排序。env和printenv命令同set命  
令的区别在于前两个命令不会对变量排序，也不会输出局部变量和用户定义变量。在这  
种情况下，env和printenv的输出是重复的。不过env命令有一个printenv没有的功能，  
这使得它要更有用一些。  










 第7 章 理解Linux 文件权限
# 第8 章 管理文件系统
# 第9 章 安装软件程序
# 第10 章 使用编辑器


第2部分 Part 2  shell 脚本编程基础
-------------

# 第11 章 构建基本脚本
# 第12 章 使用结构化命令
# 第13 章 更多的结构化命令
# 第14 章 处理用户输入
# 第15 章 呈现数据
# 第16 章 控制脚本
 

第3部分 Part 3    高级shell 脚本编程
-------------


# 第17 章 创建函数
# 第18 章 图形化桌面环境中的脚本编程
# 第19 章 初识sed 和gawk
# 第20 章 正则表达式
# 第21 章 sed 进阶
# 第22 章 gawk 进阶
# 第23 章 使用其他shell


第4部分 Part 4 创建实用的脚本
-------------


# 第24 章 编写简单的脚本实用工具
# 第25 章 创建与数据库、Web 及电子邮件相关的脚本
# 第26 章 一些小有意思的脚本












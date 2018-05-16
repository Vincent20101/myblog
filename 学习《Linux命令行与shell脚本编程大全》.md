

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
 什么是环境变量  
 创建自己的局部变量  
 删除环境变量  
 默认shell环境变量  
 设置PATH环境变量  
 定位环境文件  
 数组变量  


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

局部环境变量只能在定义它们的进程中可见.  

set命令会显示为某个特定进程设置的所有环境变量，包括局部变量、全局变量以及用户定义变量.  

命令env、printenv和set之间的差异很细微。  
set命令会显示出全局变量、局部变量以及用户定义变量。  它还会按照字母顺序对结果进行排序。  
env和printenv命令同set命令的区别在于前两个命令不会对变量排序，也不会输出局部变量和用户定义变量。在这种情况下，env和printenv的输出是重复的。

不过env命令有一个printenv没有的功能，这使得它要更有用一些。

env - run a program in a modified environment  
env可以用来设置某个环境变量来给某个命令

printenv - print all or part of environment  
这个仅仅是打印环境变量  


## 设置用户定义变量


### 设置局部变量

一旦启动了bash shell（或者执行一个shell脚本），就能创建在这个shell进程内可见的局部变
量了。可以通过等号给环境变量赋值，值可以是数值或字符串

```bash

$ echo $my_variable

$ my_variable=Hello    # 记住，变量名、等号和值之间没有空格，这一点非常重要
$
$ echo $my_variable
Hello
 

```
现在每次引用my_variable 环境变量的值，只要通过$my_variable引用即可。

如果要给变量赋一个含有空格的字符串值，必须用单引号来界定字符串的首和尾。  
没有单引号的话，bash shell会以为下一个词是另一个要执行的命令
```bash

$ my_variable=Hello World
-bash: World: command not found
$
$ my_variable="Hello World"
$
$ echo $my_variable
Hello World
$

```

所有的环境变量名均使用大写字母，这是bash shell的标准惯例。
如果是你自己创建的局部变量或是shell脚本，请使用小写字母。
变量名区分大小写。在涉及用户定义的局部变量时坚持使用小写字母，这能够避免重新定义系统环境变量可能带来的灾难。

记住，变量名、等号和值之间没有空格，这一点非常重要
```bash

$ my_variable = "Hello World"
-bash: my_variable: command not found
$

```

设置了局部环境变量后，就能在shell进程的任何地方使用它了。但是，如果生成了另外一个shell，它在子shell中就不可用。 后续我们介绍export让其在子shell中可用。
```bash
$ my_variable="Hello World"
$
$ bash
$
$ echo $my_variable
$ exit
exit
$
$ echo $my_variable
Hello World
$
```
在这个例子中生成了一个子shell。在子shell中无法使用用户定义变量my_variable。通过命
令echo $my_variable所返回的空行就能够证明这一点。当你退出子shell并回到原来的shell时，
这个局部环境变量依然可用。

类似地，如果你在子进程中设置了一个局部变量，那么一旦你退出了子进程，那个局部环境
变量就不可用。
```bash

$ echo $my_child_variable    # 这个是在父shell中执行的
$ bash                       # 进入子shell
$
$ my_child_variable="Hello Little World"  # 在子shell中设置变量值
$
$ echo $my_child_variable    # 子shell中可以打印变量值
Hello Little World
$
$ exit
exit
$
$ echo $my_child_variable   # 退出子shell后，这个变量就消失了
$

```
当我们回到父shell时，子shell中设置的局部变量就不存在了。

### exprot 设置全局环境变量

创建全局环境变量的方法是先创建一个局部环境变量，然后再把它导出到全局环境中。这个过程通过export命令来完成，变量名前面不需要加$。
```bash
$ my_variable="I am Global now"
$
$ export my_variable
$
$ echo $my_variable
I am Global now
$
$ bash
$
$ echo $my_variable
I am Global now
$
$ exit
exit
$
$ echo $my_variable
I am Global now
$


```

修改子shell中全局环境变量并不会影响到父shell中该变量的值。
```bash

$ my_variable="I am Global now"
$ export my_variable
$
$ echo $my_variable
I am Global now
$
$ bash
$
$ echo $my_variable
I am Global now
$
$ my_variable="Null"
$
$ echo $my_variable
Null
$
$ exit
exit
$
$ echo $my_variable
I am Global now
$

```
在定义并导出变量my_variable后，bash命令启动了一个子shell。在这个子shell中能够正
确显示出全局环境变量my_variable的值。子shell随后改变了这个变量的值。但是这种改变仅在
子shell中有效，并不会被反映到父shell中。


子shell甚至无法使用export命令改变父shell中全局环境变量的值。
```bash

$ my_variable="I am Global now"
$ export my_variable
$
$ echo $my_variable
I am Global now
$
$ bash
$
$ echo $my_variable
I am Global now
$
$ my_variable="Null"
$
$ export my_variable
$
$ echo $my_variable
Null
$
$ exit
exit
$
$ echo $my_variable
I am Global now
$

```
尽管子shell重新定义并导出了变量my_variable，但父shell中的my_variable变量依然保
留着原先的值。


### unset 删除环境变量
当然，既然可以创建新的环境变量，自然也能删除已经存在的环境变量。可以用unset命令
完成这个操作。在unset命令中引用环境变量时，记住不要使用$。
```bash

$ echo $my_variable
I am Global now
$
$ unset my_variable
$
$ echo $my_variable
$

```

在涉及环境变量名时，什么时候该使用$，什么时候不该使用$，实在让人摸不着头脑。
记住一点就行了：如果要用到变量，使用$；如果要操作变量，不使用$。这条规则的一
个例外就是使用printenv显示某个变量的值。


在处理全局环境变量时，事情就有点棘手了。如果你是在子进程中删除了一个全局环境变量，
这只对子进程有效。该全局环境变量在父进程中依然可用。
```bash

$ my_variable="I am Global now"
$
$ export my_variable
$
$ echo $my_variable
I am Global now
$
$ bash
$
$ echo $my_variable
I am Global now
$
$ unset my_variable
$
$ echo $my_variable
$ exit
exit
$
$ echo $my_variable
I am Global now
$

```
和修改变量一样，在子shell中删除全局变量后，你无法将效果反映到父shell中。


## 默认的shell 环境变量
默认情况下，bash shell会用一些特定的环境变量来定义系统环境。这些变量在你的Linux系
统上都已经设置好了，只管放心使用。
```bash
# bash shell支持的Bourne变量 ，列出了bash shell提供的与Unix Bourne shell兼容的环境变量

CDPATH      冒号分隔的目录列表，作为cd命令的搜索路径
HOME        当前用户的主目录
IFS         shell用来将文本字符串分割成字段的一系列字符
MAIL        当前用户收件箱的文件名（bash shell会检查这个文件，看看有没有新邮件）
MAILPATH    冒号分隔的当前用户收件箱的文件名列表（bash shell会检查列表中的每个文件，看看有没有新邮件）
OPTARG      getopts命令处理的最后一个选项参数值
OPTIND      getopts命令处理的最后一个选项参数的索引号
PATH        shell查找命令的目录列表，由冒号分隔
PS1         shell命令行界面的主提示符
PS2         shell命令行界面的次提示符


# bash shell环境变量

BASH                当前shell实例的全路径名
BASH_ALIASES        含有当前已设置别名的关联数组
BASH_ARGC           含有传入子函数或shell脚本的参数总数的数组变量
BASH_ARCV           含有传入子函数或shell脚本的参数的数组变量
BASH_CMDS           关联数组，包含shell执行过的命令的所在位置
BASH_COMMAND        shell正在执行的命令或马上就执行的命令
BASH_ENV            设置了的话，每个bash脚本会在运行前先尝试运行该变量定义的启动文件
BASH_EXECUTION_STRING 使用bash -c选项传递过来的命令
BASH_LINENO         含有当前执行的shell函数的源代码行号的数组变量
BASH_REMATCH        只读数组，在使用正则表达式的比较运算符=~进行肯定匹配（positive match）时， 包含了匹配到的模式和子模式
BASH_SOURCE         含有当前正在执行的shell函数所在源文件名的数组变量
BASH_SUBSHELL       当前子shell环境的嵌套级别（初始值是0）
BASH_VERSINFO       含有当前运行的bash shell的主版本号和次版本号的数组变量
BASH_VERSION        当前运行的bash shell的版本号
BASH_XTRACEFD       若设置成了有效的文件描述符（0、1、2），则'set -x'调试选项生成的跟踪输出
可被重定向。通常用来将跟踪输出到一个文件中
BASHOPTS            当前启用的bash shell选项的列表
BASHPID             当前bash进程的PID
COLUMNS             当前bash shell实例所用终端的宽度
COMP_CWORD COMP_WORDS变量的索引值，后者含有当前光标的位置
COMP_LINE           当前命令行
COMP_POINT          当前光标位置相对于当前命令起始的索引
COMP_KEY            用来调用shell函数补全功能的最后一个键
COMP_TYPE           一个整数值，表示所尝试的补全类型，用以完成shell函数补全
COMP_WORDBREAKS Readline库中用于单词补全的词分隔字符
COMP_WORDS          含有当前命令行所有单词的数组变量
COMPREPLY
COPROC
含有由shell函数生成的可能填充代码的数组变量
占用未命名的协进程的I/O文件描述符的数组变量
DIRSTACK            含有目录栈当前内容的数组变量
EMACS               设置为't'时，表明emacs shell缓冲区正在工作，而行编辑功能被禁止
ENV                 如果设置了该环境变量，在bash shell脚本运行之前会先执行已定义的启动文件（仅用于当bash shell以POSIX模式被调用时）
EUID                当前用户的有效用户ID（数字形式）
FCEDIT              供fc命令使用的默认编辑器
FIGNORE             在进行文件名补全时可以忽略后缀名列表，由冒号分隔
FUNCNAME            当前执行的shell函数的名称


FUNCNEST            当设置成非零值时，表示所允许的最大函数嵌套级数（一旦超出，当前命令即被终止）
GLOBIGNORE          冒号分隔的模式列表，定义了在进行文件名扩展时可以忽略的一组文件名
GROUPS              含有当前用户属组列表的数组变量
histchars           控制历史记录扩展，最多可有3个字符
HISTCMD             当前命令在历史记录中的编号
HISTCONTROL         控制哪些命令留在历史记录列表中
HISTFILE            保存shell历史记录列表的文件名（默认是.bash_history）
HISTFILESIZE        最多在历史文件中存多少行
HISTTIMEFORMAT      如果设置了且非空，就用作格式化字符串，以显示bash历史中每条命令的时间戳                   
HISTIGNORE          由冒号分隔的模式列表，用来决定历史文件中哪些命令会被忽略
HISTSIZE            最多在历史文件中存多少条命令
HOSTFILE            shell在补全主机名时读取的文件名称
HOSTNAME            当前主机的名称
HOSTTYPE            当前运行bash shell的机器
IGNOREEOF           shell在退出前必须收到连续的EOF字符的数量（如果这个值不存在，默认是1）
INPUTRC             Readline初始化文件名（默认是.inputrc）
LANG                shell的语言环境类别
LC_ALL              定义了一个语言环境类别，能够覆盖LANG变量
LC_COLLATE          设置对字符串排序时用的排序规则
LC_CTYPE            决定如何解释出现在文件名扩展和模式匹配中的字符
LC_MESSAGES         在解释前面带有$的双引号字符串时，该环境变量决定了所采用的语言环境设置
LC_NUMERIC          决定着格式化数字时采用的语言环境设置
LINENO              当前执行的脚本的行号
LINES               定义了终端上可见的行数
MACHTYPE            用“CPU公司系统”（CPU-company-system）格式定义的系统类型
MAPFILE             一个数组变量，当mapfile命令未指定数组变量作为参数时，它存储了mapfile所读入的文本
MAILCHECK           shell查看新邮件的频率（以秒为单位，默认值是60）
OLDPWD              shell之前的工作目录
OPTERR              设置为1时，bash shell会显示getopts命令产生的错误
OSTYPE              定义了shell所在的操作系统
PIPESTATUS          含有前台进程的退出状态列表的数组变量
POSIXLY_CORRECT     设置了的话，bash会以POSIX模式启动
PPID bash shell     父进程的PID
PROMPT_COMMAND      设置了的话，在命令行主提示符显示之前会执行这条命令
PROMPT_DIRTRIM      用来定义当启用了\w或\W提示符字符串转义时显示的尾部目录名的数量。被删除的目录名会用一组英文句点替换
PS3                 select命令的提示符
PS4                 如果使用了bash的-x选项，在命令行之前显示的提示信息
PWD                 当前工作目录
RANDOM              返回一个0～32767的随机数（对其的赋值可作为随机数生成器的种子）
READLINE_LINE       当使用bind –x命令时，存储Readline缓冲区的内容
READLINE_POINT      当使用bind –x命令时，表示Readline缓冲区内容插入点的当前位置
REPLY               read命令的默认变量
SECONDS             自从shell启动到现在的秒数（对其赋值将会重置计数器）
SHELL               bash shell的全路径名
SHELLOPTS           已启用bash shell选项列表，列表项之间以冒号分隔
SHLVL               shell的层级；每次启动一个新bash shell，该值增加1
TIMEFORMAT          指定了shell的时间显示格式
TMOUT               select和read命令在没输入的情况下等待多久（以秒为单位）。默认值为0，表示无限长
TMPDIR              目录名，保存bash shell创建的临时文件
UID                 当前用户的真实用户ID（数字形式）

```
你可能已经注意到，不是所有的默认环境变量都会在运行set命令时列出。尽管这些都是默
认环境变量，但并不是每一个都必须有一个值





## 设置PATH 环境变量

当你在shell命令行界面中输入一个外部命令时 ，shell必须搜索系统来找到对应的程序。
```bash
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:
/sbin:/bin:/usr/games:/usr/local/games
$

```
输出中显示了有8个可供shell用来查找命令和程序。PATH中的目录使用冒号分隔。
如果命令或者程序的位置没有包括在PATH变量中，那么如果不使用绝对路径的话，shell是没
法找到的。如果shell找不到指定的命令或程序，它会产生一个错误信息：
```bash

$ myprog
-bash: myprog: command not found
$

```
问题是，应用程序放置可执行文件的目录常常不在PATH环境变量所包含的目录中。解决的办法是保证PATH环境变量包含了所有存放应用程序的目录。
可以把新的搜索目录添加到现有的PATH环境变量中，无需从头定义。PATH中各个目录之间
是用冒号分隔的。你只需引用原来的PATH值，然后再给这个字符串添加新目录就行了。可以参考下面的例子。
```bash

$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:
/sbin:/bin:/usr/games:/usr/local/games
$
$ PATH=$PATH:/home/christine/Scripts
$
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/
games:/usr/local/games:/home/christine/Scripts
$
$ myprog
The factorial of 5 is 120.
$

```
窍门 如果希望子shell也能找到你的程序的位置，一定要记得把修改后的PATH环境变量导出。 exort PATH.

```
$ PATH=$PATH:.
$
$ cd /home/christine/Old_Scripts
$
$ myprog2
The factorial of 6 is 720
$

```
将单点符也加入PATH环境变量。



## 定位系统环境变量

启动bash shell有3种方式
登录时作为默认登录shell  
作为非登录shell的交互式shell  
作为运行脚本的非交互shell  


当你登录Linux系统时，bash shell会作为登录shell启动。登录shell会从5个不同的启动文件里读取命令：
```
/etc/profile
$HOME/.bash_profile
$HOME/.bash_login
$HOME/.profile
$HOME/.bashrc
```
/etc/profile文件是系统上默认的bash shell的主启动文件。系统上的每个用户登录时都会执行这个启动文件。  
/etc/profile文件都用到了同一个特性：for语句。它用来迭代/etc/profile.d目录  

剩下的启动文件都起着同一个作用：提供一个用户专属的启动文件来定义该用户所用到的环境变量。大多数Linux发行版只用这四个启动文件中的一到两个.
shell会按照按照下列顺序，运行第一个被找到的文件，余下的则被忽略：  
```
$HOME/.bash_profile  
$HOME/.bash_login  
$HOME/.profile  
```

如果你的bash shell不是登录系统时启动的（比如是在命令行提示符下敲入bash时启动），那  
么你启动的shell叫作交互式shell。  
如果bash是作为交互式shell启动的，它就不会访问/etc/profile文件，只会检查用户HOME目录  
中的.bashrc文件。  

是非交互式shell。系统执行shell脚本时用的就是这种shell。不同的地方在于它  
没有命令行提示符。但是当你在系统上运行脚本时，也许希望能够运行一些特定启动的命令。  
为了处理这种情况，bash shell提供了BASH_ENV环境变量。当shell启动一个非交互式shell进  
程时，它会检查这个环境变量来查看要执行的启动文件。如果有指定的文件，shell会执行该文件  
里的命令，这通常包括shell脚本变量设置。  

那如果BASH_ENV变量没有设置，shell脚本到哪里去获得它们的环境变量呢？别忘了有些  
shell脚本是通过启动一个子shell来执行的（参见第5章）。子shell可以继承父shell导出过的变量。  
举例来说，如果父shell是登录shell，在/etc/profile、/etc/profile.d/*.sh和$HOME/.bashrc文件中  
设置并导出了变量，用于执行脚本的子shell就能够继承这些变量。  
要记住，由父shell设置但并未导出的变量都是局部变量。子shell无法继承局部变量 。  
对于那些不启动子shell的脚本，变量已经存在于当前shell中了。所以就算没有设置  
BASH_ENV，也可以使用当前shell的局部变量和全局变量。  


对全局环境变量来说（Linux系统中所有用户都需要使用的变量），可能更倾向于将新的或修  
改过的变量设置放在/etc/profile文件中，但这可不是什么好主意。如果你升级了所用的发行版  ，   
这个文件也会跟着更新，那你所有定制过的变量设置可就都没有了。   
最好是在/etc/profile.d目录中创建一个以.sh结尾的文件。把所有新的或修改过的全局环境变  
量设置放在这个文件中。   

存储个人用户永久性```bash shell```变量的地方是```$HOME/.bashrc```文件。这一 点适用于所有类型的shell进程。    
但如果设置了BASH_ENV变量，那么记住，除非它指向的是$HOME/.bashrc，否则你应该将非交互式shell的用户变量放在别的地方。   


## 数组变量

数组是能够存储多个值的变量。这些值可以单独引用，也可以作为整个数组来引用。
```
$ mytest=(one two three four five)
$
```

没什么特别的地方。如果你想把数组像普通的环境变量那样显示，你会失望的。
```
$ echo $mytest
one
$
```
只有数组的第一个值显示出来了。要引用一个单独的数组元素，就必须用代表它在数组中位
置的数值索引值。索引值要用方括号括起来。
```
$ echo ${mytest[2]}
three
$
```
要显示整个数组变量，可用星号作为通配符放在索引值的位置。
```
$ echo ${mytest[*]}
one two three four five
$
```
也可以改变某个索引值位置的值。
```
$ mytest[2]=seven
$
$ echo ${mytest[*]}
one two seven four five
$
```
甚至能用unset命令删除数组中的某个值，但是要小心，这可能会有点复杂。看下面的例子。
```
$ unset mytest[2]
$
$ echo ${mytest[*]}
one two four five
$
$ echo ${mytest[2]}

$ echo ${mytest[3]}
four
$
```
这个例子用unset命令删除在索引值为2的位置上的值。显示整个数组时，看起来像是索引
里面已经没这个索引了。但当专门显示索引值为2的位置上的值时，就能看到这个位置是空的。
最后，可以在unset命令后跟上数组名来删除整个数组。
```
$ unset mytest
$
$ echo ${mytest[*]}
$
```




# 第7 章 理解Linux 文件权限

本章内容  
 理解Linux的安全性  
 解读文件权限  
 使用Linux组  




用户权限是通过创建用户时分配的用户ID（User ID，通常缩写为UID）来跟踪的。
UID是数值，每个用户都有唯一的UID，但在登录系统时用的不是UID，而是登录名。



## /etc/passwd 文件

```
$ cat /etc/passwd                                                                                                              [mamh@10.0.63.43 ] 18-04-23 17:25  /home/mamh/work/jenkins-core
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
syslog:x:101:104::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
usbmux:x:103:46:usbmux daemon,,,:/home/usbmux:/bin/false
dnsmasq:x:104:65534:dnsmasq,,,:/var/lib/misc:/bin/false
avahi-autoipd:x:105:113:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
kernoops:x:106:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
rtkit:x:107:114:RealtimeKit,,,:/proc:/bin/false
saned:x:108:115::/var/lib/saned:/bin/false
whoopsie:x:109:116::/nonexistent:/bin/false
speech-dispatcher:x:110:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/sh
avahi:x:111:117:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
lightdm:x:112:118:Light Display Manager:/var/lib/lightdm:/bin/false
colord:x:113:121:colord colour management daemon,,,:/var/lib/colord:/bin/false
hplip:x:114:7:HPLIP system user,,,:/var/run/hplip:/bin/false
pulse:x:115:122:PulseAudio daemon,,,:/var/run/pulse:/bin/false
sshd:x:116:65534::/var/run/sshd:/usr/sbin/nologin
systemd-timesync:x:118:127:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:119:128:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:120:129:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:121:130:systemd Bus Proxy,,,:/run/systemd:/bin/false
uuidd:x:100:101::/run/uuidd:/bin/false
_apt:x:122:65534::/nonexistent:/bin/false
sddm:x:123:132:Simple Desktop Display Manager:/var/lib/sddm:/bin/false
ntop:x:124:133::/var/lib/ntop:/bin/false
statd:x:125:65534::/var/lib/nfs:/bin/false
ftp:x:126:134:ftp daemon,,,:/srv/ftp:/bin/false
postgres:x:127:135:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
mysql:x:128:137:MySQL Server,,,:/nonexistent:/bin/false
Debian-exim:x:129:138::/var/spool/exim4:/bin/false
tomcat8:x:130:139::/usr/share/tomcat8:/bin/false
tomcat7:x:131:140::/usr/share/tomcat7:/bin/false
jenkins:x:117:125:Jenkins,,,:/var/lib/jenkins:/bin/bash
gerrit:x:1001:1001::/var/gerrit:
mamh:x:1000:1000::/home/mamh:/usr/bin/zsh
postfix:x:132:141::/var/spool/postfix:/bin/false
redis:x:133:143::/var/lib/redis:/bin/false
gitlab-www:x:999:999::/var/opt/gitlab/nginx:/bin/false
git:x:998:998::/var/opt/gitlab:/bin/sh
gitlab-redis:x:997:997::/var/opt/gitlab/redis:/bin/false
gitlab-psql:x:996:996::/var/opt/gitlab/postgresql:/bin/sh
gitlab-prometheus:x:995:995::/var/opt/gitlab/prometheus:/bin/sh
nexus:x:1002:1002:,,,:/home/nexus:/bin/bash
jetty:x:134:144::/usr/share/jetty9:/bin/false
```

root用户账户是Linux系统的管理员，固定分配给它的UID是0  
Linux系统会为各种各样的功能创建不同的用户账户，而这些账户并不是真的用户。这些账户叫作系统账户，是系统上运行的各种服务进程访问资源用的特殊账户.  

/etc/passwd文件的字段包含了如下信息：
```
登录用户名
用户密码
用户账户的UID（数字形式）
用户账户的组ID（GID）（数字形式）
用户账户的文本描述（称为备注字段）
用户HOME目录的位置
用户的默认shell
```
/etc/passwd文件中的密码字段都被设置成了x，这并不是说所有的用户账户都用相同的密码。
在早期的Linux上，/etc/passwd文件里有加密后的用户密码。但鉴于很多程序都需要访问
/etc/passwd文件获取用户信息，这就成了一个安全隐患。随着用来破解加密密码的工具的不断演
进，用心不良的人开始忙于破解存储在/etc/passwd文件中的密码。Linux开发人员需要重新考虑这
个策略。
现在，绝大多数Linux系统都将用户密码保存在另一个单独的文件中（叫作shadow文件，位置
在/etc/shadow）。只有特定的程序（比如登录程序）才能访问这个文件。

/etc/passwd是一个标准的文本文件。你可以用任何文本编辑器在/etc/password文件里直接手动
进行用户管理（比如添加、修改或删除用户账户）。但这样做极其危险。如果/etc/passwd文件出现
损坏，系统就无法读取它的内容了，这样会导致用户无法正常登录（即便是root用户）。用标准的
Linux用户管理工具去执行这些用户管理功能就会安全许多。


## /etc/shadow 文件

/etc/shadow文件对Linux系统密码管理提供了更多的控制。只有root用户才能访问/etc/shadow文件，这让它比起/etc/passwd安全许多
```
$ sudo cat /etc/shadow                                                                                                         [mamh@10.0.63.43 ] 18-04-26 16:25  /home/mamh/work/jenkins-core
root:!:16981:0:99999:7:::
daemon:*:16848:0:99999:7:::
bin:*:16848:0:99999:7:::
sys:*:16848:0:99999:7:::
sync:*:16848:0:99999:7:::
games:*:16848:0:99999:7:::
man:*:16848:0:99999:7:::
lp:*:16848:0:99999:7:::
mail:*:16848:0:99999:7:::
news:*:16848:0:99999:7:::
uucp:*:16848:0:99999:7:::
proxy:*:16848:0:99999:7:::
www-data:*:16848:0:99999:7:::
backup:*:16848:0:99999:7:::
list:*:16848:0:99999:7:::
irc:*:16848:0:99999:7:::
gnats:*:16848:0:99999:7:::
nobody:*:16848:0:99999:7:::
syslog:*:16848:0:99999:7:::
messagebus:*:16848:0:99999:7:::
usbmux:*:16848:0:99999:7:::
dnsmasq:*:16848:0:99999:7:::
avahi-autoipd:*:16848:0:99999:7:::
kernoops:*:16848:0:99999:7:::
rtkit:*:16848:0:99999:7:::
saned:*:16848:0:99999:7:::
whoopsie:*:16848:0:99999:7:::
speech-dispatcher:!:16848:0:99999:7:::
avahi:*:16848:0:99999:7:::
lightdm:*:16848:0:99999:7:::
colord:*:16848:0:99999:7:::
hplip:*:16848:0:99999:7:::
pulse:*:16848:0:99999:7:::
mamh:$6$Gi6ZqWPy$nl9MvdIPaQLZnsBloMgapvrryXf8IzaB2ovIFG/V90qhNYw5xnhl2SyTLR9uHjaZUZt64COGiyd0FovPi02gK/:16981:0:99999:7:::
sshd:*:16981:0:99999:7:::
jenkins:*:17017:0:99999:7:::
systemd-timesync:*:17028:0:99999:7:::
systemd-network:*:17028:0:99999:7:::
systemd-resolve:*:17028:0:99999:7:::
systemd-bus-proxy:*:17028:0:99999:7:::
uuidd:!:16848:0:99999:7:::
_apt:*:17028:0:99999:7:::
sddm:*:17029:0:99999:7:::
ntop:*:17099:0:99999:7:::
gerrit:!:17102:0:99999:7:::
statd:*:17134:0:99999:7:::
ftp:*:17169:0:99999:7:::
postgres:*:17183:0:99999:7:::
mysql:!:17338:0:99999:7:::
Debian-exim:!:17344:0:99999:7:::
tomcat8:*:17372:0:99999:7:::
tomcat7:*:17372:0:99999:7:::
postfix:*:17403:0:99999:7:::
redis:*:17403:0:99999:7:::
gitlab-www:!:17421::::::
git:!:17421::::::
gitlab-redis:!:17421::::::
gitlab-psql:!:17421::::::
gitlab-prometheus:!:17421::::::
nexus:!:17520:0:99999:7:::
jetty:*:17550:0:99999:7:::
```


在/etc/shadow文件的每条记录中都有9个字段：
```
与/etc/passwd文件中的登录名字段对应的登录名
加密后的密码
自上次修改密码后过去的天数密码（自1970年1月1日开始计算）
多少天后才能更改密码
多少天后必须更改密码
密码过期前提前多少天提醒用户更改密码
密码过期后多少天禁用用户账户
用户账户被禁用的日期（用自1970年1月1日到当天的天数表示）
预留字段给将来使用

```


## 添加新用户,useradd命令

useradd命令使用系统的默认值以及命令行参数来设置用户账户。系统默认值被设置在/etc/default/useradd文件中.
```
$ useradd -D                                                                                                                    
GROUP=100             新用户会被添加到GID为100的公共组
HOME=/home            新用户的HOME目录将会位于/home/loginname
INACTIVE=-1           新用户账户密码在过期后不会被禁用；
EXPIRE=               新用户账户未被设置过期日期；
SHELL=/bin/sh         新用户账户将bash shell作为默认sh；
SKEL=/etc/skel        系统会将/etc/skel目录下的内容复制到用户的HOME目录下
CREATE_MAIL_SPOOL=no  系统为该用户账户在mail目录下创建一个用于接收邮件的文件
$                      
```

倒数第二个值很有意思。useradd命令允许管理员创建一份默认的HOME目录配置，然后把
它作为创建新用户HOME目录的模板。这样就能自动在每个新用户的HOME目录里放置默认的系
统文件。在Ubuntu Linux系统上，/etc/skel目录有下列文件
```
$ ll /etc/skel -alh                                                                                                            
total 40K
drwxr-xr-x   2 root root 4.0K 5月  23  2017 ./
drwxr-xr-x 191 root root  12K 4月  20 10:47 ../
-rw-r--r--   1 root root 3.7K 6月  24  2016 .bashrc
-rw-r--r--   1 root root  220 4月   9  2014 .bash_logout
-rw-r--r--   1 root root  655 6月  24  2016 .profile
-rw-r--r--   1 root root 8.8K 10月  4  2013 examples.desktop
```


```
-c comment 给新用户添加备注
-d home_dir 为主目录指定一个名字（如果不想用登录名作为主目录名的话）
-e expire_date 用YYYY-MM-DD格式指定一个账户过期的日期
-f inactive_days 指定这个账户密码过期后多少天这个账户被禁用；0表示密码一过期就立即禁用，1表示
禁用这个功能
-g initial_group 指定用户登录组的GID或组名
-G group ... 指定用户除登录组之外所属的一个或多个附加组
-k 必须和-m一起使用，将/etc/skel目录的内容复制到用户的HOME目录
-m 创建用户的HOME目录
-M 不创建用户的HOME目录（当默认设置里要求创建时才使用这个选项）
-n 创建一个与用户登录名同名的新组

-r 创建系统账户
-p passwd 为用户账户指定默认密码
-s shell 指定默认的登录shell
-u uid 为账户指定唯一的UID
```

修改一下系统的默认值,可以在-D选项后跟上一个指定的值来修改系统默认的新用户设置.
```
-b default_home 更改默认的创建用户HOME目录的位置
-e expiration_date 更改默认的新账户的过期日期
-f inactive 更改默认的新用户从密码过期到账户被禁用的天数
-g group 更改默认的组名称或GID
-s shell 更改默认的登录shell
```

## 删除用户 userdel命令

userdel命令会只删除/etc/passwd文件中的用户信息，而不会删除系统中属于该账户的任何文件。.

-r参数，userdel会删除用户的HOME目录以及邮件目录.


## 修改用户 usermod命令

usermod命令是用户账户修改工具中最强大的一个。它能用来修改/etc/passwd文件中的大部分字段.
-c修改备注字段，  
-e修改过期日期，  
-g修改默认的登录组  
-l修改用户账户的登录名。  
-L锁定账户，使用户无法登录。  
-p修改账户的密码。  
-U解除锁定，使用户能够登录。  


## 修改密码 passwd和chpasswd 命令

passwd和chpasswd
如果只用passwd命令，它会改你自己的密码。系统上的任何用户都能改自己的密码，但只有root用户才有权限改别人的密码。  
-e选项能强制用户下次登录时修改密码。你可以先给用户设置一个简单的密码，之后再强制在下次登录时改成他们能记住的更复杂的密码。  
如果需要为系统中的大量用户修改密码，chpasswd命令可以事半功倍。chpasswd命令能从标准输入自动读取登录名和密码对（由冒号分割）列表，给密码加密，然后为用户账户设置.


## 修改用户 chsh、chfn和chage
chsh命令用来快速修改默认的用户登录shell。使用时必须用shell的全路径名作为参数，不能只用shell名。
```
# chsh -s /bin/csh test
Changing shell for test.
Shell changed.
#
```
chfn命令提供了在/etc/passwd文件的备注字段中存储信息的标准方法。chfn命令会将用于
Unix的finger命令的信息存进备注字段，而不是简单地存入一些随机文本（比如名字或昵称之
类的），或是将备注字段留空。finger命令可以非常方便地查看Linux系统上的用户信息。
```
$ finger mamh
Login: mamh           			Name: 
Directory: /home/mamh               	Shell: /usr/bin/zsh
On since Mon Apr 23 07:43 (CST) on tty7 from :0
     4 days 1 hour idle
On since Mon Apr 23 07:47 (CST) on pts/5 from 10.0.63.42
    2 seconds idle
On since Fri Apr 27 08:03 (CST) on pts/20 from :0
   48 minutes 38 seconds idle
On since Mon Apr 23 09:54 (CST) on pts/22 from linux-pc
   3 days 23 hours idle
On since Mon Apr 23 10:00 (CST) on pts/23 from linux-pc
   17 hours 23 minutes idle
Last login Thu Apr 26 20:36 (CST) on pts/26 from 10.0.13.159
No mail.
No Plan.
$ finger root
Login: root           			Name: root
Directory: /root                    	Shell: /bin/bash
Never logged in.
No mail.
No Plan.

```
出于安全性考虑，很多Linux系统管理员会在系统上禁用finger命令，不少Linux发行版甚至都没有默认安装该命令。我的ubuntu上就没有。

chfn命令时没有参数，它会向你询问要将哪些适合的内容加进备注字段。

chage命令用来帮助管理用户账户的有效期。
```bash
-d 设置上次修改密码到现在的天数
-E 设置密码过期的日期
-I 设置密码过期到锁定账户的天数
-m 设置修改密码之间最少要多少天
-W 设置密码过期前多久开始出现提醒信息

```

chage命令的日期值可以用下面两种方式中的任意一种：
 YYYY-MM-DD格式的日期
 代表从1970年1月1日起到该日期天数的数值

chage命令中有个好用的功能是设置账户的过期日期。有了它，你就能创建在特定日期自动
过期的临时用户，再也不需要记住删除用户了！过期的账户跟锁定的账户很相似：账户仍然存在，
但用户无法用它登录。

## /etc/group 文件
每个组都有唯一的GID——跟UID类似，在系统上这是个唯一的数值。除了GID，每个组还有唯一的组名
Ubuntu就会为每个用户创建一个单独的与用户账户同名的组。
```bash

$ cat /etc/group   
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,mamh
tty:x:5:mamh
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:mamh
fax:x:21:
voice:x:22:
cdrom:x:24:mamh
floppy:x:25:
tape:x:26:
sudo:x:27:mamh
audio:x:29:pulse
dip:x:30:mamh
www-data:x:33:
backup:x:34:
operator:x:37:
list:x:38:
irc:x:39:
src:x:40:
gnats:x:41:
shadow:x:42:
utmp:x:43:
video:x:44:
sasl:x:45:
plugdev:x:46:mamh
staff:x:50:
games:x:60:
users:x:100:
nogroup:x:65534:
netdev:x:102:
crontab:x:103:
syslog:x:104:
fuse:x:105:
messagebus:x:106:
ssl-cert:x:107:postgres
lpadmin:x:108:mamh
scanner:x:109:saned
mlocate:x:110:
ssh:x:111:
utempter:x:112:
avahi-autoipd:x:113:
rtkit:x:114:
saned:x:115:
whoopsie:x:116:
avahi:x:117:
lightdm:x:118:
nopasswdlogin:x:119:
bluetooth:x:120:
colord:x:121:
pulse:x:122:
pulse-access:x:123:
mamh:x:1000:
sambashare:x:124:mamh
jenkins:x:125:
systemd-journal:x:126:
systemd-timesync:x:127:
systemd-network:x:128:
systemd-resolve:x:129:
systemd-bus-proxy:x:130:
uuidd:x:101:
input:x:131:
sddm:x:132:
ntop:x:133:
gerrit:x:1001:
ftp:x:134:
postgres:x:135:
docker:x:136:
mysql:x:137:
Debian-exim:x:138:
tomcat8:x:139:
tomcat7:x:140:
postfix:x:141:
postdrop:x:142:
redis:x:143:
gitlab-www:x:999:
git:x:998:
gitlab-redis:x:997:
gitlab-psql:x:996:
gitlab-prometheus:x:995:
nexus:x:1002:
jetty:x:144:
vboxusers:x:145:
$                           

```
/etc/group文件有4个字段：
```bash
组名
组密码         # 组密码允许非组内成员通过它临时成为该组成员。这个功能并不很普遍，但确实存在。
GID
属于该组的用户列表
```
千万不能通过直接修改/etc/group文件来添加用户到一个组，要用usermod命令

## 创建新组 groupadd命令
```bash

$ groupadd --help 
Usage: groupadd [options] GROUP

Options:
  -f, --force                   exit successfully if the group already exists,
                                and cancel -g if the GID is already used
  -g, --gid GID                 use GID for the new group
  -h, --help                    display this help message and exit
  -K, --key KEY=VALUE           override /etc/login.defs defaults
  -o, --non-unique              allow to create groups with duplicate
                                (non-unique) GID
  -p, --password PASSWORD       use this encrypted password for the new group
  -r, --system                  create a system account
  -R, --root CHROOT_DIR         directory to chroot into
      --extrausers              Use the extra users database

```
在创建新组时，默认没有用户被分配到该组。groupadd命令没有提供将用户添加到组中的选项，但可以用usermod命令来弥补这一点
```bash
# /usr/sbin/usermod -G shared rich
# /usr/sbin/usermod -G shared test

```
为用户账户分配组时要格外小心。如果加了-g选项，指定的组名会替换掉该账户的默认组。-G选项则将该组添加到用户的属组的列表里，不会影响默认组。

## 修改组 groupmod命令
修改组名时，GID和组成员不会变，只有组名改变。由于所有的安全权限都是基于GID的，
你可以随意改变组名而不会影响文件的安全性。


## 文件权限符
```bash
$ ls –l
total 68
-rw-rw-r-- 1 rich rich 50 2010-09-13 07:49 file1.gz
-rw-rw-r-- 1 rich rich 23 2010-09-13 07:50 file2
-rw-rw-r-- 1 rich rich 48 2010-09-13 07:56 file3
-rw-rw-r-- 1 rich rich 34 2010-09-13 08:59 file4
-rwxrwxr-x 1 rich rich 4882 2010-09-18 13:58 myprog
-rw-rw-r-- 1 rich rich 237 2010-09-18 13:58 myprog.c
drwxrwxr-x 2 rich rich 4096 2010-09-03 15:12 test1
drwxrwxr-x 2 rich rich 4096 2010-09-03 15:12 test2
$

```
输出结果的第一个字段就是描述文件和目录权限的编码。这个字段的第一个字符代表了对象的类型：
```
 -代表文件
 d代表目录
 l代表链接
 c代表字符型设备
 b代表块设备
 n代表网络设备

之后有3组三字符的编码。每一组定义了3种访问权限：
r代表对象是可读的
w代表对象是可写的
x代表对象是可执行的
```


## 默认文件权限    umask

umask命令用来设置所创建文件和目录的默认权限。
```bash
$ touch newfile
$ ls -al newfile
-rw-r--r-- 1 rich rich 0 Sep 20 19:16 newfile
$


$ umask
0022
$
```

umask命令设置没那么简单明了，想弄明白其工作原理就更混乱了。第一位代表了一项特别的安全特性，叫作粘着位（sticky bit）

八进制模式的安全性设置先获取这3个rwx权限的值，然后将其转换成3位二进制值，用一个八进制值来表示
```bash

--- 000 0 没有任何权限
--x 001 1 只有执行权限
-w- 010 2 只有写入权限
-wx 011 3 有写入和执行权限
r-- 100 4 只有读取权限
r-x 101 5 有读取和执行权限
rw- 110 6 有读取和写入权限
rwx 111 7 有全部权限

```

## 改变权限 chmod命令

```bash
chmod options mode file


$ chmod 760 newfile
$ ls -l newfile
-rwxrw---- 1 rich rich 0 Sep 20 19:16 newfile
$

```

```
 u代表用户
 g代表组
 o代表其他
 a代表上述所有
```

后面跟着的符号表示你是想在现有权限基础上增加权限（+），还是在现有权限基础上移除权限（），或是将权限设置成后面的值（=）
```
 X：如果对象是目录或者它已有执行权限，赋予执行权限。
 s：运行时重新设置UID或GID。
 t：保留文件或目录。
 u：将权限设置为跟属主一样。
 g：将权限设置为跟属组一样。
 o：将权限设置为跟其他用户一样。
```

-R选项可以让权限的改变递归地作用到文件和子目录

## 改变所属关系  chown命令

```bash
chown options owner[.group] file

```
-R选项配合通配符可以递归地改变子目录和文件的所属关系。  
-h选项可以改变该文件的所有符号链接文件的所属关系.   


## 粘着位, SUID , SGID 
Linux还为每个文件和目录存储了3个额外的信息位。
```bash
 设置用户ID（SUID）：当文件被用户使用时，程序会以文件属主的权限运行。
 设置组ID（SGID）：对文件来说，程序会以文件属组的权限运行；对目录来说，目录中创建的新文件会以目录的默认属组作为默认属组。
 粘着位：进程结束后文件还驻留（粘着）在内存中。
```

# 第8 章 管理文件系统

本章内容  
 文件系统基础  
 日志文件系统与写时复制文件系统  
 文件系统管理  
 逻辑卷布局  
 使用Linux逻辑卷管理器  



## ext文件系统
Linux操作系统中引入的最早的文件系统叫作扩展文件系统（extended filesystem，简记为ext）。
ext文件系统采用名为索引节点的系统来存放虚拟目录中所存储文件的信息。索引节点系统
在每个物理设备中创建一个单独的表（称为索引节点表）来存储这些文件的信息。
ext文件系统名称中的extended部分来自其跟踪的每个文件的额外数据，包括：
* 文件名
* 文件大小
* 文件的属主
* 文件的属组
* 文件的访问权限
* 指向存有文件数据的每个硬盘块的指针

## ext2文件系统
最早的ext文件系统有不少限制，比如文件大小不得超过2 GB。第二代扩展文件系统，叫作ext2.
ext2文件系统还将允许的最大文件大小增加到了2 TB（在ext2的后期版本中增加到了32 TB），以容纳数据库服务器中常见的大文件。


## ext3文件系统
它采用和ext2文件系统相同的索引节点表结构，但给每个存储设备增加了一个日志文件，
以将准备写入存储设备的数据先记入日志。
日志文件系统为Linux系统增加了一层安全性。它不再使用之前先将数据直接写入存储设备
再更新索引节点表的做法，而是先将文件的更改写入到临时文件（称作日志，journal）中。在数
据成功写到存储设备和索引节点表之后，再删除对应的日志条目。
如果系统在数据被写入存储设备之前崩溃或断电了，日志文件系统下次会读取日志文件并处
理上次留下的未写入的数据。

## ext4文件系统
扩展ext3文件系统功能的结果是ext4文件系统.

## Reiser文件系统

## JFS文件系统
JFS（Journaled File System，日志化文件系统①）是IBM在1990年为其Unix衍生版AIX开发的.

## XFS文件系统

## ZFS文件系统

## Btrf文件系统
Btrfs文件系统是COW的新人，也被称为B树文件系统。。OpenSUSE Linux发行版最近将Btrfs作为其默认文件系统。


## 创建分区fdisk命令
有时候，创建新磁盘分区最麻烦的事情就是找出安装在Linux系统中的物理磁盘。Linux
采用了一种标准格式来为硬盘分配设备名称，但是你得熟悉这种格式。对于老式的IDE驱
动器，Linux使用的是/dev/hdx。其中x表示一个字母，具体是什么要根据驱动器的检测顺
序（第一个驱动器是a，第二个驱动器是b，以此类推）。对于较新的SATA驱动器和SCSI
驱动器，Linux使用/dev/sdx。其中的x具体是什么也要根据驱动器的检测顺序（和之前一
样，第一个驱动器是a，第二个驱动器是b，以此类推）。在格式化分区之前，最好再检查
一下是否正确指定了驱动器。

```text

a 设置活动分区标志
b 编辑BSD Unix系统用的磁盘标签
c 设置DOS兼容标志
d 删除分区
l 显示可用的分区类型
m 显示命令选项
n 添加一个新分区  ，                              n命令在该存储设备上创建新的分区
o 创建DOS分区表
p 显示当前分区表 ，                               p命令将一个存储设备的详细信息显示出来
q 退出，不保存更改
s 为Sun Unix系统创建一个新磁盘标签
t 修改分区的系统ID
u 改变使用的存储单位
v 验证分区表
w 将分区表写入磁盘                                w命令将更改保存到存储设备上
x 高级功能

```

有些发行版和较旧的发行版在生成新分区之后并不会自动提醒Linux系统。如果是这样的
话，你要么使用partprob或hdparm命令（参考相应的手册页），要么重启系统，让系统
读取更新过的分区表。



## 创建分区parted命令

分区表分为2种，一种限制较多的MBR分区表，一种是较新限制较少的GPT分区。GPT分区支持的空间可以超过2TB。
```bash
# parted /dev/sdb

进入到(parted) 交互界面下了

 
(parted) mklabel gpt

(parted) mkpart primary 0GB 17TB  

(parted) q. （退出）

 

 

创建文件系统，ext4格式的文件系统

# mkfs.ext4 /dev/sdb1



挂载到 /work 下面

# mount /dev/sdb1 /work




释放保留空间这里设置为100mb
#   tune2fs -m 0.1 /dev/sdb1

```


## 创建文件系统
```text
mkefs 创建一个ext文件系统
mke2fs 创建一个ext2文件系统
mkfs.ext3 创建一个ext3文件系统
mkfs.ext4 创建一个ext4文件系统
mkreiserfs 创建一个ReiserFS文件系统
jfs_mkfs 创建一个JFS文件系统
mkfs.xfs 创建一个XFS文件系统
mkfs.zfs 创建一个ZFS文件系统
mkfs.btrfs 创建一个Btrfs文件系统

```

```bash
sudo mkfs.ext4 /dev/sdb1

```
将新文件系统挂载到虚拟目录中需要额外空间的任何位置
```bash
$ ls /mnt
$
$ sudo mkdir /mnt/my_partition
$
$ ls -al /mnt/my_partition/
$
$ ls -dF /mnt/my_partition
/mnt/my_partition/
$
$ sudo mount -t ext4 /dev/sdb1 /mnt/my_partition
$
$ ls -al /mnt/my_partition/
total 24
drwxr-xr-x. 3 root root 4096 Jun 11 09:53 .
drwxr-xr-x. 3 root root 4096 Jun 11 09:58 ..
drwx------. 2 root root 16384 Jun 11 09:53 lost+found
$

```
这种挂载文件系统的方法只能临时挂载文件系统。当重启Linux系统时，文件系统并不会
自动挂载。要强制Linux在启动时自动挂载新的文件系统，可以将其添加到/etc/fstab文件。


## 文件系统的检查与修复
fsck命令能够检查和修复大部分类型的Linux文件系统，包括本章早些时候讨论过的ext、
ext2、ext3、ext4、ReiserFS、JFS和XFS。

```text
-a 如果检测到错误，自动修复文件系统
-A 检查/etc/fstab文件中列出的所有文件系统
-C 给支持进度条功能的文件系统显示一个进度条（只有ext2和ext3）
-N 不进行检查，只显示哪些检查会执行
-r 出现错误时提示
-R 使用-A选项时跳过根文件系统
-s 检查多个文件系统时，依次进行检查
-t 指定要检查的文件系统类型
-T 启动时不显示头部信息
-V 在检查时产生详细输出
-y 检测到错误时自动修复文件系统

```
只能在未挂载的文件系统上运行fsck命令。对大多数文件系统来说，你只需卸载文件系
统来进行检查，检查完成之后重新挂载就好了。但因为根文件系统含有所有核心的Linux
命令和日志文件，所以你无法在处于运行状态的系统上卸载它。
这正是亲手体验Linux LiveCD的好时机！只需用LiveCD启动系统即可，然后在根文件系
统上运行fsck命令。


## 逻辑卷管理

在逻辑卷管理的世界里，硬盘称作物理卷（physical volume，PV）。
每个物理卷都会映射到硬盘上特定的物理分区。多个物理卷集中在一起可以形成一个卷组（volume group，VG）。
整个结构中的最后一层是逻辑卷（logical volume，LV）。


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













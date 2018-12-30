---
title: Linux入门知识整理
date: 2018-09-17 17:40:44 
tags: Linux
description: #你對本頁的描述 可以省略
---

# （一）硬件基础
作者： Q-Z-one

## CPU
+ 精简指令集RISC
+ 复杂指令集CISC
+ L2 Cache 第二层高速缓存

## 内存
+ RAM
+ ROM
   用CMOS记录开机相关的参数，需要主板上单独的电池供电

## 显卡
+ 又名VGA：Vedio Graphic Array
+ 内嵌3D加速芯片（GPU）

# （二）Linux历史
作者： Q-Z-one

1. gcc = GNU C Compiler，是GUN计划的产物
2. GUN中的GPL授权
3. Linux的内核版本可以用命令`uname -r`查询
4. Ubuntu是Linux的一个distribution


# （三）如何学习
作者： Q-Z-one

1. 官方Document：www.tldp.org
2. /var/log/ 下的log file会记录网络问题，其他问题可以在terminal里直接看
3. 好习惯：
    + 把错误信息和相应的解决方法归档
    + 文件夹名称和路径精心设计
    + 好文章可以留存


# （四）文件
作者： Q-Z-one


## 权限
1. owner/group/others分别有read/write/excute的权限
2. ls -l(或者ll)命令下，会显示例如`[file type]rwxrw-r--`，file type中`-`代表文件，`d`代表文件夹
3. 三个命令：`chown, chgrp, chmod`
    + 以把一个文件权限变成rwxrw-r--为例
    `chmod 764 filename`
    `chmod u=rwx,g=rw,o=r filename` 注意第一个是user的u
    + chown ownername filename 但ownername必须是`/etc/passwd`里有的
    + chgrp groupname filename 但groupname必须是`/etc/group`里有的

## 目录
FHS文件目录系统有明确规定。先上表格

目录 | 功能 | 目录 | 功能
 --- | ---- |  ---- | ---- 
/bin | 可执行 | /mnt | 挂载设备
/boot | 开机有关 | /opt | 预置第三方软件
/dev | 设备 | /tmp | 临时文件
/etc | 配置 | /home | 使用者主文件夹，等同于~ 
/lib | 函数库 | /usr  /var| 下文展开

 | 可分享的 | 不可分享的
 --- | ---- |  ---- 
不变的 | /usr : /opt | / etc : /boot
可变的 | /var/mail : /var/spool/news | /var/run : /var/lock

1. usr = Unix Software Resource 并不是user的缩写，比较重要的子目录如下

目录 | 功能 
 --- | ----  
/usr/bin/ | 可执行，是/bin/的链接，和/bin/一模一样
/usr/share/ | 只读的可共享文件，几乎都是纯文本，例如/usr/share/man
**/usr/local/** | 自己安装的软件
**/usr/src/** | 源代码
/usr/include/ | C/C++的包含档

2. /var 是经常变动的文件，比如Cache、logfile等等
3. 我们常用.和..两个目录，但还有一个比较常用的目录是-（短横线），代表上一个使用过的目录
4. **和编辑目录有关的常用命令**
`mkdir -p dir1/dir2/dir3` 通过`-p`可以一次创建多层目录
`rm -f` `rm -i` 分别是悄无声息地删除和需要确认再删除，f代表force，i代表info


## 查看
1. `cat  -b` 显示行号，相当于`nl`，也可以使用`cat -n`，两者的区别在于`-b`不对空白行显示行号
2. head/tail
3. more/less 尽量多用less，用/XXX可以向下搜寻，？YYY向前搜寻。寻找下一个使用n

> 一个例子，打印出文档problem1.py的11-20行用`cat -n problem1.py | head -n 20 | tail -n 10`

4. 非纯文本文件用`od`命令查看（文件类型用`file`命令查看）

> 输入: `file problem1.py`
> 输出: `problem1.py: Python script, ASCII text executable`

## 搜索
1. `which filename`用于搜索**可执行文件**，其中filename必须是**完整**的文件名
2. `whereis filename`是在特定的几个目录下搜索文件，但是速度快，其中filename必须是**完整**的文件名
3. `locate filename`是在系统维护的数据库里搜索文件，filename可以不完整，数据库定期自动更新，手动更新需输入命令`updatedb`
4. 如果以上方法都找不到，终极工具`find`包治百病，但是速度慢，费硬盘

## 压缩
1. 在linux中文件后缀名没有实际意义，只是为了好记而设置了压缩命令与压缩文件的后缀名对应关系如下：

命令 | 后缀名 
 --- | ---- 
gzip | .gz
bzip2 | .bz2
xz | .xz
tar | .tar  .tar.gz  .tar.bz2  .tar.xz

2. 在linux下压缩文件时，默认删除原文件，如果想保留，可以通过`-c`命令将压缩后的文件打印到屏幕然后重定向到压缩文件里
> bzip2 services services.bz2 #将文件services压缩为services.bz2并删除原文件services
> bzip2 -c services > services.bz2 #压缩，并保留原文件

3. 从压缩比来看，xz>bzip2>gzip
从压缩时间来看，也是一样的排序，也就是压缩得越狠，用的时间就越多

4. tar

    仅仅是一个打包命令。打包得到的文件我们成为tarfile，但是如果打包之后又得到了压缩，我们称为**tarball**
    根据打包的命令不同，后缀名也不同。例如.tar.gz和.tar.bz2


# （五）VIM
作者： Q-Z-one

## 移动光标
1.上下左右

h | j | k | l 
-- | -- | -- | -- 
← | ↓ | ↑ | → 

2.效率操作

快捷键 | 操作 
-- | -- 
10<space> |  向右10个字符
15<enter> | 向下15行
26k | 向上26行（hjkl都适用）
G | 最后一行
gg | 第一行
7G | 到第7行

## 搜索

快捷键 | 操作 
-- | -- 
/word |  向下搜索word
？word | 向上搜索word
n/N | 下一个/上一个

## 复制粘贴和删除

快捷键 | 操作 
-- | -- 
x |  delete
X | backspace
11x | 向后删除11个字符
dd | 删除1行
12,35d | 删除12-35行
yy | 复制本行
12,35y | 复制12-35行
p/P | 粘贴（插入本行后/前）
n1,n2s/word1/word2/g | 将n1-n2行的word1替换为word2
u/ctrl+r | 复原/重做（类似于Windows下的ctrl+z/y）
. | **重复执行上一动作**
ctrl+v | 按长方形范围进行选择，类似于word里的Alt

## 存储、关闭

快捷键 | 操作 
-- | -- 
:w/q/wq/wq! |  不说了
:w filename | 文件另存为
**:3,27 w filename** | **文件3-27行另存为**
11x | 向后删除11个字符
dd | 删除1行
12,35d | 删除12-35行

## 多文件编辑

快捷键 | 操作 
-- | -- 
:r filename | 把另一个文件读入光标后
vim file1 file2 |同时打开两个文件，可以用n/N在其中切换，可以在file1中yy然后去file2中p
:sp | 窗口切分，可以同时看一个文件的不同页，便于比较前后的数据，:sp filename还能同时看别的文件

## 重要指令

快捷键 | 操作 
-- | -- 
！cmd | 强制退出vim并执行cmd的命令行，比如`！ls ./`
set nu | 开启行号，如果要关闭用`set nonu`
ctrl+s | 冻结屏幕，可以用ctrl+q恢复

## 中文编码
如果文件出现乱码，可以从以下几个方面检查：
1. Linux默认支持的语言系统见`/etc/locale.conf`
2. bash的语系见环境变量 LANG、LC_ALL
3. 文件本来的编码，可以用`file`命令查询
4. 窗口接口支持的语言编码




# （六）BASH
作者： Q-Z-one

## 什么是Shell
为了保护操作系统的kernel，系统提供了一些交互的接口，这些接口就像壳（shell）一样把kernel包在里边。Shell本身只是一个接口，通常还要调用其他已有的可执行程序，比如在Bash里使用`ls`等命令，Bash（Bourne Again shell）是Shell的一种

## 变量
1. 变量分为环境变量和一般变量，通常用大写代表环境变量，小写代表一般变量。环境变量可以被子程序继承，而一般变量不能。用`env`查看环境变量；用`set`查看所有变量
2. 申明一般变量，`var=XXXX`，默认XXXX是字符串，=前后没有空格，如果要声明整型（integer），需要用`declare -i`
2. 转换为环境变量，通过`export var`可以把一般变量var变成环境变量；`declare +x var`将环境变量恢复为一般变量
4. 删除变量，用`unset var`
3. 查看变量，用`echo $var`
6. 修改变量，除了直接=赋值，可以用类似于`PATH=${PATH}:/home/bin`的语句在原来的基础上添加
注：\$符号代表提取变量内部的值，为了避免混淆最好用{}和其他部分隔开；在命令行里$后涉及到运算时，用（），比如`ls -ld $(locate man)`

####  变量类型
1. 默认为字符串，所以`echo 1+2`的结果是‘1+2’
2. bash只支持int型计算，1/3=0
3. declare/typeset可以改变变量类型，具体用法如下

命令 | 功能 
-- | -- 
declare -a | 声明为array
declare -i | 声明为integer
declare -x | 声明为环境变量
declare -r | 声明为readonly变量

## 数据流重定向
1. stdin < <<，<是输入，<<是输入结束
2. stdout 1> 1>>，>是覆盖文件，>>是在文件最后添加新的数据流，下同
3. stderr  2> 2>>
注：如果只写> >>，默认是1> 1>>，即stdout
4. 不需要打印在屏幕上的信息，直接>到`/dev/null`（垃圾桶）
5. 需要同时把stdout和stderr重定向到文件，特殊语法`&>`或者`2&>1`
6. 一种替代cp的操作：`cat < srcfile > desfile`
7. 需要同时把数据存储到文件又打印到屏幕，用tee命令，例如`ls -lm /home | tee -a ~/homefile | more`

## 执行多重命令
1. `cmd1;cmd2;cmd3 #顺序执行cmd1/2/3`
2. `cmd1&&cmd2 #若cmd1成功则执行cmd2`
3. `cmd1||cmd2 #若cmd1失败则执行cmd2，成功则不执行2`
4. `cmd1&&cmd2||cmd3 #若1成功则执行2，否则执行3`
5. 如果命令太长要分行写，行尾加上\再[Enter]

## 重要指令

命令 | 功能 
-- | -- 
ctrl+u| 向前删除这行命令
ctrl+k | 向后删除这行命令
history | 在执行后！num执行第num条历史命令，！！执行上一条命令，**！cmd执行最近含有cmd字符串的命令**
基本正则表达式 | *代表任意多个字符（0-$\inf$），？代表一个，[abcd]代表这一位必是abcd中的一个，[^0-9]代表这一位不是数字

## 管线（Pipe）
1. 首先明确下概念，管线命令的每一步一定是把stdin转换为stdout的命令，过程大致如下：
[![PvE1Ig.png](https://s1.ax1x.com/2018/08/31/PvE1Ig.png)](https://imgchr.com/i/PvE1Ig)
2. 常用的管线命令
+ cut/grep (撷取命令)
+ sort/uniq (排序命令)
+ less（展示命令）
+ wc #统计字数


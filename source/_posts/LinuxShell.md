---
title: Shell
date: 2017-10-24 18:16:38
categories: DevOps
tags:
  - Shell
  - Bash
  - Linux
  - Vim
---


参考：

- Linux Shell脚本攻略
- 鸟哥的Linux私房菜
- k-vim: <https://github.com/wklken/k-vim>




<br>
<br/>

---

<!--more-->

<br/>
<br/>
















# inode

参考:

- wiki: <https://zh.wikipedia.org/wiki/Inode>
- [关于 inode](https://www.ibm.com/developerworks/cn/aix/library/au-speakingunix14/index.html)
- [理解inode](http://www.ruanyifeng.com/blog/2011/12/inode.html)


<br/>
<br/>


## 简介


**inode**是UNIX操作系统中的一种数据结构，它包含了与文件系统中各个**文件系统对象**(文件、目录、设备文件、socket、管道...)的元数据信息。在UNIX中创建文件系统时，同时将会创建大量的inode。通常，文件系统磁盘空间中大约百分之一空间分配给了inode表。

Unix先驱丹尼斯·里奇说，inode这个命名的来源可能是文件系统的存储组织为一个扁平数组，分层目录信息使用一个数作为文件系统这个扁平数组的索引值（index）。

<br>

理解inode，要从文件储存说起。

文件储存在硬盘上，硬盘的最小存储单位叫做**扇区**（Sector）。每个扇区储存512字节（相当于0.5KB）。
操作系统读取硬盘的时候，不会一个个扇区地读取，这样效率太低，而是一次性连续读取多个扇区，即一次性读取一个**块**（block）。这种由多个扇区组成的"块"，是文件存取的最小单位。"块"的大小，最常见的是4KB，即连续八个sector组成一个block。
文件数据都储存在**块**中，那么很显然，我们还必须找到一个地方储存文件的元信息，比如文件的创建者、文件的创建日期、文件的大小等等。这种储存文件元信息的区域就叫做**inode**，中文译名为**索引节点**。




<br/>
<br/>
<br/>




## 细节


文件系统创建（格式化）时，就把存储区域分为两大连续的存储区域。一个用来保存文件系统对象的元信息数据，这是由inode组成的表，每个inode默认是256字节或者128字节。另一个用来保存**文件系统对象**的内容数据，划分为512字节的扇区，以及由8个扇区组成的4K字节的块。块是读写时的基本单位。一个文件系统的inode的总数是固定的。这限制了该文件系统所能存储的文件系统对象的总数目。典型的实现下，所有inode占用了文件系统1%左右的存储容量。

文件系统中每个文件系统对象对应一个inode数据，并用一个整数值来辨识。这个整数常被称为inode号码（i-number或inode number）。由于文件系统的inode表的存储位置、总条目数量都是固定的，因此可以用inode号码去索引查找inode表。

Inode存储了文件系统对象的一些元信息，如所有者、访问权限（读、写、执行）、类型（是文件还是目录）、内容修改时间、inode修改时间、上次访问时间、对应的文件系统存储块的地址，等等。知道了1个文件的inode号码，就可以在inode元数据中查出文件内容数据的存储地址。

文件名与目录名是文件系统对象便于使用的别名。一个文件系统对象可以有多个别名，但只能有一个inode，并用这个inode来索引文件系统对象的存储位置。

- inode不包含文件名或目录名的字符串，只包含文件或目录的元信息
- Unix的文件系统的目录也是一种文件。打开目录，实际上就是读取目录文件。目录文件的结构是一系列目录项（dirent）的列表。每个目录项，由两部分组成：所包含文件或目录的名字，以及该文件或目录名对应的inode号码
- 文件系统中的一个文件是指存放在其所属目录的目录文件中的一个目录项，其所对应的inode的类别为文件；文件系统中的一个目录是指存放在其父目录文件中的一个目录项，其所对应的inode的类别为目录。可见，多个文件可以对应同一个inode;多个目录可以对应同一个inode
- 文件系统中如果两个文件或者两个目录具有相同的inode号码，那么就称它们是**硬链接**关系。实际上都是这个inode的别名。换句话说，一个inode对应的所有文件（或目录）中的每一个，都对应着文件系统某个目录文件中唯一的一个目录项
- 创建一个目录时，实际做了3件事：在其父目录文件中增加一个条目；分配一个inode；再分配一个存储块，用来保存当前被创建目录包含的文件与子目录。被创建的目录文件中自动生成两个子目录的条目，名称分别是：`.`和`..`。前者与该目录具有相同的inode号码，因此是该目录的一个硬链接。后者的inode号码就是该目录的父目录的inode号码。所以，任何一个目录的硬链接总数，总是等于它的子目录总数（含隐藏目录）加2。即每个子目录文件中的`..`条目，加上它自身的目录文件中的`.`条目。再加上父目录文件中的对应该目录的条目。
- 通过文件名打开文件，实际上是分成三步实现：首先，操作系统找到这个文件名对应的inode号码；其次，通过inode号码，获取inode信息；最后，根据inode信息，找到文件数据所在的block，读出数据。




<br/>
<br/>
<br/>



## 讨论


- 一个文件系统对象可以有多个名字，这些具有硬链接关系的文件系统对象名字具有相同的inode号码，彼此是平等的。即首个被创建的文件并没有特殊的地位。这与符号链接不同。一个符号链接文件有自己的inode，符号链接文件的内容是它所指向的文件的名字。因此删除符号链接所指向的文件，将导致这个符号链接文件在访问时报错
- 删除一个文件或目录，实际上是把它的inode的链接数减1。这并不影响指向此inode的别的硬链接
- 一个inode如果没有硬链接，此时inode的链接数为0，文件系统将回收该inode所指向的存储块，并回收该inode自身
- 从一个inode，通常是无法确定是用哪个文件名查到此inode号码的。打开一个文件后，操作系统实际上就抛掉了文件名，只保留了inode号码来访问文件的内容。库函数`getcwd()`用来查询当前工作目录的绝对路径名。其实现是从当前工作目录的inode逐级查找其上级目录的inode，最后拼出整个绝对路径的名字
- 历史上，对目录的硬链接是可能的。这可以使目录结构成为一个有向图，而不是通常的目录树的有向无环图。一个目录甚至可以是自身的父目录。现代文件系统一般禁止这些混淆状态，只有根目录保持了特例：根目录是自身的父目录。`ls /..`就是根目录的内容
- 一个文件或目录在文件系统内部移动时，其inode号码不变。文件系统碎片整理可能会改变一个文件的物理存储位置，但其inode号码不变。非UNIX的FAT及其派生的文件系统是无法实现inode不变这一特点。
- inode文件系统中安装新库十分容易。当一些进程正在使用一个库时，其它进程可以替换该库文件名字的inode号码指向新创建的inode，随后对该库的访问都被自动引导到新inode所指向的新的库文件的内容。这减少了替换库时重启系统的需要。而旧的inode的链接数已经为0，在使用旧库的进程结束后，旧的inode与旧库文件会被系统自动回收。






<br/>
<br/>
<br/>




## 结构


POSIX标准强制规范了文件系统的行为。每个文件系统对象必须具有：

- 设备ID，标识容纳该文件的设备
- 以字节为单位的文件大小
- 磁盘块
- 用户(uid)
- 组(gid)
- r/w/x权限
- 时间戳
  - ctime: inode自身被修改的时间；
  - mtime：内容修改的时间；
  - atime：最后一次被访问的时间
- 链接数，有多少硬链接指向此inode

使用`stat`系统调用可以查询一个文件的inode号码及一些元信息。

```sh
# 使用stat查看某个文件inode
stat  _config.yml
  File: '_config.yml'
  Size: 2522            Blocks: 8          IO Block: 4096   regular file
Device: fd02h/64770d    Inode: 52354       Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/ zhang)   Gid: ( 1000/ zhang)
Access: 2019-04-22 09:39:28.735991059 +0800
Modify: 2018-07-07 15:37:50.000000000 +0800
Change: 2018-12-11 07:38:26.287109004 +0800
 Birth: -
```




<br/>
<br/>
<br/>




## 大小

inode也会消耗硬盘空间，所以硬盘格式化的时候，操作系统自动将硬盘分成两个区域。一个是数据区，存放文件数据；另一个是inode表（inode table），存放inode所包含的信息。

由于每个文件都必须有一个inode，因此有可能发生inode已经用光，但是磁盘还未存满的情况。这时，就无法在磁盘上创建新文件。

```sh
# 查看inode
df -i
Filesystem                 Inodes  IUsed     IFree IUse% Mounted on
overlay                  26214400 597690  25616710    3% /
tmpfs                      485005     17    484988    1% /dev
tmpfs                      485005     16    484989    1% /sys/fs/cgroup
/dev/mapper/centos-home 215418880 297955 215120925    1% /home/test
/dev/mapper/centos-root  26214400 597690  25616710    3% /etc/hosts
shm                        485005      1    485004    1% /dev/shm
tmpfs                      485005      1    485004    1% /proc/acpi
tmpfs                      485005      1    485004    1% /proc/scsi
tmpfs                      485005      1    485004    1% /sys/firmware
```




<br/>
<br/>
<br/>



## 硬软链接


<br/>


### 硬链接

一般情况下，文件名和inode号码是"一一对应"关系，每个inode号码对应一个文件名。但是，Unix/Linux系统允许，多个文件名指向同一个inode号码。

这意味着，可以用不同的文件名访问同样的内容；对文件内容进行修改，会影响到所有文件名；但是，删除一个文件名，不影响另一个文件名的访问。这种情况就被称为"硬链接"（hard link）。

运行上面这条命令以后，源文件与目标文件的inode号码相同，都指向同一个inode。inode信息中有一项叫做"链接数"，记录指向该inode的文件名总数，这时就会增加1。
反过来，删除一个文件名，就会使得inode节点中的"链接数"减1。当这个值减到0，表明没有文件名指向这个inode，系统就会回收这个inode号码，以及其所对应block区域。

这里顺便说一下目录文件的"链接数"。创建目录时，默认会生成两个目录项："."和".."。前者的inode号码就是当前目录的inode号码，等同于当前目录的"硬链接"；后者的inode号码就是当前目录的父目录的inode号码，等同于父目录的"硬链接"。所以，任何一个目录的"硬链接"总数，总是等于2加上它的子目录总数（含隐藏目录）。



<br/>
<br/>



### 软链接

文件A和文件B的inode号码虽然不一样，但是文件A的内容是文件B的路径。读取文件A时，系统会自动将访问者导向文件B。因此，无论打开哪一个文件，最终读取的都是文件B。这时，文件A就称为文件B的"软链接"（soft link）或者"符号链接（symbolic link）。

这意味着，文件A依赖于文件B而存在，如果删除了文件B，打开文件A就会报错："No such file or directory"。这是软链接与硬链接最大的不同：文件A指向文件B的文件名，而不是文件B的inode号码，文件B的inode"链接数"不会因此发生变化。





















<br/>
<br/>

---

<br/>
<br/>















# vim


在Linux中使用文本编辑器来编辑你的Linux参数配置文件是一件很重要的事情，因此系统管理员至少应该熟悉一种文本编辑器。

在Linux中，绝大部分的配置文件都是以ASCII(键盘上可找到)的纯文本形式。因此利用简单的文本编辑器就能修改。

ASCII（发音：/ˈæski/ ass-kee[1]，American Standard Code for Information Interchange，美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统。从[0000 0000 - 0111 1111]共128个字符。

<br/>

vi文本编辑器

- 所有的Unix Like系统都会内置vi文本编辑器，其他的文本编辑器则不一定存在
- 很多软件的编辑接口都会主动调用vi(如 crontab等命令)
- vim是vi的高级版本
	+ vim具有程序编辑的能力，可以主动以字体颜色辨别语法的正确性，方便程序设计
- 程序简单，编辑速度相当快速
- vi中的tab键所得结果与空格符不一样
- vi中，数字是很有意义的
	+ 数字通常代表重复做几次，或去到第几个的意思


<br>
<br>


## 警告信息

当我们使用vim时，vim会在当前目录下再创建一个名为filename.swp的暂存文件。

由于vim的工作被不正常中断，导致暂存盘无法通过正常流程来结束，所以暂存文件就不会消失。此时编辑文件就会出现某些异常情况。

- 可能有其他人或程序同时在编辑这个文件
- 在前一个vim的环境中，可能因为某些不明原因导致vim中断(crashed)


<br>
<br/>


## 三种模式

vim包括三种模式：

- 一般模式
- 编辑模式
- 命令行模式

![VIM](/images/vim.png)


<br/>

### 一般模式

命令 | 说明
- | -
x | 向后删除一个字符
X | 向前删除一个字符
nx,nX | 向前/后 删除n个字符
dd | 删除当前行
D | 删除当前行所有字符，使之成为空行
ndd | 删除光标所在行的向下n行
d1G | 删除光标所在行到第一行
dG | 删除光标所在行到最后一行
yy |  复制光标所在行
y1G | 复制光标所在行到第一行
yG | 复制光标所在行到最后一行
ynj | 复制光标所在行和向下n行
dnj | 删除光标所在行和向下n行
p | 将复制的数据粘贴到光标下一行
P | 将复制的数据粘贴到光标上一行
J | 将光标所在行与下一行结合成一行
u | undo,恢复前一个操作
ctrl+r | 重做上一个操作
. | 重复前一个操作


<br/>


### 编辑模式

命令 | 说明
- | -
i | 在当前光标所在处插入文字
I | 在光标所在行第一个非空字符插入文字
a | 在当前光标后插入文字
A | 在当前光标所在行最后插入文字
o | 在光标所在行的下一行行首插入字符
O | 在光标所在行的上一行行首插入字符
r | 替换光标所在那一个字符
R | 一直替换光标所指的文字，直到退出
Esc | 退出，回到一般模式


<br/>

### 命令模式

命令 | 说明
- | -
h | 方向左
j | 方向下
k | 方向上
l | 方向右
+ | 光标移到下一行的第一个非空字符
- | 光标移到当前行的第一个非空字符
0 | 光标移到当前行的第一个字符
$ | 光标移到当前行的最后一个字符
n空格 | 光标在当前行向右移动n个字符
G | 光标移到最后一行的第一个非空字符
gg | 光标移到第一行的第一个非空字符，相当于1G
nG | 光标移到第n行的第一个非空字符
/word | 在光标之后查找word字符串
?word | 在光标之前查找word字符串
n/N | 重复前一个查找
:s/word1/word2 | 在光标当前行将word1替换成word2
:n1,n2s/word1/word2/g | 在n1行-n2行间将word1替换成word2
%s/word1/word2/gc | 全局将word1替换成word2，在替换前让用户确认(confirm)
:w | 保存到文件
:w file2 | 保存到file2文件
:r file3 | 从file3文件读取数据并写入
:wq/:x | 保存并退出
:q | 退出
:q! | 强制退出
:!cmd | 执行命令
:r!cmd | 将执行命令写入
:set nu | 显示行号
:set nonu | 取消行号
:n1,n2w file4 | 将n1行-n2行的内容保存到file4文件


<br>
<br/>
<br>



## Visual模式

参考:

- <https://vimjc.com/vim-visual-mode.html>


Visual Mode下可以选择一块编辑区域，然后对选中的文件内容执行**复制**、**粘贴**、**插入**、**删除**、**替换**、**改变大小写**等操作，是vim使用过程中常用的一种模式。

在vim命令模式下，使用`v`或`V`或`ctrl+v`都可进入可视化模式。这三种模式的主要区别在于:

- `v`字符选择模式: 选中光标经过的所有字符
- `V`行选择模式: 选中光标经过的所有行
- `ctrl+v`块选择模式: 选中一整个矩形框表示的所有文本

具体细致的操作就不写了，但却是非常使用。



<br>
<br>
<br>







## 环境设置与记录


因为vim会主动将你曾经的行为记录下来，好方便下次操作。这个文件是自动生成的。

- ~/.vim.info
- ~/.vim.rc
- 整体vim设置
	+ /etc/vimrc

此外，每个Distribution对vim的默认环境都不太相同。所以你可能需要设置成你自己的工作方式。

参数 | 说明
- | -
:set nu <br> :set nonu | 行号设定
:set hlsearch <br/> :set nohlsearch | 高亮设定
:set autoindent <br/> :set noautoindent | 自动缩排设定
:set backup | 自动备份设定
:set ruler | 状态栏设定
:set showmode | 模式显示设定，如INSERT
:set backspace=(012) | 设定退格(backspace)值
:set all | 显示所有环境参数
:set | 显示与系统默认值不同的参数
:syntax on/off | 程序语法显示
:set bg=dark/light | 设定背景颜色

<br>

栗子：

```sh
vim /root/.vimrc


"这是注释"
set nu
set ruler
set bg=dark
syntax on
set hlsearch
```



<br/>
<br/>



## 注意事项


<br>

### 中文编码问题

修改语系编码： `LANG=zh_CN.utf-8`


<br>

### Linux与Dos的换行字符

- Linux的换行(Enter)为LF符号($)
- Dos的换行(Enter)为CRLF符号(^M$)

不同系统之间复制纯文本文件可能会有问题，此时可以转换：

- unix2dos file newfile
- dos2unix file newfile


<br>

### 语系编码转换

`iconv` - convert text from one character encoding to another

```sh
#iconv -f 源编码 -t 新编码 filename [-o newfile]
#-o，转换到新文件

iconf -f big4 -t utf8 old.big5 -o new.utf8
```




<br/>
<br/>
<br/>



## 使用正则


**VIM里的正则用着太爽了，不用不知道，用了非常好！请注意转义哈！**

<br>

在vim里的查找(`/, ?`)和替换(`:s, :1,ns, :%s`)功能中，使用正则可极大提高效率。

vim毕竟是个编辑器，如果直接饮用正则表达式的元字符会造成一些麻烦。所以需要对正则元字符进行转义。

vim有一个`magic`，可以设置哪些正则元字符需要转义，哪些不需要。

```shell
vim

:set magic
:set nomagic


magic (\m)：除了 $ . * ^ 之外的元字符都需要转义，也就是反斜杠(\)
/\m.*    #查找任意字符串

nomagic (\M)：除了`$ ^`之外的元字符都需要转义
/\M.*    #查找特定字符串.*
```

<br>

我不建议使用，这样使得vim使用很错乱。还是老老实实使用转义好些。



<br/>

正则表达式的元字符参考本文档的**正则表达式**章节。

栗子:

```
vim 1.txt


# \s，空格
# 删除每行开通的空格
:%s/^\s*//g


# 去掉开头的行号
11, 20s/^[0-9]\{2\}//g



# 查找
/[0-9]\{3,5\}


# 查找多个
/aaa\|bbb\|ccc
#替换多个
:%s/aaa\|bbb/HAHAHAHA/g
```



<br/>
<br/>
<br/>




## 插件


插件是一种扩展VIM功能的方法。VIM将插件分为：
- 全局插件(global)：无条件加载和操作
- 文件类型插件(filetype)：仅为特定文件类型加载和操作，参阅`vim --> :help filetype`

VIM默认会在特定位置查找插件：
- Linux/OS X： `~/.vim/plugin`
- Windows： `$HOME/vimfiles/plugin`
- 文件类型插件为`ftplugin`

<br>

插件只是VIM脚本，因此你可以使用它们来定义函数、映射和命令，就像在`.vimrc`中一样。
插件通常不仅仅是位于相应目录中的单个`.vim`文件。它们通常还包括自动加载脚本(`:help autoload`)，语法脚本(`:help syntax`)和缩进处理脚本。将这些脚本中的所有代码打包在一起，提供强大的钩子来增强VIM。

VIM內建的帮助(`:help plugin`)包含各种详细信息。有一些优秀资源:

- [Learn Vimscript the Hard Way ](http://learnvimscriptthehardway.stevelosh.com/)
- [Writing Vim Plugins](http://stevelosh.com/blog/2011/09/writing-vim-plugins/)

<br>

可使用放在适当的目录的插件并启动VIM。当然，有些插件可能有比较复杂的安装过程(比如YouCompleteMe插件)。
目前，像Pathogen和Vundle这样的插件管理器是手动安装插件文件的流行替代品，特别是因为插件通常带有多个文件。


<br/>


### vim插件管理器

当没有插件管理器时，Vim 用户必须手动下载 tarball 包形式的插件，并将它们解压到 ~/.vim 目录中。在少量插件的时候可以。但当他们安装更多的插件时，就会变得一团糟。所有插件文件分散在单个目录中，用户无法找到哪个文件属于哪个插件。此外，他们无法找到他们应该删除哪个文件来卸载插件。这时 Vim 插件管理器就可以派上用场。插件管理器将安装插件的文件保存在单独的目录中，因此管理所有插件变得非常容易。

介绍几个常见的VIM插件管理器:

- Pathogen: <https://github.com/tpope/vim-pathogen>
- Vundle: <https://github.com/VundleVim/Vundle.vim>
- Plug: <https://github.com/junegunn/vim-plug>


<br>


#### Vundle

安装Vundle前请先安装`vim`和`git`。

```bash
sudo yum install -y vim git

# 下载Vundle
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

# 配置vundle
vim ~/.vimrc

#加入以下内容
set nocompatible              " required
filetype off                  " required
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
Plugin 'gmarik/Vundle.vim'
call vundle#end()            " required
filetype plugin indent on    " required

" 设置包括vundle和初始化相关的runtime path
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" 另一种选择, 指定一个vundle安装插件的路径
"call vundle#begin('~/some/path/here')

" 让vundle管理插件版本,必须
Plugin 'VundleVim/Vundle.vim'

" 以下范例用来支持不同格式的插件安装.
" 请将安装插件的命令放在vundle#begin和vundle#end之间.
" Github上的插件
" 格式为 Plugin '用户名/插件仓库名'
Plugin 'tpope/vim-fugitive'
" 来自 http://vim-scripts.org/vim/scripts.html 的插件
" Plugin '插件名称' 实际上是 Plugin 'vim-scripts/插件仓库名' 只是此处的用户名可以省略
Plugin 'L9'
" 由Git支持但不再github上的插件仓库 Plugin 'git clone 后面的地址'
Plugin 'git://git.wincent.com/command-t.git'
" 本地的Git仓库(例如自己的插件) Plugin 'file:///+本地插件仓库绝对路径'
Plugin 'file:///home/gmarik/path/to/plugin'
" 插件在仓库的子目录中.
" 正确指定路径用以设置runtimepath. 以下范例插件在sparkup/vim目录下
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" 安装L9，如果已经安装过这个插件，可利用以下格式避免命名冲突
Plugin 'ascenator/L9', {'name': 'newL9'}

" 你的所有插件需要在下面这行之前
call vundle#end()            " 必须
filetype plugin indent on    " 必须 加载vim自带和插件相应的语法和文件类型相关脚本
" 忽视插件改变缩进,可以使用以下替代:
"filetype plugin on
"
" 简要帮助文档
" :PluginList       - 列出所有已配置的插件
" :PluginInstall    - 安装插件,追加 `!` 用以更新或使用 :PluginUpdate
" :PluginSearch foo - 搜索 foo ; 追加 `!` 清除本地缓存
" :PluginClean      - 清除未使用插件,需要确认; 追加 `!` 自动批准移除未使用插件
"
" 查阅 :h vundle 获取更多细节和wiki以及FAQ
" 将你自己对非插件片段放在这行之后


# 安装插件
# vim中
:PluginInstall

# bash
vim _PluginInstall +qall



# 查找插件
# vim
:PluginSearch
# 要从vimscripts网站刷新本地的列表
:PluginSearch!


# 查看已安装插件
# vim
:PluginList


# 更新插件
# vim
:PluginUpdate
```


<br/>
<br/>


#### Plug

它是一个速度非常快的、极简的 vim 插件管理器。它可以并行地安装或更新插件。你还可以回滚更新。

```bash
# 下载安装
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim


# 配置
vim ~/.vimrc
# 以 call plug#begin(PLUGIN_DIRECTORY) 开始，并以 call plug#end() 结束
# 栗子
" Specify a directory for plugins
" - For Neovim: ~/.local/share/nvim/plugged
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

" Make sure you use single quotes

" Shorthand notation; fetches https://github.com/junegunn/vim-easy-align
Plug 'junegunn/vim-easy-align'

" Any valid git URL is allowed
Plug 'https://github.com/junegunn/vim-github-dashboard.git'

" Multiple Plug commands can be written in a single line using | separators
Plug 'SirVer/ultisnips' | Plug 'honza/vim-snippets'

" On-demand loading
Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle' }
Plug 'tpope/vim-fireplace', { 'for': 'clojure' }

" Using a non-master branch
Plug 'rdnetto/YCM-Generator', { 'branch': 'stable' }

" Using a tagged release; wildcard allowed (requires git 1.9.2 or above)
Plug 'fatih/vim-go', { 'tag': '*' }

" Plugin options
Plug 'nsf/gocode', { 'tag': 'v.20150303', 'rtp': 'vim' }

" Plugin outside ~/.vim/plugged with post-update hook
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }

" Unmanaged plugin (manually installed and updated)
Plug '~/my-prototype-plugin'

" Initialize plugin system
call plug#end()



# 重载.vimrc以使用vim
source ~/.vimrc
# vim
:PlugInstall
:PlugUpdate
:PlugClean


# 升级plug
# vim
:PlugUpgrade
```


<br/>
<br/>
<br/>



### vim常见插件

- [NERDTree](https://github.com/scrooloose/nerdtree)： 文档树
- [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)： 代码补全
- [Rainbow](https://github.com/luochen1990/rainbow)： 彩虹括号
- [UndoTree](https://github.com/mbbill/undotree)： 关闭vim后也可代码撤回
- [vim-gitgutter](https://github.com/airblade/vim-gitgutter)：显示git信息
- [ctrlp](https://github.com/kien/ctrlp.vim)：
- [tagbar](https://github.com/majutsushi/tagbar)：浏览当前文件的标签并获得其结构的概述




<br/>
<br/>
<br/>




## k-vim

k-vim: <https://github.com/wklken/k-vim>


k-vim是一份很好的vim配置，我个人使用的是这个配置。


<br/>
<br/>


### 代码折叠

vim支持多种代码折叠:

- manual: 手工定义折叠
- indent: 用缩进表示折叠
- expr: 用表达式来定义折叠
- syntax: 用语法高亮来定义折叠
- diff: 对没有更改的文本进行折叠
- marker: 用标志折叠

<br>

k-vim里配置的折叠方法是`indent`。此缩进方法的操作如下：

- `za`: 折叠缩进处的代码
- `,zz`: k-vim配置的`za`
- `zM`: 关闭所有的折叠
- `zR`: 打开所有的折叠



<br/>
<br/>
<br/>



## 调试器

介绍vim下常用的代码调试器(debugger)。


<br>

### GDB

- 官网: <https://www.gnu.org/software/gdb/>
- 教程: <https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/gdb.html>

GDB(GNU debugger)，是GNU软件系统中的标准调试器。支持C、C++等。


<br/>
<br/>


### pdb和ipdb

Pythone调试器:

- pdb: Python内建的调试器，用法与gdb一样。
- ipdb: 一个开源的Python调试器，它和pdb有相同的接口，但是，它相对于pdb，具有语法高亮、tab补全、更友好的堆栈信息等高级功能。

pdb是python的标准库，无需安装就可以使用。而ipdb是一个第三方库，需要使用pip安装。

这两个调试器有两种使用方法：

- 一是集成在源代码中加入断点，但需要修改源码，麻烦。

```py
import ipdb

xxx
ipdb.set_strace()
```

- 二是交互式命令调试

```sh
python3 -m ipdb xxx.py
ipdb>

ipdb> help

Documented commands (type help <topic>):
========================================
EOF    cl         disable  interact  next    psource  rv         unt
a      clear      display  j         p       q        s          until
alias  commands   down     jump      pdef    quit     source     up
args   condition  enable   l         pdoc    r        step       w
b      cont       exit     list      pfile   restart  tbreak     whatis
break  continue   h        ll        pinfo   return   u          where
bt     d          help     longlist  pinfo2  retval   unalias
c      debug      ignore   n         pp      run      undisplay

Miscellaneous help topics:
==========================
exec  pdb
```

<br>

**ipdb常用命令**

| 命令 | 描述 |
| - | - |
| b/break | 设置断点 |
| tbreak | 设置临时断点 |
| cl/clear | 清除断点 |
| c/continue | 继续执行程序 |
| l/list | 查看指定的代码行 |
| ll/longlist | 查看整个代码 |
| s/step | 执行会进入函数 |
| n/next | 执行不会进入函数 |
| a | 可列出当前函数的参数 |
| r/return | 执行代码直到从当前函数返回 |
| j | 忽略某段代码 |
| pp | 打印表达式的值 |
| run/restart | 重启，重启后断点、设置等会保留 |
| q/quie/exit | 中止并退出 |
| h/help | 帮助 |

<br>

栗子：

```
python3 -m ipdb xxx.py
# 在第3行设置断点
ipdb> b 3

# 执行到断点处
ipdb> c

# 查看2-7行代码
ipdb> l 2,7
```




<br/>
<br/>

---

<br/>
<br/>








# 编码


参考：

- ASCII: <https://zh.wikipedia.org/wiki/ASCII>
- Unicode: <https://zh.wikipedia.org/wiki/Unicode>


<br>
<br>


## Unicode

计算机处理的是数字(二进制文件)。他们在存储字符时要给每个字符分配一个数值。

![主要编码使用情况](/images/Utf8webgrowth.png)

<br>

早期的编码系统称为 ASCII（美国信息交换标准码）， 一共有128（0-127）个值，每个值用7bit 保存。ASCII可以满足小写、大写、数字标点符号和一些控制字符的处理。

人们曾尝试将ASCII字符扩展到8bit，这种新的被称为“扩充ASCII”的编码一直没有成为国际性标准。

<br>

为了克服ASCII和扩充ASCII先天上的不足，Unicode Consortiun（多语言软件生产商群体）创建了一种能够提供广泛字符集的通用编码系统，称为**Unicode**。

Unicode最初设置为2Byte的字符集。但版本3的Unicode用的是4Byte编码，并且与ASCII与扩充的ASCII完全兼容。

现在被称为Basic Latin（基本拉丁文）的ASCII字符集就是前25位全部置零的Unicode码。
现在被称为 Latin-1（拉丁文1）的扩充ASCII字符集就是前24位全部置零的Unicode码。

![Unicode兼容性](/images/unicode.png)

<br>

Unicode中的每个字符或符号由一个32bit数来定义，因此这种编码可以定义高达2的32次方(4 294 067 296)个字符或符号。
它的记法使用了十六进制数字，格式如下：

```
U-XXXXXXXX

#每个 X 都是一个十六进制的数字
#因此，它的数值从U-00000000到U-FFFFFFFF
```


<br>
<br>


## ASCII

美国信息交换码（American Standard Code of Information Internet，ASCII）是一种7bit码，设计来为128个大多数是美国英语里使用的符号提供编码。
今天的ASCII码已成为Unicode的一部分，它占据了Unicode中的前128个码（00000000-0000007F）。

ASCII的一些特点：

- space(20-sp)字符，是一个可打印的字符，打印出一个空格
- 大写字母从(41-A)开始，小写字母从(61-a)开始。按ASCII比较时，大写字母的数值会小于小写字母
- 大写字母与小写字母在他们的7bit编码中只有1bit不同，A(1000001)，a(1100001)，两者相差(20)十六进制
- 小写字母并没有紧跟在大写字母后面，这两者之间还有几个标点符号(5B-60)
- 数字从(30-0)开始
- 从00到1F这最开始的32个字符加上最后一个字符(7F)全都是非打印字符。字符(00)被用作定界符，已定义字符串的结束。字符(7F)是删除字符，它被某些编程语言用来删除前一个字符。剩下的非打印字符称为控制字符，用于数据通信

<br>

**ASCII控制字符：**

二进制 | 十进制 | 十六进制 | 缩写 | Unicode<br>表示法 | 脱出字符<br>表示法 | 名称/意义
- | - | - | - | - | - | -
0000 0000 | 0 | 00 | NUL | ␀ | ^@ | 空字符（Null）
0000 0001 | 1 | 01 | SOH | ␁ | ^A | 标题开始
0000 0010 | 2 | 02 | STX |␂	| ^B	| 本文开始
0000 0011 | 3 | 03 | ETX | ␃ | ^C	| 本文结束
0000 0100 | 4 | 04 | EOT | ␄ | ^D	| 传输结束
0000 0101 | 5 | 05 | ENQ | ␅ | ^E	| 请求
0000 0110 | 6 | 06 | ACK | ␆ | ^F	| 确认回应
0000 0111 | 7 | 07 | BEL | ␇ | ^G	| 响铃
0000 1000 | 8 | 08 | BS	| ␈	| ^H	| 退格
0000 1001 | 9 | 09 | HT | ␉	| ^I	| 水平定位符号
0000 1010 | 10 | 0A	| LF | ␊ | ^J	| 换行键
0000 1011 | 11 | 0B	| VT | ␋ | ^K	| 垂直定位符号
0000 1100 | 12 | 0C	| FF | ␌ | ^L	| 换页键
0000 1101 | 13 | 0D	| CR | ␍ | ^M	| Enter键
0000 1110 | 14 | 0E	| SO | ␎ | ^N	| 取消变换（Shift out）
0000 1111 | 15 | 0F	| SI | ␏ | ^O	| 启用变换（Shift in）
0001 0000 | 16 | 10	| DLE | ␐ | ^P	| 跳出数据通讯
0001 0001 | 17 | 11	| DC1 | ␑ | ^Q	| 设备控制一（XON 激活软件速度控制）
0001 0010 | 18 | 12	| DC2 | ␒ | ^R	| 设备控制二
0001 0011 | 19 | 13	| DC3 | ␓ | ^S	| 设备控制三（XOFF 停用软件速度控制）
0001 0100 | 20 | 14	| DC4 | ␔ | ^T | 设备控制四
0001 0101 | 21 | 15	| NAK | ␕ | ^U	| 确认失败回应
0001 0110 | 22 | 16	| SYN | ␖ | ^V | 同步用暂停
0001 0111 | 23 | 17	| ETB | ␗ | ^W | 区块传输结束
0001 1000 | 24 | 18	| CAN | ␘ | ^X | 取消
0001 1001 | 25 | 19	| EM | ␙ | ^Y | 连接介质中断
0001 1010 | 26 | 1A	| SUB | ␚ | ^Z | 替换
0001 1011 | 27 | 1B	| ESC | ␛ | ^[ | 退出键
0001 1100 | 28 | 1C	| FS | ␜ | ^\ | 文件分区符
0001 1101 | 29 | 1D	| GS | ␝ | ^] | 组群分隔符
0001 1110 | 30 | 1E	| RS | ␞ | ^^ | 记录分隔符
0001 1111 | 31 | 1F	| US | ␟ | ^_ | 单元分隔符
0111 1111 | 127 | 7F | DEL| ␡ | ^? | 删除


<br>

**ASCII可显示字符:**

进制 | 十进制 | 十六进制 | 图形
- | - | - | -
0010 0000 | 32 | 20 | (space)
0010 0001 | 33 | 21 | !
0010 0010 | 34 | 22 | "
0010 0011 | 35 | 23 | #
0010 0100 | 36 | 24 | $
0010 0101 | 37 | 25 | %
0010 0110 | 38 | 26 | &
0010 0111 | 39 | 27 | '
0010 1000 | 40 | 28 | (
0010 1001 | 41 | 29 | )
0010 1010 | 42 | 2A | *
0010 1011 | 43 | 2B | +
0010 1100 | 44 | 2C | ,
0010 1101 | 45 | 2D | -
0010 1110 | 46 | 2E | .
0010 1111 | 47 | 2F | /
0011 0000 | 48 | 30 | 0
0011 0001 | 49 | 31 | 1
0011 0010 | 50 | 32 | 2
0011 0011 | 51 | 33 | 3
0011 0100 | 52 | 34 | 4
0011 0101 | 53 | 35 | 5
0011 0110 | 54 | 36 | 6
0011 0111 | 55 | 37 | 7
0011 1000 | 56 | 38 | 8
0011 1001 | 57 | 39 | 9
0011 1010 | 58 | 3A | :
0011 1011 | 59 | 3B | ;
0011 1100 | 60 | 3C | <
0011 1101 | 61 | 3D | =
0011 1110 | 62 | 3E | >
0011 1111 | 63 | 3F | ?
0100 0000 | 64 | 40 | @
0100 0001 | 65 | 41 | A
0100 0010 | 66 | 42 | B
0100 0011 | 67 | 43 | C
0100 0100 | 68 | 44 | D
0100 0101 | 69 | 45 | E
0100 0110 | 70 | 46 | F
0100 0111 | 71 | 47 | G
0100 1000 | 72 | 48 | H
0100 1001 | 73 | 49 | I
0100 1010 | 74 | 4A | J
0100 1011 | 75 | 4B | K
0100 1100 | 76 | 4C | L
0100 1101 | 77 | 4D | M
0100 1110 | 78 | 4E | N
0100 1111 | 79 | 4F | O
0101 0000 | 80 | 50 | P
0101 0001 | 81 | 51 | Q
0101 0010 | 82 | 52 | R
0101 0011 | 83 | 53 | S
0101 0100 | 84 | 54 | T
0101 0101 | 85 | 55 | U
0101 0110 | 86 | 56 | V
0101 0111 | 87 | 57 | W
0101 1000 | 88 | 58 | X
0101 1001 | 89 | 59 | Y
0101 1010 | 90 | 5A | Z
0101 1011 | 91 | 5B | [
0101 1100 | 92 | 5C | \
0101 1101 | 93 | 5D | ]
0101 1110 | 94 | 5E | ^
0101 1111 | 95 | 5F | _
0110 0000 | 96 | 60 | `
0110 0001 | 97 | 61 | a
0110 0010 | 98 | 62 | b
0110 0011 | 99 | 63 | c
0110 0100 | 100 | 64 | d
0110 0101 | 101 | 65 | e
0110 0110 | 102 | 66 | f
0110 0111 | 103 | 67 | g
0110 1000 | 104 | 68 | h
0110 1001 | 105 | 69 | i
0110 1010 | 106 | 6A | j
0110 1011 | 107 | 6B | k
0110 1100 | 108 | 6C | l
0110 1101 | 109 | 6D | m
0110 1110 | 110 | 6E | n
0110 1111 | 111 | 6F | o
0111 0000 | 112 | 70 | p
0111 0001 | 113 | 71 | q
0111 0010 | 114 | 72 | r
0111 0011 | 115 | 73 | s
0111 0100 | 116 | 74 | t
0111 0101 | 117 | 75 | u
0111 0110 | 118 | 76 | v
0111 0111 | 119 | 77 | w
0111 1000 | 120 | 78 | x
0111 1001 | 121 | 79 | y
0111 1010 | 122 | 7A | z
0111 1011 | 123 | 7B | {
0111 1100 | 124 | 7C | l(管道线)
0111 1101 | 125 | 7D | }
0111 1110 | 126 | 7E | ~

ASCII缺点：
ASCII的局限在于只能显示26个基本拉丁字母、阿拉伯数目字和英式标点符号。因此现在的软件系统大多采用Unicode。


<br/>
<br/>


## UTF-8

UTF-8（8-bit Unicode Transformation Format）是一种针对Unicode的可变长度字符编码，也是一种前缀码。它可以用来表示Unicode标准中的任何字符，且其编码中的第一个字节仍与ASCII兼容，这使得原来处理ASCII字符的软件无须或只须做少部分修改，即可继续使用。

UTF-8最大的一个特点，就是它是一种变长的编码方式。它可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。

UTF-8的编码规则很简单，只有二条:

1. 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的
2. 对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的unicode码




<br/>
<br>

---

<br/>



# Bash


<br>


## bash与shell


管理整个计算机硬件的其实是操作系统的内核(kernel)。这个内核是需要被保护的，所以一般用户就只能通过shell来跟内核通信，让内核达到我们想要达到的工作。


<br>

### 硬件、内核与shell

我们必须通过shell，将我们输入的命令与内核通信，让内核可以控制硬件正确无误的工作。

操作系统其实是一组软件。由于这组软件在控制整个硬件与管理系统的活动监测，如果这组软件能被用户随意操作，若用户应用不当，将会使得整个系统奔溃。因为操纵系统管理的就是整个硬件功能，所以当然不能够被随便一些没有管理能力的终端用户随意使用。
但我们总是需要让用户操作系统的，所以就有了在操作系统上面发展的应用程序。用户可以通过应用程序来指挥内核，让内核达到我们所需要的硬件任务。

也就是说，只要能够操作应用程序的接口都能够称为shell。
狭义的shell指的是命令行方面的软件，包括bash等。广义的shell则包括图形界面的软件。


<br/>

### 命令行shell

- 各Distribution的命令行界面都一样
- 远程管理非常快速
- Linux的任督二脉


<br/>

### 系统合法shell与/etc/shells

由于shell依据发布者的不同就有许多版本，例如Bourne SHell（sh）、C SHell、K SHell、TCSH等。

Linux默认使用的这一版本就是**Bourne Again SHell(bash)**，这个shell是Bourne SHell的增强版，也是基于GNU框架下发展出来的。

检查系统可用shell: `cat /etc/shells`
合法shell要写入`/etc/shells`，系统某些服务在运行过程中，会去检查用户能够使用的shell。

查看用户shell权限： `cat /etc/passwd`，最后一行便是默认shell。


<br>

### bash shell

bash是GNU计划中重要的工具软件之一，目前也是Linux distributions 的标准shell。
bash主要兼容于sh，并且依据一些用户的需求而加强shell 版本。

bash的优点：

- 命令记忆能力`history`
- 命令与文件补全功能`tab`
- 命令别名设置功能`alias`
- 作业控制、前台、后台控制(job control, foreground, background)
- 使用前台、后台的控制可以让作业进行得更为顺利。至于作业控制(jobs)的用途更广，可以让我们随时将工作丢到后台中执行，而不怕不小心使用`ctrl+c`来中断该进程
- 程序脚本`shell  script`
- 通配符(Wildcard)


<br>


### type命令

`type`命令用于判断一个命令是內建命令还是外部命令(非bash提供)。

```sh
type ls


type -t ls
#file，外部命令
#alias，别名
#builtin，內建命令
```


<br>
<br>


## shell变量


变量就是以一组文字或符号等，来替代一些设置或者是一串保留的数据。


<br>

### 变量显示与设置

- `echo`: 显示变量
	+ `echo $PATH`
- `unset`: 取消变量
	+ `unset $ZHANG`


<br>

### 变量设置规则

- 变量与变量内容以一个等号`=`连接，如`myname=zhang`
- 等号两边不能有空格符，否则错误
- 变量名称只能是英文字母和数字，开头字符不能是数字
- 变量内容若有空格，可使用双引号或单引号
- 双引号内的特殊符号，保有原本的特性
- 单引号内的特殊字符则仅为一般字符
- 转义字符`\\`，将特殊符号变成一般字符
- 在一串命令中，还需要使用其他命令，使用反单引号
	+  反引号\`\`内的命令将被优先执行，而其执行结果将作为外部的输入信息
- 若该变量为了增加变量内容时，可用**$变量名称 **或**${变量}**累加内容
	+ `myname=${myname}xxx`
- 若该变量需要在其他子进程执行，请以`export`来使变量变成环境变量
- 通常大写字符为系统默认变量，自行设置变量可以使用小写字符，方便判断


什么是子进程？在我目前这个shell下，去打开另一个新的shell。新的那个shell就是子进程。
在一般状态下，父进程定义的变量是无法在子进程内使用的，要通过`export`将变量变成环境变量后才可以。

<br>

注意**单引号、双引号和反引号**。


<br>

### 环境变量

环境变量可以帮我们达到很多功能，包括主文件夹的变换、提示符的显示、执行文件查找的路径等。

- `env`: 显示环境变量
-  `set`: 查看所有变量
	+ 包括环境变量和自定义变量



```
#HOME，用户主目录
#SHELL，当前环境使用的shell
#HISTSIZE，历史命令
#PATH，执行文件查找路径
#LANG，语系
#$PS1，命令提示符
#PS2，第二行提示符
```

设置$PS1，$PS2:

```
\d		#可显示出『星期 月 日』的日期格式，如："Mon Feb 2"

\H		#完整的主机名

\h		#仅取主机名在第一个小数点之前的名字

\t		#显示时间，24小时格式的『HH:MM:SS』

\T		#显示时间，为12小时格式的『HH:MM:SS』

\A		#显示时间，为24小时格式的『HH:MM』

\@		#显示时间，为12小时格式的『am/pm』

\u		#目前使用者的账号名称，如『root』

\v		#BASH的版本信息

\w		#完整工作路径名，由根目录写起的目录名称。但家目录会以 ~ 取代

\W		#利用basename函数取得工作目录名称，所以仅会列出最后一个目录名。

\#		#下达的第几个命令

\$		#提示字符，root时，提示字符为#；否则就是$
```

<br>

`$`钱字号本身也是变量，代表当前shell的PID --> `echo $$`

`?`问号也是一个特殊变量，代表上一个运行命令的回传值 --> `echo $?`

- 0
	+ 命令运行成功
- errorcode
	+ 命令运行错误


<br>

### 语系变量

`locale` - get locale-specific information.

设置LANG的时候，其他的语系变量就会被这个变量所替代。


<br>

### 变量键盘读取、数组与声明

- `read`： 读取来自键盘输入的变量
- `declare`,`typeset`: 声明变量类型
	+ 变量的默认类型为字符串
	+ 若不指定变量类型，则`1+2`就是一个字符串而不是计算式
- 数组变量类型
	+ var[1]='varray1'
	+ var[2]='varray2'
	+ echo "${${var[1]}, ${var[2]}}"


<br>
<br/>


## bash shell操作环境


自定义我们登录主机的时候屏幕上面会有一些说明文字，并且登录的时候还可以给用户提供一些信息或者欢迎文字，或环境变量和命令别名等。


<br>

### 路径与命令查找顺序

命令的运行顺序：

1. 以绝对/相对路径执行命令
2. 由alias找到该命令来执行
3. 由bash内置的（builtin）命令来执行
4. 通过$PATH这个变量的顺序找到的第一个命令来执行


<br>

### bash登录与欢迎消息

- `/etc/issue` --> 终端登录消息
	+ CentOS Linux 7 (core).....
- `/etc/motd` --> 用户登录后取得一些消息
	+ Welcome to aliyun ECS


<br/>

### bash环境配置文件

操作系统有一些环境配置文件的存在，让bash在启动时直接读取这些配置文件，以规划好bash的操作环境。
这些配置文件又可以分为全体系统的配置文件以及用户个人偏好配置文件。

命令别名、自定义的变量在你注销bash后就会失效。所以你想要保留你的设置，就得要将这些设置写入配置文件才行。

- login shell
	+ 取得bash需要完整的登录流程
- non-login shell
	+ 取得bash接口的方法不需要登录


<br>

### bash shell快捷键

- `Ctrl+C` --> 终止当前命令
- `Ctri+D` --> 输入结束(EOF)
- `Ctri+M` --> Enter
- `Ctrl+S` --> 暂停屏幕输出
- `Ctrl+Q` --> 恢复屏幕输出
- `Ctrl+U` --> 在提示字符下，将整列命令删除
- `Ctrl+Z` --> 暂停目前命令


<br>

### 通配符与特殊符号

通配符：

符号 | 说明
- | -
* | 代表0-∞个 任意字符
? | 代表一定有一个 任意字符
[-] | 中括号内任一字符
[^] | 非中括号内字符


bash常见特殊符号，理论上文件名不要用到上述字符。

符号 | 说明
- | -
# | 注释
\ | 转义字符
1 | 管道线
; | 连续命令分隔符
~ | 用户主目录
$ | 取变量前导符
& | 将命令放入后台
! | 逻辑非
/ | 目录符号
>, >> | 输出定向
<, << | 输入定向
'' | 单引号
"" |　双引号
() | 子shell
{} | 命令区块混合


<br/>
<br>


## 重定向


数据流重定向就是将某个命令执行后应该要出现在屏幕上的数据传输到其他的地方，如文件或设备。

- 标准输入(stdin)，代码为0，使用`<`或者`<<`
- 标准输出(stdout)，代码为1，使用`>`或者`>>`
- 标准错误(stderr)，代码为2，使用`2>`或者`2>>`
- `>`表示以覆盖方式写入，`>>`表示以追加方式写入


<br>
<br/>


## 管道


管道命令使用 " | " 这个界定符号。
管道命令" | " 仅能处理经由前面一个命令传来的正确信息。所以对stderror没有直接处理能力。

在每个管道后面接的第一个数据必定是命令，而且这个命令必须要能够接收standard input的数据才行，这样的命令才可以是管道命令。




<br>
<br>

---

<br/>



# Bash特殊符号



在编写shellscripts的时候，特殊符号也有其重要的功能。

<br>

符号  | 描述  | 栗子
-  | -  | -
#!  |  shellban，申明脚本所使用的shell  | #!/bin/bash
\  | 转义字符  | \n
l  | 管道  | stdout l grep
>,>>  | 输出定向  | > 1.txt
<,<<  | 输入定向  | < 1.txt
2>  | 错误定向  | 2> error.txt
;  | 连续命令分隔符  | cmd1;cmd2
&&  | 与，只有当前命令完成后才执行后一个命令  | cmd1 && cmd2
ll  | 或，或此或彼  | cmd1 ll cmd2
~ | 用户家目录  | cd ~
#  | 注释符  | #comments
$  | 取用变量前导符 |  `$PATH`或`${PATH}`
&  | 工作控制，将命令放入后台(bg)  | command&
* <br> ? <br/> [] <br/> [-] <br> [^]  | 通配符  | *.sh <br> ?.sh <br/> [a-z]*.txt <br/> [^zhang].txt
!  | 逻辑非  | !=
= 两边无空格  | 赋值符号  | `name=zhang`
= 两边有空格  | 比较符号  | `if  [ $name  =  zhang ]`
`$0` | 执行文件脚本名  | `/root/zhang.sh`
`$1`, `$2` | 第1,2个...变量  | `./zhang.sh start`
`$#`  | 参数个数  | `if [ $# -ne 2 ]；then echo 'Usage: $0 arg1 arg2'`
`$@`  | 代表`$1`,`$2`,`$3`...之意 | 每个变量是独立的
`$*`  | 代表`$1`c`$2`c`$3`...之意  | c为分割字符，默认为空格键
`$?`  | 命令状态码，成功为0  | `$?`
`$$`  | 当前shell的PID  | `echo $$`
'单引号'  |  单引号内特殊字符仅为一般字符  | `echo '$host'--$host`
"双引号"  | 双引号内特殊符号，可保有原本特性  | `echo "$host" --localhost`
\`反引号\`  | 运行命令 | 反引号内命令先执行
()  | 以子shell方式执行  | $(date)
{}  | 命令区块的组合  |
PS1  | 命令提示符  | $PS1
PS2  | 第二行以后的提示字符  | $PS2
`shift`  | 移动参数  | shift后面可以接数字，代表拿掉最前面的几个参数
`set`  |  查看所有变量  | `set`
`unset`  | 取消变量  | `unset name`，没有$符号
`export`  | 使某变量成为环境变量  | `export name`，没有$符号
`source` | source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录 | `source file`




<br/>
<br>

---

<br/>



# shell script



shell script 有点像早期的批处理程序，即将一些命令汇整起来一次执行.但shell script拥有更强大的功能，可以进行类似程序(program)的编写，并且不需要经过编译(compile)就能执行。


<br>
<br/>


## shell script介绍


shell script是利用shell的功能写的一个程序(program)。这个程序是使用纯文本文件，将一些shell的语法与命令(含外部命令)写在里面，搭配正则表达式、命令管道与数据流重定向等功能，还提供了数组、循环、条件与逻辑判断等重要功能， 以达到我们所想要的处理目的。

shell script用在系统管理上面是很好的一项工具，但用在处理大量数值运算上就不够好。因为shell script的速度较慢，且使用的cpu资源较多，造成主机资源的分配不良。

<br>

使用shell script的优势：

- 自动化管理的重要依据
- 追踪与管理系统的重要工具
- 简单入侵检测功能
- 连续命令单一化
- 简单的数据处理
- 跨平台支持与学习历程较短

shell script注意事项：

- 命令的执行是从上到下从左到右，分析与执行
- 命令的执行中：命令、参数间的多个空白都会被忽略掉
- 空白行也将被忽略，tab按键所得的空白同样视为空格键
- 读取到一个Enter符号(CR)，就尝试开始执行该行命令
- 一行内容太多，则可以使用`\[Enter]`来扩展到下一行
- 任何加在#后面的内容都将被视为注释而被忽略

shell script文件的执行方式：

- 直接命令执行
	+ `.sh`文件必须具有可读和可执行权限，使用绝对路径或相对路径来执行
- 以bash进程来执行
	+ `bash xx.sh`
	+ `sh xx.sh`

shell script执行方式的区别：

- 直接执行，script是在子进程的bash中执行的。当子进程完成后，子进程内的各项变量或操作将会结束而不会传回到父进程中。
- source来执行，在父进程中执行


<br/>
<br>


## 编写一个shell script


一个良好的shell script应该纪录好如下信息：

- script的功能
- script的版本信息
- script的作者
- script的版权声明方式
- script的History（历史记录）
- script内较特殊的命令，使用绝对路径的方式来执行
- script执行时需要的环境变量预先声明与设置
- 在较为特殊的程序代码部分，建议务必要加上批注说明



<br>
<br/>


## shell script判断式


当我要检测系统上某些文件或相关属性时，使用`test`命令。

```sh
test -e /root/test.txt && echo 'Exist' || 'Not exist'
```


<br/>

**文件类型判断**：

选项 | 说明
- | -
-e | 是否存在
-f | 是否存在文件
-d | 是否存在目录
-b | 是否存在block device
-c | 是否存在character device
-S | 是否存在Socket文件
-p | 是否存在pipe文件
-L | 是否存在链接文件

<br/>

**文件权限判断**：

选项 | 说明
- | -
-r | 是否可读
-w | 是否可写
-x | 是否可执行
-u | 是否具有SUID
-g | 是够具有SGID
-k | 是否具有Sticky bit
-s | 是否为非空白文件

<br/>

**文件之间的比较**：

选项 | 说明
- | -
-nt | newer than
-ot | old than
-ef | 是否为同一个文件

<br>

**整数之间的比较**：

选项 | 说明
- | -
-eq | equal
-ne | not equal
-gt | greater than
-lt | less than
-ge | greater or equal
-le | less or equal

<br/>

**字符串之间的比较**：

选项 | 说明
- | -
-z | 是否为空
-n | 非空
str1 = str2 | 是否相等
!= | 不等于

<br>

**多重条件判断**：

选项 | 说明
- | -
-a | and
-o | or
! | 非

<br/>

**判断符号[]**:

如果需要在bash中使用中括号来作为shell的判断式时，必须要注意中括号的两端需要有空格符来分隔。

- 中括号内的变量，每个最好都用双引号括起来
- 中括号内的常量，最好都以单或双引号括起来


<br/>
<br>


## shell script的默认变量: $0,$1...


```
/root/test.sh opt1 opt2 opt3
	$0	$1	$2	$3
```

- 执行文件的脚本名就是$0
- 文件后接的第一个参数就是$1，以此类推
- $#，表示参数个数
- $@，表示"$1", "$2"...
- `shift`，参数变量号码偏移
	+ `shift n`，代表拿掉前面几个参数的意思



<br>
<br/>


## 条件判断语句


<br>

### if...then语句

if...then 是最常见的条件判断式。


单层条件判断：

```sh
if [ confition ]; then
	xxx
fi
```

<br>

多层条件判断：

```sh
if [ condition ]; then
	xxx;
else
	xxx;
fi
```

```sh
if [ confition1 ]; then
	xxx;
elif [ condition2 ]; then
	xxx;
else
	xxx;
fi
```


<br>

### case...esac语句

有多个既定变量内容，那么只需要针对这几个变量来设置状况就好。

```sh
case $变量名 in
"$var1")
	xxx
    ;;
"$var2")
	xxx
    ;;
*)
	xxx
    ;;
esac



####栗子
#/etc/init.d/network

case "$1" in
start)
	xxx
	;;
stop)
	xxx
    ;;
restart)
	xxx
    ;;
status)
	xxx
    ;;
esac
```

<br>


### function功能

什么是函数？函数可以在shell script 当中做出一个类似自定义执行命令的东西。最大的动能是，可以简化很多的程序代码。

因为shell script的执行方式是由上而下、由左而右。因此在shell script当中，function的定义一定要在程序的最前面，这样才能够在执行时被找到可用的程序段。

```sh
vim func.sh



function fname () {

}


####栗子
function Zhang() {
	echo $1 $2
}

Zhang "$1" "$2"



#执行
sh func.sh aaa bbb
```


<br>
<br>


## 循环(loop)语句


<br>


### while do done(不定循环)

`while`是当condition条件成立时，就进行循环，condition条件不成立就停止。

```sh
while [ condition1 ]
do
	xxx
done
```


<br/>

### until do done(不定循环)

`until`是当condition条件成立时，终止循环；否则就持续进行循环的循环。

```sh
until [ condition ]
do
	xxx
done
```


<br/>

### for do done(固定循环)

```sh
for i in con1 con2 con3 ...
do
	xxx
done


####栗子
for i in 192.168.1.{1,2,3}
do
	ping -c 1 $i
done
```

<br/>

`for do done`的数值处理：

```sh
for ((初始值;限制值；步长))
do
	xxx
done


####栗子
for ((i=0;i<10;i++))
do
	echo $i
done
```


<br>
<br/>


## shell script的追踪与调试(debug)

最好在shell script执行之前先行调试。

```sh
sh [-nvx] xxx.sh


#-v 运行脚本前，先将脚本内容输入到屏幕
#-n 仅查询语法问题
#-x 边显示边执行

当然也可以把这几个调试参数写到shellbang中
#!/bin/bash -x
```




<br/>
<br/>

---

<br>



# 小试牛刀

## 简介

```sh
#bash(Bourne Again Shell)，shell环境使得用户能与操作系统的内核进行交互操作

#!/bin/bash
#date
#description

cmd1; cmd2
cmd3


#sh /path/xx.sh
#Bash还有一个历史记录文件 ~/.bash_history
```


<br/>

## 终端打印(`echo`)

```sh
#终端作为交互式工具，用户可以通过它与shell环境进行交互

echo '$var'
echo $var
echo -e "1\t2\t3"
echo -e '\e[1;31m Red color \e[0m'    #彩色


echo {1..10}		#输出1到10
echo {A..H}		#for i in {a..z}


cat << EOF
11
22
33
EOF


# \转义字符
printf "%-5s %-10s $-4.2f\n" 001 Zhang 56.789
#格式替代符%s %d %c %f, -左对齐
```


<br>

## 玩转变量和环境变量

```sh
#Bash中，每一个变量默认值值都是字符串形式
#环境变量和自定义变量

echo $SHELL
echo $UID


var=value	#这是赋值
#var = value这是相等操作
echo $var
echo ${var}
echo ${#var}	#字符数


#export用来设置环境变量，此后，任何shell中的程序都会继承环境变量
ZHANG=Gentleman
export ZHANG


PATH="$PATH:/home/zhang/bin"
export $PATH
```


<br/>

## 通过shell进行数学运算

```sh
#let, expr, bc, [], (())
#要注意默认是字符串类型哦

n1=1;n2=2
let sum=n1+n2
let n1++;let n2-=1

sum=$[ n1 + n2 ]
sum2=$(( sum + 3 ))

sum=`expr 3 + 4`


#浮点计算 bc
echo "8 * 1.1" | bc


#设置小数点精度
echo "scale=2; 3/8" | bc


#进制转换
num=100
echo "obase=2; $num" | bc

num=1100100
echo "obase=10; ibase=2; $num" | bc


#平方和平方根
echo "sqrt(100)" | bc
echo "10^2" | bc
```


<br>

## 文件描述符重定向

```sh
#最常用的文件描述符是 stdin(0), stdout(1), stderr(2); 通过内容过滤将输出重定向到文件

echo "This is a sample text 1" > temp.txt	#覆盖
echo "This is sample text 2" >> temp.txt	#追加

ls + >stdout.txt 2>stderr.txt
cmd 2>&1 /dev/null  == com &> /dev/null		#null设备也被称为黑洞


#当一个command发生错误并退回时，它会返回一个非0的状态码
echo $?


#tee命令，一方面可将数据重定向到文件，另一方面还可提供一份重定向数据的副本作为后续命令的stdin
#tee默认覆盖文件，-a选项追加
cat temp.txt | tee tee.txt | cat -n
```


<br/>

## 数组和关联数组

```sh
#数组借助索引将多个独立的数据存储为一个集合
#普通数组只能使用整数作为数组索引，而关联数组可以使用字符串作为数组索引
#还可将数组定义成一组索引-值(index-value)


arr=(1 two 3 four 5)
echo ${arr[0]}
arr[0]=One


index=3
echo ${arr[$index]		#arr[3]
echo ${arr[*]}
echo ${#arr[*]}		#arr-length


#关联数组可用任意文本作为数组索引
declare -A ass_arr
ass_arr=([index1]=val1 [index2]=val2 ...)	#内嵌索引-值
ass_arr[index3]=val3	#独立索引-值
echo ${!ass_arr[*]}		#列出数组索引
```


<br>

## 别名(`alias`)

```sh
#alias作用是暂时的，关闭终端后别名就失效；
#为使别名一直保持，可将其写入 ~/.bashrc，因为每一个新的shell都会执行~/.bashrc中的命令
#新设置的别名将取代已有别名


alias vi=vim;
unalias vi
echo "alias ll='ls -l --color=auto'" >> ~/.bashrc


#\对别名命令进行转义，执行原本的命令。避免攻击者利用别名将某些特权命令替换成别有用心的命令
\vi test.sh
```


<br/>

## 获取、设置日期和延时(`date`)

```sh
#很多应用程序需要以不同的格式打印日期，设置日期和时间，以及根据日期和时间执行操作;
#延时通常用于在程序执行过程中提供一段等待时间;
#在Unix-like系统中，日期被存储为一个整数，其大小为世界标准时间1970年1月1日0时0分0秒起所流逝的秒数；
#这种计时方式被称之为 纪元时或Unix时间；
#通过纪元时间，可知道两个日期之间相隔了多少秒
#编写以循环方式运行的监视脚本时，设置时间间隔是必不可少的

date +%s

#!/bin/bash
start=$(date +%s)
commands
sleep 1
end=$(date +%s)
diff=$((end - start))
echo "$diff seconds"


#显示指定时间
date +%F -d -1days
date +%H -d -3hours



#将标准时间转换为原子时间
date -d '2018-02-07 14:05:53' +%s
1517983553

#将原子时间转换为标准时间
date --date='@1517983553'
Wed Feb  7 14:05:53 CST 2018
```


<br>

## 调试脚本(`sh`)

```sh
#调试功能能在出现一些异常情况时生成运行信息

#!/bin/bash -xv
sh -x
sh -n
```


<br/>

## 函数和参数(`function`)

```sh
function fname()
{
statements
}


fname()
{
echo $1, $2		#访问第参数1和参数2,$n第n个参数
echo "$@"		#以列表的形式一次性打印所有参数
echo "$*"		#类似于$@，但参数被作为单个实体
return 0		#f返回值
}
fname 1 22 333	#返回上面定义的变量


#递归函数，能够调用自身，不断地生成新的进程，最终会造成xx
#导出函数，使用export导出，这样函数作用域就可以扩展到子进程
export -f fname
#读取命令返回值
echo $?
```


<br>

## 读取命令序列输出(\` \`, $() )

```sh
#输入通常是stdin，输出stderr或stdout,这些命令称为 过滤器(filter)。我们使用 管道(pipe) 来连接每一个过滤器
cmd1 | cmd2 | cmd3


#子shell，子shell生成独立的进程，不会对当前shell有任何影响，所做改变仅限于子shell内
zhang=$(ls | cat -n)


#反引用
zhang=`ls | cat -n`
```


<br/>

## 读取字符(`read`)

```sh
#read是一个重要的从标准输入中读取文本的命令
#可以使用read以交互的形式来读取用户的输入

read -n 5 zhang		#读取字符数
echo $zhang

read -s passwd		#不回显
echo $passwd

read -t 5 zhang		#超时时间
echo $zhang

read -p zhang		#显示提示信息
echo $zhang

read -d ":" zhang		#定界符结束输入
123：
echo $zhang
```


<br>

## 字段分隔符和迭代器

```sh
#内部字段分隔符(Internal Field Separator, IFS)是shell中的一个重要概念
#IFS的默认值为空白字符(换行符、制表符、空格)
awk -F: '{print $1,$3}' /etc/passwd		#IFS=":"

#对一些列值进行迭代，循环非常有用
for i in {1..10}
do
cmd
done

while condition
do
cmd
done

until condition
do
cmd
done
```


<br/>

## 比较与测试

```sh
#程序中的流程控制是由比较和测试语句来处理的
if condition1 || condition2
then
	cmd1
elif condition3 && condition4
then
	cmd2
else
	cmd3
fi


#算术比较
if [ $num -ge 10 -a $num -lt 20 ]

-eq
-gt
-ge
-lt
-le
-a
-o


#文件系统相关
if [ -f $file1 -o -x $file2]

-x
-w
-r
-f
-d
-e
-b		#block
-l


#字符串比较
[[ $str1 = $str2]]

=		#=号旁有空格--是比较关系；=号旁没空格，是赋值语句
!=
>
<
-z		#空字符
-n		#非空字符


#使用test命令来执行条件检测
if [ $num -eq 0 ]  -- if test $num -eq 0
```



<br>
<br/>

# 命令之乐

## 简介

各种命令可谓Unix-Like系统中优美的部分，它能帮我们搞定各种繁杂的任务。
一旦你尝试过Linux提供的这些利器，你一定会感到惊讶：以前没有这些命令的时候，自己是什么熬过来的。
最钟爱的莫过于 `grep`, `awk`, `sed`, `find` 命令了！

本章将会为你介绍一些最有趣同时也是最实用的命令。


<br>

## 用`cat`进行拼接

```sh
#cat命令通常用于读取、显示或拼接文件内容，不过它所具备的能力远不止此
#cat(concatenate, 拼接)
cat file1 file2 ···
echo "Ahaha" | cat - file1 file2		#-指stdin文本文件名

cat -s file3  -- cat file3 | tr -s '\n'		#压缩空白行
cat -T test.py		#将制表符显示为 ^I, 避免制表符和连续空格误用, 产生错误缩进
cat -n file4		#显示行号
```


<br/>

## 录制与回放终端会话(`script`)

当你需要准备一个命令行教程时，如果将我们输入命令后的一切按照先后次序记录下来，再进行回放，是不是很nice！
通过 `script`, `scriptreplay` 命令, 把终端会话记录到文件，并回放。

```sh
#-t,将时间数据输出到标准错误； -a,追加输出
script -t 2> timing.log -a output.session		#两个文件随意取名, 如不将错误重定向会显示在屏幕上导致很乱
输入命令
cmd2
···
exit		#退出录制


scriptreplay -t timing.log output.session		#播放
```


<br>

## 文件查找与文件列表(`find`)

`find` 是Unix/Linux命令行工具箱中最棒的工具之一。
`find` 命令沿着文件层次结构向下遍历，匹配符合条件的文件，并执行相应的操作。

find - search for files in a directory hierarchy

```sh
#基于文件名及正则表达式搜索
find /home/zhang		#列出/home/zhang目录及其子目录线所有文件和文件夹
find /home/zhang -name "*.txt"
find . -name "*.sh" -o -iname "zhang*"		#匹配多个
find /home/zhang -path "201710*"		#-path将文件路径作为一个整体进行匹配
find . -regex	".*\(\.txt|\.[0-9]+\)$"		#匹配以.txt或数字结尾的文件



#使用-maxdepth, -mindepth参数，来限制find的遍历深度
#-type, 根据文件类型搜索。 f(普通文件)，d(目录)，b(块设备)，l(符号链接)，s(套接字)等
find /home -maxdepth 1 -type f(d)		#参数顺序也会影响find的查找效率



#根据文件类型搜索
find /dev -type b		#查看/dev及其子目录下设备文件
find / -maxdepth 1 -type l		#查找/下链接文件



#根据文件时间进行搜索
#Unix/Linux文件系统中的每一个文件都有三种时间戳(timestamp),-表示小于，+表示大于
#Unix中并没有所谓的 "创建时间" 的概念
#访问时间(-atime,以天为单位； -amin,以分钟为单位):用户最近一次访问文件时间；
#修改时间(-mtime,以天为单位； -mmin,以分钟为单位):文件最后一次修改时间；
#变化时间(-ctime,以天为单位； -cmin,以分钟为单位):文件元数据(如权限，所有权)最后一次变化时间；
find /home/zhang -type f -mtime 7		#7天前被修改的普通文件
find /home/zhang -type f -amin -10		#搜索10分钟内被修改的普通文件
find . -type f -newer file1.txt		#找出比file1.txt新的文件



#基于文件大小的搜索
#b(块，512字节), c(字节), w(字，2字节), k(千字节), M(兆字节), G(吉字节)
find . -type -f -size +100k



#删除匹配的文件
find . -type f -name "*.swp" -delete



#基于文件权限和所有权的匹配
find . -type f -perm 644
find /var/apache -type f -name "*.php" -perm 644		#搜索基于权限的文件
find /var -maxdepth 2 -type f -user zhang		#搜索基于用户的文件



#执行命令或动作
#find命令可以借助-exec与其他命令进行结合
#{}是一个特殊字符串，将替换为相应文件名
find . -type f -perm 764 -user zhang -exec chmod 644 {} \;		#将所属用户zhang，权限764的文件权限修改为644
find . -type f -mmin +30 -name "*.txt" -exec cp {} {}.old \;		#复制最近30内修改的名字为.txt的文件



#-exec结合多个命令
#我们无法在-exec参数中直接使用多个命令，不过我们可以把多个命令写到一个shellscript中，然后执行
-exec ./test.sh {} \;
find . -type f -name "*.sh" -mmin -10 -exec sh {} \;



#让find跳过特定目录
-prune



#利用find搭配tar打包
#查找7天内的文件并打包
#建议使用绝对路径，管道无效，所有要定向到文件
find /dir/path/zhang -type -f -mmtime -7 > /dir/path/zhang/zhang.list && tar -T /dir/path/zhang/zhang.list -czvf /dir/path/zhang123.tar.gz
#检查是否正确
tar -tf /dir/path/zhang123.tar.gz

#不能使用find -exec tar，这样打包以后只有最后一个文件
```


<br>

### 利用`stat`命令查看atime, mtime, ctime

`stat` - display file or file system status

```
stat 1.txt

#Access:
#Modify:
#Change:
```

<br>

### 利用`touch`命令修改atime, mtime, ctime

touch - change file timestamps

```
#-a     change only the access time
#-m     change only the modification time
#-d     instead of current time
#-t     instead of current time
```


<br>

## 玩转`xargs`

xargs - build and execute command lines from standard input

```sh
#xargs能够处理stdin并将其转换为特定命令的命令行参数
#也可以将单行或多行输入文本转换成其他格式(如多行变单行)
cmd | xargs


#将多行输入转换为单行输出
echo -e "1\n2\n3" | xargs	#将换行符替换为空格
#将单行输入转换成多行输出
echo "1 2 3" | xargs -n 1	#每行一个参数
echo "hahaZhahaZhahaZhaha" | xargs -n 2 -d Z	#-d指定分隔符


#读取stdin，将格式化参数传递给命令
cat test.txt | xargs -n 1 ./zhang.sh	#zhang.sh arg1; zhang.sh arg2... 每次提供一个参数
cat test.txt | xargs -n X ./zhang.sh	#X为参数个数，一次提供全部参数


#指定替换字符串
cat test.txt | xargs -I {} ./zhang.sh {}


#结合find使用xargs
find . -type f -name "*.txt" -print0 | xargs -0 ls	#-print0无换行输出, -0将\0作为输入界定符


#统计某文件行数
find /path -type f -name "*.c" -print0 | xargs -0 wc -l


#结合stdin，运用while和子shell
cat file.txt | while read arg; do cat $arg; done == cat file.txt | xargs - {} cat {}
cmd0 | (cmd1; cmd2; cmd3) | cmd4	#子shell
```


<br/>

## 用`tr`进行转换

tr - translate or delete characters

```sh
#tr命令经常用来编写优美的单行命令
#tr可对来自stdin的字符 进行替换、删除以及压缩


echo "AH WONDERFUL" | tr 'A-Z' 'a-z'	#转换大小写
echo "AH WONDERFUL" | tr 'A-Z' 'a-b' --> ab bbbbbbbbb
#tr [option] set1 set2
#如果两个字符集长度不相等，那么set2会不断重复其最后一个字符，直到长度与set1相同


echo 12345 | tr '0-9' '9876543210'	#数字加密
echo 87654 | tr '9876543210' '0-9'	#数字解密


echo 'He is a cool boy, and she is a beautiful girl' | tr 'A-Za-z' 'NOPQRSRUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm'	#加密
echo 'Ur vf n pbby obl, naq fur' | tr 'NOPQRSRUVWXYZABCDEFGHIJKLMnopqrstuvwxyzabcdefghijklm' 'A-Za-z'	#解密

cat 1.txt | tr '\t' ' '	#将制表符转换为空格


#删除字符
echo "Hello 530 World" | tr -d '0-9'	#-d删除，删除数字
Hello  World
echo "Hello 520 World" | tr -d -c '0-9'	#-c补集
 520


#压缩字符，将连续的重复字符压缩为单个字符
echo "GNU's    not   Unix" | tr -s ' '	#-s压缩，压缩空格
GNU's not Unix


echo -e "1\n2\n3\n4\n5" > sum.txt
cat sum.txt | echo $[ $(tr '\n' '+') 0 ] -- echo $[1+2+3+4+5+0]


#tr字符类
\a	终端鸣响
\b	退格
\f	换页
\n	换行
\r	回车
\t	水平制表符
\v	垂直制表符
string1-stringN	#从字符1到字符N升序过程中的所有字符
[字符*次数]
[:alnum:]	#所有字母和数字
[:alpha:]	#所有字母
[:digit:]	#所有数字
[:lower:]	#所有小写字母
[:upper:]	#所有大写字母
[:graph:]	#所有可打印字符，不含空格
[:print:]	#所有可打印字符，包含空格
[:blank:]	#所有水平排列的空白字符
[:cntrl:]	#所有控制字符
[:punct:]	#所有标点字符
[:space:]	#所有空白字符
[:xdigit:]	#所有十六进制数
[=字符]	#指定字符
```


<br/>

## 校验和 与 核实文件完整性(`md5sum`)

```sh
#校验和(checksum)程序从文件中生成校验和密钥，然后利用校验和密钥核实文件的完整性
#校验和对于编写备份脚本或系统维护脚本非常重要，因为它们都会涉及通过网络传输文件
#通过使用校验和核实，我们就可以识别那些在网络传输过程中出现损坏的文件，并重传，从而确保数据完整性
#校验和对于核实数据完整性非常有用
#广泛使用的校验和技术有：md5sum, sha1sum


#对单个文件进行校验
md5sum sum.txt > sum.md5
#302c28003d487124d97c242de94da856  sum.txt
md5sum -c sum.md5	#-c检查
#sum.txt: 确定


#对目录进行校验
#对目录计算校验和意味着我们需要对目录中的所有文件以递归的方式进行计算
yum install -y md5deep
md5deep -r ./dir > dir.md5	#recursive递归
md5sum -c dir.md5


#可以将测试dir下某个文件更改一下，校验的时候会报错
```


<br>
## 排序、单一、重复(`sort`,`uniq`)

```sh
#sort - 对文本文件进行行排序
#uniq - 删除排序文件中的重复行


echo -e "333\n1" > 1.txt; echo -e "22\n22" > 2.txt
sort 1.txt 2.txt -o ./sorted.txt
#1
#22
#22
#333
cat sortec.txt | uniq
#1
#22
#333


sort -n	#按数字进行排序
sort -r	#逆向排序
sort -M	#按月份排序
sort -C	#检查是否排序
sort -b	#忽略空白


#依据键或列进行排序
sort -k 2 data.txt	#依据第二列来排序


#uniq要么使用管道，要么使用排过序的文件作文输入
uniq -u sorted.txt	#只显示唯一的行(即没有重复出现的行)
uniq -d sorted.txt	#只显示重复的行
uniq -s 2 -w 2 sorted.txt	#-s忽略前2个字符，-w指定用于比较的最大字符数
```


<br/>

## 临时文件命名、随机数

```sh
#在编写shell脚本时，我们经常需要存储临时文件。最适合存储临时数据的位置是 /tmp
#/tmp目录中的内容会在系统重启后被清空

filename=$RANDOM	#RANDOM返回一个随机数
filename2=$$	#当前shell的PID
filename3=$((date +%F))	#通过日期命令
```


<br>

## 分割文件和数据(`split`)

```sh
#某些情况下，需要把文件分割成多个更小的片段
dd if=/dev/zero bs=100k count=1 of=./data.file	#生成一个大小100k内容全是0的文件
split -b 20k data.file	#-d指定分割大小
#data.file xaa xab xac xad xae,这五个文件都为20k
#我测试了一下，几个文件加起来数据没变，几个文件总行数没变
#单位有 k, m, G, c(byte), w(word)
#-d以数字为后缀， -a指定后缀长度


split data.file -b 20k -d -a 2 spt	#增加前缀名'spt'
#data.file  spt00  spt01  spt02  spt03  spt04


split -l 10 data.file	#-l按行数来分割文件
#split只能根据大小或行数分割文件


#csplit可以根据文件本身特点进行分割
-f	#指定分割后文件前缀
-n	#指定分割后文件后缀数字个数
-b	#指定后缀格式
```


<br>

## 根据扩展名切分文件名

```sh
#借助%操作符将名称从 “名称.扩展名” 格式中提取出来


file="zhang.txt"
name1=${file%.*}	#删除位于%右侧的通配符(.*)所匹配的字符串，通配符从右向左进行匹配
#zhang
#*号通配符，.号
#%属于非贪婪匹配(non-greedy),它会匹配通配符最短结果
#%%属于贪婪匹配(greedy)，它会匹配符号条件的最长字符串



name2=${file#*.}	#删除位于#右侧的通配符(*.)所匹配的字符串，通配符从左向右进行匹配
#txt
# #属于非贪婪匹配
# ##属于贪婪匹配


#栗子
URL=“www.google.com”
echo ${URL%.*}	#非贪婪匹配，移除最右边.及其后面内容
www.google
echo ${URL%%.*}	#贪婪匹配
www
echo ${URL#*.}	#非贪婪匹配，移除最左边.及其前面内容
google.com
echo ${URL##*.}	#贪婪匹配
com
```


<br/>

## 批量重命名和移动

综合运用`find`、`rename`、`mv`命令。



<br>

## 拼写检查与词典操作

```sh
#Linux大多数发行版都含有一份词典文件，另外还有一个被称为aspell的拼写检查命令
#words --> /usr/share/dict/linux.words

grep "^good" /usr/share/dict/linux.words

aspell
```


<br/>

## 交互输入自动化

```sh
#写一个读取交互式输入脚本
vi jiaohu.sh

#!/bin/bash
read -p "Input a number:" num
read -p "Input name:" name
echo "You have enterd number:$num, name:$name"

echo -e "1\nzhang" | ./jiaohu.sh
You have entered number:1, name:hello


#or
echo -e "1\nzhang" > input.txt
./jiaohu.sh < input.txt


#交互式输入自动化
#用expect实现自动化
yum install -y expect
vim auto_expect.sh


#!/bin/expect
spawn ./jiaohu.sh	#spawn指定需要自动化哪一个命令
expect "Input a number:"	#expect提供需要等待的消息
send "1\n"		#send是要发送的消息
expect "Input name:"
send "zhang"
expect eof	#expect eof指明命令交互结束

./auto_expect.sh
```




<br>
<br/>

# 以文件之名

## 简介

Unix将操作系统中的一切都视为文件。


<br>

## 生成任意大小的文件(`dd`)

由于各种原因，可能需要生成一个包含随机数据的文件。

```sh
#dd命令会克隆给定的输入内容，然后将一模一样的副本写到输出
#如果不指定if，dd会从stdin中读取输入；如果不指定of，dd会输出到stdout
#/dev/zero是一个字符设备，它会不断返回0值字节(\0)

dd if=/dev/zero of=junk.data bs=1M count=1
```


<br>

## 文本文件的交集与差集

```sh
#comm命令用于两个文件之间的比较
#交集(intersection),差集(set difference), 求差
#comm必须使用排过序的文件作为输入


echo -e "1\n2\n3" > A.txt && echo -e "3\n2\n3" > B.txt
sort -n A.txt -o A.txt && sort -n B.txt -o B.txt
comm A.txt B.txt
#输出第一列为A独有，第二列为B独有，第三列为交集


comm A.txt B.txt -1 -2
#-1从输出中删除第一列，-2删除第二列，-3删除第三列
```


<br/>

## 查找并删除重复文件

```sh
#重复文件指的是那些虽然名字不同但内容却一模一样的文件

ls -lS	#以文件大小排序，识别大小相等的文件
md5sum	#接下来计算这些文件的校验和
```

<br/>

## 创建长路径目录

```
mkdir -p /home/zhang/1/22/333 2>/dev/null
```


<br>

## 文件权限、所有权和粘滞位

```sh
#用户(user)，用户组(group)，其他用户(other)

ll ./*
#d目录，c字符设备，b块设备，l符号链接，s套接字，p管道，-普通文件


#用户还有一个称为setuid(S)的特殊权限，它出现在用户的x位置
#setuid权限允许用户以其拥有者的权限来执行可执行文件，即便这个文件是由其他用户运行的
-rwSrw-r--



#组也拥有一个setgid(S)权限，它出现在组的x位置
#它允许以同该目录拥有者所在组相同的有效组权限来运行可执行文件
-rwxrwSr--



#目录有一个特殊权限，叫做粘滞位(sticky bit)(T或t)，出现在其他用户的x位置
#当一个目录设置了粘滞位，只有创建该目录的用户才能删除目录中的文件,即便group和other有w权限
-rwxr--rwT



chmod u=rwx g=rw o=r file1
chmod u+x g-w file2
chmod 744 file3


chmod a+x . -R	#以递归方式设置权限
chown user.group . -R	#以递归方式设置所有权


chmod a+t dir1	#设置粘滞位


chmod +s fiel4
chown root.root file4
chmod +s file4
./file4	#每次file4都是以root运行

#setuid的使用不是无限制的，它只能应用在Linux ELF格式二进制，而不能用于脚本文件。
```


<br/>

## 创建不可修改文件

```sh
#不可修改(immutable),是保护文件不被修改的安全手段之一。
#一旦文件被设置为不可修改，任何用户(包括root)都不能修改，除非将其不可修改属性移除

chattr	#修改文件在Linux第二扩展文件系统(E2fs)上的特有属性
chattr +i file1	#这样就无法删除file1
chattr -i file1
```


<br>

## 批量生成空白文件

```sh
#touch命令可用来生成空白文件，如果文件存在，则可以用它修改文件的时间戳

for name in {1..100}.txt;do
touch $name
done


touch -a/-m	#更改文件访问/修改时间
touch -d "Thu Oct 31 14:20:13 CST 2017" file1	#指定特定时间戳
```


<br/>

## 查找符号链接及其指向目标

```sh
#符号链接(软链接)只不过是指向其他文件的指针

ln -s /usr/bin /bin
ls -l / | grep "^l"
find / -maxdepth 1 -type l

readlink /bin	#找出链接目标
```


<br>

## 列举文件类型统计信息

```sh
#在Unix/Linux系统中，文件类型并不是由文件扩展名决定的

file /etc/passwd
file -b /etc/passwd
```


<br/>

## 环回文件与挂载(`mount`)

```sh
#环回文件系统是指那些在文件中而非物理设备中创建的文件系统

dd if=/dev/zero of=loopback.file bs=1G count=1
mkfs.ext4 loopback.file
mount -o loop loopback.file /mnt/loopback	#-o loop来挂载环回文件
df -h
umount /mnt/loopback


#将ISO文件作为环回文件挂载
mount -o loop linux.iso /mnt/iso
```


<br/>

## 生成ISO文件以及混合ISO

```sh
#可引导光盘自身具备引导能力，也可以运行操作系统或其他软件。不可引导光盘则做不到这些。

cat /dev/cdrom  > /dev/sdc	#sdc指U盘
dd if=/dev/cdrom of=/dev/sdc	#将ISO写入usb存储设备
mkisofs -V "Label" -o /dev/sdc /dev/cdrom


cdrecord -v dev=/dev/cdrom image.iso
```


<br>

## 查找文件差异并进行修补

diff - compare files line by line

```sh
#补丁文件(patch file)
#diff命令可以生成差异文件

diff -u file1 file2	#一体化形式输出
diff -u file1 file2 > diff.patch
patch -p1 file1 < diff.patch	#得到file2
patch -p1 file2 < diff.patch	#得到file1
patch -R file1 < diff.patch; patch -R file2 < diff.patch	#还原


#diff也能够以递归的形式作用于目录，它对目录中所有内容生成差异输出
diff -Naur dir1 dir2
#-N将所有确实文件视为空文件， -a将所有文件视为文本文件
#-u生成一体化输出， -r遍历目录下所有文件
```

栗子：

```sh
echo -e '1\n1\n1\n1' > /tmp/1.txt
echo -e '1\n1\n0\n1' > /tmp/2.txt


#比较
diff -u 1.txt 2.txt
--- 1.txt       2018-12-14 16:08:36.457495835 +0800
+++ 2.txt       2018-12-14 16:08:37.574495820 +0800
@@ -1,4 +1,4 @@
 1
 1
-1
+0
 1


#解释
--- 1.txt       2018-12-14 16:08:36.457495835 +0800
+++ 2.txt       2018-12-14 16:08:37.574495820 +0800
#第一部分，是文件的基本信息
#---表示变动前的文件
#+++表示变动后的文件


@@ -1,4 +1,4 @@
#第二部分，变动的位置用两个@作为起首和结束。
#-号表示第一个文件(1.txt), 1表示第一行，4表示连续四行。也就是第一个文件从第一行开始连续四行
#+号表示第二个文件(2.txt), 1表示第一行，4表示连续四行。


 1
 1
-1
+0
 1
#第三部分表示变动的具体内容
#除了有变动的那些行以外，也是上下文各显示3行。它将两个文件的上下文合并显示在一起，所以称为合并显示
#每一行最前面的标志位，空表示无变动，减号表示第一个文件删除的行，加号表示第二个文件新增的行
```


<br>

## `head`与`tail`

```sh
head file1; tail file1	#head与tail默认打印10行
head -n 5 file1; tail -n 6 file1	#指定行数
head -n -5 file1	#打印除了最后5行外所有行
tail -n +(5+1) file1	#打印除了开始5行外所有行

tail -f /var/log/nginx/access.log	#--follow，动态关注文件
```


<br/>

## 只列出目录的其他方法

```
ls -d .
ls -l . | grep "^d"
ls -F . | grep "/$"
find . -maxdepth 1 -type d
```


<br/>
## pushd和popd

```sh
#在命令行中使用pushd和popd快速定位，pushd和popd以栈的方式运作
#当没有鼠标时，复制粘贴就不怎么实用了
#pushd和popd可以用于在多个目录之间进行切换而无需复制并粘贴目录路径

pushd /home/user1; pushd /home/user2; pushd /home/user3	#将路径添加到栈
pushd +2	#切换到/home/user3

popd	#移除最近添加入栈的目录


cd /root; cd /home/user
cd -	#回到上次的目录
cd ..	#切换到上一级目录
cd ~	#切换到用户主目录
```


<br>

## 统计文件的行数、单词数、字符数

```sh
#wc(word count)，是一个统计工具

wc -l file1	#统计行数
wc -w file1	#统计单词数
wc -c file	#统计字符数
wc -L file	#打印最长行长度
wc file1	#行、单词、字符数
```


<br/>

## 目录树

```sh
#tree命令是以图形化的树状结构打印文件和目录,在Linux发行版中默认未安装

yum install -y tree
tree /home/zhang
tree /home/zhang -P "*.sh"	#只标记出.sh文件
tree /home/zhang -I "*.sh"	#标记出除.sh文件外所有文件
tree /home/zhang -h	#显示大小

tree /home/zhang -H http://localhost -o tree.html	#以html形式输出目录树
```



<br>
<br/>

# 让文本飞

## 简介

shell脚本可以将`sed`, `awk`, `grep`, `cut`等这类优美的工具组合在一起，用于解决文本处理相关问题。

<br>


## 正则表达式

Regular Expression


正则表达式是一种用于文本匹配的形式小巧、具有高度针对性的编程语言。只依靠通配符技术，能够匹配的文本范围相当有限。

<br>

**正则表达式基本组成**

正则表达式 | 描述
 -	| -
^	| 行起始标记
$	| 行尾标记
.	| 匹配任意一个字符
[]	| 匹配包含在[]中的任意一个字符
[^]	| 匹配出[^]之外任意一个字符
[-]	| 匹配[]中范围内的任意一个字符
？	| 重复0或1次
+	| 重复>=1次
*	| 重复>=0次
()	| 创建一个用于匹配的子串
{n}	| 重复n次
{n, }	| 重复>=n次
{n,m}	| 重复n到m次
\	| 转义字符
竖线l	| 匹配竖线l两边任意一项


<br>

**POSIX字符类**

POSIX字符类(POSIX character class),是一个形如[:...:]的特殊元序列，它用于匹配特定的字符范围。<br>

正则表达式	| 描述
-	| -
[:alnum:]	| 字母与数字字符
[:alpha:]	| 字母字符
[:blank:]	| 空格与制表符
[:digit:]	| 数字字符
[:lower:]	| 小写字母
[:upper:]	| 大写字母
[:punct:]	| 标点符号
[:space:]	| 所有空白字符


<br>

**元字符**

元字符(meta character)，是一种Perl风格的正则表达式，只有一部分文本处理工具支持它。<br/>

正则表达式	| 描述
-	| -
\A  | 字符串的开头
\b	| 单词边界
\B	| 非单词边界
\d	| 单个数字字符
\D	| 单个非数字字符
\f  | 换页字符
\n	| 换行符
\r	| 回车
\s	| 单个空白字符
\S	| 单个非空白字符
\t  | 跳进字符
\v  | 垂直跳进字符
\w	| 单个单词字符(数字，字母和_)
\W	| 单个非单词字符
\z  | 字符串的结尾
\Z  | 匹配字符串的结尾，或结尾的换行符之前


<br>


```sh
#匹配一个ipv4地址
[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}


#匹配一个邮箱地址
[\w]+@[\w]\.com
```


<br>

## 用`grep`在文件中搜索文本

`grep`命令是Unix中用于文本搜索的工具，它能够接受正则表达式和通配符。

```sh
grep "匹配文本/通配符" file1 file2... --color=auto	#重点标记匹配
grep -E "正则表达式" file
egrep "正则" file


grep -v	#反向匹配
grep -c	#统计匹配行数
grep -n	#打印出匹配的行号
grep -o	#唯一匹配
grep -l	"匹配" file1 file2	#返回匹配的文件名
grep -R	#递归匹配
grep -i	#忽略大小写
grep -e "匹配1" -e "匹配2"	#匹配多个样式
grep -f match.txt file1	#从match.txt文件读取匹配
grep "匹配" --include=*.{sh,txt} --exclude=*.log --exclude-dir=/home/user -r /home	#包括或排除文件
-A/-B n	#输出匹配 之后/之前 n行
-c n	#输出匹配 前后 n行



#正则匹配多个
egerep "(a|b)"
```


<br>

## 用`cut`按列切分文件

`cut`是一个将文本按列进行切分的小工具，它也可以指定每列定界符。在cut的术语中，每列都是一个字段。<br>

```sh
#制表符'\t' 是cut默认的定界符
cut -d' ' -f1 1.txt	#-d指定分隔符，-f打印第几个字段
cut -f1,2,3	#打印1,2，3列
-c字符；	-b字节；
cut -c 1-5 1.txt	#打印1-5字符
cut -c -2 1.txt	#打印前2个字符
cut -c 3-	#打印第3个字符到行尾
```


<br/>

## 统计特定文件词频

```sh
#单词解析可以用 关联数组,正则表达式配合sed,awk,grep等工具来完成
#关联数组中，将单词作为数组索引，单词次数作为数组值

egrep -o "\b[:alpha:]+\b"	#匹配单词
```


<br/>

## `sed`入门

`sed`是stream editor(流编辑器)的缩写，它是文本处理中非常重要的工具。能够完美地配合正则表达式使用。

```sh
#sed - stream editor for filtering and transforming text
#字符/在sed中最为定界符使用


#替换
#sed 's/匹配样式/替代字符串/'
sed 's/pattern/repalce/' file	#替换
sed -i 's/pattern/repalce/' file	#将替换应用于file


echo "1.txt" > 1.txt && sed 's/txt/haha' 1.txt	#在输出中用haha替换txt
sed -i 's/txt/haha/' 1.txt	#将1.txt文件中的txt用haha替换掉
#-i选项替换原文件


echo "hahaha" | sed 's/ha/HA/g'	#全部替换
echo "hahaha"	| sed 's/ha/HA/2g'	#指定位置替换，从第2处开替换全局


#移除匹配样式的行
sed '/pattern/d
sed '/^$/d'	##移除空白行


#在sed中用&标记已匹配字符串
echo "A wonderful goal" | sed 's/\w\+/[&]/g'	#\w\+匹配每一个单词


#子串匹配标记\1,\2...
echo "1st 2nd 3rd" | sed 's/\(\w\+\) \(\w\+\) \(\w\+\)/\2 \1 \3/'
2nd 1st 3rd
#将\2和\1交换次序，(),+等在sed中要转义，否则要报错


#组合多个表达式
sed 'expression1; expression2; ...
echo "aabbcc" | sed 's/a/A/; s/b/B/; s/c/C/g'
AaBbCC


#双引号 " " 内的特殊符号（如$等），可以保有原本的特性
#单引号 ' ' 内的特殊字符则仅为一般字符（纯文本）
#引用
text=hello
echo 'hello world' | sed "s/$text/HELLO/"
HELLO world
```


<br/>

## `awk`入门

`awk`被设计用于数据流，它可以对列和行进行操作。

```sh
#awk ‘begin{print "start"} pattern {command} end{print "end"}’ file

awk '{sum += $1}; {print sum}'

#awk脚本由:begin块、end块和能使用模式(pattern)匹配的通用语句块 组成
#3个部分都是可选的
#awk也可以从stdin中读取内容

cat /etc/passwd | awk -F: '{print $1}'	#-F指定界定符


#awk中的特殊变量
#NR：记录数量(number of records)，对应于当前行号
#NF：字段数量(number of fields)，对应于当前行的字段数
#$0：执行过程中当前行的文本内容
#$1,$2...$NF：第1个/2个.../最后一个 字段的内容

echo -e "L1 1\nL2 22\nL3 333" | awk '{print NR NF $0 $1 $2}'
# NR NF $0    $1  $2 $NF=最后一个=$2
  1  2  L1 1  L1  1  1
  2  2  L2 2  L2  2  2
  3  2  L3 3  L3  3  3


#将外部变量传递给awk
#-v选项可将外部值传递给awk
#  -v var=val  --assign=var=val

var='12345'
echo | awk -v v1=$var '{print v1}'


#多个变量
var1=111; var2=222
echo | awk '{print v1,v2}' v1=$var1 v2=$var2


#变量来自文件而非标准输入
awk '{print v1,v2}' v1=$var1 v2=$var2 file


#用样式对awk进行过滤处理
awk 'NR < 3,NR==4' 1.txt	#行号<5的行
awk '/linux/' 1.txt	#匹配带有linux的行（可用re）
awk '!/linux/' 1.txt #!匹配不带linux的行


#设置定界符
awk -F: '{print $1}' /etc/passwd
awk '{FS=":"} {print $1}' /etc/passwd
awk '{FS=":"; print $1}' /etc/passwd


#从awk中读取命令输出，用getline读取行
echo | awk '{"grep root /etc/passwd" | getlin out; print out}'
root:x:0:0:root:/root:/bin/bash


#在awk中使用循环
awk '{for(i=1;i<4;i++) {print $i}}' 2.txt	#输出第1,2,3列



#使用awk删除某列
awk -F' '    '{$1=null;$2=null;print}' ./file

```

<br>

## 对文件中的行、单词、字符进行迭代

```sh
#迭代文件中的每一行
echo -e "1\n22\n333" | while read line;do echo $line;done
grep "bash" /etc/passwd | while read line;do echo $line;done
#1
#22
#333


#迭代一行中的每一个单词
echo "1 22 333" | while read line;do for word in $line;do echo $word;done;done
#1
#22
#333


#迭代一个单词中的每一个字符
echo "abc" | while read line;
do
	for word in $line;
	do
    	for((i=0;i<${#word};i++));
		do
        	echo ${word:i:1};
		done;
	done;
done

#写成一行
echo "abc" | while read line; do for word in $line; do for((i=0;i<${#word};i++)); do echo ${word:i:1}; done; done; done
#a
#b
#c
#${#word}返回变量word的长度
```


<br/>

## 按列合并文件(`paste`)

可以使用`paste`命令实现列拼接
```sh
#paste - merge(整合) lines of files
echo -e  "1\n2\n3" > 1.txt && echo -e "Line1\nLine2\nLine3" > 2.txt
paste 1.txt 2.txt
1	Line1
2	Line2
3	Line3
#默认定界符是制表符，用-d指定
paste 1.txt 2.txt -d','
```


<br>
## 打印文件或行中的第n个单词或n列

```sh
awk -F':' '{print $1,$3}' file1
cut -d':' -f 1,3 file1
```


<br/>
## 打印不同行或样式之间的文本

```sh
awk 'NR==1,NR==10' /etc/passwd
awk 'NR==1,NR==10' /etc/passwd | awk -F":" '{print $1,$NF}' #打印特定行内的特定列

awk '/start_pattern/, /end_pattern/' file	#打印start到end之间的内容,可使用re
awk '/root/, /zhang/' /etc/passwd	#打印root到zhang之间内容
awk '/^ro.?t'/, /bash$/' /etc/pass
```


<br>

## 以逆序形式打印行

可以使用`awk`, `tac`完成。tac就是反过来的cat。

```sh
#tac - 反转显示文件中的行，行内的内容无法用tac反向排列
tac 1.txt
awk '{lifo[NR]=$0; lno=NR} END{ for(;lno>-1;lno--) {print lifo[lno]};}' 1.txt
```



<br/>
## 解析文本中的电子邮件和URL

从给定的文件中解析出所需要的文本是我们从事文本处理时的一项任务。

grep, egrep, fgrep - print lines matching a pattern

```sh
#egrep


#匹配一个邮箱地址
egrep -o '[a-zA-Z0-9.]+@[0-9a-zA-Z.]+\.[a-zA-Z]{2,4}' emails.txt


#匹配一个URL地址
egrep -o "http://[a-zA-Z0-9.]+\.[a-zA-Z]{2,3}" urls.txt
```


<br>

## 打印某个样式之前/之后n行(`grep`)

```sh
grep "zhang" /etc/passwd -A 5	#Ater
grep "zhang" /etc/passwd -B 5	#Before
grep "zhang" /etc/passwd -C 5	#前后五行都打印
```


<br/>
## 在文件中移除包含某个单词的句子

只要能写出正确的正则表达式(Regular Expression)，那就手到擒来

```sh
sed 's/[^.]*handsome boy[^.]*\.//g' file.txt	#句子以.结束
```


<br>
## 文本切片与参数操作

```sh
#替换变量内容中的部分文字
var="One two three"
echo ${var/t/T}	#只替换了一个
#One Two three


#指定字符串起始位置和长度
#${变量:开始部分:长度}
${vari:start:length}
echo {var:0:2}	#On
echo {var:1:6}	#ne two


#起始字符的索引是0,将最后一个字符索引记为-1
echo ${var:(-1)}	#e
echo ${var:(-3):3}	#ree
```



<br>
<br/>

# 一团乱麻？没这回事


## 入门

本章会研究一些用于解析网站内容、下载数据、发送数据表单以及网站颇为任务自动化之类的实例。我们可以仅用几行脚本就将很多原本需要通过浏览器交互进行的活动管理自动化。通过命令行工具利用HTTP协议所提供的功能，我们可以用脚本解决大部分Web自动化的问题。


<br>

## 网站下载(`wget`,`curl`)

使用一些命令行下载工具，从给定的URL中下载文件或网页。<br>

wget是一个用于文件下载的命令行工具，选项多且用法灵活。

```sh
#Wget - The non-interactive(非交互式) network downloader

wget URL1 URL2...
wget http://xxx.com/nginx-1.12.0.tag.gz
wget https://xxx/a.rpm http://xxxx/bb.rpm


#指定文件名，指定信息输出(wget默认是stdout)
wget http://mirrors.aliyun.com/repo/Centos-7.repo -O aliyun.repo -o ./wget.log
wget URL -t 5	#-t，重试次数


#下载限速
wget --limit-rate=10m URL	#下载限速
wget -Q 100m URL	#指定下载配额


#端点续传
#wget进行的下载在完成前被中断，从断点开始下载
wget -c URL


#用cURL下载
#cURL是一个比wget更强大的高级命令工具
#和wget不同，curl并不将下载数据写入文件，而是写入stdout，因此必须重定向到文件


#复制或镜像整个网站
#wget有一个选项可以使其像爬虫一样以递归方式手机网页上所有URL链接，并逐个下载
#这样一来就可以下载一个网站的所有页面
wget --mirror URL
#-m(--mirror) -N -r -l inf --no-remove-listing 的缩写形式。
或 wget -r -N -l DEPTH URL
#-r递归下载，-l指定递归深度，-N(timestamp)只获取比本地时间新的文件


#访问需要认证的HTTP或FTP页面
wget --user "username" --password "pass" URL
#如未在命令行内输入密码，则会由网页提示手动输入
```


<br/>

## 以格式化纯文本下载网页(`links`)

网页其实就是包含HTML标记和其他诸如Javascript，CSS等元素的HTML页面。HTML标记是网页的基础，也许需要解析网页来查找特定的内容。<br/>

links,是一个基于命令行的Web浏览器

```sh
#links - lynx-like alternative character mode WWW browser

#在命令行中浏览一个网页
links www.baidu.com


#以ASCII形式下载网页
links --dump URL > URL.txt


#打开本地html文件
links 1.html
```


<br>
## cURL入门

cURL支持包括HTTP、HTTPS、FTP在内的众多协议。它还支持POST、cookie、认证、从指定偏移处下载部分文件、参照页(referer)、用户代理字符串、扩展头部(extra header)、限速、文件大小限制、进度条等特性。

```sh
#curl - transfer a URL
#cURL通常将下载文件输出到stdout，将进度信息输出到stderr
#要想避免显示进度信息，可使用--silent
#curl可用来下载、发送各种HTTP请求、指定HTTP头部等操作

curl URL --silent	#输出到stdout


#-O写入文件，文件名从URL中解析
curl http://www.baidu.com/index.html -O --silent	#创建index.html


#-o将数据写入指定文件
curl URL -o baidu.html --progress	#--progress显示进度条
links baidu.html


#端点续传
#和wget不同，cURL包含更高级的下载恢复特性，能够从特定的文件偏移处继续下载
#curl可以通过指定一个偏移量来下载部分文件
手动：curl URL/file -C offset	#偏移量以Byte为单位的整数
自动：curl -C -URL	#自动续传


#用cURL设置参照页字符串, --referer
#参照页(referer)是位于HTTP头部中的一个字符串，用来标识用户从哪个页面到达当前页面的
#如果用户点击网页A中某个链接，转到了网页B。那么网页B头部的referer会包含网页A的URL
curl --referer Referer_URL target_URL
curl --referer http://www.baidu.com http://jianshu.com


#用cURL设置cookie, --cookie
#可以用curl来存储HTTP操作过程中使用到的cookie
#cookie用key=value形式，指定多个用 分号 分隔
curl URL --cookie "user=AAA;name=bbb"
curl URL --cookie-jar cookie.txt	#将cookie另存为


#用cURL设置用户代理字符串, --user-agent
#如果不指定代理，一些需要用户代理的网页就无法显示
curl URL --user-agent(-A) "Mozilla"


#用-H "头部信息"传递多个头部信息
curl -
H "Host:www.haha.com" -H "Accept-language: en" URL

#限定cURL可占用的带宽
curl URL --limit-rate 10m


#指定最大下载量
curl URL --max-filesize 大小(Bytes)


#用cURL进行认证，-u username:password指定用户名和密码
curl -u user:pass URL
curl -u user URL	#手动输入密码


#只打印响应头部信息(无数据部分), -I
curl -I URL
```


<br/>

## 从命令行访问163邮箱

```sh
curl -u user http://mail.163.com
#手动输入密码
```


<br/>

## 制作图片抓取器及下载工具

可以用脚本解析图像文件并将图片自动下载下来。

```sh
curl -s URL | grep -o "<img src=[^>]*>" | sed 's/<img src=//g; s/>//g' > img.list
#匹配图片的URL，可能还需要细化修改
#不同的URL可能有不同的规则，根据实际情况取出img的URL


#下载图片
wget $URL 或 curl -s -O $URL
```


<br/>

## 查找网站中的无效链接(`lynx`)

将查找无效链接的工作自动化，那就比纯手动厉害多了！

```sh
lynx -traversal URL #会将URL中所有链接生成到reject.dat文件中
sort -u reject.dat | while read link
do
	output=`curl -I $link -s | grep "HTTP/.*OK"`
    if [[ -z $output ]]
    then
    	echo $link
    fi
done < links.txt
```


<br>
## 跟踪网站变更(`curl+diff`)

可以编写一个定期运行的变更跟踪器(change tracker)，一旦发生变更，跟踪器便会发出声音或发送提示信息。
在不同时间检索网站，然后利用 `diff` 命令进行比对。

```sh
curl URL --silent -o `date +%F`.html	#第一次
curl URL --silent -o `date +%F`.html	#第二次
diff -u 第一次 第二次
```


<br/>

## 以POST方式发送网页并读取响应

POST 和 GET 是HTTP协议中用于发送或检索信息的两种请求类型。
在GET请求方式中，利用网页的URL来发送参数(“键-值”)；而POST方式用于提交表单，如提交用户名、密码以及检索登录页面等。

```sh
curl URL -d “postarg=AABBCC” #-d,http post data
curl URL -d "post1=key1&post2=key2&post3..."	#指定多个数据

wget URL -post-data "post1=key1"
```



<br>
<br/>

# Plan B

<br>

## 简介

提取快照和备份数据都是重要的工作，我们可以通过shell脚本来实现备份自动化。
归档和压缩对于SA来说同样很重要，有多种压缩格式。
加密是一种保护数据的方法，为了减少加密数据的大小，文件在加密前通常需要先归档和压缩。



<br/>

## 用`tar`归档

`tar`命令可以用来归档文件(tar archives tar)。可以将多个文件和文件夹打包为单个文件，同时还能保留所有的文件属性。
由`tar`命令创建的文件通常称为tarball。

```sh
#归档文件，-c(create file)
tar -cf 1.tar [sources]	#-f(specify filename)指定文件名


#文件名必须紧跟在-f之后
tar -cvf txt.tar *.txt	#-v(verbose)详细信息


#向已归档文件中添加文件，-r
tar -rvf txt.tar *.html


#列出归档文件中的内容，-t
tar -tf txt.tar	#列出归档内容
tar -tvf txt.tar	#列出内容详细信息


#从归档文件中提取文件或文件夹，-x(exact)
tar -xf txt.tar	#默认提取到当前目录
#-C指定提取目录
tar -xvf txt.tar -C /dir/path
#只提取归档中特定文件
tar -xf txt.tar 1.txt 1.html -C /tmp	#只会提取1.txt和1.html文件


#在tar中使用stdin和stdout
tar -cvf - *.text | tar -xvf - -C /tmp


#拼接两个归档文件，-A
tar -Af txt.tar html.tar
tar -tvf txt.tat	#验证是否成功


#添加选项，可以将指定的任意文件加入到归档文件中。如果同名文件已存在，不会覆盖源文件，那么结果就是归档中包含了多个同名文件
#通过检查时间戳来更新对党文件中的内容，-u
#只有比归档文件中同名文件 更新(newer) 才添加
tar -uvf html.tar 1.html


#比较归档文件与文件系统中的内容，-d
tar -df txt.tar 1.txt 2.txt


#从归档文件中删除文件，--delete
tar -f txt.tar --delete 1.txt 2.txt


#从归档文件中排除部分文件,--exclude
tar -cf all.tar ./* --exclude="*.html"	#排除.html文件
tar -cvf txt.tar *.txt --exclude="1.txt"


#打印总字节数,--totals
tar -cf all.txt ./* --totals


#压缩tar归档文件，指定不同压缩格式

#-z,	.tar.gz
#-j,	.tar.bz2
#--lzma,	.tar.lzma,
#.tar.lzo
tar -czvf txt.tar.gzip *.txt
tar -xzvf txt.tar -C /dir/path


#tar后删除原文件
tar -czvf txt.tar.gz ./txt --remove-files
```


<br/>

## 用`cpio`归档

`cpio`是类似于tar的另一种归档格式。它多用于RPM软件包、Linux内核和initramfs文件等。
cpio通过stdin获取输入，并将归档写入stdout。

```sh
touch file{1..4}

echo file1 file2 file3 file4 | cpio -ov file.cpio
#-o指定输出，-v打印归档文件列表


#-i指定输入，-t列出归档中文件
cpio -it < file.cpio
```


<br>

## 用`gunzip`或`gzip`压缩

gzip是GNU/Linux下常用压缩格式。`gzip`,`gunzip`都可处理gzip压缩文件类型。
`gzip`只能够压缩单个文件，而无法对目录和多个文件进行归档。因此需要先交给`tar`，然后再用`gzip`压缩

```sh
gzip file	#file.gz，会覆盖原文件
gunzip file.gz	#file，也会删除原文件


#列出压缩文件的属性信息，-l
gzip -l file.gz


#指定gzip的压缩级别，--fast或--best
--fast	最低压缩比，最快速度完成
--best	最高压缩比，最慢速度完成


#将gzip与归档文件结合，-z
tar -czvf txt.tar.gzip ./*.txt
#-a指定从文件扩展名自动判断压缩格式
tar -cavf txt.tar.gzip ./*.txt


#tar只能从命令行中接收有限个文件，要解决这个问题，可以写一个循环并添加-r选项


#解压缩，-x
tar -xzvf txt.tar.gzip
tar -xavf txt.tar.gzip -C /dir/path
```


<br/>

## 用`bunzip`或`bzip`压缩

`bzip2`通常能够生成比gzip更小(压缩比更高)的文件。

```sh
bzip2 file	#file.bz2,同理会覆盖原文件
bzip2 file -k	#保留原文件
bunzip2 file.bz2	#解压缩
bunzip file.bz2 -k


#从stdin读入并写到stdout
cat file | bzip2 -c > file.bz2


#将bzip2与归档文件结合，-j
tar -cvjf 1.tar.bz2 ./1.*
tar -cavf 1.tar.bz2 ./1.*	#-a根据文件扩展名自动判断压缩格式
tar -xjvf 1.tar.bz2
tar -xavf 1.tar.bz2 -C /tmp


#压缩比
#从1级(速度最快，压缩率最低)到9级
bzip -9 -k file


#对成千上万的文件进行归档，需要借助 循环和-r选项
```


<br/>

## `lzma`压缩

`lzma`是一个较新的压缩工具，它提供了比gzip或bzip2更好的压缩率。
xz, unxz, xzcat, lzma, unlzma, lzcat - Compress or decompress .xz and .lzma files

```sh
lzma file	#file.lzma,同样也会删除原文件
lzma file -k 	#保留原文件
unlzma file.lzma

#从stdin读入并写入stdout
cat file | lzma -C > file.lzma

#与tar相结合,--lzma
tar -cvf 1.tar.lzma ./1.* --lzma
tar -cavf 1.tat.lzma ./1.*	#自动判断
tar -xvf 1.tar.lzma --lzma
tar -xavf 1.tar.lzma -C /tmp

#压缩率
#从1级到9级(压缩级别最高，速度最慢)

#对成千上万的文件，需要使用循环和-r选项
```


<br>
## zip归档和压缩

`zip`在Linux下不如`gzip`,`bzip2`那么广泛，但在Internet上的文件通常都采用这种格式。
zip - package and compress (archive) files

```sh
zip file.zip file
unzip file.zip
#与lzma,gzip,bzip2相比，zip完成后不会删除原文件


#对目录和文件进行递归操作,-r
zip -r dir.zip /root/test ./file


#向归档文件中增加内容，-u
zip dir.zip -u newfile


#从压缩文件中删除内容，-d
zip -d dir.zip file


#列出归档文件中内容
unzip -l dir.zip
```


<br>

## 超高压缩率的`squashfs`文件系统

`squashfs`是一种只读型的超高压缩率文件系统。这种文件系统能够将 2GB-3GB的数据压缩成一个700MB的文件。
你有没有想过Linux Live CD是怎样运行的？当Live CD启动后，它会加载一个完整的Linux环境。这就是利用了一种被称为squashfs的只读型压缩文件系统。它将根文件系统保存在一个压缩过的文件系统文件中。这个文件可以使用环回的形式来挂载并对其中的文件进行访问。一次当进程需要某些文件，可以将它们解压，然后载入内存中使用。
如果需要构建一个定制的Live OS，或是需要超高压缩率的文件并且无需解压就可以访问文件，那么squashfs的相关知识就能派上用场。要解压个头较大的压缩文件，需要花费不少时间。但如果将文件以环回形式挂载，速度就飞快，因为只有出现访问请求的时候，对应的那部分压缩文件才会被解压缩。而普通的解压缩方式是首先解压缩所有的数据。

环回文件系统就是指那些在文件中而非物理设备中创建的文件系统。比如我们可以创建一个文件，然后把这个文件格式化为我们常见ntfs、exfat或者ext4等文件系统格式，然后把它挂载在一个目录上使用。

如果你有一张Ubuntu CD，可以在CDRom Root/casper/filesystem.squashfs中找到文件.squashfs。
squashfs在内部采用了gzip和lzma这类压缩算法。

mksquashfs - tool to create and append to squashfs filesystems

```sh
yum install squashfs-tools -y

#创建squashfs文件
mksquashfs source compressfile.squashfs

mksquashfs /etc etc.squashfs
#/etc(67M) --> etc.suqashfs(18M)


#要挂载squashfs文件，利用环回形式进行挂载
mkdir /mnt/squash
mount -o loop etc.squashfs /mnt/squash
#此处挂载使用etc.squashfs文件系统
#如果直接查看etc.squashfs，就是一个普通文件，但是挂载以后所有文件都出现了
umount /mnt/squash


#在创建squashfs文件时排除指定文件，-e
mksquashfs /etc etc.squashfs -e /etc/passwd /etc/shadow /etc/*.txt
#在挂载之后就没有相关文件了
```


<br/>

## 加密工具与散列

加密技术主要用于防止数据遭受未经授权的访问。
Linux下某些工具用于执行加密和解密，使用加密算法散列值来验证数据完整性。

**`crypt`, `gpg`, `base64`, `md5sum`, `sha1sum`, `openssl`的用法**

### `ccypt`

ccrypt是为了取代UNIX crypt而设计的，这个实用工具可用于文件和数据流加密及解密。

ccrypt - encrypt and decrypt files and streams

```sh
ccrypt 1.txt	#会要求输入口令(encryption key)
#之后会生成1.txt.cpt覆盖原文件


#更改key,-x
ccrypt -x 1.txt.cpt	#输入old key和new key


#解密，-d(--decrypt)
ccrypt -d 1.txt.cpt	#输入key解密
```

### `gpg`

gpg(GNU privacy guard,GNU隐私保护)，是一种应用广泛的加密方案。
它采用签名密钥技术保护文件内容，只有经过认证的用户才能访问数据。我们对gpg签名早已耳熟能详。

gpg - OpenPGP encryption and signing tool

```sh
#加密，-c(--symmetric)对称加密
gpg -c file	#会要求输入口令(Passphrase)，生成file.gpg

#解密
gpg file.gpg
```

### `base64`

`base64`是一组类似的编码方案(encoding scheme)，它通过将ASCII字符转换成以64为基数的形式(radix-64 representation)来用ASCII字符串描述二进制数据。base64可用来对 编码和解码 base64字符串。

base64 - base64 encode/decode data and print to standard output

```sh
#将文件编码为base64格式
base64 file > outputfile
cat file | base64 > outputfile

#解码,-d
base64 -d outputfile > file
```

### `md5sum`与`sha1sum`

`md5sum` 和 `sha1sum` 都是单向散列算法(unidirecrional hash algorithm)，均无法逆推出原始数据。
它们通常用于验证数据完整性或为特定数据生成唯一的密钥，因为通过分析文件内容，它们可以为每个文件生成一个唯一的密钥。

这种类型的散列算法是存储密码的理想方案。密码使用其对应的散列值来存储。如果某个用户需要认证，读取该用户提供的密码并转换成散列值，然后将其与之前存储的散列值进行比对。
将密码以明文的形式存储是非常危险的事情，它面临密码泄露的危险。而因为 md5sum和sha1sum 是单向散列算法，所以密码使用散列值存储是很安全的。

```sh
echo "1.txt" > 1.txt
md5sum 1.txt	#生成密钥到stdout
#39061daa34ca3de20df03a88c52530ea  1.txt


sha1sum file	#生成密钥到stdout
#659fcbc505db207c03b5c4c0b6981d63286abe21  1.txt


#查看/etc/shadow中密码的散列值
awk 'NR==1' /etc/shadow | awk -F: '{print $2}'	#root密码散列
#$6$BxpV48gPsjuq6.pF$wE7pUDwtOI.v64kd5folG68yUt2UAQDTUGgKa5Iz69GaupEoRAdCeerP8nRKXo48c4azutUCGhnDgzd1qe8YX0
```

### shadowlike散列(salted散列)

shadow密码通常都是salted密码，所谓SALT就是额外的一个字符串，用来起一个混淆的作用，使加密更加不同里被破解。salt由一些随机位组成，被用作密钥生成函数的输入之一，以生成密码的salted散列值。

```sh
#/etc/passwd里面的密码散列类型就是salted散列


#查看root密码对应的散列值
head -1 /etc/shadow
root:$6$ZlHRCZG2iRwQUXAu$RAEDH97nPdZB2RK20npua6Qf6jB7osatoC99ow3LtPQ6aORdLISYC7/4iTYU162emkQLt4ZafdgjyAeoSB7IU0::0:99999:7:::


#openssl - OpenSSL command line tool

#shadow密码是使用openssl生成
#将SALT_STRING替换为随机字符串，同时将pass替换成你想测试的密码
openssl -1 -salt SALT_STRING passwd
```


<br>

## 用`rsync`备份系统

`rsync`借助差异计算以及压缩技术来最小化数据传输量。相较于`cp`命令，它的优势在于使用了高效的差异算法(difference algorithm)。
它还支持网络数据传输。在进行复制的同时，rsync会比较源端和目的端的文件，只有当文件有更新是才进行复制。默认情况下，rsync并不会在目的端删除源端已不存在的文件。

rsync - a fast, versatile, remote (and local) file-copying tool
inotifywait - wait for changes to files using inotify

```sh
#-a进行归档，-v详细信息
rsync -av source destination
rsync -av /etc /tmp


#异地cp
rsync -av source username@host:PATH
rsync -av username@host:PATH destination
#rsync借助于ssh，可以使用ssh无秘钥认证
rsync -av /etc zhang@192.168.1.11:~

#-z, --compress  compress file data during the transfer
rsync -avz zhang@192.168.1.11:/etc /tmp


#注意，路径格式
rsync /etc /tmp	#整个/etc目录
rsync /etc/ /tmp	#/etc目录下所有内容


#显示进度，--progress
rsync -avz --progress /etc /tmp


#排除部分文件，--exclude
rsync -avz /etc /tmp --exclude=/etc/nginx --exclude "*.txt"


#更新rsync时，删除不存在的文件，--delete
#默认情况下，rsync并不会在目的端删除源端已不存在的文件
rsync -avz /etc zhang@192.168.1.1:~ --delete


#定期调度
crontab -e
0 */10 * * * rsync -avz /etc user@host:PATH


#实时同步，inotifywait+rsync
yum install inotify-tools -y

#-m(monitor),-r(recursive),-q(--quiet)静默模式，-e(event)

vi inotify_rsync.sh
inotifywait -mrq -e creat,delete,modify,move --exclude "^.*\.filepart$" /etc | while read file
do
rsync -az --exclude=".*" --exclude="*.swp" --exclude=".filepart" --delete /etc /tmp > /dev/null 2>$1
done
```


<br/>
## 用Git备份版本控制

维护和恢复变更最好的方法是使用版本控制系统。由于代码变更频繁，版本控制系统多用于软件开发和代码维护。
Git(GNU it)是有名气也是最高效的版本控制系统。我们可在非编程环境下用Git备份普通文件。

git - the stupid content tracker

```sh
mkdir /home/zhang/gittest
cd /home/zhang/gittest



#在源主机中添加用户信息
git config --global user.name "username"	#设置用户名
git config --global user.email "someone@example.com"	#设置邮箱


#创建一个空的Git版本库或初始化一个老版本
git init


#记录变更到版本库
git commit


#添加远程git目录并同步备份
git remote add origin user@host:/home/zhang/gittest


#为git跟踪(git tracking)添加或删除文件
#add,添加内容至索引
git add *
#git add *.txt; git add *.ph	#添加部分文件


#删除不需要跟踪的文件和文件夹
#rm,从工作去和索引删除文件
git rm file
#git rm *.txt


#检查点或创建备份点(check point)
git commit -m "Commit Message"


#push,更新远程
git push


#用Git恢复数据
#log,显示提交日志
git log


#返回之前某个版本或状态
git checkout xxxxxxxx(Commit ID)


#clone,克隆一个版本库到本地
git clone URL
git clone user@host:PATH
```


<br>
## 用`dd`克隆磁盘

`dd`命令能用于克隆任何类型的磁盘，如硬盘、闪存、CD、DVD及软盘。
可能需要创建所有分区的副本而不仅仅是复制内容，包括硬盘分区、引导记录、分区表等信息。

使用dd的时候，要留意参数的顺序。错误的参数会损毁全部数据。dd基本上算是一个比特流复制器(bitstream duplicator),它可以将来自磁盘的比特流写入文件，也可以将来自文件的比特流写入硬盘。

dd - convert and copy a file

```sh
dd if=source of=target bs=block_size count=count
#bs块大小，count块数

dd if=/tmp/centos7.iso of=/dev/sdc


#/dev/zero是一个字符设备，它总是返回字符'\0'
dd if=/dev/zero of=./file bs=10m count=100


#用环回(loop back)方法可将任何由dd生产的文件镜像进行挂载
mount -o loop file /mnt
```



<br>
<br/>

# 无网不利

## 简介

网络是计算机系统中重要的部分。我们以Tcp/Ip为协议栈，所有操作都是基于它进行的。

一些使用网络的应用通过打开并连接到防火墙端口进行运作，而有的管理任务可以通过网络进行。



<br>

## 网络小知识

网络接口(Interface)用来连接网络。在每个系统中，默认都有一个称之为环回接口的lo，这个接口指向当前主机本身。
操作系统维护者一个被称为路由表(routing table)的表格，它包含了分组如何转发以及通过网络中的哪些节点转发的消息。
metric是路由算法用以确定到达目的地的最佳路径的计量标准，如路径长度。

```sh
#显示网络接口、子网掩码等详细信息
ifconfig	#/sbin/ifconfig


#显示某个特定接口
ifconfig eth0


#提取IP地址
ifconfig eth0 | egrep -o "inet [^ ]*" | grep -o "[0-9.]*"


#设置网络接口的IP地址和子网掩码
ifconfig eht0 192.168.1.11
ifconfig eth0 192.168.1.11 netmask 255.255.255.0
#远程的时候，千万别乱改IP，不然连不上你就要去机房了


#MAC地址欺骗
ifoconfig eth0 hw ether 11:22:33:44:55:66


#域名服务器与DNS
cat /etc/resolv.conf
#添加域名服务器
echo "name 114.114.114.114" >> /etc/resolv.conf
#nameserver 114.114.114.114


#一个域名可以分配多个地址，DNS只会返回其中一个
#要想获得域名所有IP地址，需要使用DNS查找工具


#DNS查找工具
host www.baidu.com
nslookup www.baidu.com


#自定义解析
cat /etc/hosts
echo "192.168.1.11 www.zhang.me" >> /etc/hosts


#设置默认网关，显示路由表信息
#路由表
route
route -n	#以数字形式显示地址


#设置默认网关
route add default gw $ip $interface
route add default gw 192.168.1.1 eht0


#显示分组途经的所有网关地址
traceroute www.baidu.com
```


<br/>

## `ping`

`ping`使用 **网际控制报文协议(Internet Control Message Protocol,ICMP)**的echo分组。如果分组能够送达且该主机为活动主机，那它就会发送一条回应。一旦主机不可到达，ping返回错误信息"Destination Host Unreachable"。

```sh
ping 192.168.1.1

#往返时间(Round Trip Time,RTT)

#发送分组数量
ping $URL -c 6
```


<br/>

## 列出网络上所有活动主机

当涉及大型局域网时，可能需要检查网络上的其他主机的活动状态。
一台非活动主机可能是：没有开机；网络连接有问题；主机禁ping；防火墙问题。

当我们要检测ip时，在一个脚本中，每一次`ping`都是依次执行。即使所有的ip地址都是彼此独立，由于编写的是顺式程序(sequential program)，`ping`命令也只能按顺序执行。每次执行一个`ping`命令。都要经历一段延迟——“发送echo分组，并接收或等待回应超时”。

要是处理几百个ip地址的话，这个延时就真不短了。我们可以使用并行方式来加速所有ping命令的执行。
可以将`ping`命令中的循环体放入**( )&** 中，**( )** 使其中的命令可作为子shell来执行，**&** 使之在后台继续运行。

```sh
#编写G一个并行方式的ping脚本
fo ip in 192.168.1.{1..255}
do
	(
	ping $ip -c2 &> /dev/null;
	if[ $? -eq 0 ]
		then
			echo "$ip is alive"
	fi
	)&
wait
done
#wait命令是脚本只有在所有子进程或后台进程全部终止或完成后才能结束


#使用fping,-a显示活动主机，-g生成目标列表,-u显示无法到达主机
fping -a 192.168.0.0/24 -g 2> /dev/null
fping -a 192.168.0.1 192.168.3.255 -g 2> ./unreach.txt
#将unreach主机找出
cat unreach.txt | egrep -o "to [0-9.]+$" | grep -o "[0-9.]*"
```


<br>

## 传输文件

有很多不同的方法可以在网络节点上传输文件，常见的协议有**FTP, SFTP, RSYNC, SCP**。

通过FTP传输文件可使用`lftp`命令；
通过SSH传输文件可使用`sftp`；
RSYNC使用`SSH`与`rsync`命令；
`scp`通过SSH进行传输。

<br>

文件传输协议(File Transfer Protocol, FTP)，使用21端口。FTP是明文传输，So...
需要远程主机上启用了FTP服务器才能使用FTP。

```sh
lftp user@ftp-host
#输入密码后便可以操作如下命令
cd -- lcd(本地)
mkdir
get filename	#下载文件
put filename	#上传文件
quit	#退出
```

<br/>

SFTP(Secure FTP,安全FTP)，运行在SSH连接之上。利用SSH连接模拟FTP接口。
它不需要源端运行FTP服务器，不要运行OpenSSH。SFTP是一个交互式命令，提供了命令提示符。


rsync广泛用于网络文件与系统快照的备份。


SCP(Secure Copy,安全复制)，远程文件复制工具。通过SSH加密通过进行传输。
```sh
scp SOURCE DESTINATION

scp /path/file user@host:PATH
scp usr@host:/dir/file /home/zhang
#需要输入密码，可以用SSH无秘钥认证

#-r递归复制,-p保持文件权限和模式
scp -r /etc user@host:/tmp
scp -rp user@host:/var/www  /var
```


<br/>

## SSH无秘钥认证

特别是在定时任务传输备份文件时，无秘钥认证就很方便了。SSH服务默认在22端口，你可以在配置文件中修改。

具体步骤：
1. 创建SSH密钥(公钥和私钥)；
2. 将客户端公钥上传给需要连接的主机，并写入~/.ssh/authorized_keys文件；
3. 修改相关目录(700)和文件权限(600)；

```sh
ssh-keygen -t rsa
#后续操作默认即可
#生成~/.ssh/id_rsa.pub和id_rsa


#写入远程主机
ssh user@host "cat >> ~/.ssh/authorized_keys" < ~/.ssh/id_rsa.pub
```


<br>

## 用SSH在远程主机上运行命令

```sh
#连接远程主机
ssh user@host


#非默认端口
ssh user@host -p 2211


#在远程主机中运行命令
ssh user@host 'command'
ssh user@host 'cmd1'; 'com2'...

ssh user@host 'whoami'


#-C压缩功能，当带宽有限时
ssh -C user@host 'cmd'
```


<br/>

## 在本地挂载远程驱动器(`sshfs`)

在执行读写数据操作时，通过本地挂载远程主机文件系统。利用SSH和sshfs来实现这一功能。
sshfs是FUSE文件系统的一个扩展，FUSE允许其支持的操作系统像使用本地文件系统一样挂载各类数据。
`sshfs`允许将远程文件系统挂载到本地挂载点上。

相当于便捷的NFS，但并不需要搭建NFS服务。

SSHFS - filesystem client based on ssh

```sh
#挂载远程文件到本地
ssh user@host:PATH /mnt/sshfs

umout /mnt/sshfs
```


<br>

## 网络流量和端口分析

应用程序在主机上打开端口，然后与远程主机中打开的端口实现通信。
出于安全方面的考虑，必须留意系统中打开及关闭的端口。

恶意软件和rootkit可能会利用特定的端口及服务运行在系统之中，从而进行攻击。
通过分析开放端口列表以及运行在端口上的服务，我们便可以分析并检查恶意软件，保证主机安全。

了解及使用各种端口分析工具。

lsof - list open files
`lsof`列出系统中开放端口以及运行在端口上的服务的详细信息，文件被哪个程序使用。

```sh
-a：列出打开文件存在的进程
-c<进程名>：列出指定进程所打开的文件
-g：列出GID号进程详情
-d<文件号>：列出占用该文件号的进程
+d<目录>：列出目录下被打开的文件
+D<目录>：递归列出目录下被打开的文件
-n<目录>：列出使用NFS的文件
-i<条件>：列出符合条件的进程（4、6、协议、:端口、 @ip ）
-p<进程号>：列出指定进程号所打开的文件
-u：列出UID号进程详情
-h：显示帮助信息
-v：显示版本信息


lsof /var/log/messages
COMMAND    PID USER   FD   TYPE DEVICE  SIZE/OFF     NODE NAME
rsyslogd 12231 root    5w   REG  253,0 539973467 68539162 /var/log/messages
```


netstat查看开放端口与服务
`netstat` - 显示网络连接，路由表，接口状态，伪装连接，网络链路信息和组播成员组;


iftop - display bandwidth usage on an interface by host
`iftop` - 展示带宽使用情况；


ifstat - handy utility to read network interface statistics
`ifstat` - 展示某时刻网络状态；


nload - displays the current network usage
`nload` - 可查看系统总带宽；


nethogs - Net top tool grouping bandwidth per process
`nethogs`- 可查看每个进程流量情况；
ethtool - query or control network driver and hardware settings
`ethtool` - 检查网卡支持的带宽


```sh
#lsof的每一项都对应着一个打开了特定端口的服务
lsof -i:port


#查看开放端口和服务
netstat -nltp


#查看网络实时状态
iftop


#查看当前网络状态
ifstat


#查看系统带宽
nload


#查看进程流量
nethogs
```


<br>
<br/>


## tcpdump


tcpdump是一款嗅探工具，也就是命令行格式的wireshark。

```
tcpdump - dump traffic on a network

tcpdump [options]
````

```
-a：尝试将网络和广播地址转换成名称；
-c<数据包数目>：收到指定的数据包数目后，就停止进行倾倒操作；
-d：把编译过的数据包编码转换成可阅读的格式，并倾倒到标准输出；
-dd：把编译过的数据包编码转换成C语言的格式，并倾倒到标准输出；
-ddd：把编译过的数据包编码转换成十进制数字的格式，并倾倒到标准输出；
-e：在每列倾倒资料上显示连接层级的文件头；
-f：用数字显示网际网络地址；
-F<表达文件>：指定内含表达方式的文件；
-i<网络界面>：使用指定的网络截面送出数据包；
-l：使用标准输出列的缓冲区；
-n：不把主机的网络地址转换成名字；
-N：不列出域名；
-O：不将数据包编码最佳化；
-p：不让网络界面进入混杂模式；
-q ：快速输出，仅列出少数的传输协议信息；
-r<数据包文件>：从指定的文件读取数据包数据；
-s<数据包大小>：设置每个数据包的大小；
-S：用绝对而非相对数值列出TCP关联数；
-t：在每列倾倒资料上不显示时间戳记；
-tt： 在每列倾倒资料上显示未经格式化的时间戳记；
-T<数据包类型>：强制将表达方式所指定的数据包转译成设置的数据包类型；
-v：详细显示指令执行过程；
-vv：更详细显示指令执行过程；
-x：用十六进制字码列出数据包资料；
-w<数据包文件>：把数据包数据写入指定的文件。
```

<br>

栗子：

```sh
#tcpdump默认将监视第一个网络接口上流过的数据包
tcpdump


#指定网络接口
tcpdump -i eth1 -w /tmp/1.cap


#指定主机
tcpdump host $hostname
tcpdump host $hostname1 and $hostname2


#指定源和目标主机
tcpdump -i eth0 src host $hostname
tcpdump -i eth0 dst host $hostname


#指定主机和端口
tcpdump tcp port 22 host 192.168.1.11
tcpdump udp port 53
```



<br>
<br/>

---

<br/>



# 当个好管家


## 简介

操作系统(Operation System,OS)，是由一系列用于不同目的、服务于不同任务的系统软件组成。
日志记录(logging)和监视是很重要的，能帮助我们从大量数据中收集信息。

监视系统活动的各种命令，日志技术及其使用方法。


<br>

## 统计磁盘使用情况(`df+du+fdisk`)

磁盘空间是一种有限资源，我们需要了解磁盘的可用空间。

`df`, `du`, `fdisk`是Linux中的磁盘管理三板斧
df(disk free): 报告文件系统磁盘空间的使用情况;
du(disk usage): 报告磁盘空间使用情况; 使用`du`时，要确保对其遍历的目录和文件拥有适合的读权限。
fdisk: Linux分区表操作工具软件。

```sh
du file1	#默认以字节为单位

#-a,显示目录下所有文件大小
du -a /home/zhang
du /home/zhang	#只显示目录大小


#-h,以可读形式打印
du -h /home/zhang


#-c,显示使用总量
du -c file1 /dir2
du -c *.txt *.sh


#-s，打印摘要
du -s /dir
du -sh /home/zhang


#-b,-k,-m,-B，用特定单位打印
du -k file1
du -m file2


#--exclude,从磁盘统计中排除部分文件
du --exclude="*.swap" -sh /home/zhang


#--max-depth,指定最大遍历深度
du -h --max-depth n /dir
du -h --max-depth=2 /home/zhang


#-x,将/mnt中所有挂载点排除在磁盘统计之外
du -xh /dir


#找出目录中最大的文件
du -ak /dir | sort -nrk 1 | head -n 5
#此输出包含了目录大小，需要细化
#利用find替du过滤文件
find /dir -type f --exec du -ak {} \; | sort -nrk 1 | head


#df,磁盘可用空间信息
df -h
```


<br>

## 计算命令执行时间

当测试一个应用程序或比较不同的算法时，程序的执行时间非常重要。所以需要计算命令执行时间。

所有的Unix-Like操作系统都包含time命令，可将time放在需要计算执行时间的命令前。

> time命令有个可执行二进制文件位于/usr/bin/time，还有一个shell built-in命令也叫作time；
> 当运行time时，默认调用的是shell built-in命令。內建time命令选项有限；
> 因此，如果我们需要使用另外的功能，就应该使用/usr/bin/time命令。

```sh
#计算命令执行时间
time command
time ls


#real,挂钟时间(wall clock time),命令从开始执行到结束的时间；
#user,指进程花费在用户模式(user-mode)中的CPU时间。这是唯一用于执行进程所花费的时间；
#sys，指进程花费在内核模式(in the kernel)中的CPU时间。它代表在内核中执行系统调用所使用的时间。


#-o,将命令执行时间写入文件
/usr/bin/time -o exetime.txt ls /

#-a,不影响原文件
/usr/bin/time -a -o exetime.txt ls /home

#-f,格式化时间输出
#时间格式字符串
#real	%e
#user	%U
#sys	%S
/usr/bin/time -f "FORMAT STRING" command
/usr/bin/time -f "Rtme: %e" -a -o timing.log uname
/usr/bin/time -f "Rtime: %e\nUtime: %U\nStime: %S" -ao timing.log uname
```


<br/>

## 当前登录用户、启动日志、启动故障的相关信息(`w+who+lastb+last`)

收集与操作系统、当前登录用户、主机运行时间、启动故障等相关信息很有用处。

```sh
#获取当前登录用户
who	#显示已经登录的用户
w	#显示已经登录的用户以及他们在做什么
#会显示用户使用的伪终端(pseudo TTY)，对应设备文件出现在/dev/pts/n

#列出登录主机的用户列表
users

#查看系统运行时间
uptime

#显示用户登录列表
last
#获取某个用户登录信息
last zhang
#获取重启会话信息
last reboot

#获取失败的用户登录信息
lastb
```


<br>

## 打印10条最常使用的命令(`history`)

终端是用来访问shell的工具，在shell中我们可以输入并执行命令。我们可以找出在shell中运行最多的命令。

~/.bash_history，默认保留1000个最近执行命令。或者`history`命令。

```sh
cat .bash_history | sort -n | uniq -c | sorn -nr | head
```


<br>

## 列出占用CPU最多的进程

CPU时间是一项重要资源，有时需要跟踪占用CPU周期最多的进程。
对于需要处理大量请求的服务器来说，CPU是极其重要的资源。通过监视某个时期内CPU的使用情况，可以找出长期占用CPU的进程并对其进行优化，或是调试其他问题。

用`ps`命令收集系统中进程的详细信息。
ps - report a snapshot of the current processes

```sh
#-e,以标准语法显示每个进程
ps -e
ps -ef


#ax,以BSD语法显示每个进程
ps ax
pa axu


#获取安全信息
#ps -eo euser,ruser,suser,fuser,f,comm,pcpu,label


#comm显示命令，pcpu显示CPU使用率
ps -eo comm,pcpu


#监视并计算一小时内CPU使用情况的shell脚本
secs=3600
unit_time=60
steps=$(($secs / $unit_time))

echo "Whatching CPU usage..."

for((i=0; i<steps; i++))
do
	ps -eo comm,pcpu | tail -n +2 >> /tmp/cpu_usage.$$
    sleep $unit_time
done

echo "CPU eaters: "

cat /tmp/cpu_usage.$$ | \
awk '{process[$1]+=$2}
END{
	for (i in process) {
    	printf("%-20s %s",i,process[i]);
    }
}' | sort -nrk 2 | head

#tail -n +K，从第K行开始输出。上面输出第一行是 COMAND 和 %CPU

#$1,command; $2,%CPU
#process[$1]是一个关联函数，相当于arr[command]
#arr[command]=arr[command]+ $2，计算同一命令的累积时间
#i指命令，process[i]指命令运行时间
```


<br/>

## 用`watch`监视命令输出

可能需要在在某段时期内以固定的间隔时间不短监视某个命令的输出。可利用`watch`命令。

watch - execute a program periodically, showing output fullscreen

```sh
#watch命令可以用来在终端以固定的间隔监视命令输出，默认2秒间隔
watch command
watch 'command'


watch ls
watch 'ls -l'


#-n,指定时间间隔
watch -n 5 'yum update -y'


#-d，突出(highlighting)watch输出中的差异
watch -d -n 1'dd if=/dev/zero of=/tmp/zero.test'
```


<br>

## 对文件及目录访问进行记录(`inotifywait`)

记录重要文件及目录访问，对于追踪文件和目录的变化很有帮助。
`inotifywait`命令可以用来收集有关文件访问的信息。
`inotifywait`和`rsync`用户实时同步哦！

inotifywait - wait for changes to files using inotify

```sh
yum install -y inotify-tools

#-q,减少冗余信息
inotifywait -m -r -q -e create,move,delete /dir
inotifywait -m -r -q -e create,move,modify,delete /home/zhang >> inotifywait.log

#利用inotifywait检测，rsync同步
inotifywait -mrq -e create,move,modify,delete /dir --exclude="*.swap" | while read file
do
rsync -av --exclude="*.swqp" --delete /dir user@host:PATH > /dev/null 2>&1
done
```


<br/>

## 用`logrotate`管理日志文件

日志文件是Linux系统维护中必不可少的组成部分。日志文件可以帮助跟踪系统中多种服务所发生的事件，这有助于排除系统问题。
但随着时间推移，日志文件会变得越来越大。因而必须对日志文件进行管理。

我们可以利用一种称为“轮询(rotation)”的技术来限制日志文件的体积。一旦日志文件超过了限定大小，就要对它的内容进行抽取(strip)，同时将日志文件的旧条目归档到文件中。

`logratate`是每一位Linux系统管理员都应该了解的命令。它能够将日志文件大大小限制在给定的SIZE内。
logrotate配置文件位于`/etc/logrotate.d`

logrotate ‐ rotates, compresses, and mails system logs

```sh
vim /etc/logrotated.d/custom

/var/log/custom.log {
	missingok	#日志文件丢失，则忽略
    notifempty	#仅当源日志文件非空时才进行轮替
    size 30k	#限制实施轮替的日志文件大小
    compress	#压缩旧日志
    weekly	#轮询时间，daily,weekly,yearly
    rotate 7	#保留旧日志数量
    create 0600 root root	#创建的日志文件模式，用户和用户组
#还有一些其他选项
}
```


<br>

## 用sys记录日志

在Linux系统中，在/var/log中创建并写入日志信息的是由被称为syslog的协议处理的。它由守护进程syslogd负责执行。
每一个标准应用进程都可以用syslog记录日志信息。

syslog处理/var/log下的多个日志文件。但是当logger发送消息时，它用标记字符串来确定应该纪录到哪一个日志文件中。
syslogd使用与日志相关联的TAG来决定应该将其记录到哪一个文件中。
可以从`/etc/rsyslog.d/`目录的配置文件中看到与日志文件相关联的标记字符串。

Linux中一些重要日志文件：
> /var/log/boot.log， 系统启动信息；
> /var/log/message， 内核启动信息；
> /var/log/auth.log， 用户认证日志；
> /var/log/dmesg， 系统启动信息；
> /var/log/mail.log， 邮件服务器日志。

logger - a shell command interface to the syslog

```sh
#logger命令，默认记录日志信息到/var/log/messages
logger "test log message to messages"
tail -n 1 /var/log/message

#-t，指定特定TAG
logger -t TAG "test log message to messages"
```



<br>
<br/>

# 管理重任


## 简介

GNU/Linux的生态系统是由运行的程序、服务、连接的设备、文件系统、用户等组成。按照我们需要的方式对整个系统有一个微观并对操作系统进行整体上的管理，这就是系统管理的主要目的。


## 收集进程信息(`top+ps+pgrep`)

进程是程序运行实例(runing instance)。
同一程序的多个实例可以同时运行，但他们的进程ID却互不相同。

进程管理相关的重要命令是：
- `top`, display Linux processes;
- `ps`, report a snapshot of the current processes;
- `pgrep`, look up or signal processes based on name and other attributes.

```sh
#ps命令
#-f, 显示更多进程信息
ps -f

#-e,every; -a,all
ps -ef
ps -ax

#-o, 指定想要的列
ps -e -o parameter1,parameter2...
ps -eo comm,pcpu,pmem


#pccpu	CPU占用率
#pid	进程ID
#ppid	父进程ID
#pmem	内存使用率
#comm	命令名
#cmd	简单命令
#user	启动进程的用户
#nice	优先级
#time	累积的CPU时间
#etime	进程启动后度过的时间
#tty	所关联的TTY设备
#euid	有效用户ID
#stat	进程状态


#--sort,根据参数对ps输出进行排序
#+升序，-降序
ps -eo comm,pcpu,pmem --sort -pcpu
ps -eo comm,pcpu,pmem --sort -pcpu,+pmem


#-C, 给定命令全名找出PID
ps -C cmd -o comm,pid


#-u, 指定有效用户列表
#-U, 指定真实用户列表
ps -u root -U zhang -o user,pcpu


#-t, 用TTY过滤输出
ps -t TTY1,TTY2...
ps -t pts/0,pts/1 -ef


#-L, 显示进程相关信息
#LWP线程ID， NLWP线程数量
ps -efL


#pgrep命令, 获得一个特定命令的PID列表
#它只需要命令的一部分即可
pgrep cmd
pgre inotif
pgrep bas

#-d, 指定定界符
pgrep rsync -d ":"

#-u, 指定进程的用户
pgrep -u root,zhang rsync

#-c, 返回匹配的进程数量
pgrep -c rsync


#top命令
top
```


<br/>
<br/>



## 杀死进程以及发送响应信息(`kill+killall+trap`)

在Unix-Like环境中与进程有关的一个重要概念就是信号。
信号是一种进程间通信机制，它用来中断运行的进程以执行某些操作。终止程序也是通过使用信号技术来实现的。

像`ctrl+C`,`ctrl+Z`这种作业都属于信号。

- `kill` 命令可用来向进程发送信号;
- `trap` 命令用来处理所接收的信号;
- `killall` 以名字方式来杀死进程.


```sh
#列出所有可用信号
kill -l


#-s, 发送信号
#信号名称和信号数都可以
kill -信号数 PID
kill -s SIGNAL PID


#常用信号
#SIGHUP   1    终端断线(对控制进程或终端进行挂起检测(hangup detection))
#SIGINT   2    中断(当按下Ctrl+C时发送该信号)
#SIGQUIT  3    退出(同Ctrl+\)
#SIGKILL  9    强制终止(强行杀死进程)
#SIGTERM  15   终止进程
#SIGCONT  18   继续(与STOP相反，fg/bg命令)
#SIGTST0P 19   暂停(当按下crtl+z时发送该信号)


#killall, 通过命令名终止进程
killall -s SIGNAL PName
killall -信号数 PName


#trap, 捕捉并响应信号
trap 'signal-handler-func' SIGNAL LIST
```

<br>

**kill信号详解**
参考: <https://www.imooc.com/article/48534>

```
$ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```

Linux信号列表：

- `SIGHUP 1`:  A 终端挂起或者控制进程终止
- `SIGINT 2`:  A 键盘中断（如break键被按下）
- `SIGQUIT 3`:  C 键盘的退出键被按下
- `SIGILL 4`:  C 非法指令
- `SIGABRT 6`:  C 由abort(3)发出的退出指令
- `SIGFPE 8`:  C 浮点异常
- `SIGKILL 9`:  AEF Kill信号
- `SIGSEGV 11`:  C 无效的内存引用
- `SIGPIPE 13`:  A 管道破裂: 写一个没有读端口的管道
- `SIGALRM 14`:  A 由alarm(2)发出的信号
- `SIGTERM 15`:  A 终止信号
- `SIGUSR1 30,10,16`:  A 用户自定义信号1
- `SIGUSR2 31,12,17`:  A 用户自定义信号2
- `SIGCHLD 20,17,18`:  B 子进程结束信号
- `SIGCONT 19,18,25`:  进程继续（曾被停止的进程）
- `SIGSTOP 17,19,23`:  DEF 终止进程
- `SIGTSTP 18,20,24`:  D 控制终端（tty）上按下停止键
- `SIGTTIN 21,21,26`:  D 后台进程企图从控制终端读
- `SIGTTOU 22,22,27`:  D 后台进程企图从控制终端写

处理动作中的字母含义：

- `A`: 缺省的动作是终止进程
- `B`: 缺省的动作是忽略此信号，将该信号丢弃，不做处理
- `C`: 缺省的动作是终止进程并进行内核映像转储（dump core），内核映像转储是指将进程数据在内存的映像和进程在内核结构中的部分内容以一定格式转储到文件系统，并且进程退出执行，这样做的好处是为程序员 提供了方便，使得他们可以得到进程当时执行时的数据值，允许他们确定转储的原因，并且可以调试他们的程序。
- `D`: 缺省的动作是停止进程，进入停止状况以后还能重新进行下去，一般是在调试的过程中（例如ptrace系统调用）
- `E`: 信号不能被捕获
- `F`: 信号不能被忽略



<br/>
<br/>



## `which`, `whereis`, `file`, `whatis`与平均负载

`which` hows the full path of (shell) commands。找出某个命令的位置;
`whereis` locate the binary, source, and manual page files for a command。不仅返回命令路径，还能打印命令手册的位置以及命令源代码路径;
`file` determine file type。用来确定文件类型;
`whatis` display manual page descriptions。输出简短描述信息;
平均负载(load average),是系统运行总负载量的一个重要参数。它指明了系统中可运行进程总量的平均值。平均负载由三个值来指定，第一个指明1分钟内的平均值，第二个指明5分钟内的平均值，第三个指明15分钟内的平均值。

* 单核CPU，类似于单车道，负载在 0.00-1.00 之间正常；
* 多核CPU，类似于多车道，负载在 核数*(0.00-1.00) 之间正常；
* 安全的系统负载，单核应该在 0.7 以下；

```sh
#查看平均负载
uptime
cat /proc/loadavg
#0.00 0.01 0.05 1/355 44955
#分母355表示系统进程总数, 分子表示正在运行的进程数, 最后一个数字表示最近运行进程ID
```


<br/>

## 向用户终端发送消息

系统管理员可能需要向网络中所有主机上的所有用户或特定用户的终端发送消息。
`wallrsync -av --exclude="*.s命令用来向所有当前登录用户的终端写入消息。

在Linux系统中，终端是作为设备存在的。因此那些打开的终端在`dev/pts/`中都会与对应的设备节点文件。向特定设备写入数据将会在对应的终端显示出消息。

```sh
echo "It's just a test" | wall

#查看用户对应的/dev/pts/, 并向某一个用户终端发送信息
ll /dev/pts | awk '{print $3,$6}'
echo"Haha" > /dev/pts/[1,2,3...]
```


<br/>

## 收集系统信息

包括主机名、内核版本、Linux发行版本、CPU信息、内存信息、磁盘分区信息等。

```sh
#主机名
hostname
uname -n

#内核版本，架构
uname -r
uname -m
uname -a

#Linux发行版本
cat /etc/redhat-release

#CPU相关信息
lscpu
cat /proc/cpuinfo
cat /proc/cpuinfo | grep 'model name'

#内存详细信息
free -h
cat /proc/meminfo

#分区信息
cat /proc/partitions
fdisk -l

#系统详细信息
lshw
```


<br>

## 用/proc收集信息

在GNU/Linux操作系统中，/proc是一个位于内存中的伪文件系统(in-memory pseudo filesystem)。
它的引用是为了提供一个可以从用户空间(user space)读取系统参数的接口。

可以对`/proc`中的文件和子目录进行`cat`来获取信息，所有内容都是易读的格式化文本。

> /proc/下的数字目录，包含了对应进程的相关信息；
> /proc/environ，包含于进程相关联的环境变量；
> /proc/cwd，是一个到进程工作目录的符号链接；
> /proc/fbcat，包含了由进程所使用的文件描述符。



<br/>

## 用cron进行调度

GNU/Linux系统包含了各种用于调度任务的工具。cron就是其中之一，它通过守护进程crond使得任务能够以固定的时间间隔在系统后台自动运行。
cron利用的是一个被称为“cron表(cron table)”的文件，这个文件中存储了需要执行的脚本或命令的调度列表以及执行时间。

```sh
#分 时 日 月 周
#*  *  *  *  *  cmd
#分钟(0-59)
#小时(0-23)
#天(1-31)
#月(1-12)
#工作日(0-7)，0和7都代表周天
#命令

#*号,所有值
#,号,范围。1,3,5,7,9
#-号,连续范文。1-10
#/号,*/10;0-8/20



#栗子
crontab -e

* 0-6 * * * /home/zhang/test.sh
1,3,5,7,9 * * * * /home/zhang/test.sh
*/5 * * * * /home/zhang/test.sh

#-l,查看cron表
crontab -l

#-r,移除cron表
crontab -r
```

### `cron`的高级写法

栗子：

```sh
@reboot  #在启动的时候运行一次
#其实@reboot类似于rc.local，开机启动

@yearly == @annually == 0 0 1 1 *  #一年一次

@monthly == 0 0 1 * *  #每月一次

@weekly  == 0 0 * * 0  #每周一次

@daily == @midnight == 0 0 * * *  #每天一次

@hourly == 0 * * * *  #每小时一次


crontab -e
@reboot /bin/mongod -f /etc/mongod_27018.conf

vim /etc/rc.d/rc.local
/bin/mongod -f /etc/mongod_27018.conf
chmod a+x /etc/rc.d/rc.local
```




<br>

## 用户管理常用命令

```sh
#添加用户
useradd


#删除用户
userdel
--remove-all-file删除与用户相关的所有文件


#修改shell
chsh


#修改用户属性
usermod


#修改密码过期时间
chage


#修改密码
passwd


#登录到一个新组
newgrp


#添加、删除组
groupadd
groupdel


#指纹
finger
```



<br>
<br/>

---

<br/>



# iptables和firewalld


**firewalld与iptables比较: **

- iptables与firewalld都不是真正的防火墙，它们都只是用来定义防火墙策略的防火墙管理工具而已。或者说，它们只是一种服务
- firewalld可以动态修改单条规则，动态管理规则集，允许更新规则而不破坏现有会话和连接；iptables在修改了规则后必须得全部刷新才可以生效
- firewalld使用区域和服务而不是链式规则
- firewalld默认是拒绝的，需要设置以后才能放行；iptables默认是允许，需要拒绝的才去限制
- firewalld自身并不具备防火墙的功能，而是和iptables一样需要通过内核的netfilter来实现。真正使用规则干活的是内核的netfilter
- firewalld是iptables的一个封装，可以让你更容易地管理iptables规则。它并不是iptables的替代品
- firewalld拥有CLI和GUI的两种管理方式


<br/>
<br/>


## firewalld


### 区域管理


通过将网络划分成不同的区域，制定出不同区域之间的访问控制策略来控制不同程序区域间传送数据流。
firewalld的默认区域是public区域。

九大区域：

- 阻塞区域（block）
任何传入的网络数据包都将被阻止

- 工作区域（work）
相信网络上的其他计算机，不会损害你的计算机

- 家庭区域（home）
相信网络上的其他计算机，不会损害你的计算机

- 公共区域（public）
不相信网络上的任何计算机，只有选择接受传入的网络连接

- 隔离区域（DMZ）
隔离区域也称为非军事区域，内外网络之间增加的一层网络，起到缓冲作用。对于隔离区域，只有选择接受传入的网络连接

- 信任区域（trusted）
所有的网络连接都可以接受

- 丢弃区域（drop）
任何传入的网络连接都被拒绝

- 内部区域（internal）
信任网络上的其他计算机，不会损害你的计算机。只有选择接受传入的网络连接

- 外部区域（external）
不相信网络上的其他计算机，不会损害你的计算机。只有选择接受传入的网络连接


firewalld有三种配置方法：

- firewll-config(GUI)
- firewall-cmd(CLI)
- 编辑XML配置文件

<br>

firewalld默认提供了九个区域的配置文件，它们位于`/usr/lib/firewalld/zones`:

```
ls /usr/lib/firewalld/zones

block.xml  dmz.xml  drop.xml  external.xml  home.xml  internal.xml  public.xml  trusted.xml  work.xml
```

常用命令:

```
yum  install  firewalld  firewall-config
systemctl start firewalld
firewall-cmd --version
firewall-cmd --help

#永久生效需加上 --permannent
firewall-cmd xxx --permannent


firewall-cmd --state


#查看网络接口使用的区域
firewall-cmd --get-active-zones


#查看指定区域的所有配置
firewall-cmd --zone=public --list-all


#查看所有区域配置
firewall-cmd --list-all-zones


#查看默认区域
firewall-cmd --get-default-zone


#设置默认区域
firewall-cmd --set-default-zone=internal


#查看指定接口所属区域
firewall-cmd --get-zone-of-interface=eth0


#将接口添加到区域，默认接口都在public
firewall-cmd --zone=public --add-interface=eth0


#拒绝|开启 所有包
firewall-cmd --panic-on|off


#查看是否拒绝
firewall-cmd --query-panic


#无需断开连接更新防火墙规则
firewall-cmd --reload


#类似于重启更新规则
firewall-cmd --complete-reload


#查看所有打开的端口
firewall-cmd --zone=dmz --list-ports


#加入一个端口的区域
firewall-cmd --zone=dmz --add-port=8080/tcp

```

<br/>
<br/>

### 与服务一起使用


firewalld可以根据特定网络服务的预定义规则来允许相关流量。你可以自定义系统规则，并将它们添加到任何区域。

- 默认支持的服务的配置文件位置: `/usr/lib/firewalld/services`
- 创建的服务文件位置: `/etc/firewalld/services`

```
cat /usr/lib/firewalld/service/elasticsearch.xml

<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>Elasticsearch</short>
  <description>Elasticsearch is a distributed, open source search and analytics engine, designed for horizontal scalability, reliability, and easy management.</description>
  <port protocol="tcp" port="9300"/>
  <port protocol="tcp" port="9200"/>
</service>
```

常用命令:

```
#查看默认可用服务
firewall-cmd --get-services


#永久启用或禁用HTTP服务
firewall-cmd --zone=区域 --(add|remove)-service=http --permanent


#添加123456端口的tcp流量
firewall-cmd --zone=public --add-port=123456/tcp --permanent


#将80端口的流量转发到123456端口
firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=123456
```


<br>
<br/>


## iptables

iptables/ip6tables — administration tool for IPv4/IPv6 packet filtering and NAT

切记谨慎使用iptables命令，特别是在远程连接的时候。
规则是有顺序的，规则的顺序很重要。当规则顺序排列错误时，会产生很严重的错误。

```
iptables --help

-t<表>： 指定要操纵的表，默认为filter

-P： 设置默认策略

-A <链>： 在规则链的末尾中添加条目
-I <链>： 在规则链的头部中插入条目
#请注意-A与-I，这两者的插入顺序是不一致的，-I顺序更高
-D <链>： 从规则链中删除条目
-R： 替换规则链中的条目

-L： 显示规则链中已有的条目
-F： 清楚规则链中已有的条目
-Z： 清空规则链中的数据包计算器和字节计数器
-X： 删除用户定义的链
-N： 创建新的用户自定义规则链

-p： 指定要匹配的数据包协议类型(tcp, udp, icmp...)

-s： 指定要匹配的数据包源ip地址(ip/mask, !ip)
-d： 匹配 目标地址
--sport： 匹配源端口号
--dport： 匹配目的端口号

-i<网络接口>： 指定数据包进入本机的网络接口
-o<网络接口>： 指定数据包要离开本机所使用的网络接口

-j target： 指定要跳转的目标
-m match： 扩展匹配
-g chain： jump to chain with no return
```


<br/>
<br/>


### 表格/链/动作

为什么称为iptables? 因为此软件里面有多个表格(table)，每个表格定义了自己的默认策略和规则，且每个表格的用途都不相同。

**表(Table)**

- raw: 高级功能
- mangle: 数据包修改
- nat: 网络地址转换
	+ PREROUTING
	+ POSTROUTING
	+ OUTPUT

- filter: 包过滤，是默认表
	+ INPUT
	+ OUTPUT
	+ FORWARD

<br>

**链(Chain)**

- INPUT：处理输入数据包
- OUTPUT：处理输出数据包
- FORWARD：处理转发数据包
- PREROUTING：用于目标地址转换(DNAT)
- POSTOUTING：用于源地址转换(SNAT)

<br>

**动作(Action)**

- ACCEPT： 接收数据包
- DROP： 丢弃数据包
- REJECT: 拒绝
- REDIRECT： 重定向、映射、透明代理
- SNAT： 源地址转换
- DNAT： 目标地址转换
- MASQUERADE： IP伪装（NAT），用于ADSL
- LOG： 日志记录


<br>
<br/>


### 常用命令


```sh
#格式
iptables -t 表名 <-A/I/D/R> 规则链名 [规则号] <-i/o 网卡名> -p 协议名 <-s 源IP/源子网> --sport 源端口 <-d 目标IP/目标子网> --dport 目标端口 -j 动作



-A: 新增一条规则，在最后面
-I: 插入一条规则，如果没有指定顺序，默认变成第一条规则
--line-numbers： 显示规则行号


-i: 包所进入的那个网络接口
-o: 包所传出的那个网络接口


-p: 指定网络协议
-p tcp --syn(ack, rst)
-p udp
-p icmp
-p all

-s: 源IP或网段
-d: 目标IP或网段
-s 192.168.1.11
-d 192.168.1.0/24(192.168.1.0/255.255.255.0)
-s !192.168.2.0/24


-j: 后接动作


#记录，此日志默认追加到messages
-j LOG --log-prefix=‘IPTABLES-’




#端口号可以是连续的
--sport 1026:65535
--dport 80


-m: 一些iptables外部模块
-m state: 状态模块
-m mac: 网卡地址


--state: 数据包状态
--state INVALID： 无效的数据包
--state ESTABLISHED: 已建立连接
--state NEW: 想要新建连接的数据包
--state RELATED: 表示这个数据包是我们主机发送出去的


--mac-source: 源主机的硬件地址
```

<br>

**清除防火墙规则:**

```sh
iptables  [ -t  tables ]  [ -FXZ ]

#清除所有已制定的规则
iptables -F

#清除用户 "自定义"
iptables -X

#将所有链表的计数与流量统计都归零
iptables -Z


#这三个命令会将本机防火墙的所有规则都清除，但却不会改变 默认策略
```

<br>

**定义默认策略**

```sh
--policy, -P

iptables -P INPUT DROP
iptables -P OUTPUT ACCESS
iptables -P FORWARD ACCEPT
```

<br>

**开放某个端口**

```sh
#允许本地回环接口(即运行本机访问本机)
iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT

#允许已建立的或相关连的通行
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

#针对网卡执行的放行和防御
iptables  -A  INPUT  -m  mac  --mac--source  aa:bb:cc:11:22:33  -j  DROP

#允许所有本机向外的访问
iptables -A OUTPUT -j ACCEPT

#允许访问22端口
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

#允许访问80端口
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# #禁止其他未允许的规则访问
iptables -A INPUT -j reject

#禁止其他未允许的规则访问
iptables -A FORWARD -j REJECT

```

<br>

**屏蔽IP**

```sh
#屏蔽单个IP的命令
iptables -I INPUT -s 123.45.6.7 -j DROP

#封整个段即从123.0.0.1到123.255.255.254的命令
iptables -I INPUT -s 123.0.0.0/8 -j DROP

```

<br>

**查看规则**

```sh
#推荐使用iptables-save
iptables-save

#备份和恢复
iptables-save >/etc/sysconfig/iptables
iptables-restore </etc/sysconfig/iptables


iptables  -L  -n
iptables -L -n --line-numbers
```

<br>

**删除已添加规则**

```sh
iptables -L -n --line-numbers

iptables -D INPUT 5
iptables -D OUTPUT 3
```


<br>
<br/>


### 解决重启失效


```sh
iptables-save >/etc/sysconfig/iptables

#把此加入开机启动
iptables-restore </etc/sysconfig/iptables

```



<br/>
<br/>

---

<br/>


# 分屏显示


`tmux`命令 — terminal multiplexer

- 上下分屏: `ctrl+b -> "`
- 左右分屏： `ctrl_b -> %`
- 切换屏幕： `ctrl+b -> o`
- 关闭终端： `ctrl+b -> x`
- 上下屏与左右屏切换： `ctrl+b -> 空格`
- 帮助： `ctrl+b -> ?`
- 命令模式： `ctrl+b -> :`










<br/>
<br/>

---

<br/>
<br/>











# 运维常见命令


以下命令来自：[知乎-运维工程师技能-知道创宇的回答](https://www.zhihu.com/question/23665108)

![](/images/Shell/ops-cmds.jpg)


<br/>

- **I/O, Device**:
  - `blktrace`: 收集磁盘IO信息中当IO进行到块设备层时的详细信息
  - `perf`: 全称Performance Event，是用来进行软件性能分析的工具。它不但可以分析指定应用程序的性能问题，也可以用来分析内核的性能问题，当然也可以同时分析应用代码和内核，从而全面理解应用程序中的性能瓶颈。
  - `iotop`: 用来监视磁盘I/O使用状况的top类工具
  - `iostat`: 用于监视系统输入输出设备和CPU的使用情况。它的特点是汇报磁盘活动统计情况，同时也会汇报出CPU使用情况。同vmstat一样，iostat也有一个弱点，就是它不能对某个进程进行深入分析，仅对系统的整体情况进行分析。
- **Network**:
  - `ping`: 测试主机之间网络的连通性。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。
  - `ip`: 来显示或操纵Linux主机的路由、网络设备、策略路由和隧道，是Linux下较新的功能强大的网络配置工具。
  - `ifconfig`: 配置和显示Linux内核中网络接口的网络参数。用ifconfig命令配置的网卡信息，在网卡重启后机器重启后，配置就不存在。要想将上述的配置信息永远的存的电脑里，那就要修改网卡的配置文件了。
  - `dig`: 域名查询工具，可以用来测试域名系统工作是否正常。
  - `iftop`: 实时流量监控工具,监控TCP/IP连接等,缺点就是无报表功能。
  - `ifstat`: 网络接口监测工具,比较简单看网络流量。
  - `nload`: 查看系统带宽
  - `neghogs`: 查看进程流量
  - `ethtool`: 检查网卡支持的带宽
  - `tcpdump`: 是一款sniffer工具，它可以打印所有经过网络接口的数据包的头信息
  - `netstat`: 打印Linux中网络系统的状态信息，可让你得知整个Linux系统的网络情况。
  - `nicstat`: 是一款分析网卡流量信息的工具
  - `sar`: 是Linux下系统运行状态统计工具，它将指定的操作系统状态计数器显示到标准输出设备。sar工具将对系统当前的状态进行取样，然后通过计算数据和比例来表达系统的当前运行状态。它的特点是可以连续对系统取样，获得大量的取样数据。取样数据和分析的结果都可以存入文件，使用它时消耗的系统资源很小。
  - `/proc`: 一个虚拟文件系统
- **FS**:
  - `fdisk`: 用于观察硬盘实体使用情况，也可对硬盘分区
  - `du`: 对文件和目录磁盘使用的空间的查看
  - `df`: 显示磁盘分区上的可使用的磁盘空间
- **Scheduler, VM**:
  - `strace`: strace命令是一个集诊断、调试、统计与一体的工具，我们可以使用strace对应用的系统调用和信号传递的跟踪结果来对应用进行分析，以达到解决问题或者是了解应用工作过程的目的。
  - `vmstat`: 显示虚拟内存状态，但是它可以报告关于进程、内存、I/O等系统整体运行状态。
  - `slabtop`: 以实时的方式显示内核slab缓冲区的细节信息
  - `dstat`: 是一个全能系统信息统计工具
  - `free`: 显示当前系统未使用的和已使用的内存数目，还可以显示被内核使用的内存缓冲区
  - `perf`:
  - `top`: 实时动态地查看系统的整体运行情况，是一个综合了多方信息监测系统性能和运行信息的实用工具
  - `pidstat`: 用于监控全部或指定进程的cpu、内存、线程、设备IO等系统资源的占用情况
  - `mpstat`: 主要用于多CPU环境下，它显示各个可用CPU的状态






























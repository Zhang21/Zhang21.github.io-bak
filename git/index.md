# Git



参考：

- [git-scm文档](https://git-scm.com/book/zh/v2)
- [廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [Git内部原理](https://www.kancloud.cn/kancloud/progit/70239)
- [Git内部原理-伯乐在线](http://blog.jobbole.com/26209/)




<br>
<br/>

<!--more-->

---

<br/>
<br/>






# 介绍


git(`/ɡɪt/`)是一个分布式版本控制软件,最初由林纳斯·托瓦兹（Linus Torvalds）创作，于2005年以GPL发布。Git是免费的。

林纳斯·托瓦兹自嘲地取了这个名字“git”，该词源自英国俚语，意思大约是“混账”。

<br>

![Git](/images/git.jpg)


<br/>


## 集中式与分布式

集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。
集中式版本控制系统最大的毛病就是必须联网才能工作。
常用集中式版本控制系统有：CVS、SVN。

分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
常用分布式版本控制系统有：Git。

和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。


<br/>
<br/>



# 常用命令


![](/images/Git/Git_Commands.png)

几个专用名词:

- `Workspace`：工作区
- `Index/Stage`：暂存区
- `Repository`：仓库区（或本地仓库）
- `Remote`：远程仓库


<br>

常用命令：

```sh
#配置
#Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下
#--global全局配置
git config --global user.name "Username"
git config --globla user.email "Email"





#创建版本库
#虽然在任意目录下都可创建git-repo，但还是建议在一个空目录下创建git-repo
mkdir gitest&&cd gitest

#init, Create an empty Git repository or reinitialize an existing one
git init
#生成了一个.git目录，这个目录是git用来追踪管理版本库的，不要随意修改此目录的内容

echo "First Git test" > README




#增加/删除文件

#所有的版本控制系统，只能跟踪文本文件的改动
#把文件添加到暂存区

#git add file
#git add dir
#当前目录
#git add .
git add　README


#删除
#git rm file

#改名
#git move file





#提交

#把暂存区提交到仓库区

#-m, 为本次提交的说明信息
git commit -m "Update Readme"



#为什么Git添加文件需要add, commit一共两步呢？
#因为commit可以一次提交很多文件，所以你可以多次add不同的文件。
git add file1 file2 file3
git commit -m "add 3files"







#分支

#查看分支
git branch
git branch -a

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 新建一个分支，与指定的远程分支建立追踪关系
git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
git checkout [branch-name]

# 建立追踪关系，在现有分支与指定的远程分支之间
git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
git merge [branch]


# 删除分支
git branch -d [branch-name]

# 删除远程分支
git push origin --delete [branch-name]
git branch -dr [remote/branch]







#标签
git tag







#远程同步
# 下载远程仓库的所有变动
git fetch [remote]

# 显示所有远程仓库
git remote -v

# 显示某个远程仓库的信息
git remote show [remote]

# 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
git pull [remote] [branch]

# 上传本地指定分支到远程仓库
git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force

# 推送所有分支到远程仓库
git push [remote] --all






#撤销

# 恢复暂存区的指定文件到工作区
git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
git stash
git stash pop







#其它

# 生成一个可供发布的压缩包
$ git archive
```


<br/>
<br/>

---

<br/>
<br/>



# 内部原理


Git 的内部工作原理和实现方式，学习这些内容对于理解 Git 的用处和强大功能是非常重要的。
首先要弄明白一点，从根本上来讲 Git 是一套**内容寻址**(content-addressable) 文件系统——它是一个相当酷的东西，在此之上提供了一个 版本控制系统(VCS)用户界面。


<br/>


## 底层命令和高层命令


由于 Git 最初是一套面向版本控制系统的工具集，而不是一个完整的、用户友好的版本控制系统，所以它还包含了一部分用于完成底层工作的命令。 这些命令被设计成能以 UNIX 命令行的风格连接在一起，抑或藉由脚本调用，来完成工作。 这部分命令一般被称作**底层命令**（plumbing），而那些更友好的命令则被称作**高层命令**（porcelain）。

Git Repo下有一个`.git`目录，几乎所有 Git 存储和操作的内容都位于该目录下。如果你要备份或复制一个库，基本上将这一目录拷贝至其他地方就可以了。

`.git`内容:

- `branches/`
- `config`: 包含项目特有的配置选项
- `description`: 仅供 GitWeb 程序使用
- `HEAD`： Git核心部分，指向当前分支
- `hooks/`： 客户端或服务端钩子脚本
- `index`： Git核心部分，保存了暂存区域信息
- `info/`:  保存了一份不希望在`.gitignore`文件中管理的忽略模式的全局性排除文件
- `objects/`： Git核心部分，存储所有数据内容
- `refs/`： Git核心部分，存储指向数据 (分支) 的提交对象的指针

#可能还有其它文件

- `COMMIT_EDITMSG`
- `FETCH_HEAD`
- `logs/`
- `ORIG_HEAD`



<br/>
<br/>
<br/>



## Git对象


Git 是一套内容寻址文件系统。从内部来看，Git 是简单的 key-value 数据存储。它允许插入任意类型的内容，并会返回一个键值，通过该键值可以在任何时候再取出该内容。可以通过底层命令 `hash-object` 来示范这点，传一些数据给该命令，它会将数据保存在 .git 目录并返回表示这些数据的键值。
Git 初始化了 `objects` 目录，同时在该目录下创建了 `pack` 和 `info` 子目录。 新建文件后，此目录中会出现新目录(目录内容为哈希值)。


```sh
echo &#039;test content&#039; | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4

#也可以直接添加文件
#git hash-object -w test.txt


find .git/objects -type f
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4


#使用git cat-file取回对象内容
git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4
test content
```



<br/>
<br/>



### 树对象

tree object


tree 对象可以存储文件名，同时也允许存储一组文件。Git 以一种类似 UNIX 文件系统但更简单的方式来存储内容。所有内容以 tree 或 blob 对象存储，其中 tree 对象对应于 UNIX 中的目录，blob 对象则大致对应于 inodes 或文件内容。

```sh
git cat-file -p master^{tree}
100644 blob a38f463bb1bdb05ce38cf7d9cfd4ed11286a708c    README.md
040000 tree f7cda840304ce5729444dd60904a0bbdf95fa0b5    test


#master^{tree} 表示 branch 分支上最新提交指向的 tree 对象
#请注意 test 子目录并非一个 blob 对象，而是一个指向别一个 tree 对象的指针
git cat-file -p f7cda840304ce5729444dd60904a0bbdf95fa0b5
040000 tree 02e9e8af4ae37db0fcd1943fdc832b27ad03ea03    2018


#这才是两个blob对象
git cat-file -p 02e9e8af4ae37db0fcd1943fdc832b27ad03ea03
100644 blob 7066d2a8d6f8e3c6c44bdbf3a775c96cc9d0a3b9    2018-12-14.md
100644 blob c0a915bf91f77db873a425058391e500c1504bae    2018-12-21.md
```

<br>

你可以自己创建 tree 。通常 Git 根据你的暂存区域或 index 来创建并写入一个 tree 。因此要创建一个 tree 对象的话首先要通过将一些文件暂存从而创建一个 index 。

```sh
#指定了文件模式为 100644，表明这是一个普通文件
git update-index --add --cacheinfo 100644 \
83baae61804e65cc73a7201a7252750c76066a30 test.txt
```


<br/>
<br/>



### 提交对象

commit object


```sh
#创建commit 对象
echo &#039;first commit&#039; | git commit-tree d8329f
fdf4fc3344e67ab068f836878b6c4951e3b15f3d


#查看
git cat-file -p fdf4fc3
```



<br/>
<br/>
<br/>



## Git References

引用(references, refs)，可在`.git/refs`下查看相关内容。

```sh
#有三个目录
ls .git/refs
heads  remotes  tags
```



<br/>
<br/>



### HEAD

当你执行 `git branch <branch-name>` 这条命令的时候，Git 怎么知道最后一次提交的散列值呢？答案就是 HEAD 文件。HEAD 文件是一个指向你当前所在分支的引用标识符。

```sh
#这就是上面.git/refs目录下

cat .git/HEAD
ref: refs/heads/test


git checkout master

cat .git/HEAD
ref: refs/heads/master



#你也可以设置它的值
#但不能设置为refs以外的值，会报错
git symbolic-ref HEAD refs/heads/test

cat .git/HEAD
ref: refs/heads/test
```



<br/>
<br/>



### Tags

Tag 对象指向一个 commit 而不是一个 tree。它就像是一个分支引用，但是不会变化——永远指向同一个 commit，仅仅是提供一个更加友好的名字。
Tag 有两种类型：annotated 和 lightweight 。



<br/>
<br/>



### Remotes


如果你添加了一个 remote 然后推送代码过去，Git 会把你最后一次推送到这个 remote 的每个分支的值都记录在refs/remotes 目录下。


```sh
git log
commit 1deea62fa622d00db113abe136e9b22e1f3eac4c
xxxxxxxxx

cat .git/refs/heads/origin/master
1deea62fa622d00db113abe136e9b22e1f3eac4c


#切换分支
git checkout test

git log
commit 3e1fcc1f8c035320d985b0202e18a6a00fe068c0

cat .gt/refs/heads/origin/test
3e1fcc1f8c035320d985b0202e18a6a00fe068c0
```



<br/>
<br/>
<br/>



## Packfiles


Git 往磁盘保存对象时默认使用的格式叫松散对象 (loose object) 格式。Git 时不时地将这些对象打包至一个叫 packfile 的二进制文件以节省空间并提高效率。当仓库中有太多的松散对象，或是手工调用git gc 命令，或推送至远程服务器时，Git 都会这样做。
Git 打包对象时，会查找命名及尺寸相近的文件，并只保存文件不同版本之间的差异内容。
Git 自动定期对仓库进行重新打包以节省空间。当然也可以手工运行 git gc 命令来这么做。

```sh
#举个栗子

find .git/objects -type f
.git/objects/01/55eb4229851634a0f03eb265b69f5a2d56f341 # tree 2
.git/objects/1a/410efbd13591db07496601ebc7a059dd55cfe9 # commit 3
.git/objects/1f/7a7a472abf3dd9643fd615f6da379c4acb3e3a # test.txt v2
.git/objects/3c/4e9cd789d88d8d89c1073707c3585e41b0e614 # tree 3
.git/objects/83/baae61804e65cc73a7201a7252750c76066a30 # test.txt v1
.git/objects/95/85191f37f7b0fb9444f35a9bf50de191beadc2 # tag
.git/objects/ca/c0cab538b970a37ea1e769cbbde608743bc96d # commit 2
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4 # &#039;test content&#039;
.git/objects/d8/329fc1cc938780ffdd9f94e0d364e0ea74f579 # tree 1
.git/objects/fa/49b077972391ad58037050f2a75f74e3671e92 # new.txt
.git/objects/fd/f4fc3344e67ab068f836878b6c4951e3b15f3d # commit 1
```



<br/>
<br/>
<br/>



## Refspec


```sh
#假设添加了一个远程Repo
git remote add origin git@ github.com:schacon/simplegit-progit.git

#之后可在.git/config中查看

```

缺省情况下 refspec 会被 `git remote add` 命令所自动生成， Git 会获取远端上 `refs/heads/` 下面的所有引用，并将它写入到本地的 `refs/remotes/origin/`:

```sh
git log origin/master
git log remotes/origin/master
git log refs/remotes/origin/master

#它们全是等价的，因为 Git 把它们都扩展成 refs/remotes/origin/master
```

<br>

如果你想让 Git 每次只拉取远程的 master 分支，而不是远程的所有分支，你可以把 fetch 这一行修改成这样：

```sh
#默认
#fetch = +refs/heads/*:refs/remotes/origin/*
fetch = +refs/heads/master:refs/remotes/origin/master
```

<br>

你可以使用命名空间来达到这个目的。如你有一个QA组，他们推送一系列分支，你想每次获取 master 分支和QA组的所有分支，你可以使用这样的配置段落：

```sh
[remote &quot;origin&quot;]
       url = git@ github.com:schacon/simplegit-progit.git
       fetch = +refs/heads/master:refs/remotes/origin/master
       fetch = +refs/heads/qa/*:refs/remotes/origin/qa/*
```

<br>

**push Refspec**

如果QA组成员想把他们的 master 分支推送到远程的 qa/master 分支上，可以这样运行：

```sh
git push origin master:refs/heads/qa/master

```

<br>

**删除 Refspec**

以使用 refspec 来删除远程的引用，是通过运行这样的命令：

```sh
#因为 refspec 的格式是 : 
#通过把 部分留空的方式，这个意思是是把远程的topic 分支变成空，也就是删除它
git push origin :topic
```



<br/>
<br/>
<br/>



## 传输协议


Git 可以通过两种主要的方式在版本库之间传输数据：**dumb Protocol** 和 **smart protocol**


<br/>


### Dumb protocol

Git 基于HTTP之上传输通常被称为 dump  protocol，这是因为它在服务端不需要有针对 Git 特有的代码。这个获取过程仅仅是一系列 `GET` 请求，客户端可以假定服务端的Git仓库中的布局。

```sh
#栗子
git clone http://github.com/schacon/simplegit-progit.git
```



<br/>
<br/>



### Smart protocol

dump procotol 虽然很简单但效率略低，且它不能从客户端向服务端发送数据。
智能协议是更常用的传送数据的方法，但它需要在服务端运行一个进程，而这也是 Git 的智能之处——它可以读取本地数据，理解客户端有什么和需要什么，并为它生成合适的包文件。 总共有两组进程用于传输数据，它们分别负责上传和下载数据。
为了上传数据至远端， Git 使用 `send-pack` 和 `receive-pack` 进程。这个 `send-pack` 进程运行在客户端上，它连接至远端运行的 `receive-pack` 进程。
当你在下载数据时， `fetch-pack` 和 `upload-pack` 进程就起作用了。客户端启动 `fetch-pack` 进程，连接至远端的 `upload-pack` 进程，以协商后续数据传输过程。


举例来说，在项目中使用命令 `git push origin master` 时, `origin` 是由基于 SSH 协议的 URL 所定义的。 Git 会运行 `send-pack` 进程，它会通过 SSH 连接你的服务器。 它会尝试通过 SSH 在服务端执行命令



<br/>
<br/>
<br/>



## 维护和恢复


有的时候，你需要对仓库进行清理 - 使它的结构变得更紧凑，或是对导入的仓库进行清理，或是恢复丢失的内容。


<br/>


### 维护

Git 会不定时地自动运行一个叫做 `auto gc` 的命令。 大多数时候，这个命令并不会产生效果。 然而，如果有太多松散对象（不在包文件中的对象）或者太多包文件，Git 会运行一个完整的 `git gc` 命令。
这个命令通常并不会产生效果。 大约需要 7000 个以上的松散对象或超过 50 个的包文件才能让 Git 启动一次真正的 gc 命令。 你可以通过修改 `gc.auto` 与 `gc.autopacklimit` 的设置来改动这些数值。

```sh
#栗子
git gc --auto

```



<br/>
<br/>



### 数据恢复

在你使用 Git 的时候，你可能会意外丢失一次提交。 通常这是因为你强制删除了正在工作的分支，但是最后却发现你还需要这个分支；亦或者硬重置了一个分支，放弃了你想要的提交。 如果这些事情已经发生，该如何找回你的提交呢？

首先需要判断当前处于什么位置，然后恢复到之前某个位置。

```sh
#查看commit
git log --pretty=oneline
3e1fcc1f8c035320d985b0202e18a6a00fe068c0 update 1.txt
a3f3bdd9cf22ce499d1b3cdff5bb26c94032a15d add 2.txt
f4c90e173ef18e738f0287af71ae0878beec9316 add 1.txt
748d8d3152b3a9a81068e29ae00fa797c2223520 add README


#重置
git reset --hard ${commit-id}




#也可以使用
git reflog
1a410ef HEAD@{0}: reset: moving to 1a410ef
ab1afef HEAD@{1}: commit: modified repo.rb a bit
484a592 HEAD@{2}: commit: added repo.rb



#查看log
git log -g
commit 1a410efbd13591db07496601ebc7a059dd55cfe9
Reflog: HEAD@{0} (Scott Chacon <schacon@gmail.com>)
Reflog message: updating HEAD
Author: Scott Chacon <schacon@gmail.com>
Date:   Fri May 22 18:22:37 2009 -0700

		third commit
```

<br>

你可以通过创建一个新的分支指向这个提交来恢复它：

```sh
#748d8d3152b3a9a81068e29ae00fa797c2223520 add README
git branch  test 748d8d3152b3a9a81068e29ae00fa797c2223520

git checkout test

ls
README.md



#删除分支
git checkout master
git branch -D  test
```


<br/>
<br/>



### 移除对象

Git 有许多过人之处，不过有一个功能有时却会带来问题：git clone 会将包含每一个文件的所有历史版本的整个项目下载下来。如果项目包含的仅仅是源代码的话这并没有什么坏处，毕竟 Git 可以非常高效地压缩此类数据。
不过如果有人在某个时刻往项目中添加了一个非常大的文件，那们即便他在后来的提交中将此文件删掉了，所有的签出都会下载这个 大文件。因为历史记录中引用了这个文件，它会一直存在着。

警告：这个操作对提交历史的修改是破坏性的。 它会从你必须修改或移除一个大文件引用最早的树对象开始重写每一次提交。 如果你在导入仓库后，在任何人开始基于这些提交工作前执行这个操作，那么将不会有任何问题 - 否则，你必须通知所有的贡献者他们需要将他们的成果变基到你的新提交上。

```sh
git rm  bigfiles.tar.gz
git commit -m "remove big files"


#查看清理了多少空间
git gc
#git count-objects -v

```



<br/>
<br/>
<br/>



## 环境变量


Git 总是在一个 `bash shell` 中运行，并借助一些 shell 环境变量来决定它的运行方式。


<br/>


### 全局行为

- `GIT_EXEC_PATH`: 决定 Git 到哪找它的子程序
- `PREFIX`:  也类似，除了用于系统级别的配置
- `GIT_PAGER`:  控制在命令行上显示多页输出的程序
- `GIT_EDITOR`:  当用户需要编辑一些文本（比如提交信息）时， Git 会启动这个编辑器



<br/>
<br/>



### 版本库位置

- `GIT_DIR`:  是 `.git` 目录的位置
- `GIT_CEILING_DIRECTORIES`:  控制查找 `.git` 目录的行为
- `GIT_WORK_TREE`:  是非空版本库的工作目录的根路径 如果没指定，就使用 `$GIT_DIR` 的父目录
- `GIT_INDEX_FILE`:  是索引文件的路径
- `GIT_OBJECT_DIRECTORY`:  用来指定 `.git/objects` 目录的位置
- `GIT_ALTERNATE_OBJECT_DIRECTORIES`:  一个冒号分割的列表 (格式类似`/dir/one:/dir/two:…`) 用来告诉 Git 到哪里去找不在 `GIT_OBJECT_DIRECTORY` 目录中的对象. 如果你有很多项目有相同内容的大文件，这个可以用来避免存储过多备份。



<br/>
<br/>


### 路径规则

- `GIT_GLOB_PATHSPECS`, `GIT_NOGLOB_PATHSPECS`:  控制通配符在路径规则中的默认行为
- `GIT_LITERAL_PATHSPECS`:  禁用上面的两种行为；通配符将不能用，前缀覆盖也不能用
- `GIT_ICASE_PATHSPECS`:  让所有的路径规格忽略大小写



<br/>
<br/>



### 提交

- `GIT_AUTHOR_NAME:  是 “author” 字段的可读的名字
- `GIT_AUTHOR_EMAIL`:  是 “author” 字段的邮件
- `GIT_AUTHOR_DATE`:  是 “author” 字段的时间戳
- `GIT_COMMITTER_NAME`:   是 “committer” 字段的可读的名字
- `GIT_COMMITTER_EMAIL`:  是 “committer” 字段的邮件
- `GIT_COMMITTER_DATE`:  是 “committer” 字段的时间戳



<br/>
<br/>



### 网络

- `GIT_SSL_NO_VERIFY`:  告诉 Git 不用验证 SSL 证书。 这在有些时候是需要的， 例如你用一个自己签名的证书通过 HTTPS 来提供 Git 服务， 或者你正在搭建 Git 服务器，还没有安装完全的证书
- `GIT_HTTP_USER_AGENT`:  设置 Git 在通过 HTTP 通讯时用到的 user-agent



<br/>
<br/>


### 比较与合并

- `GIT_DIFF_OPTS`:  这个有点起错名字了 有效值仅支持 -u<n> 或 --unified=<n>，用来控制在 `git diff` 命令中显示的内容行数
- `GIT_EXTERNAL_DIFF`:  用来覆盖 diff.external 配置的值。 如果设置了这个值， 当执行Gitgit diff 时，Git 会调用该程序
- `GIT_DIFF_PATH_COUNTER` 和 `GIT_DIFF_PATH_TOTAL`:  对于 `GIT_EXTERNAL_DIFF` 或`diff.externa`l 指定的程序有用。 前者表示在一系列文件中哪个是被比较的（从 1 开始），后者表示每批文件的总数。
- `GIT_MERGE_VERBOSITY`:  控制递归合并策略的输出。 允许的值有下面这些：
	- 0:  什么都不输出，除了可能会有一个错误信息
	- 1:  只显示冲突
	- 2:  还显示文件改变(默认值)
	- 3:  显示因为没有改变被跳过的文件
	- 4:  显示处理的所有路径
	- 5:  显示详细的调试信息



<br/>
<br/>


### 调试

- `GIT_TRACE`:  控制常规跟踪，它并不适用于特殊情况。 它跟踪的范围包括别名的展开和其他子程序的委托
- `GIT_TRACE_PACK_ACCESS`:  控制访问打包文件的跟踪信息 
- `GIT_TRACE_PACKET`:  打开网络操作包级别的跟踪信息
- `GIT_TRACE_PERFORMANCE`:  控制性能数据的日志打印
- `GIT_TRACE_SETUP`:  显示 Git 发现的关于版本库和交互环境的信息


<br/>
<br/>


### 其它

- `GIT_SSH`
- `GIT_ASKPASS`
- `GIT_NAMESPACE`
- `GIT_FLUSH`
- `GIT_REFLOG_ACTION`






<br/>
<br/>

---

<br/>
<br/>



# `fetch`与`pull`


Git中从远程的分支获取最新的版本到本地有两个命令:

- `fetch`: 当于是从远程获取最新版本到本地，不会自动merge
- `pull`: 相当于是从远程获取最新版本并merge到本地

<br>

几个概念：

- `FETCH_HEAD`: 是一个版本链接，记录在本地的一个文件中，指向着目前已经从远程仓库取下来的分支的末端版本
- `commit-id`
- `git fetch`: 这将更新git remote中所有的远程仓库所包含分支的最新commit-id, 将其记录到`.git/FETCH_HEAD`文件中
- `git pull`: 基于本地的`FETCH_HEAD`记录，比对本地的`FETCH_HEAD`记录与远程仓库的版本号，然后`git fetch`获得当前指向的远程分支的后续版本的数据，然后再利用`git merge`将其与本地的当前分支合并。

<br>


首先，你的每一个操作都是要指明**来源**和**目标**，对于`pull`来说，`目标`就是当前分支。
其次，你得清楚`git`是有`tracking `的概念的，所谓`tracking`就是把**来源**和**目标**绑定在一起，节省一些操作是需要输入的参数。
默认是在当前分支，平时养成好习惯，没谱的时候把来源和目标都带上。

```
#直接使用git fetch
#创建并更新本 地远程分支
git fetch


#只是手动指定了要fetch的remote。在不指定分支时通常默认为master
#git fetch origin [branch]
git fetch origin


#指定远程remote和FETCH_HEAD，并且只拉取该分支的提交
git fetch origin dev




#git pull <远程主机名> <远程分支名>:<本地分支名>
#取回远程主机某个分支的更新，再与本地的指定分支合并。

#当你在 master 下
git pull
#等于 fetch origin，然后 merge origin/master

#当你在 develop 下
git pull
#等于 fetch origin，然后 merge origin/develop


#多个分支
#切换到dev
git checkout dev

#这会在当前分支合并test
git pull origin test
```

<br>

```
#查看 .git/config

[remote "origin"]
        url = git@xxx:xxx/xxx.git
        fetch = +refs/heads/*:refs/remotes/origin/*
#它指明了 fetch 动作的来源
#也就是说， git fetch 的操作就是取下上述目标的更新



#但是——取下的东西到底在哪儿？
#查看 .git/FETCH_HEAD
```


<br>

可以简单的这样理解：

```
#fetch      从另外一个版本库下载对象和引用
#pull       获取并合并另外的版本库或一个本地分支


git pull = git fetch + merge to local

#git fetch origin master
#git merge FETCH_HEAD


#如果搞不清楚，指明源和目标是最稳当的
git pull [remote] [branch]
```



<br>
<br/>

---

<br>



# 时光穿梭机



```

#查看repo当前状态
git status



#查看改变
git diff

```


<br>


## 版本回退


每当文件修改到一定程度的时候，就可以提交一次。这样即使误操作后，还可以从最近的`commit`中恢复，而不是把工作成果全部丢失。


```
#查看提交记录
#git的commit id是一个SHA1的16进制散列
git log

    Update README.md

commit e89d28373c19321466f99e15cd3cdcc5fffe868f
Author: zhang21 <elite_zhang21@163.com>
Date:   Thu Apr 5 23:40:13 2018 +0800



#版本回退，如果文件误删，可以从commit中恢复
#查看提交记录，能看到Commit ID(sha1sum散列值)
#在Git中，用HEAD表示当前版本，也就是最新的Commit ID
#上一版本HEAD^, 上上版本HEAD^^, 倒数第十个版本HEAD~100



#HEAD指的是当前版本
#重置当前HEAD到指定状态
git reset --hard HEAD^

#也可以利用commit id回退
git reset --hard $commit_id



#查看历史命令
git reflog



#在本地回退之后，是无法push到远程的
#必须强制push
git push origin master --force

```


<br>
<br/>


## 工作区和暂存区


- `git add`实质是吧文件修改添加到**暂存区**
- `git commit`实质是把暂存区的所有内容提交到当前分支


<br>
<br/>


## 管理修改


为什么git比其它版本控制系统设计的更优秀，因为它跟踪并管理的是修改，而非文件。
如果修改后的文件没有使用`git add`放入暂存区的话，那么`git commit`也不会生效的。


<br>
<br/>


## 撤销修改

如果要纠正文件，可以手动修改文件并恢复到上一版本状态。
但也可以使用git命令。


```

#丢弃工作区的修改
#--很重要，没有--就变成了切换分支的命令
git checkout -- filename



#当你不但改乱了工作区某个文件的内容，还添加到了暂存区时。想丢弃修改，分两步。
#第一步用命令git reset HEAD file，就回到了场景1，第二步git checkout --file。
git reset HEAD file && git checkout -- file

```


<br>
<br/>


## 删除文件


在git中，删除也是一个修改操作。

有两种情况：

- 误删除
- 真删除

```

#rm，从工作区和索引中删除文件
#如果一个文件已经被提交到版本库，那么你永远不用担心误删
git rm README


#误删某文件，需要恢复
git checkout -- README

```


<br>
<br/>

---

<br>



# 远程仓库


用于验证推送，GitHub与本地仓库使用SSH加密传输，所以这需要创建一对密钥。


```
#生成SSH Key
ssh-keygen -t rsa -C "email-address"

#会生成.ssh目录，里面包含公私钥
#将公钥id_ras.pub填入GitHub

```


<br>
<br/>


## 添加远程仓库


```

#origin是默认的远程仓库名，你可以修改
git remote add origin git@xxx.com:username/xxx.git



#推动本地仓库到远程
#实际上是推动本地的master分支到远程
#-u关联了本地master和远程master
git push -u origin master

#之后
git push origin master

# 推送本地特定分支到远程特定分支
git push origin local-1:remote-2
```


<br>
<br/>


## 从远程库克隆


```
#将远程仓库克隆到本地
#如果是多人协作开发，那么每个人各自从远程克隆一份就可以了
#可以使用ssh协议或https协议(每次都要输入口令)
git clone git@xxx.com:username/xxx.git

#克隆指定分支
git clone -b test URL

```



<br>
<br/>

---

<br>



# 分支管理


你可以创建一个自己的分支，别人看不到，还继续在原来的分支上正常工作。而你在自己的分支上干活，想提交就提交，而不会影响到其他人。


<br>
<br/>


## 创建于合并分支


`HEAD`严格来说不是指向提交，而是指向分支(如`master`)，分支才是指向提交。

当工作完成后，便可合并分支，然后删除额外的分支。

```

#查看分支
#*代表当前工作分支
git branch



#创建分支
git branch <branch-name>
#切换分支
git checkout <branch-name>



#创建并切换分支，等于上面的创建和切换分支
git checkout -b <branch-name>



#在test分支下新建test.txt
git checkout test
echo 'Just a test' > ./test.txt
git add test.txt
git commit -m 'Just a test branch'



#回到master
git checkout master
#此分支下并没有test.txt
#也就是说并没有其它分支提交的内容



#合并分支到当前分支
git merge <branch-name>

#合并test分支到当前的master分支
git merge test



#删除分支
git branch -d <branch-name>

#合并完成后删除test分支
git brancd -d test

```


<br>
<br/>


## 解决冲突


合并分支玩玩也不是一帆风顺的！

可能在你创建了新分支后，master分支又进行了提交，而你的新分支也做了提交，这是合并分支便会带了问题。
当git分支无法合并时，就必须首先要解决冲突。解决冲突后，再提交和合并。

```

#查看分支合并图
git log --graph

```


<br>
<br/>


## 分支管理策略


- 在实际开发中，`master`分支应该是非常稳定的。也就是只用来发布新版本，不能在上面干活
- 干活应在其它分支上(如`dev`)，干完后合并到`master`
- 工作人员都在`dev`上干活，每个人都有自己的分支，然后将自己的分支合并到`dev`就可以了

![分支策略](/images/gitbranch.png)

<br>

> 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。


<br>
<br/>


## Bug分支


Git提供了一个`stash`功能，可以把工作现场**储藏**起来，等以后恢复现场后继续工作。

```
git stash

#创建debug分支
git checkout -b 'issue-25'

git checkout master
git merge --no-ff -m "debug 25" 'issue-25'



#切回工作区
git stash list
git stash apply stash@xxx

#手动删除stash
git stash drop


#恢复同时也删除stash
git stash pop
```


<br>
<br/>


## Feature分支


添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

丢弃一个没有被合并过得分支，可通过`git branch -D <branch-name>`强行删除。


<br>
<br/>


## 多人协作


```
#查看远程仓库
git remote

#显示远程仓库详细信息
git remote -v



#推送指定分支
git push origin test



#抓取分支
git clone

#更新分支
git pull

#合并分支
git merge

#推送分支
git push
```


<br>

## 版本库（Repository）

隐藏目录.git是Git的版本库。
Git版本库里面存放了很多东西，其中最重要的就是 stage(或index)的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向master的一个指针叫`HEAD`。

- 用`git add`把文件添加进去，实际是把文件添加到暂存区；
- 用`git commit`提交更改，实际是把暂存区的所有内容提交到当前分支。默认`git commit`就是往`master`上提交更改。



<br>
<br/>

---

<br>
<br/>









# rebase

使用rebase(变基)命令重新整理提交记录。尽量不要对任何已经提交到公共仓库中的commit进行修改，除非你自己明白。











<br/>
<br/>

---

<br/>
<br/>












# SSHKey


1. 创建SSHKey并在本地关联多个SSH

```
#把你的github邮箱地址
ssh-keygen -t rsa -C "email@example.com"
#会生成 ~/.ssh，包含 私钥：id_rsa，公钥：id_rsa.pub
```

2. 将公钥写入Github
在Github--Account settings--SSH Keys--Add SSH Key里面，添加你的id_rsa.pub公钥文件。
当然，你可以添加多个Key哦，毕竟可能你有多台登陆设备。
这个就相当于SSH无密钥认证。

3. 在主机上关联多个git

```sh
vim ~/.ssh/config


#One
Host git.xxx.com
        IdentityFile ~/.ssh/id_rsa
        Hostname IP
        User git
        Port 10022
#two
Host github
	IdentityFile ~/.ssh/id_rsa
	Hostname github.com
	User git
	Port 22
#three
#这样可用于ssh登录
Host zhang21
	Hostname ip
    User username
    Port 22
    IdentityFile ~/.ssh/id_rsa



#一定要记着修改权限
chmoe 600 ~/.ssh/*


#测试连接
ssh -T git@github.com
```



<br>
<br/>

---

<br>



# 标签管理


发布一个新版本时，通常先在版本库中打一个标签(tag)，这样，就唯一确定了打标签时刻的版本。
将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

`tag`其实是指向 `commit id`的。
git有commit，为什么还要引入tag? commit id 是一串散列值，并不简单明了。但是tag,我可以写为"v1.0","v1.2"...
让`tag`，"v1.0"指向对应的`commit id`，很方便明了。


<br>
<br/>


## 创建标签


```
#切换到需要打tag的分支上
git brach test



#创建tag
默认tag是打在最新提交的commit上
#git tag <tag-name>
git tag v1.0



#查看所有tag
git tag



#指定tag对应的commit
#git tag <tag-name> <commit_id>
git tag v1.0 65432ba



#标签不是按时间顺序列出的，而是按照字母排序
git show $tag-name
git show v1.0



#创建带有说明的标签
#git tag -a <tag-name> -m "v1.1 released" <commit-id>
git tag -a v1.1 -m "V1.1" 6543bb


#查看标签说明
git show <tag-name>



#用私钥签名一个标签
#依赖GPG
#git tag -s <tag-name> -m "pri-key" <commit-id>
git tag -s v1.2 -m "pri-key v1.2" 6543bc

```


<br>
<br/>


## 操作标签


```

#删除标签
#git tag -d <tag-name>
git tag -d v1.2



#推送某个标签到远程
#git pust origin <tag-name>
git push origin v1.0



#推送全部标签
git push origin --tags



#删除远程标签
git push origin :refs/tags/<tag-name>

```


<br>
<br/>

---

<br/>
<br/>



# 忽略特殊文件

创建一个`.gitignore`特殊文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

各种模板： <https://github.com/github/gitignore>


检查`.gitignore`文件: `git check-ignore .gitignore `



<br/>
<br/>

---

<br/>



# 搭建Git服务器


常见的Git服务器有：

- GitLab: <https://gitlab.com/>
- Gogs（go git service）: <https://gogs.io/>






















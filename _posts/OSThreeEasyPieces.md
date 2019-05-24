---
title: OSTEP
date: 2019-04-28 08:59:12
mathjax: true
categories: DevOps
tags:
  - OperatingSystem
  - 操作系统
  - Linux
  - Optimization
  - 性能优化
---


参考:

- OS Three Easy Pieces: <https://book.douban.com/subject/19973015/>
- OSTEP: <http://pages.cs.wisc.edu/~remzi/OSTEP/>

<br/>

环境:

- ELRH7x86_64









<br/>
<br/>

---

<!--more-->

<br/>
<br/>

















# 操作系统介绍

Introduction to Operating Systems


如果你正在攻读本科操作系统课程，你应该已经知道计算机程序运行时的想法。如果没有，这本书将很难。

那么，程序运行时会发生什么呢？
好吧，正在运行的程序做了一件非常简单的事情: **它执行指令(it executes instructions)**。
每秒都有成千上万次，处理器从内存(mem)中取出(fetch)指令，对其进行解码(decode)——即确定这是哪条指令，并执行它。完成此指令后，进程将继续执行下一条指令，以此类推，直到程序最终完成。

刚刚描述了**冯-诺依曼计算模型(Von Neumann model of computing)**的基础知识。在本书中，我们将学习在程序运行的同时，还有许多其它的东西在运行，其主要目标是使系统易于使用。

事实上，有一大堆软件负责使应用程序运行变得容易，允许程序共享内存(share mem)，使程序与设备交互...该软件主体称为**操作系统(Operating System)**，它负责确保系统操作以易于使用的方式正确有效地运行。

操作系统执行此操作的主要方式是通过我们称为**虚拟化(Virtualization)**的通用技术。也就是说，操作系统采用**物理资源(Physical Resource)**(如处理器，内存，磁盘)并将其转换为更通用，功能强大且易于使用的虚拟形式。因此，我们有时将操作系统称为**虚拟机(Virtual Machine)**。

> **问题的关键: 如何虚拟化资源**
> 本书的一个核心问题：操作系统如何虚拟化资源？操作系统为什么要虚拟化资源——它使得系统更易于使用。因此，我们关注：操作系统使用什么机制和策略来实现虚拟化？操作系统如何有效地进行操作？需要什么硬件支持？

<br>

当然，为了告诉用户操作系统做什么，从而利用虚拟机的功能(如运行程序、分配内存、访问文件...)，操作系统还提供了一些可调用的接口(API)。事实上，典型的操作系统会**导出(Export)**数百个可供应用程序访问的**系统调用(System Call)**。
由于操作系统提供这些系统调用来运行程序、访问内存和设备、以及其它相关操作，有时也会说操作系统为应用程序提供了一个**标准库(Standard Library)**。

最后，因为虚拟化允许多个程序运行(共享CPU)，并且许多程序同时(Concurrently)访问它们自己的指令和数据(共享内存)，以及许多程序访问设备(共享磁盘等)，操作系统有时被称为**资源管理器(Resource Manager)**。每个CPU、MEM、DISK都是系统的资源。因此，操作系统的角色是管理这些资源，有效或公平地执行。



<br/>
<br/>


## 虚拟化CPU

Virtualizing the CPU


```c
// Code That Loops and Prints (cpu.c)

#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>
#include <assert.h>
#include "common.h"

int
main(int argc, char *argv[])
{
    if (argc != 2) {
        fprintf(stderr, "usage: cpu <string>\n");
        exit(1);
    }
    char *str = argv[1];
    while (1) {
        Spin(1);
        printf("%s\n", str);
    }
    return 0;
}
```

<br>

上图的程序，它所做的只是调用`Spin()`——这是一个重复检查时间的函数。
接下来我们具有单个处理器的系统上编译和运行它:

```shell
# 编译
# 此处遇到两个错误
# 1. cpu.c:(.text+0xe0)：对‘pthread_create’未定义的引用
# 2. cpu.c:(.text+0x127)：对‘pthread_join’未定义的引用
# 网上方法: 在编译时需要添加 -lpthread 参数来使用 libpthread.a 库进行编译
gcc -o cpu cpu.c -Wall -lpthread


# 运行
./cpu "ABC"
ABC
ABC
...
# 需手动终止
Ctrl+C
```

<br>

现在让程序复杂一点:

```shell
./cpu A &  ./cpu B &  ./cpu C &  ./cpu D &

[1] 7353
[2] 7354
[3] 7355
[4] 7356
A
B
D
C
A
B
D
C
A
C
B
D
...
```

现在事情变得更有趣了。即使只有一个处理器，但不知何故，所有四个程序都在同时运行！这种魔力是如何发生的呢？

事实证明，操作系统在硬件的帮助下负责这种**错觉(illusion)**——即系统具有大量虚拟CPU的错觉。将单个CPU转换为看似无限数量的CPU，从而允许许多程序看起来像是一次运行，这就是我们所说的虚拟化CPU，这是本书第一部分的重点。

当然，要运行程序并停止它，以及告诉操作系统运行哪些程序，需要使用一些接口(API)来将你的需求传递给操作系统。实际上，它们是大多数用于与操作系统交互的主要方式。

你可能还注意到，一次运行多个程序会引发各种新问题。如，如果两个程序需要在特定时间运行，哪个应该运行。这些问题由操作系统的策略来回答，策略在操作系统中的许多不同位置用户回答这些类型的问题。





<br/>
<br/>
<br/>




## 虚拟内存

Virtualizing Memory


```c
// A Program that Accesses Memory (mem.c)

#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include "common.h"

int
main(int argc, char *argv[])
{
    int *p = malloc(sizeof(int)); // a1
    assert(p != NULL);
    printf("(%d) memory address of p: %08x\n",
        getpid(), (unsigned) p); // a2
    *p = 0; // a3
    while (1) {
        Spin(1);
        *p = *p + 1;
        printf("(%d) p: %d\n", getpid(), *p); // a4
    }
    return 0;
}
```

<br>

现在让我们考虑一下**内存(memory)**。现代机器提供的物理内存的模型非常简单。内存只是一个**字节数组(a array of bytes)**。要读取内存，必须指定一个地址才能访问存储在那里的数据。要写入或更新(write/update)内存，还必须指定要写入数据的给定地址。

程序运行时始终访问内存。程序的所有数据结构保存在内存中，并通过各种指令访问它们，如`loads`, `stores`或其它在执行工作时访问内存的显式指令。不要忘记程序的每条指令也在内存中，每次**取(fetch)指令**时访问内存。

让我们来看下通过调用`malloc()`来分配一些内存的上面那个程序:

```shell
./mem
(6941) memory address of p: 01f13010
(6941) p: 1
(6941) p: 2
^C

# 该程序做了几件事。首先，它分配一些内存(a1)。然后，它打印出内存的地址(a2)，然后将数字零放入新分配的存储器的第一个插槽(a3)。最后，它循环，延迟1秒并递增存储在p中保存的地址的值。它还打出程序的PID。


# Running The Memory Program Multiple Times
./mem & ./mem &
[1] 7544
[2] 7545
(7544) memory address of p: 012f3010
(7545) memory address of p: 006cf010
(7544) p: 1
(7545) p: 1
(7544) p: 2
(7545) p: 2
(7544) p: 3
(7545) p: 3
```

实际上，这正是这里发生的事情，因为操作系统正在虚拟化内存。每个进程都访问自己的**私有虚拟地址空间(private virtual address space)**(有时也称为地址空间)，操作系统以某种方式将其映射到计算机的物理内存中。一个正在运行的程序中的内存引用不会影响其它进程的地址空间。就运行程序而言，它具有物理内存。然而，现实是物理内存是由操作系统管理的共享资源。




<br/>
<br/>
<br/>
<br/>




## 并发

Concurrency


本书另一个主题是**并发性(concurrency)**。使用这个术语来指代同一程序中同时处理多个事件(即并发)。并发问题首先出现在操作系统自身，如前面的虚拟化程序，操作系统同时处理多个事情。

```c
// AMulti-threaded Program (threads.c)

 #include <stdio.h>
 #include <stdlib.h>
 #include "common.h"

 volatile int counter = 0;
 int loops;

 void *worker(void *arg) {
     int i;
     for (i = 0; i < loops; i++) {
         counter++;
     }
     return NULL;
}

 int
 main(int argc, char *argv[])
 {
     if (argc != 2) {
     fprintf(stderr, "usage: threads <value>\n");
     exit(1);
     }
     loops = atoi(argv[1]);
     pthread_t p1, p2;
     printf("Initial value : %d\n", counter);

     Pthread_create(&p1, NULL, worker, NULL);
     Pthread_create(&p2, NULL, worker, NULL);
     Pthread_join(p1, NULL);
     Pthread_join(p2, NULL);
     printf("Final value : %d\n", counter);
     return 0;
}


// >本程序使用Pthread create()创建两个线程
// 你可将线程视为与其它函数相同的内存空间中运行的函数，一次使用多个函数
```

<br>

> **如何构建正确的并发程序?**
> 当同一个内存空间中有许多并发执行的线程时，我们如何构建一个正常工作的程序？操作系统需要哪些原语？硬件提供哪些机制？如何使用它们来解决并发问题？

<br>

运行:

```shell
./threads
usage: threads <value>

./threads 1000
Initial value : 0
Final value : 2000


# 看看更高的值
./threads 10000
Initial value : 0
Final value : 17726

./threads 10000
Initial value : 0
Final value : 18741

./threads 10000
Initial value : 0
Final value : 20000
```

上面出现了既正常又奇怪的结果。这些结果与指令的执行方式有关。不幸的是，上面程序的一个关键部分，共享计数器递增，需要三个指令：

- 一个用于将计数器的值从内存加载到寄存器；
- 一个用于递增；
- 一个用于将其存储回内存。

因为这三个指令不是原子地执行(一次全部执行)，所以会发生奇怪的事。




<br/>
<br/>
<br/>
<br/>




## 持久化

Persistence


```c
// Program That Does I/O (io.c)

#include <stdio.h>
#include <unistd.h>
#include <assert.h>
#include <fcntl.h>
#include <sys/types.h>

int
main(int argc, char *argv[])
{
    int fd = open("/tmp/file", O_WRONLY | O_CREAT | O_TRUNC, S_IRWXU);
    assert(fd > -1);
    int rc = write(fd, "hello world\n", 13);
    assert(rc == 13);
    close(fd);
    return 0;
}
```

<br>

第三个主题是**持久化(persistence)**。在系统内存中，数据很容易丢失，因为如DRAM的设备是以易失的方式存储值。当断电或系统奔溃时，内存中的任何数据都会丢失。因此，我们需要硬件和软件能够持久存储数据。硬件以某种I/O设备的形式出现。

通常，操作系统中管理磁盘的软件被称为**文件系统(file system)**。它负责将用户创建的任何文件以可靠和有效的方式存储在磁盘上。

与操作系统为CPU何MEM提供的抽象不同，操作系统不会为每个应用程序创建专用的虚拟化磁盘。相反，它假设用户经常想要共享文件中的信息。

来看下上面的代码，它打开`/tmp/file`文件，并将`hello world`写入文件。
```shell
./io

cat /tmp/file
hello world
```

<br>

> **如何持久存储数据？**
> 文件系统是负责管理持久化数据的操作系统的一部分。正确地处理需要哪些技术呢？面对硬件和软件故障，如何实现可靠性？

要完成此任务，程序会对操作系统进行三次调用。这些系统调用被路由到称为文件系统的操作系统部分，然后处理请求并向用户返回某种错误代码。

- 第一，调用`open()`打开文件；
- 第二，调用`write()`将数据写入文件；
- 第三，调用`close()`关闭文件。

你可能想知道操作系统为了写入磁盘而执行的操作。文件系统必须完成相当多的工作，首先确定这些新数据将驻留在磁盘上的哪个位置，然后在文件系统维护的各种结构中跟踪它。这样做需要向底层存储设备发出I/O请求，以读取现有结构或更新它们。
任何编写设备驱动程序的人都知道，让设备代表你做某事是一个复杂和详细的过程。它需要深入了解低级设备接口及其确切语义。幸运的是，操作系统提供了一个的标准和简单的方式——通过系统调用(system call)访问设备。因此，操作系统有时被视为**标准库(standard library)**。

为了处理写入期间系统崩溃的问题，大多数文件系统都包含某种复杂的写入协议。(如journaling或copy-on-write)。仔细写入磁盘以确保在写入序列期间发生故障时，系统之后可以恢复到合理的状态。为了使不同额公共操作高效，文件系统采用许多不同的数据结构和访问方法，从简单的列表到复杂的BTREE。




<br/>
<br/>
<br/>
<br/>




## 设计目标

Design Goals


现在我们知道了操作系统实际上做了什么：它使用物理资源(CPU, MEM, DISK...)，并虚拟化它们。它处理与并发相关的棘手的问题。它可以持久存储文件，从而使它们长期安全。最基本的目标是建立一些抽象，以使系统方便和易于使用。抽象是我们计算机科学中所做的一切的基础。

设计和实现操作系统的一个目标是提供**高性能(High Performance)**；另一个目标是尽量减少操作系统的**开销(overhead)**。虚拟化和使操作系统易于使用是值得的，但不是不惜任何代价。因此，我们必须提供虚拟化和其它操作系统功能，而无需过多开销。这些开销以多种形式出现：额外时间、额外空间。

另一个目标是在应用程序之间，以及操作系统和应用程序之间提供**保护(Protection)**。由于我们系统多个程序同时运行，所有希望确保每一个程序的恶意或偶然的不良行为不会伤害到其它程序或操作系统。保护操作系统的主要核心原则始操作系统的**隔离(Isolation)**。将进程彼此隔离是保护的关键，因此是操作系统必须做的大部分工作的基础。

操作系统必须不间断地运行，当它失败时，系统上运行的所有应用程序也会失败。由于这种依赖性，操作系统通常努力提供**高度可靠性(high degree of reliability)**。随着操作系统越来越复杂，构建可靠的操作系统是一个相当大的挑战，这也是一个确切的研究性问题。

其它目标也是有意义的:

- **能源效率**：在绿色世界中越发重要；
- **安全**：对恶意应用程序的安全性至关重要；
- **可移植性**：随着操作系统在越来越小的设备上运行，可移植性也变得很重要。





<br/>
<br/>
<br/>
<br/>




## 一些历史

Some History


让我们简单介绍下操作系统的发展过程。与人类构建的任何系统一样，随着时间的推移，操作系统中积累了许多好的想法。


<br/>


### Early Operating Systems: Just Libraries

一开始，操作系统并没有做太多。基本上，它只是一组常用函数库(function library)。

通常，在这些旧的主机系统上，一个程序由人工控制运行一次。许多你认为的现代操作系统将执行的大部分操作是由人工执行的。如果你和操作员很好，则他可以将你的工作移动到队列的前面。
这种计算方式称为**批处理(batch processing)**，因为设置了很多作业，然后由人工以批处理的方式运行。到目前为止，计算机并没有以**交互式(interactive)**方式使用。因为成本：让用户坐在电脑面前使用它太昂贵了，因为大多数时候它只是闲置，而每小时需要花费数十万美元。



<br/>
<br/>



### Beyond Libraries: Protection

作为一个简单的常用服务库，操作系统在管理机器方面发挥了中心角色的作用。其中一个重要的方面是认识到运行操作系统自身的代码是特殊的。它控制了设备，因此应该与正常的应用程序代码区别对待。
为什么这样？设想一下，如果你允许任何应用程序可以从磁盘的任何地方读取，隐私的概念就会消失，因为任何程序都可以读取任何文件。因此，实现文件系统作为一个库是没有任何意义的。

因此，**系统调用(system call)**的想法产生了。这里的想法是添加一对特殊的硬件指令和硬件状态，以便将操作系统转换为更正式、受控制的流程，而不是将操作系统例程(routine)作为库提供(你只需要进行过程调用以访问它们)。

**系统调用(system call)**和**过程调用(procedure call)**之间的关键区别在于，系统调用将控制转移到中，同时提高**硬件权限级别(hardware privilege level)**。用户应用程序在所谓的**用户模式(user mode)**下运行，这意味着硬件限制应用程序可以执行的操作。例如，以用户模式运行的应用程序通常不能发起对磁盘的I/O请求，但可以访问物理内存页面或在网络上发送数据包。
当启动系统调用时，硬件将控制转移到预先指定的陷阱处理程序(trap handler)，并同时将特权级别提升到**内核模式(kernel mode)**。在内核模式下，操作系统可以完全访问系统的硬件，因此可以执行如启动I/O请求等操作。当操作系统完成对服务的请求时，它通过特殊返回陷阱指令(return-from-trap instrction)将控制权传递给用户，该指令恢复到用户模式，同事将控制权传递回应用程序停止的位置。



<br/>
<br/>



### The Era of Multiprogramming

操作系统真正起飞的时代是超大型计算时代，即minicomputer时代。成本的下降影响了使用者和开发者，从而使计算机系统更加有趣和美好。

特别是，由于希望更好地利用机器资源，**多程序设计(multiprogramming)**变得司空见惯。操作系统不是一次只运行一个作业，而是将大量作业加载到内存中并在它们之间快速切换，从而提高CPU利用率。这种切换特别重要，因为I/O设备很慢，而CPU很快。在I/O正在服务时让CPU等待程序是在浪费CPU时间。相反，为什么不切换到另一个工作运行呢？

在存在I/O和中断的情况下支持多程序设计和重叠的愿望迫使操作系统的概念开发沿着多个方向进行创新。内存保存等问题变得很重要，我们不希望一个程序能够访问另一个程序的内存。了解如何处理多程序设计引入的并发问题也很关键，尽管存在中断，确保操作系统正常运行是一项巨大的挑战。

当时一个主要的进步是Unix操作系统的引入。Unix从不同的操作系统获得了很多好主意，但使它们更简单易用。很快，这个团队向世界各地的人们发送了包含Unix源代码的磁带，随后有许多人加入到了这个项目中来。



<br/>
<br/>



### The Modern Era

除了minicomputer之外，还出现了一种更便宜、速度更快的新机器，我们今天称之为PC(personal computer)。

不幸的是，对于操作系统而言，PC最初代表了一个巨大的飞跃，因为早期的系统忘记了在minicomputer时代学到的经验教训。例如，早期的操作系统，如DOS(the Disk Operating System, from Microsoft)，并不认为内存保护很重要。因此，恶意(或编程不佳)的应用程序可能会乱写内存。第一代Mac OS采用合作方式进行作业调度。因此，一个意外陷入无限循环的线程可以接管整个系统，迫使重启。这一代系统中缺少的操作系统功能的痛苦太多了...

幸运的是，经过几年的苦难，微机操作系统的旧功能开始找到它们的方式进入桌面系统。例如，Mac OS X/Mac OS的核心是Unix，包含了人们对这种成熟系统所期望的所有功能。Windows同样采用了计算历史中的许多好主意，特别是从Windows NT开始，这是Microsoft OS技术的一次重大飞跃。即便是今天的手机也运行这操作系统(如Linux)，这些操作系统更像是1970s年代的微型机，而不是1980s年代的PC。

<br>

> 旁白：**Unix的重要性**
> 在操作系统的历史中，很难夸大Unix的重要性。受其它早期系统的影响，Unix汇集了许多伟大的想法，并使得系统既简单又强大。
> 贝尔实验室的基础Unix是构建小型且强大程序的统一原则，这些程序可以连接在一起形成更大的工作流。shell提供了mete-level programing，当你输入命令，它将程序串联起来以完成更大的任务变得很容易。
> Unix环境对编程人员和开发人员都很友好，同时也为C编程语言提供了编译器。编程人员可以轻松编写自己的程序并共享它们，这使得Unix非常受欢迎。它还是免费的。
> 同样重要的是代码的可读性和可访问性。拥有一个用C编写的漂亮的小内核(kernel)并邀请别人试玩、添加新的酷的功能。
> 不幸的是，随着公司试图主张版权并从中获利，Unix的传播速度便有所放缓。许多公司都有自己的变体，如SunOS、HPUX...贝尔实验室和其它玩家之间的法律纠纷在Unix上投下了一片乌云，许多人想知道它是否能够活下来，特别是在Windows被引入并占据了PC市场的大部分时...

<br>

> 旁白：**然后来了Linux**(ASIDE: AND THEN CAME LINUX)
> 对于Unix，幸运的是，一位名叫**Linus Torvalds**的年轻芬兰Hacker决定编写它自己的Unix版本，该版本大量借用原始系统背后的原则和思想，但不是来自代码库，因此避免了合法性问题。他获得了世界各地许多人的帮助，利用了已经存在的复杂的GNU工具，很快Linux就诞生了(以及现代开源软件运动)。
> 随着互联网时代的带来，大多数公司(如Google、Amazon、Facebook..)选择运行Linux，因为它是免费的，可以随时修改以满足自己的实际需求。随着智能手机成为一个占主导地位的面向用户的平台，由于许多相同的原因，Linux也在那里找到了一个据点(Android)。



<br/>
<br/>



### 摘要

Summary


因此，我们介绍了操作系统。今天的操作系统相对易于使用，而你今天使用的几乎所有操作系统都受到将在本书中讨论的发展的影响。
不幸的是，书中不会介绍的很详细。例如，网络代码、图形设备、安全性。

但是，我们将介绍许多重要的主题，包括CPU和MEM的虚拟化知识，并发性以及通过设备和文件系统的持久性。别担心，虽然有很多方面可以覆盖，但大部分内容都非常酷，而且在路的尽头，你将对计算机系统的真正工作方式有了新的认识。现在开始吧！














<br/>
<br/>

---

<br/>
<br/>






















# 虚拟化

Virtualization


CPU虚拟化：

- A Dialogue on Virtualization
- The Abstraction: The Process
- Interlude: Process API
- Mechanism: Limited Direct Execution
- CPU Scheduling
- Scheduling: The Multi-Level Feedback Queue
- Scheduling: Proportional Share
- Multi-CPU Scheduling
- Summary Dialogue on CPU Virtualization

<br>

MEM虚拟化：

- A Dialogue on Memory Virtualization
- The Abstraction: Address Spaces
- Interlude: Memory API
- Mechanism: Address Translation
- Segmentation
- Free-Space Management
- Paging
- Paging: Faster Translations
- Paging: Smaller Tables
- Beyond Physical Memory: Swapping Mechanisms
- Beyond Physical Memory: Swapping Policies
- Complete Virtual Memory Systems
- Summary Dialogue on Memory Virtualization


<br/>
<br/>


## 进程

[The Abstraction: The Process](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-intro.pdf)


本章，我们将讨论操作系统为用户提供的最基本的抽象之一：**进程(process)**。进程的定义非常简单：它是一个**正在运行的程序(running program)**。程序本身是一个没有生命的东西，它位于磁盘上，一对指令(可能是一些静态数据)，等待开始行动。操作系统采用这些字节并使它们运行，将程序转换为有用的东西。

事实证明，人们经常想要同时运行多个程序。如运行浏览器、音乐播放器、邮件程序...实际上，典型的操作系统似乎可能同时运行数百个进程。这样做使系统易于使用，因为不需要关心CPU是否可用。

<br>

> TIP: USE TIME SHARING (AND SPACE SHARING)
> **时间共享(time sharing)**是操作系统用于共享资源的基本技术。通过允许资源被一个实体使用一段时间，然后另一个实体使用一段时间...资源可以被许多实体共享。时间共享对应于**空间共享(space sharing)**，其中资源在希望使用它的人之间被划分。例如，磁盘空间是一个空间共享资源，一旦将块分配给文件，在用户删除原始文件之前，通常不会将其分配给另一个文件。

<br>

我们的挑战是:

> 如何提供许多CPU的错觉？
> 虽然只有少数物理CPU可用，但操作系统如何提供几乎无穷无尽的CPU供应的错觉？

操作系统通过虚拟化(virtualizating)CPU来提供这种错觉。通过运行一个进程，然后停止它并运行另一个进程，等等。操作系统可以促使存在许多虚拟CPU存在的错觉，而实际上只有一个(几个)物理CPU。这种基本技术称为CPU的**时间共享/分时(time sharing)**，允许用户运行任意数量的并发进程(concurrent process)。潜在的成本是性能(Performance)，因为如果必须共享CPU，每个进程都会运行得更慢。

要实现CPU的虚拟化，操作系统需要一些低级机制(low-level machinery)和一些高级智能(high-level intelligence):

- **低级机制(low-level machinery mechanisms)**，此机制是实现所需功能的低级方法或协议。例如，我们稍后将学习如何实现**上下文切换(context switch)**，这使操作系统能够停止运行一个程序并在给定的CPU上开始运行另一个程序。所有现代操作系统都采用这个**分时机制(time-sharing mechanism)**。
- 操作系统中还存在一些智能的**策略(policy)**，策略是在操作系统中做出某种决定的算法。例如，给定一些可能在CPU上运行的程序，操作系统运行哪个程序？操作系统中的调度策略将做出此决定，可能使用历史信息，工作负载信息，性能指标...来做出决定。


<br/>
<br/>


### 一个进程

The Abstraction: A Process


为了理解进程的构成，我们必须了解其**机器状态(machine state)**：程序在运行时可以读取(read)或更新(update)的内容。在任何给定的时间，机器的哪些部分对于执行该程序很重要？包含进程的机器状态的一个明显组件是其内存(memory)。指令行在内存中，运行程序读写的数据也在内存中。因此，进程可以寻址的内存(称为其地址空间(address space))是进程的一部分。

**寄存器(registers)**也是进程机器状态的一部分。许多指令明确地读取或更新寄存器，因此它们对于执行过程很重要。

请注意，有一些特殊的寄存器构成了这种机器状态的一部分。如，**program counter(instruction pointer)**告诉我们当前正在运行哪个程序指令；**Stack Pointer**和相关的**frame pointer**用于管理函数参数、局部变量和返回地址的堆栈。

最后，程序通常也访问持久存储设备。此类I/O信息可能包括进程当前打开的文件列表。

<br>

> TIP: SEPARATE POLICY AND MECHANISM
> 在许多操作系统中，常见的设计范例是将高级策略与其低级机制分开。如，操作系统如何执行上下文切换？操作系统现在应该运行哪个进程？将两者分开可以很容易地改变策略，而不必重新考虑该机制，因此是一种模块化形式，一般的软件设计原则。



<br/>
<br/>



### Process API


先了解操作系统的任何接口中必须包含的内容，这些API以某种形式可用于任何现代操作系统。

- **Create**：操作系统必须包含一些创建新进程的方法；
- **Destroy**：由于存在创建进程的接口，因此系统还提供了强制销毁进程的接口。当然，许多进程都会运行并在完成后自动退出。然而，当它们不这样做时，用户可能希望杀死它们；
- **Wait**：有时，等待进程停止运行时有用的；
- **Miscellaneous Control**：除了杀死或等待进程之外，有时还有其它可能的控制措施。如暂停进程，然后恢复它；
- **Status**：通常还有接口来获取有关进程的一些状态信息，如运行了多久...

![](/images/OSTEP/processToProcess.png)



<br/>
<br/>



### 进程创建

Process Creation: A Little More Detail

我们应揭开的一个谜团是如何将**程序(program)**转换为**进程(process)**。具体来说，操作系统如何启动并运行程序？进程创建实际上如何运作？

操作系统运行程序必须做的第一件事是将其代码和任何静态文件数据(如变量...)加载(load)到程序的地址空间中(address space of process)。程序最初以某种可执行格式驻留在磁盘(disk)上。因此，将程序和静态数据加载到内存中的过程需要操作系统从磁盘读取这些字节，并将它们放在内存中。
在早期操作系统中，加载过程是**热切地(eagerly)**完成，即在运行程序之前一次完成；现代操作系统**懒惰地(lazily)**执行该过程，即仅在程序执行期间需要加载代码或数据。要真正了解代码和数据的延迟加载是如何工作的，你必须更多地了解**分页(paging)**和**交换(swapping)**的机制，这将在内存虚拟化里讨论。现在只需记住，在运行任何操作之前，操作系统显然必须要做一些工作才能将重要的程序从磁盘放入内存。

一旦将代码和静态数据加载到内存中，操作系统在运行该进程之前还需要执行一些其它操作。必须为程序的**运行时栈(runtime stack)**分配一些内存。如C程序将堆栈用于局部变量、函数参数和返回地址。操作系统分配此内存并将其提供给进程。操作系统也可能使用参数初始化堆栈，具体来说，它将填充`main()`函数的参数(`argc, argv`数组)。

操作系统还可以为程序的**堆(heap)**分配一些内存。在C程序中，堆用于显式请求的动态分配数据调用`malloc()`来请求这样的空间，并通过调用`free()`显式释放它。数据结构需要堆，如链表(linked list)、哈希表(hash table)、树(tree)和其它又去的数据结构。堆最初会很小，当程序运行并通过`malloc()`库API请求更多内存时，操作系统可能会参与并为进程分配更多内存以满足此类调用。

操作系统还将执行一些其它初始化任务，尤其是与I/O相关的任务。例如，在Unix系统中，默认情况下每个进程都有三个打开的**文件描述符(file descriptors)**，用于stdin, stdout, stderr。这些描述符使程序可以轻松地从终端读取输入并将输出打印到屏幕。将在持久化中详细介绍I/O和文件描述符。

通过将代码和静态数据加载到内存中，通过创建和初始化堆，通过执行与I/O设置相关的其它工作，操作系统最终为程序执行设置了阶段。它还有最后一个任务：启动在入口点运行的程序，即`main()`。通过跳转到`main()`例程，操作系统将CPU的控制权转移到新创建的进程，从而程序开始执行。



<br/>
<br/>



### 进程状态

Process States

现在我们已经知道一个进程是什么以及如何创建它。现在来看看一个进程在给定事件可以处于的不同状态。进程可处以以下三种状态：

- **Running**：进程正在处理器上运行，这意味着它正在执行指令；
- **Ready**：进程已准备好，但由于某种原因，操作系统已选择不在此刻运行它；
- **Blocked**：进程执行某种操作，使其在其它事件发生之前不准备运行。例如，当进程向磁盘发起I/O请求时，它会被阻塞，因此其它一些进程可以使用该处理器。

如下图所示，可以根据系统的判断在准备和运行之间移动进程。从准备到运行意味着该进程已**调度(scheduled)**好；从运行转移到准备意味着该进程被**取消调度(discheduled)**。一旦进程被**阻塞(blocked)**，操作系统将保持这样知道某些事件完成，此时进程再次进入就绪状态。

![](/images/OSTEP/processStateTransitions.png)

<br>

来看一个栗子，两个进程如果通过其中一些状态转换的示例。

| Time | Process0 | Process1 | Notes |
| - | - | - | - |
| 1 | Running | Ready | |
| 2 | Running | Ready | |
| 3 | Running | Ready | |
| 4 | Running | Ready | Process0 now done |
| 5 | – | Running | |
| 6 | – | Running | |
| 7 | – | Running | |
| 8 | – | Running | Process1 now done |

<br>

这个栗子中，process0在运行一段时间后发出I/O请求。此时，该进程被阻塞，使另一个进程有机会运行。
更具体地说，process0启动I/O并被阻塞等待它完成。例如，从磁盘读取或等待来自网络的数据包时，进程会被阻止。操作系统识别process0未使用CPU并开始运行process1。当process1运行时，process0的I/O完成，将process0移回准备状态。最后，process1完成，process0运行然后完成。

| Time | Process0 | Process1 | Notes |
| - | - | - | - |
| 1 | Running | Ready | |
| 2 | Running | Ready | |
| 3 | Running | Ready | Process0 initiates I/O |
| 4 | Blocked | Running | Process0 is blocked |
| 5 | Blocked | Running | so Process1 runs |
| 6 | Blocked | Running | |
| 7 | Ready | Running | I/O done |
| 8 | Ready | Running | Process1 now done |
| 9 | Running | – | |
| 10 | Running | – | Process0 now done |

请注意，即使在这个简单的示例中，操作系统也必须做出许多决定。首先，系统必须在process0发出I/O时运行process1；这样做可以通过保持CPU忙碌来提高资源利用率。其次，系统决定在其I/O完成时不切换会process0。目前尚不清楚这是否是一个好的决定。这些类型的决策是由操作系统调度程序做出的。



<br/>
<br/>



### 数据结构

Data Structures

操作系统是一个程序，与任何程序一样，它有一些追踪各种相关信息的关键**数据结构(data structure)**。例如，为了追踪每个进程的状态，操作系统可能会为所有准备好的进程保留某种进程列表，并提供一些其它信息来追踪当前正在运行的进程。操作系统还必须以某种方式追踪被阻塞的进程；当I/O事件完成时，操作系统应确保唤醒正确的进程并准备好再次运行。

下面显示了操作系统需要跟踪内核中每个进程的信息类型。类似的过程结构存在于真实操作系统中，如Linux、Mac OSX、Windows...看看它们有多复杂。你可以看到操作系统追踪进程的几个重要信息。
对于已停止的进程，**寄存器上下文(register context)**将保持其寄存器的内容。当进程停止时，其寄存器将保持到该内存位置(memory location)。通过恢复这些寄存器，操作系统可以恢复运行该进程。这在以后上下文切换中详细介绍。

```c
// the registers xv6 will save and restore
// to stop and subsequently restart a process
struct context {
int eip;
int esp;
int ebx;
int ecx;
int edx;
int esi;
int edi;
int ebp;
};

// the different states a process can be in
enum proc_state { UNUSED, EMBRYO, SLEEPING,
                  RUNNABLE, RUNNING, ZOMBIE };

// the information xv6 tracks about each process
// including its register context and state
struct proc {
  char *mem; // Start of process memory
  uint sz; // Size of process memory
  char *kstack; // Bottom of kernel stack
        // for this process
  enum proc_state state; // Process state
  int pid; // Process ID
  struct proc *parent; // Parent process
  void *chan; // If non-zero, sleeping on chan
  int killed; // If non-zero, have been killed
  struct file *ofile[NOFILE]; // Open files
  struct inode *cwd; // Current directory
  struct context context; // Switch here to run process
  struct trapframe *tf; // Trap frame for the
        // current interrupt
};
```

还可从图中看出，除了running, ready, blocked之外，还有一些进程可以处于其它状态。
有时，系统将具有该进程在创建是所处的**初始状态(initial state)**。此外，可将进程置于已退出但尚未清除的**最终状态(final state)**(在基于Unix的系统中，这称为**僵尸状态(zombie state)**)。这个最终状态非常有用，因为它允许其它进程(通常是创建此进程的父进程)检查进程的返回代码并查看刚刚完成的进程是否成功运行(通常，程序在基于Unix系统中的返回码为0时，表示已成功完成任务，否则返回非0)。完成后，父进程将进行最后一次调用以等待孩子进程的完成，并且还向操作系统指示它可以清理任何涉及现在已经灭绝的进程的相关数据结构。

<br>

> ASIDE: DATA STRUCTURE — THE PROCESS LIST
> 操作系统充满了各种重要的数据结构。进程列表(process list)，也称为任务列表(task list)。它是比较简单的一个，都是现在能够同时运行多个程序的操作系统都会有类似于这种结构的东西，以便追踪系统中所有正在运行的程序。有时，人们会将存储过程信息的单个结构称为**进程控制块(PCB, process control block)**。

<br>

> ASIDE: KEY PROCESS TERMS(关键进程术语)
> **进程(process)**是正在运行的程序的主要操作系统抽象。在任何时间点，该进程都可以通过其状态来描述：其地址空间中的内存内容、CPU寄存器的内容、有关I/O的信息。
> **进程API**由可使进程相关联的调用程序组成。通常，这包括创建、销毁、其它有用的调用。
> 进程存在许多不同的**进程状态(process state)**，包括running、ready、blocking。不同的时间将进程从这些状态之一转换到另一个状态。
> **进程列表(process list)**包含有关系统中所有进程的信息。每个条目有时称为进程控制块(PCB)，它实际上只是一个包含特定进程信息的结构。




<br/>
<br/>
<br/>
<br/>




## 进程API

[Process API](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf)


> ASIDE: INTERLUDES
> **插曲(interludes)**将涵盖系统的多个实际方面，包括特别关注系统API以及如何使用它们。如果你不喜欢实际的事物(practical things)，你可跳过它。但你应该了解它，因为它们通常在现实生活中很有用。

<br>

> CRUX: HOW TO CREATE AND CONTROL PROCESSES
> 操作系统应该为进程创建和控制提供哪些接口？如何设计这些接口以实现强大的功能、易用性和高性能？

<br>

在此插曲中，将讨论Unix系统中的进程创建。Unix提供了一种使用一对**系统调用(system call)**创建新进程的最有趣的方法:

- `fork()`
- `exec()`

第三个例程，可以由希望等待进程创建完成的进程使用：

- `wait()`


<br/>
<br/>


### fork系统调用

The fork() System Call

`fork()`系统调用用于创建新进程。但是，要预先警告：这是你将要调用的最奇怪的例行程序。更具体的说，你有一个正在运行的程序，代码如下所示。输入并运行它。

```c
// Calling fork() (p1.c)

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid());
    int rc = fork();
    if (rc < 0) { // >>>fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) { // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
    } else { // parent goes down this path (main)
       printf("hello, I am parent of %d (pid:%d)\n",
            rc, (int) getpid());
    }
    return 0;
}
```

运行:

```bash
./p1
hello world (pid:14506)
hello, I am parent of 14507 (pid:14506)
hello, I am child (pid:14507)
```

当它第一次运行时，该进程打印出`hello world`消息，包含在该消息中的**进程标识符(PID, process identifier)**。在Unix系统中，如果想要对进程执行某些操作(如停止它)，则使用PID来命名进程。

现在有趣的部分开始了：该进程调用`fork()`系统调用，操作系统提供该此方法来创建新进程。奇怪的部分：创建的进程是调用进程的精确副本。这意味着对于操作系统来说，现在看起来的两个进程都是p1程序运行的副本，并且两个进程都从`fork()`系统调用返回。新创建的进程(child)不会像`main()`那样开始在`main()`上运行(hello world只打印了一次)。相反，它刚出现时，好像它已经调用了`fork()`本身。

你可能注意到，子进程不是一个精确的副本。尽管它有自己的地址空间、寄存器、PC...，它返回给`fork()`调用者的值是不同的。具体来说，当父进程接收新创建的子进程PID时，子进程接收返回码0.

你可能还注意到：p1的输出不确定。系统中有两个活动的父进程和子进程。假设在单个CPU的系统上运行，可能会发生相反的情况。

```bash
./p1
hello world (pid:29146)
hello, I am child (pid:29147)
hello, I am parent of 29147 (pid:29146)
```

CPU调度器，确定哪个进程在给定时刻运行。因为调度程序很复杂，我们通常不能对它将选择做什么做出强有力的假设，如最先运行哪个进程。这些不确定性导致了一些有趣的问题，特别是在多线程程序中，这将在并发中讨论。



<br/>
<br/>



### wait系统调用

The wait() System Call

到目前为止，我们还没有做太多工作：只创建了一个打印消息并退出的子进程。有时，事实证明，父进程等待子进程完成它一直在做的事情时非常有用的。这个任务是通过`wait()`系统调用完成的。

在下面的栗子中，父进程调用`wait()`以延迟执行，直到子进程执行完毕。子进程完成后，`wait()`返回父进程。添加了`wait()`调用使得数据具有稳定性，你们明白为什么吗？

```c
// Calling fork() And wait() (p2.c)

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid());
    int rc = fork();
    if (rc < 0) { // >>>fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) { // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
    } else { // parent goes down this path (main)
        int rc_wait = wait(NULL);
        printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n",
            rc, rc_wait, (int) getpid());
    }
    return 0;
}
```

```bash
 ./p2
hello world (pid:29266)
hello, I am child (pid:29267)
hello, I am parent of 29267 (rc_wait:29267) (pid:29266)
```

使用此代码，我们现在知道子进程将首先打印。但是，如果父进程碰巧先运行，它会立即调用`wait()`，这个系统调用在子进程运行并退出之前不会返回。因此，即使父进程先运行，它礼貌地等待子进程完成运行，然后`wait()`返回，然后父进程打印它的消息。



<br/>
<br/>



### exec系统调用

The exec() System Call

进程创建API的最后一个重要部分是`exec()`系统调用。当你想要运行与调用程序不同的程序时，此系统调用很有用。例如，在p2中调用`fork()`仅在你希望继续运行同一程序的副本时才有用。但是，通常你想运行一个不同的程序，`exec()`就是这么做的。

在下面的栗子中，子进程调用`execvp()`以运行程序wc(word count)。实际上，它从p3上运行wc，返回行数、词数和字节数。

```c
// Calling fork(), wait(), And exec() (p3.c)

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid());
    int rc = fork();
    if (rc < 0) { // >>>fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) { // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
        char *myargs[3];
        myargs[0] = strdup("wc"); // program: "wc" (word count)
        myargs[1] = strdup("p3.c"); // argument: file to count
        myargs[2] = NULL; // marks end of array
        execvp(myargs[0], myargs); // runs word count
        printf("this shouldn’t print out");
    } else { // parent goes down this path (main)
    int rc_wait = wait(NULL);
    printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n",
         rc, rc_wait, (int) getpid());
    }
    return 0;
}
```

```bash
./p3
hello world (pid:29383)
hello, I am child (pid:29384)
29 107 1030 p3.c
hello, I am parent of 29384 (rc_wait:29384) (pid:29383)
```

`fork()`系统调用很奇怪，它的伙伴`exec()`也不是那么正常。它的作用：给定可执行文件的名称和一些参数，从该可执行文件加载代码和静态数据并覆盖其当前代码段，重新初始化堆和栈以及程序内存空间的其它部分。然后操作系统运行该程序，传入任意参数为该进程的argv。因此，它不会创建新的进程。相反，它将当前运行的程序(p3)转换为不同的运行程序(wc)。在子进程的`exec()`之后，几乎就好像p3从未运行过，成功调用`exec()`永远不会有返回。



<br/>
<br/>



### Motivating The API

Why? Motivating The API

当然，可能会遇到一个大问题：为什么要建立一个奇怪的接口来创建一个新进程？事实证明，`fork()`和`exec()`的分离对于构建Unix shell至关重要，因为它允许shell在调用`fork()`之后，在调用`exec()`之前运行代码。此代码可以改变即将运行的程序的环境，从而可以轻松构建各种有趣的功能。

<br>

> TIP: GETTING IT RIGHT
> 简单和抽象都不能代替正确。有很多方法可以为进程创建设计API，但是，`fork()`和`exec()`的组合非常简单和强大。在这里，Unix设计师做对了。

<br>

shell只是一个用户程序。它会向你显示提示，然后等待你输入内容。你输入一个命令，在大多数情况下，shell确定文件系统中可执行文件所在的位置，调用`fork()`创建一个新的子进程来运行命令，调用`exec()`的某个变体来运行命令，然后通过调用`wati()`命令来等待命令的完成。当子进程完成时，shell从`wait()`返回并再次打印出一个提示，为下一个命令做好准备。

`fork()`和`exec()`的分离允许shell很容易地完成一堆有用的东西。例如: `wc p3.c > 1.txt`
shell完成此任务的方式非常简单，在创建子进程时，在调用`exec()`之前，shell关闭stdout并打开文件`1.txt`。通过这样做，即将运行的此程序的任何输出都被发送到文件而不是屏幕。

下面的程序便完成这样的操作:

```c
// All Of The Above With Redirection (p4.c)

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) {
    int rc = fork();
    if (rc < 0) { // >>>fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) { // child: redirect standard output to a file
        close(STDOUT_FILENO);
        open("./p4.output", O_CREAT|O_WRONLY|O_TRUNC, S_IRWXU);
        // now exec "wc"...
        char *myargs[3];
        myargs[0] = strdup("wc"); // program: "wc" (word count)
        myargs[1] = strdup("p4.c"); // argument: file to count
        myargs[2] = NULL; // marks end of array
        execvp(myargs[0], myargs); // runs word count
    } else { // parent goes down this path (main)
        int rc_wait = wait(NULL);
    }
    return 0;
}
```

执行:

```bash
./p4

cat p4.output
 29 116 874 p4.c
```

首先，当运行p4时，看起来好像什么也没有发生过。shell只是打印命令提示符，并立即为下一个命令做准备。但事实并非如此，p4确实调用`fork()`来创建一个新子节点，然后通过调用`execvp()`来运行wc程序。你没有看到任何输出打印到屏幕是因为它已被重定向到文件中。

Unix **管道(pipe)** 以类似的方式实现，但使用`pipe()`系统调用。这种情况下，一个进程的输出连接到一个内核管道(即队列)，另一个进程的输入连接到同一个管道。因此，一个进程的输出无缝地用作下一个进程的输入，并且长而有用的命令链可以串在一起。如这个栗子: ` grep -o foo file | wc -l`。

现在，只需说`fork()`和`exec()`组合是一种创建和操作进程的强大方法就足够了。

<br>

> ASIDE: **READ THE MAN PAGES**
> 本书中，当提到特定的系统调用或库调用时，会让你阅读**手册(man/manual pages)**。花时间阅读手册石喜彤程序员成长的关键一步，这些手册页中隐藏了大量有用的花絮。
> 最后，阅读手册可让你避免一些尴尬。当你向别人询问问题是，别人可能会叫你阅读文档。



<br/>
<br/>



### 进程控制和用户

Process Control And Users

在Unix系统中，除了`fork()`, `exec()`, `wait()`之外，还有许多其它接口可与进程进行交互。例如，`kill()`系统调用用于向进程发送**信号(signal)**，包括暂停(pause)、死亡(die)和其它有用的指令。为了方便起见，在大多数Unix Shell中，某些键组合被配置为向当前运行的进程传递特定信号。栗子如下:

| 键组合 | 信号 | 描述 |
| - | - | - |
| `ctrl+c` | SIGINT(2) | 中断 |
| `ctrl+z` | SIGTSTOP(19) | 停止(暂停) |

整个信号子系统提供了丰富的基础设施，可为进程提供外部事件，包括在各个进程中接收和处理这些信号的方法，以及向各个进程以及整个进程组(process groups)发送信号的方法。要使用这种通信形式，进程应使用`signal()`系统调用来捕获(catch)各种信号。这样做可确保当特定信号传递到进程时，它将暂停正常执行并运行特定代码以响应该信号。

这自然会提出一个问题：**谁可以向进程发送信号，谁不能发送？**
通常，系统可以让多个用户同时使用。如果其中一个人可以随意发送信号(如`SIGINT`)，则系统的可用性和安全性将受到影响。因此，现代系统包含用户的强烈概念。用户在输入密码建立凭据后，登录以获取对系统资源的访问权限。然后，用户可以启动一个或多个进程，并对它们进行完全控制(pause, kill...)，用户通常只能控制自己的进程。操作系统的工作是将资源(cpu, mem, disk...)分配给每个用户(及其进程)以满足整体系统目标。



<br/>
<br/>



### 有用的工具

Useful Tools

有许多命令行工具也很有用。如下:

- `ps`
- `top`
- `sar`
- `kill`
- `killall`



<br/>
<br/>



### 摘要

Summary

我们介绍了一些处理Unix进程创建的API: `fork(), exec(), wait()`。但是，我们刚刚撇去了表面。

**ASIDE: KEY PROCESS API TERMS**

- 每个进程都有一个名字，在大多数系统中，该名称为**PID**；
- `fork()`系统调用在Unix系统中用于创建新进程。创建者被称为**父进程（parent）**，被创建的新进程被称为**子进程（child）**；
- `wait()`系统调用允许父进程等待其子进程完成执行；
- `exec()`系统调用允许子进程摆脱与父进程的相似性并执行一个全新的程序；
- Unix Shell通常使用`fork(), exec(), wait()`来启动用户命令。`fork()`和`exec()`的分离支持I/O重定向、管道...；
- 进程控制以信号的方式提供，这可能导致作业停止、继续或终止；
- 可由特定用户控制哪些进程被封装在用户的概念中。操作系统允许多个用户同时登录，并确保用户只能控制自己的进程；
- **超级用户(superuser)**可以控制所有进程。出于安全考虑，请不要使用此用户进行直接操作。




<br/>
<br/>
<br/>




## 直接执行

[Mechanism: Limited Direct Execution](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-mechanisms.pdf)


为了虚拟化CPU，操作系统需要以某种方式在同时运行的许多作业中共享物理CPU。基本思路很简单：运行一个进程一段时间，然后运行另一个进程，以此类推。通过**分时共享(time sharing)**CPU，实现了虚拟化。

然而，构建这样的虚拟化也存在一些挑战：

- 首先是**性能（Performance）**：如何在不增加系统过多开销的情况下实现虚拟化？
- 第二是**控制（Control）**：如何保持在对CPU的控制的同时有效地运行进程？控制对操作系统尤其重要，，因为它控制资源。如果没有控制权，进程就可以永远运行并接管机器，或者访问不应该被它访问的信息。

因此，要在保持控制的同时获得高性能是构建操作系统的核心挑战之一。

> 关键：**如何通过控制有效地虚拟化CPU**
> 操作系统必须以有效地方式虚拟化CPU，同时保持对系统的控制。为此，需要硬件和操作系统的支持。操作系统通常会使用明智的硬件支持来完成其工作。



<br/>
<br/>



### 基本技术：有限的直接执行

Basic Technique: Limited Direct Execution

为了使程序以预期的速度运行，操作系统开发人员提出了一种技术——**有限的直接执行（limited direct execution）**。**直接执行（direct execution）**的想法很简单：只需在CPU上直接运行程序即可。因此，当操作系统启动一个程序运行时，它会在进程列表中为它创建一个进程条目，为它分配一些内存，将程序代码加载到内存中，找到它的入口点(`main()`例程或类似的东西)，跳转到它，并开始运行用户的代码。

<br>

**Direct Execution Protocol (Without Limits)**

| OS | Program |
| - | - |
| 创建进程列表条目<br>为程序分配内存<br>将程序加载到内存<br>使用`argc/argv`设置堆栈<br>清除寄存器<br>执行调用`main()` | |
| | 运行`main()`<br>从main执行return |
| 释放进程内存<br>从进程列表中删除条目 |

听起来很简单，但这种方法在我们尝试虚拟化CPU的过程中会产生一些问题：

- 如果我们只运行一个程序，操作系统如何确保程序不执行我们不希望它执行的操作，同时仍然有效地运行它？
- 当运行一个程序时，操作系统如何阻止它运行并切换到另一个进程，从而实现我们虚拟化CPU所需的分时共享？



<br/>
<br/>



### 问题1：受限制的操作

Problem1: Restricted Operations


直接执行具有快速的明显优势，程序直接在原生CPU硬件上运行，因此可按照预期的速度执行。但是在CPU上运行会引发一个问题：如果进程希望执行某种受限制的操作（如向磁盘发出I/O请求，访问更多系统资源(cpu, kernel)...），该怎么办？

> **如何执行受限制的操作**
> 进程必须能够执行I/O或其它一些受限制的操作，但不能让进程完全控制系统。操作系统和硬件该如何协同工作？

<br>

> **为什么系统调用看起来像程序调用**
> 你可能想知道为什么对系统调用（如`open(), read()...`）的调用看起来与C中的典型过程调用（procedure call）完全相同。也就是说，它看起来就像一个过程调用，系统如何知道它是一个系统调用，并做了所有正确的事情？原因很简单：它是一个过程调用，但隐藏在过程调用内部的是著名的**陷阱指令（trap instruction）**。举个栗子，当调用`open()`时，你正在执行对C library的过程调用。其中，无论是对于`open()`还是其它的系统调用，库都使用与内核达成一致的调用约定，将参数放在众所周知的位置（stack或register），也将系统调用号放入一个众所周知的位置(stack或register)，然后执行上述陷阱指令。陷阱解压后库中的代码将返回值，并将控制权返回给发出系统调用的程序。因此，进行系统调用的C库部分是在汇编中手工编码的，因为它们需要仔细准遵循约定，以便正确处理参数和返回值，以及执行特定于硬件的陷阱指令。这个汇编代码已经有人替你做了。

<br>

一种方法是让任何进程在I/O和其它相关操作方面做任何它们想做的事情。然而，这样做会妨碍构建所需的多种操作系统。例如，如果我们希望构建一个在授予文件访问权限之前检查权限的文件系统，我们不能简单地让任何用户进程向磁盘发出I/O。如果这样做了，一个进程可以简单地读写整个磁盘，因此所有的保护都将丢失。

因此，我们采用一种新的处理器模式——**用户模式（user mode）**。在用户模式下运行的代码受限于它们可以执行的操作。例如，在用户模式下运行时，进程无法发出I/O请求，这样做了会导致处理器引发异常，操作系统可能会杀死这个进程。

与用户模式相反，**内核模式（kernel mode）**是操作系统运行的模式。在此模式下，运行的代码可以执行其喜欢的操作，包括发出I/O请求、执行所有类型的受限制的指令。

但是，当**用户进程（user process）**希望执行某种特权操作（如I/O）时应该做什么？为了实现这一点，几乎所有的现代硬件都为用户程序提供了执行系统调用的能力。系统调用允许内核小心地将某些关键功能部件暴露给用户程序。如访问文件系统、创建和销毁进程、与其它进程通信以及分配更多内存...大多数操作系统提供了几百个调用（详情请参考POSIX标准）。

<br>

> **使用受保护的控制转移**
> 硬件通过提供不同的执行模式来协助操作系统。
> 在用户模式下，应用程序无法完全访问硬件资源。
> 在内核模式下，操作系统可以访问机器的全部资源。
> 还提供了从陷阱（trap）到内核（kernel）并**从陷阱返回（return-from-trap）**到用户模式的特殊指令，以及允许操作系统告知硬件陷阱表（trap table）驻留在内存中的指令。

<br>

要执行系统调用，程序必须执行特殊的陷阱指令。该指令同时跳转到内核并将权限级别提升为内核模式。一旦进入内核，系统现在可以执行所需的任何特权操作（如果允许），从而为调用进程执行所需的工作。完成后，操作系统会调用一个特殊的**从陷阱返回（return-from-trap）**指令，该指令将返回到调用用户程序，同时将权限级别降低到用户模式。

执行陷阱(trap)时硬件需要小心，它必须确保保存足够的调用程序寄存器（caller's register），以便在操作系统发出`return-from-trap`指令时能够正确返回。例如，在x86上，处理器会将程序计数器（counter）、标志（flag）和一些其它寄存器（register）推送（push）到每个进程的**内核栈（kernel stack）**。`return-from-trap`会将这些值从栈中弹出（pop）并继续执行用户模式的程序。其它硬件系统可能有所不同，但基本概念在不同平台上是相同的。

还有一个重要细节：陷阱如何知道在操作系统中运行哪些代码？显然，调用进程无法指定要跳转的地址。这样做会让程序跳转到内核，这显然是一个非常糟糕的想法。因此内核必须消息控制在陷阱（trap）上执行的代码。

内核通过在启动时设置**陷阱表（trap table）**来实现。当机器启动时，它在特权（内核）模式下，因此可以根据需要自由配置机器硬件。操作系统首先要做的事情之一就是告诉硬件在发生某些异常事件时要运行什么代码。
例如，当发生硬盘中断、发生键盘中断或程序进行系统调用时，应该运行什么代码？操作系统通常通过某种特殊指令通知硬件这些**陷阱处理程序（trap handler）**的位置。一旦通知硬件，它会记住这些处理程序的位置，直到机器下次重启，因此当系统调用或其它异常事件发生时，硬件知道该做什么。

<br>

**Limited Direct Execution Protocol**

| os@boot | hardware |
| - | - |
| initialize trap table | |
| | remember address of... <br> syscall handler |

| os@run | hardware | program(user mode) |
| - | - | - |
| Create entry for process list<br>Allocate memory for program<br>Load program into memory<br>Setup user stack with argv<br>Fill kernel stack with reg/PC<br>return-from-trap | | |
| | restore regs(from kernel stack)<br>move to user mode<br>jump to main | |
| | | Run main()<br>...<br>Call system call<br>trap into OS |
| | save regs(to kernel stack)<br>move to kernel mode<br>jump to trap handler | |
| Handle trap<br>Do work of syscall<br>return-from-trap | | |
| | restore regs(from kernel stack)<br>move to user mode<br>jump to PC after trap | |
| | | ...<br>return from main<br>trap (via exit()) |
| Free memory of process<br>Remove from process list | | |

此时间线总结了协议。假设每个进程都有一个内核寄存器，其中寄存器在进入和退出内核时保存到硬件，并从硬件恢复。

<br>

> **警惕安全系统中的用户输入**
> 即使我们在系统调用期间都非常努力地保护操作系统（通过添加硬件陷阱机制），但实现安全操作系统还有许多其它方面必须考虑。其中之一是在系统调用边界梳理参数，操作系统必须检查用户传入的内容并确保正确指定了参数，否则拒绝该调用。例如，通过`write()`调用，用户将缓冲区的地址指定为write调用的源。如果用户(意外或恶意)传入坏地址（如，内核地址空间的一部分），操作系统必须检测到这一点并拒绝该调用。否则，用户可以读取所有内核内存。鉴于kernel（virtual）内存通常还包括其它的所有物理内存，这个小的滑动将使程序能够读取系统中的任何其它进程的内存，这是非常危险的。
> 通常，安全系统必须非常怀疑地（great suspicion）处理用户输入。不这样做容易导致软件被黑，世界是一个不安全和可怕的地方。

<br>

要指定确切地系统调用，通常会为每个系统调用分配**系统调用号（system call number）**。因此，用户代码负责将所有所需的系统调用号放在寄存器或栈上的特定位置。操作系统在陷阱处理程序内部处理系统调用时，检查此号码，确保它有效。如果有效，则执行相应的代码。这种间接性是一种保护形式，用户代码无法指定要跳转确切地址，而是必须通过号码请求特定服务。

能够执行指令告诉硬件陷阱表所在的位置是一个非常强大的功能。因此，如你所猜，它也是一种特权操作（ privileged operation）。如果你在用户模式下执行此指令，硬件不会鸟你。
如果你可以安装自己的陷阱表，你可以对系统做些什么可怕的事情？你能接管机器吗？

<br>

有限的直接执行（LDE）有两个阶段：

- 启动时，内核初始化陷阱表，CPU会记住它的位置以供后续使用；
- 内核通过特权指令执行此操作。
内核在使用`return-from-trap`指令开始执行进程之前设置了一些东西（分配进程列表、内存...）。这会将CPU切换到用户模式并开始运行此进程。当进程希望发出系统调用时，操作系统处理进程并再次通过`return-from-trap`将控制权返回给进程。然后进程完成其工作，并从`main()`返回。它通常会返回存根代码，它将正确地退出程序。此时操作系统清理完毕，就完成了。



<br/>
<br/>



### 问题2：在进程间切换

Problem2: Switching Between Processes


直接执行的下一个问题是实现**进程之间的切换（switch between process）**。进程之间的切换很简单吗？操作系统应该决定停止一个进程并启动另一个进程。这看起来简单，但实际上有点棘手。具体来说，如果一个进程在CPU上运行，这意味着操作系统没有运行。如果操作系统没有运行，它怎么能做任何事情？吐过操作系统没有在CPU上运行，它显然没有办法采取行动。

> **如何恢复控制CPU**
> 操作系统如何重新获得对CPU的控制，以便它可在进程间切换？


<br/>
<br/>


#### 合作方法：等待系统调用

A Cooperative Approach: Wait For System Calls

一些系统过去采用一种**合作方法（cooperative approach）**。在这种风格中，操作系统信任系统的进程以合理地运行。假定运行时间过长的进程会定期放弃CPU，以便操作系统可以决定运行其它任务。

因此，你可能会问，友好的进程如何在这个乌托邦世界中放弃CPU？事实证明，大多数进程通过进行系统调用来非常频繁地将CPU的控制权转移到操作系统。像这样的系统通常包括一个显式的`yield`系统调用，除了将控制权转移到操作系统（以便操作系统可以运行其进程）之外什么都不做。

应用程序在执行非法操作时也会将控制权转移到操作系统。举个栗子，如果应用程序除以零，或者尝试访问它无法访问的内存，则会为操作系统生成陷阱（trap）。然后操作系统再次获得CPU控制权（并可能终止非法进程）。

因此，在协同调度（cooperative scheduling）系统中，操作系统通过等待系统调用或某种非法操作来重新获得CPU的控制权。你可能回想，这种被动方法也不理想呀！如果一个进程（恶意或错误）最终在无限循环中结束，并且从不进行系统调用，会发生什么？操作系统可以做什么？


<br/>
<br/>


#### 非合作方法：操作系统取得控制权

A Non-Cooperative Approach: The OS Takes Control

如果没有硬件的额外帮助，当一个进程拒绝进行系统调用并因此将控制权返回给操作系统时，操作系统根本无法做很多事情。事实上，在合作方法中，当一个进程陷入无限循环时，你唯一的办法就是采用古老的办法解决计算机系统中的所有问题：重启（Reboot）。因此，我们再次提出了获得CPU控制权的一个子问题。

<br>

> **如何在没有合作的情况下获得控制权（HOW TO GAIN CONTROL WITHOUT COOPERATION）**
> 即使进程没有合作，操作系统如何才能获得对CPU的控制？操作系统可以做些什么确保流氓进程不会接管机器？

<br>

答案很简单，许多人在许多年前构建操作系统时已经发现了：**定时器终端（timer interrupt）**。可以对定时器设备进行编程，以便每隔几毫秒(ms)产生一次中断。当中断被引发时，当前正在运行的进程停止（halted），并且操作系统中预配置的中断处理程序运行。此时，操作系统重新获得CPU的控制权。因此可以随心所欲：停止当前进程并启动另一个进程。

如前面讨论的那样，系统调用时，操作系统必须通知硬件当中断定时器发生时执行什么代码。因此，在启动时，操作系统就是这样做的。其次，在引导序列期间，操作系统必须启动定时器（这当然是特权操作）。一旦计时器开始，操作系统就可以感觉安全，因为控制权最终将返回给它，因此操作系统可以自由运行用户程序。

<br>

> **处理应用程序的坏事（DEALING WITH APPLICATION MISBEHAVIOR）**
> 操作系统通常必须处理行为不当的进程，这些进程（恶意或错误）尝试做它们不应该做的事情。在现代操作系统中，操作系统处理此类不当行为的方式是简单地终止（terminate）违法者。但当你试图非法访问内存或执行非法指令时，操作系统应该做什么呢？

<br>

请注意，当发生中断时硬件有一定的责任，特别是为了保存中断发生时运行的程序的足够的状态，以便后续的`return-from-trap`指令能够正确地恢复正在运行的程序。这组操作非常类似与在显式系统调用陷阱到内核期间硬件的行为，因此各种寄存器被保存，因此可通过`return-from-trap`轻松恢复。


<br/>
<br/>


#### 保存和恢复上下文

Saving and Restoring Context

现在操作系统已经重新获得了控制权，无论是通过系统调用，还是通过定时器中断，都必须做出决定——是继续运行当前进程，还是切换到另一个进程。该决定由称为**调度程序（scheduler）**的操作系统的一部分做出，这将在后面学习。

如果决定做切换，则操作系统执行低级代码。我们称之为**上下文切换（context switch）**。上下文切换的概念很简单：所有操作系统必须做的是为当前正在执行的进程保存一些寄存器值（如，在其内核栈上），并为即将执行的进程恢复（如，来自其内核栈）。通过这样做，操作系统因此确保当最终执行`return-from-trap`指令时，系统继续执行另一个进程，而不是返回到正在运行的进程。

为了保存当前正在运行的进程的上下文，操作系统将执行一些低级汇编代码（low-level assembly code），以保存和运行当前正在运行的进程的通用寄存器、PC、内核栈指针，然后恢复所述寄存器、PC，并切换到内核堆栈，以便于即将执行的进程。通过切换栈，内核在一个进程（被中断的进程）的上下文中进行切换代码的调用，并在另一个进程（将被执行的进程）的上下文中返回。操作系统最终执行`return-from-trap`指令，即将执行的进程将成为当前正在运行的进程。因此上下文切换完成。

<br>

> **使用定时器中断来重新获得控制权**
> 定时器中断使操作系统能够在CPU上再次运行，即使进程以非协作方式运行。因此，此硬件功能对于帮助操作系统维护机器的控制至关重要。

<br>

> **重启是有用的**
> 早些时候，我们注意到在协作下抢占无限循环（infinite loops）的唯一解决办法是重启机器。虽然你可能会嘲笑，但研究人员已经证明重启可以成为构建健壮系统的一个非常有用的工具。
> 具体来时，重启是有用的。因为它将软件移回到已知且更加可测试的状态。重启还会回收陈旧或泄露的资源（如，memory），否则这些资源可能难以处理。最后，重启很容易实现自动化。

<br>

整个进程的时间线如下所示。在此示例中，进程A正在运行，然后被定时器中断所中断。硬件保存其寄存器（在其内核栈上）并进入内核（切换到内核模式）。在定时器中断处理程序中，操作系统决定从正在运行的进程A切换到进程B。此时，它调用`switch()`例程，该例程小心地保存当前寄存器值（进入A的进程结构），恢复进程B的寄存器（来自其进程结构条目），然后切换上下文，特别是通过更改栈指针来使用B的内核栈（而不是A的）。最后，操作系统执行`return-from-trap`，它恢复B的寄存器并开始运行它。


**: Limited Direct Execution Protocol (Timer Interrupt)**

| os@boot<br>kernel mode | hardware |
| - | - |
| initialize trap table | |
| | remember addresses of...<br>syscall handler<br>timer handler |
| start interrupt timer | |
| | start timer<br>interrupt CPU in X ms |

| os@boot<br>kernel mode | hardware | program<br> user mode |
| - | - | - |
| | | Process A<br>... |
| | timer interrupt<br>save regs(A) → k-stack(A)<br>move to kernel mode<br>jump to trap handler | |
| Handle the trap<br>Call `switch()` routine<br>save regs(A) → proc t(A)<br>restore regs(B) ← proc t(B)<br>switch to k-stack(B) | | |
| return-from-trap (into B) | | |
| | restore regs(B) ← k-stack(B)<br>move to user mode<br>jump to B’s PC | |
| | | Process B<br>... |



<br/>
<br/>



### 担心并发？

Worried About Concurrency?


细心的读者可能会想到：在系统调用期间发生定时器中断，会发生什么？或，当你在处理一个中断时而另一个中断发生会发生什么？在内核中难处理吗？......

操作系统确实需要关注在中断或陷阱处理期间发生其它中断会发生什么。事实上，这是本书后面关于**并发（concurrency）**的内容。为了满足读者的胃口，这里介绍操作系统如何处理这些棘手情况的一些基础知识。

操作系统可能做的一件简单的事情，在中断处理期间**禁用中断(disable interrupts)**。这样做可确保在处理一个中断时，不会将其它任何中断传递到CPU。当然，操作系统必须小心这样做，长时间禁用可能会导致中断丢失，这是不好的。

操作系统还开发了许多复杂的**锁定（locking）**方案，以保护对内部数据结构的并发访问。这使得许多活动可以同时在内核中进行，特别适用于多处理器（multiprocessors）。这种锁定可能很复杂，并导致各种有趣且难以发现的错误(bugs)。



<br/>
<br/>



### 摘要

我们描述了一些实现CPU虚拟化的关键低级机制，这是一组我们统称为**有限直接执行（limited direct execution）**的技术。基本思路很简单：只需运行你想在CPU上运行的程序，但首先要确保设置硬件以便在没有操作系统辅助的情况下限制进程可以执行的操作。

我们具有虚拟化CPU的基本机制。但是一个主要问题没有回答：我们应该在给定时间运行哪个进程？**调度器**必须回答这个问题，这是后面讨论的问题。

<br>

> **上下文切换会花费多长时间**
> 你可能回想：上下文切换需要多长时间？或系统调用？有一些工作可以准确测量这些东西，以及一些其它可能的指标。
> 这当然也和硬件配置有关系。应当注意，并非所有操作系统都追踪CPU性能。许多操作系统是内存密集型的，并且内存带宽并没有像处理器速度那样显著提高。所以，购买强大的硬件配置能加速你的操作系统。

<br>

**CPU虚拟化术语**

- CPU至少支持两种执行模式：**受限的用户模式**和**特权内核模式（非受限）**
- 典型的用户应用程序以用户模式运行，并使用系统调用来陷阱(trap)到内核中以请求操作系统服务
- 陷阱指令小心保存寄存器状态，将硬件状态更改为内核模式，并跳转到操作系统到预先指定的目标：**陷阱表（trap table）**
- 当操作系统完成对系统调用的服务时，它会通过另一个特殊的`return-from-trap`指令返回到用户程序，这会降低权限并在跳转到操作系统的陷阱后将控制权返回给指令
- 操作系统必须在引导（boot）时设置陷阱表，并确保用户程序无法轻松修改它们。所有这些都是有限直接执行协议的一部分，改写以有效地运行程序但不会丢失操作系统控制
- 程序运行后，操作系统必须使用**硬件机制（定时器中断）**来确保用户程序不会永远运行。这种方法是CPU调度的非协作方法
- 有时，在定时器中断或系统调用期间，操作系统可能希望从运行当前进程切换到另一个进程，这是一种被称为**上下文切换（context switch）**的低级技术




<br/>
<br/>
<br/>




## 调度

[CPU Scheduling](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-sched.pdf)


到现在为止，运行进程的低级机制（上下文切换）应该是清楚的。但是，我们尚未了解操作系统调度程序使用的高级策略。
事实上，调度的起源早于计算机系统。早期的方法来自运营管理领域并应用于计算机。

<br>

> **如何制定调度策略（SCHEDULING POLICY）**
> 如何开发一个思考型调度策略的基本框架？关键假设是什么？哪些指标很重要？在最早的计算机系统中使用了哪些方法？


<br/>
<br/>


### 工作负载假设

Workload Assumptions


在介绍可能的策略范围之前，让我们首先对系统中运行的进程做一些简化的假设，有时统称为**工作负载（workload）**。确定工作负载是构建策略的关键部分，对工作负载了解的越多，你的策略就越精细。

我们将对系统中的进程（有时称为作业(jobs)）做出以下假设：

- 每个作业运行相同的时间
- 所有作业都在同一时间完成
- 一旦启动，每个作业都会运行完成
- 所有作业仅使用CPU
- 每个作业的运行时间都是已知的

这些假设很多是不现实的，正如乔治奥威尔《动物农场》中的一些动物比其它动物更平等，本章的一些假设比其它假设更不切实际。特别是，每个作业的运行时间都是已知的。这样做使调度程序无所不知。



<br/>
<br/>



### 调度指标

Scheduling Metrics


除了进行工作负载假设之外，还需要一件事来使我们能够比较不同的调度策略：**调度指标（Scheduling Metrics）**。指标用来衡量某些事物，不同的指标在调度中也有不同的意义。
但是，就目前而言。我们来看一个简单的指标：**周转时间（turnaround time）**。作业的周转时间定义为作业完成时间减去作业到达系统的时间：

$$T_{turnaround} = T_{completion} − T_{arrival}$$

应该注意，周转时间是一个性能指标。这将是本章的主要关注点。另一个有趣的指标是**公平性（fairness）**。在调度方面，性能和公平性往往不一致，这也告诉我们生活并不总是完美的。



<br/>
<br/>



### 先进先出

First In, First Out (FIFO)


一个最基本的算法为**先进先出（First In First Out (FIFO)）**调度。它具有许多积极的属性，简单且易于实现。并且根据假设，它运作良好。

让我们做一个快速的栗子。想象一下，有三个作业A, B, C在大致相同的时间到达系统（$$T_{arrival}=0$$）。由于先进先出必须放置一些工作，让我们假设A-B-C的顺序，假设每个作业运行10s。这些作业的平均周转时间是多少？

![](/images/OSTEP/fifo71.png)

如图，A在10时完成，B在20时完成，C在30时完成。因此，三个作业的平均周转时间仅为$$\frac{10+20+30}{3}=30$$。

<br>

让我们举个栗子来说明不同长度的作业如何导致先进先出调度出现问题。特别是，假设A运行100s,B和C还是10s。

![](/images/OSTEP/fifo72.png)

如图所示，在B或C有机会运行之前，作业A首先整整运行100s。因此，系统的平均周转时间很长：$$\frac{100+110+120}{3}=110$$，痛苦的110s。
这个问题通常被称为**车队效应（convoy effect）**，其中资源的一些小型消费者排在重量级消费者后面。那该怎么办？我们如何开发一种更好的算法来处理？



<br/>
<br/>



### 最短作业优先

Shortest Job First (SJF)


> **最短作业优先的原则**
> 最短作业优先表示可用于任何系统的一般调度原则，其中每个作业的感知周转时间很重要。如果有关机构关心客户满意度的话，很可能他们已经考虑使用最短作业优先。

<br>

**最短作业优先（Shortest Job First(SJF)）**，它首先运行最短的作业，然后是下一个最短的作业，依此类推。

![](/images/OSTEP/sjf73.png)

如图，该图表明了最短作业优先在平均周转时间方面的表现要好得多。它将之前的平均周转时间从110s减少到50s（$$\frac{10+20+120}{3}=50$$），这是极大的改善。
事实上，鉴于我们对所有作业的假设都是同时到达，所以证明它是最优调度算法。但我们的假设相当不切实际。

<br>

这里再举个例子。假设A在`t=0`时到达并且需要运行100s，而B和C在`t=10`时到达并且每个需要运行10s。

![](/images/OSTEP/sjf74.png)

如图，即使B和C在A之后不久到达，他们仍然被迫等到A完成，因此遭遇了同样的车队问题。平均周转时间为103.33s（$$\frac{100+(110-10)+(120-10)}{3}$$）。调度程序能做什么？

<br>

> **预备调度器**
> 事实上，所有现代调度程序都是先发制人，并且非常愿意停止一个运行的进程以运行其它进程。这意味着调度程序采用我们之前学习的机制。特别是，调度程序可以进行上下文切换，暂时停止一个正在运行的进程并恢复另一个进程。



<br/>
<br/>



### 最短完成时间优先

Shortest Time-to-Completion First (STCF)


我们还需要调度程序本身内的一些机制。鉴于前面关于计时器中断和上下文切换的讨论，调度程序当然可以在B和C到达时执行其它操作：它可以抢占作业A并决定运行另一个作业，可能会在执行继续作业A。最短作业优先是**非抢先式（non-preemptive）**调度程序，因此会遇到上述问题。
幸运的是，有一个调度程序正是这样做：向最短作业优先添加抢占，称为**最短完成时间优先（Shortest Time-to-Completion First (STCF)）**，或**抢先最短作业优先（Preemptive Shortest Job First (PSJF)）**调度程序。每当新作业进入系统时，最短完成时间优先调度程序就会确定剩余作业（包括新作业）中的哪一个剩余时间最少，并安排该作业。

![](/images/OSTEP/stcf75.png)

如图，最短完成时间优先将抢占作业A并运行作业B和作业C以完成。只有当它们完成时才会安排作业A的剩余时间。
这会大大改善平均周转时间：$$\frac{(120-0)+(20-10)+(30-10)}{3}=50$$。根据假设，可证明最短完成时间优先是最优。但假设相当不切实际。



<br/>
<br/>



### 响应时间指标

A New Metric: Response Time

如果我们知道工作长度，并且工作只使用了CPU，并且我们唯一的指标是周转时间。那个STCF将是一个很好的策略。实际上，对于早期的批处理计算系统，这些类型的算法有一定意义。然而，分时（shared time）机器的引入改变了这一切。现在，用户将坐在终端上并要求系统提供交互式性能。因此，一个新的指标诞生了：**响应时间（response time）**。

响应时间：$$T_{response=T_{firstrun}-T_{arrival}}$$

例如，作业A在0时到达，作业B和作业C在10时到达。则每个作业的相应时间如下：A(0-0)，B(10-10)，C(20-10)，平均值(3.33)。

正如你可能认为那样，STCF和相关方法对响应时间并不是特别好。如果三个作业同时到达，则第三个作业必须等待前两个作业完全运行才能安排一次。虽然周转时间很好，但这种方法对于响应时间和交互性来说非常糟糕。事实上，想象一下坐在终端前，打字输入，并且不得不等待10s才能看到系统的响应，因为其它工作已安排在你前面：非常不爽。

因此，我们还有另外一个问题：如果构建一个对响应时间敏感的调度程序？



<br/>
<br/>



### 轮询

Round Robin


为了解决这个问题，将引入一种新的调度算法，通常称为**轮询调度（RR, Round Robin）**。基本思路很简单：轮询不是运行作业完成，而是运行**时间切片（time slice）**作业，然后切换到运行队列中的下一个作业。它重复这样做，知道工作完成。因此，轮询有时被称为时间切片（ time-slicing）。注意，时间片的长度必须是定时器中断周期的倍数。例如，如果定时器中断每10ms中断一次，则时间片可以是10ms, 20ms, 10Nms。

<br>

为了更详细的了解轮询，让我们来看一个栗子。假设有三个作业A, B, C在系统中同时到达，并且每个作业都希望运行5s。最短作业优先在运行另一个作业之前运行每个作业（图7.6），相比之下，时间切片为1s的轮询将快速循环作业（图7.7）。

![](/images/OSTEP/sjf76.png)

![](/images/OSTEP/rr77.png)

平均响应时间：

- 轮询（RR）：$$\frac{0+1+2}{3}=1$$
- 最短作业优先（SJF）：$$\frac{0+5+10}{3}=5$$

如你所见，时间片的长度对轮询至关重要。它越短，响应时间的指标度量下轮询的性能越好。然而，使时间片太短是有问题的：上下文切换的成本将主导整体的性能。因此，决定时间片的长度给系统设计者带来了折中，使其足够长以**分摊（amortize）**切换成本不会使系统不再响应。

<br>

> **分摊可以降低成本（ AMORTIZATION CAN REDUCE COSTS）**
> 当某些操作存在固定成本时，一般的分摊技术通常用于系统中。通过较少地产生该成本，降低了系统的总成本。例如，如果时间片设置为10ms，并且上下文切换成本为1ms，则大约10%的时间用于上下文切换，浪费了。如果我们想分摊此成本，我们可以增加时间片（如100ms）。在这种情况下，上下文切换花费的时间少于1%，因此时间切片的成本已经分摊。

<br>

请注意，上下文切换的成本不仅仅来自于操作系统保存和恢复一些寄存器的操作。程序运行时，它们在CPU Cache、TLBs、Branch Predictors和其它分片上构建了大量状态(state)。切换到另一个作业会导致刷新(flush)此状态，并且将引入与当前正在运行的作业相关的新状态，这可能导致显著的性能成本。

因此，如果响应时间是我们的唯一指标，那么具有合理时间片的轮询将是一个出色的调度程序。但我们的老朋友周转时间呢？来看个栗子。A、B、C各自需要5s运行时间，它们同时到达，并且轮询的时间片为1s。从上面轮序的运行图可看出，A在13完成，B在14完成，C在15完成，平均时间为14。

如果周转时间使我们的指标，则轮序是最糟的策略之一。轮序正在做的是延长每个作业，只要它可以，只需在移动到下一个作业之前运行每个作业一小段。由于周转时间紧关注作业何时完成，因此在很多情况下，轮询几乎是悲观的，甚至比简单的先进先出更差。

任何公平的策略（如RR），即在小时间范围内在活跃进程之间均匀划分CPU，将在如周转时间的指标上表现不佳。实际上，这是一种固有的权衡：如果你愿意不公平，你可以完成更短的工作，按时以响应时间为代价；如果你更重视公平，那么响应时间会降低，但会以周转时间为代价。这种权衡(trade-off)在系统中很常见。you can’t have your cake and eat it too.

我们介绍了两种类型的调度程序，当然这些都是基于假设下：

- SJF, STCF优化了周转时间，但对响应时间不利；
- RR优化了响应时间，但对周转时间不利；



<br/>
<br/>



### 合并I/O

Incorporating I/O


> **重叠使得更高的使用率(OVERLAP ENABLES HIGHER UTILIZATION)**
> 如果可能，重叠(overlap)操作以最大化系统的利用率。重叠在许多不同的域中都很有用，包括执行磁盘I/O或向远程计算机发送消息时。在任何一种情况下，启动操作然和切换到其它作业是一个好主意，并提高系统的整体利用率和效率。

<br>

放松假设4，假设所有程序都执行I/O。想象一个没有任何输入的程序，它每次会产生相同的输出。

当作业启动I/O请求时，调度程序明会做一个明确地决定。因为当前正在运行的作业在I/O期间不会使用CPU，它被阻止(blocked)以等待I/O完成。如果将I/O发送到磁盘驱动器，则该进程可能会被阻塞几毫秒或更长时间，具体取决于驱动器当前的I/O负载。因此，调度程序应该可能在那时在CPU上安排另一个作业。

调度程序还必须在I/O完成时做出决定。发生这种情况时，会引发中断，并且操作系统会运行并将发出I/O请求的进程从阻塞状态(blocked back)移回就绪状态(ready state)。当然，它甚至可以决定在那时开展工作。操作系统应如何处理每个工作？

为了更好地理解这个问题，让我们假设有两个作业A和B，每个作业需要50ms的CPU时间。但有一个明显的区别：A运行10ms然后发出I/O请求（假设也许10ms），而B只使用CPU 50ms并且不执行I/O。调度程序首先运行A，然后运行B。

![](/images/OSTEP/incorporatingIO78.png)

<br>

假设正在尝试构建STCF调度程序。显然，只运行一个工作然后运行另一个工作而不考虑I/O是没有意义的。

一种常见的方法是将A的每个10ms子作业视为独立工作。因此，当系统启动时，它的选择是是否安排10ms A或50ms B。使用STCF是明确的。当A的第一个子作业完成时，只剩下B，它开始运行。接着提交一个A的新子作业，它会抢占B并运行10ms。这样做允许重叠，一个进程在等待另一个进程的I/O完成时使用CPU，这样可以更好地利用该系统。

![](/images/OSTEP/incorporatingIO79.png)

<br>

因此，我们看到调度程序如何合并I/O。通过将每个CPU突发视为作业，调度程序可确保**交互(interactive)**的进程进程运行。当这些交互式作业执行I/O时，其它CPU密集型作业会运行，从而更好地利用处理器。



<br/>
<br/>



### 摘要


我们前面假设知道每个作业的长度，这可能是最糟糕的假设。实际上，在通用操作系统中，操作系统对每项工作的长度知之甚少。

我们介绍了调度背后的基本思想，并开发了两类方法。第一个运行剩余的最短作业，从而优化周转时间；第二个在所有作业之间交替运行，从而优化响应时间。两者都有好有坏，在系统中需要一个权衡。我们还看到了如何将I/O合并到调度中，但仍然没有解决操作系统基本无法看到未来的问题。
不久我们将通过构建一个使用最近过去预测未来的调度程序，来了解如何克服这个问题。此调度程序称为**多级反馈队列(multi-level feedback queue)**，它是下一章的主题。



<br/>
<br/>
<br/>



## 多级反馈队列

[Scheduling: The Multi-Level Feedback Queue](http://pages.cs.wisc.edu/~remzi/OSTEP/cpu-sched-mlfq.pdf)
































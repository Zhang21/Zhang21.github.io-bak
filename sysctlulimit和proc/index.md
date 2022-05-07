# sysctl和ulimit和proc



参考：

- [sysctl命令](http://man.linuxde.net/sysctl)
- [ulimit命令](http://man.linuxde.net/ulimit)
- [ulimit、limits.conf、sysctl和proc文件系统](http://www.jianshu.com/p/20a2dd80cbad)
- [sysctl.conf学习和调优](http://www.jianshu.com/p/9a8e383b5b49)


<!--more-->

<br>

---

<br>


# sysctl

`sysctl ` 命令被用于在内核运行时动态地修改内核的运行参数，可用的内核参数在目录 `/proc/sys` 中。它包含一些`Tcp/Ip`堆栈和虚拟内存系统的高级选项，可以通过修改某些值来提高系统性能。

`sysctl`可以读取和设置超过五百个系统变量。
`sysctl`变量的设置通常是**字符串、数字或布尔型**（布尔型用1表示yes，0表示no）。

`sysctl` - configure kernel parameters at runtime.


**语法：**

```
#sysctl [options] [variable[=value]] [...]

sysctl -w net.ipv4.tcp_syncookies=1
```

<br>

可以通过`sysctl`命令修改系统变量，也可以通过编辑`sysctl.conf`配置文件来修改系统变量。

`sysctl.conf` - sysctl preload/configuration file.

举个栗子：

```
vim /etc/sysct.conf


# Controls source route verification
# Default should work for all interfaces net.ipv4.conf.default.rp_filter = 1
# net.ipv4.conf.all.rp_filter = 1
# net.ipv4.conf.lo.rp_filter = 1
# net.ipv4.conf.eth0.rp_filter = 1


# Disables IP source routing
# Default should work for all interfaces net.ipv4.conf.default.accept_source_route = 0
# net.ipv4.conf.all.accept_source_route = 0
# net.ipv4.conf.lo.accept_source_route = 0
# net.ipv4.conf.eth0.accept_source_route = 0


# Controls the System Request debugging functionality of the kernel
kernel.sysrq = 0


# Controls whether core dumps will append the PID to the core filename
# Useful for debugging multi-threaded applications
kernel.core_uses_pid = 1


# Increase maximum amount of memory allocated to shm
# Only uncomment if needed
# kernel.shmmax = 67108864


# Disable ICMP Redirect Acceptance
# Default should work for all interfaces
net.ipv4.conf.default.accept_redirects = 0
# net.ipv4.conf.all.accept_redirects = 0
# net.ipv4.conf.lo.accept_redirects = 0
# net.ipv4.conf.eth0.accept_redirects = 0


# enable Log Spoofed Packets, Source Routed Packets, Redirect Packets
# Default should work for all interfaces
net.ipv4.conf.default.log_martians = 1
#net.ipv4.conf.all.log_martians = 1
# net.ipv4.conf.lo.log_martians = 1
# net.ipv4.conf.eth0.log_martians = 1


# Decrease the time default value for tcp_fin_timeout connection
net.ipv4.tcp_fin_timeout = 25


# Decrease the time default value for tcp_keepalive_time connection
net.ipv4.tcp_keepalive_time = 1200


# Turn on the tcp_window_scaling
net.ipv4.tcp_window_scaling = 1


# Turn on the tcp_sack
net.ipv4.tcp_sack = 1


# tcp_fack should be on because of sack
net.ipv4.tcp_fack = 1


# Turn on the tcp_timestamps
net.ipv4.tcp_timestamps = 1


# Enable TCP SYN Cookie Protection
net.ipv4.tcp_syncookies = 1


# Enable ignoring broadcasts request
net.ipv4.icmp_echo_ignore_broadcasts = 1


# Disable ping requests
net.ipv4.icmp_echo_ignore_all = 1


# Enable bad error message Protection
net.ipv4.icmp_ignore_bogus_error_responses = 1


# make more local ports available
# net.ipv4.ip_local_port_range = 1024 65000


# set TCP Re-Ordering value in kernel to 5
net.ipv4.tcp_reordering = 5


# Lower syn retry rates
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries = 3


# Set Max SYN Backlog to 2048
net.ipv4.tcp_max_syn_backlog = 2048


# Various Settings
net.core.netdev_max_backlog = 1024


# Increase the maximum number of skb-heads to be cached
net.core.hot_list_length = 256


# Increase the tcp-time-wait buckets pool size
net.ipv4.tcp_max_tw_buckets = 360000


# This will increase the amount of memory available for socket input/output queues
net.core.rmem_default = 65535
net.core.rmem_max = 8388608
net.ipv4.tcp_rmem = 4096 87380 8388608 net.core.wmem_default = 65535
net.core.wmem_max = 8388608
net.ipv4.tcp_wmem = 4096 65535 8388608
net.ipv4.tcp_mem = 8388608 8388608 8388608
net.core.optmem_max = 40960

```


重新加载内核参数：

```sh
#-p, read values from file
sysctl -p

#-a, display all variables
sysctl -a
```





<br>

---

<br/>

# ulimit

大多Unix-Like系统，都提供了限制每个进程和每个基本用户使用线程，文件和网络连接等系统资源的一些方法。

假设有这样一种情况，当一台Linux主机上同时登陆了10人，在资源无限制的情况下，这10个用户同时打开了500个文件。假设每个文件的大小有10M，这是系统的内存资源就会收到巨大挑战。
但是任何一台主机的资源都不可能是无限的。所以，资源的合理配置和分配，不仅仅是保证系统可用性的必要条件，也与系统上软件运行的性能有着密不可分的联系。

`ulimit`是指每个user使用各种资源的限制值。`ulimit` 命令用来限制系统用户对shell资源的访问，它是一种简单并且有效的实现资源限制的方式。

- `ulimit`的设置值是 per-process的，也就是说，每个进程都有自己的limits值；
- 使用`ulimit`进行修改，是立即生效的；
- `ulimit`只影响shell进程及其子进程，用户登出后失效；
- 修改`ulimit`设置之后，要重启程序修改值才会有效。可通过`/proc`文件系统查看运行进程当前的限制值;
- 使用`ulimit`对系统限制的改变在系统重启后都会恢复到默认值;
- 可以在`profile`中加入`ulimit`的设置，便能做到永久生效。



<br>

**ulimit 用于限制 shell 启动进程所占用的资源，支持以下各种类型的限制：**

- 所创建的内核文件的大小；
- 进程数据块的大小；
- Shell进程创建文件的大小；
- 内存锁住的大小；
- 常驻内存集的大小；
- 打开文件描述符的数量；
- 分配堆栈的最大大小；
- CPU时间；
- 单个用户的最大线程数；
- Shell进程所能使用的最大虚拟内存；
- 它支持硬资源(hard)和软资源(soft)的限制。

<br/>

**sort和hard**

- hard：是指用户在任何时候都可以活动的进程的最大数量，这是上限。没有任何non-root进程能够增加hard ulimit；
- soft：是对会话或进程实际执行的限制，但任何进程都可以将其增加到hard ulimit的最大值。



<br>

## 设置ulimit

可以在以下位置进行ulimit的设置：

- `/etc/profile`，所有用户有效，永久生效；
- `~/.bash_profile`,当前用户有效，永久生效；
- 直接在控制台修改，当前用户有效，临时生效；

<br/>

永久生效：

```sh
vim /etc/profile

vim ~/.bash_profile
```


临时生效：

```sh
ulimit -a


core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 7170
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 7170
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited



#修改限定值
ulimit -n 201400
ulimit -t ulimited
```




<br>

---

<br/>

# limits.conf

`limits.conf` - configuration file for the pam_limits module

`limits.conf`是`pam_limits.so`的配置文件，Linux PAM(Pluggable Authentication Modules，插入式认证模块)。突破系统默认限制，对系统资源有一定保护作用。

**pam_limits模块**对用户的会话进行资源限制，然后`/etc/pam.d/`下的应用程序调用`pam_***.so`模块。

<br>

**`limits.conf`是针对用户，而`sysctl.conf`是针对整个系统参数配置。**


- 一个shell的初始limits就是由pam_limits设定的，用户登录后，pam_limits会给用户的shell设定在limits.conf定义的值；
- pam_limits的设定值也是per-process；
- pam_limits的设置是 永久生效的。


<br>

配置limits.conf：

```shell
vim /etc/security/limits.conf
```

<br>

举个栗子：

```
#<domain>      <type>  <item>         <value>

#*               soft    core            0
#*               hard    rss             10000
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#@student        -       maxlogins       4
```


domain：

- username
- @groupname


type：

- soft
- hard
- \-


item：

- core，限制内核文件的大小
- date，最大数据大小
- fsize，最大文件大小
- memlock，最大锁定内存地址空间
- nofile，打开文件的最大数目
- rss，最大持久设置大小
- stack，最大栈大小
- cpu，以分钟为单位的最多CPU时间
- nproc，进程的最大数目
- as，地址空间限制
- maxlogins，此用户允许登录的最大数目


value：

- item值的大小



<br>

---

<br/>
# /proc

## 什么是/proc文件系统

Linux内核提供了一种通过`/proc`文件系统，在运行时访问内核内部数据结构，改变内核设置的机制。

proc文件系统是一个伪文件系统，它只存在内存当中，不占用外部空间。它以文件系统的方式为访问系统内核数据的操作提供接口。

对`/proc`中内核文件的修改，针对的是**整个系统**的**内核参数**，修改后**立即生效**，但修改是 **临时的**，重启后失效。


<br>

## /proc与sysctl.conf的对应关系

修改`/proc`文件系统中的参数是临时的，但修改`sysctl.conf`的参数确是永久有效的。

配置文件`sysctl.conf`变量在`/proc/sys`下，其对应关系如下：

```
#将文件名的 . 变为 /

#/proc/sys/net/ipv4/icmp_echo_ignore_all
#net.ipv4.icmp_echo_ignore_all

echo 0 > /proc/sys/net/ipv4/icmp_echo_ignore_all

vim /etc/sysctl.conf
net.ipv4.icmp_echo_ignore_all = 0
```

<br>

## /proc文件系统几个常用的内核文件

- /proc/meminfo    #内存信息
- /proc/cpuinfo    #CPU信息
- /proc/sys/fs/file-max    #文件打开数
- /proc/sys/fs/file-nr    #整个系统目前使用的文件句柄数量


<br>

## /proc文件系统中文件的权限

proc中的每个文件都有一组分配给它的非常特殊的文件许可权，并且每个文件属于特定的用户标识。

- 只读：任何用户都不能更改该文件，它用于表示系统信息
- root写
- root读

<br>

## 对/proc进行读写

```
cat /proc/sys/net/ipv4/icmp_echo_ignore_all
#0

echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all

#当然,也可是用sysctl来配置
```

<br>

## /proc内核文件详解

- /proc/buddyinfo 每个内存区中的每个order有多少块可用，和内存碎片问题有关
- /proc/cmdline 启动时传递给kernel的参数信息
- /proc/cpuinfo cpu的信息
- /proc/crypto 内核使用的所有已安装的加密密码及细节
- /proc/devices 已经加载的设备并分类
- /proc/dma 已注册使用的ISA DMA频道列表
- /proc/execdomains Linux内核当前支持的execution domains
- /proc/fb 帧缓冲设备列表，包括数量和控制它的驱动
- /proc/filesystems 内核当前支持的文件系统类型
- /proc/interrupts x86架构中的每个IRQ中断数
- /proc/iomem 每个物理设备当前在系统内存中的映射
- /proc/ioports 一个设备的输入输出所使用的注册端口范围
- /proc/kcore 代表系统的物理内存，存储为核心文件格式，里边显示的是字节数，等于RAM大小加上4kb
- /proc/kmsg 记录内核生成的信息，可以通过/sbin/klogd或/bin/dmesg来处理
- /proc/loadavg 根据过去一段时间内CPU和IO的状态得出的负载状态，与uptime命令有关
- /proc/locks 内核锁住的文件列表
- /proc/mdstat 多硬盘，RAID配置信息(md=multiple disks)
- /proc/meminfo RAM使用的相关信息
- /proc/misc 其他的主要设备(设备号为10)上注册的驱动
- /proc/modules 所有加载到内核的模块列表
- /proc/mounts 系统中使用的所有挂载
- /proc/mtrr 系统使用的Memory Type Range Registers (MTRRs)
- /proc/partitions 分区中的块分配信息
- /proc/pci 系统中的PCI设备列表
- /proc/slabinfo 系统中所有活动的 slab 缓存信息
- /proc/stat 所有的CPU活动信息
- /proc/sysrq-trigger 使用echo命令来写这个文件的时候，远程root用户可以执行大多数的系统请求关键命令，就好- 像在本地终端执行一样。要写入这个文件，需要把/proc/sys/kernel/sysrq不能设置为0。这个文件对root也是不可- 读的
- /proc/uptime 系统已经运行了多久
- /proc/swaps 交换空间的使用情况
- /proc/version Linux内核版本和gcc版本
- /proc/bus 系统总线(Bus)信息，例如pci/usb等
- /proc/driver 驱动信息
- /proc/fs 文件系统信息
- /proc/ide ide设备信息
- /proc/irq 中断请求设备信息
- /proc/net 网卡设备信息
- /proc/scsi scsi设备信息
- /proc/tty tty设备信息
- /proc/net/dev 显示网络适配器及统计信息
- /proc/vmstat 虚拟内存统计信息
- /proc/vmcore 内核panic时的内存映像
- /proc/diskstats 取得磁盘信息
- /proc/schedstat kernel调度器的统计信息
- /proc/zoneinfo 显示内存空间的统计信息，对分析虚拟内存行为很有用


<br>

**以下是/proc目录中进程N的信息：**

- /proc/N pid为N的进程信息
- /proc/N/cmdline 进程启动命令
- /proc/N/cwd 链接到进程当前工作目录
- /proc/N/environ 进程环境变量列表
- /proc/N/exe 链接到进程的执行命令文件
- /proc/N/fd 包含进程相关的所有的文件描述符
- /proc/N/maps 与进程相关的内存映射信息
- /proc/N/mem 指代进程持有的内存，不可读
- /proc/N/root 链接到进程的根目录
- /proc/N/stat 进程的状态
- /proc/N/statm 进程使用的内存的状态
- /proc/N/status 进程状态信息，比stat/statm更具可读性
- /proc/self 链接到当前正在运行的进程


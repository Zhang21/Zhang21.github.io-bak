---

title: "Wireshark"
date: "2017-10-25 15:47:39"
tags: "Wireshark"
categories: "Network"

---


<br>

<!--more-->


<br/>



# 过滤语法


wireshark过滤分为两种:

- 抓包过滤
- 显示过滤

<br>

> **尽量避免使用抓包过滤。即便多看几个报文，也比漏掉一个报文要好。**

<br/>
<br/>


## 抓包过滤


**类型**

- host
- net
- port

<br>

**方向**

- src
- dst

<br>

**协议**

- ether
- ip/arp
- tcp/udp
- http/dns/ftp/icmp
- ...

<br>

**逻辑运算符**

- `&&`
- `||`
- `!`

<br>

栗子：

```
#主机
host www.xx.com
src host 192.168.1.1 && dst port 80
host 193.168.1.1 || host 192.168.1.2


#广播包
!broadcast


#MAC
ether host 00:88:ab:56:12:0d
src ether host 00:88:ab:56:12:0d


#IP
host 192.168.1.1
dst host 192.168.1.1


#net
net 192.168.1.0/24
src net 192.168.1.0/24


#vlan
vlan 11


#Port
port 80
! port 443
dst port 80
udp dst port 5678
portrange 1-80

```



<br/>
<br/>


## 显示过滤


**比较操作符**

- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`

<br>

**逻辑操作符**

- `and`
- `or`
- `xor`
- `not`


<br>

**IP**

- `ip.addr`
- `ip.src`
- `ip.dst`

<br>

**Port**

- `tcp.port`
- `tcp.srcport`
- `tcp.dstport`
- `tcp.flag.syn`
- `tcp.flag.ack`

<br>

**Protocol**

- `arp`
- `ip`
- `icmp`
- `udp`
- `tcp`
- `dns`
- ...

<br>

栗子：

```
#ip
ip.addr == 1.1.1.1
ip.src == 1.1.1.1 and ip.dst == 2.2.2.2


#port
tcp.port == 80
tcp.dstport == 80
tcp.flag.syn == 1


#pro
arp
not icmp

```




<br/>
<br/>

---

<br/>
<br/>




# HTTPS


Wireshark也可以分析HTTPS加密的包，但你需要用证书将包先解密。
在Edit->Preferences->Protocol->SSL选项填写相关信息进行解密。






















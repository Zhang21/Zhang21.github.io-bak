# OpenVPN



参考:

- OpenVPN: <https://github.com/OpenVPN/openvpn>
- easy-rsa: <https://github.com/OpenVPN/easy-rsa>

<br/>

环境:

- RHEL7
- OpenVPN v2.4.6
- easy-rsa v3.0.3



<br/>
<br/>

<!--more-->

---

<br/>
<br/>



# 概述


通过在云端VPC， k8s集群内运行OpenVPN Server，让本地可以通过连接OpenVPN进行访问云资源，而不需要将云资源开放公网访问。

我是将OpenVPN运行在k8s 集群了，对它提供ELB进行公网连接。在S端配置文件中推送对应的路由信息——如集群内节点CIDR， 服务CIDR, VPC CIDR...





<br/>
<br/>

---

<br/>
<br/>




# 安装和配置


<br/>


## 安装


需要安装:

- EPEL
- openvpn
- easy-rsa: 用于制作CA证书，S端证书，C端证书

安装了EPEL源之后就可以直接安装`openvpn`和`easy-rsa`，当然也可以从GitHub上拉取。

<br>

```
yum install -y epel-release

yum install -y openvpn easy-rsa
```



<br/>
<br/>
<br/>



## 制作证书


<br/>


### 编辑vars文件

此处需注意，通过yum安装可能会没有`example.vars`这个栗子文件。没关系，请在[easy-rsa GitHub](https://github.com/OpenVPN/easy-rsa/tree/v3.0.6/easyrsa3)去下载一份过来。

```
mkdir -p /etc/openvpn/easy-rsa/server
mkdir -p /etc/openvpn/easy-rsa/client


#拷贝easy-rsa文件，用于制作证书
cp -r /usr/share/easy-rsa/3.0.3/  /etc/openvpn/easy-rsa/server/
cp -r /usr/share/easy-rsa/3.0.3/  /etc/openvpn/easy-rsa/client/


#先制作CA和S端证书
cd /etc/openvpn/easy-rsa/server/
cp vars.example vars


#修改几个配置
vim vars

#根据自己的情况进行修改
set_var EASYRSA_REQ_COUNTRY “CN” #国家
set_var EASYRSA_REQ_PROVINCE “Sichuan” #省份
set_var EASYRSA_REQ_CITY “ChengDu” #城市
set_var EASYRSA_REQ_ORG “TianFu” #非盈利组织，此处可填公司之类
set_var EASYRSA_REQ_EMAIL “abc@xyz.com” #邮箱地址
set_var EASYRSA_REQ_OU “My OpenVPN” #组织单元
```


<br/>
<br/>



### 创建证书和秘钥

- **CA证书和S端证书**

```sh
cd /etc/openvpn/easy-rsa/server
./easyrsa -h


#初始化，会读取vars文件
./easyrsa init-pki



#创建根证书
#这里会要求输入PEM pass，这个请记住，后面签名需要此密码
./easyrsa build-ca
#这里生成CA证书
#pki/ca.crt



#创建S端证书
#nopass选项表示不加密
./easyrsa gen-req server nopass
#这里生成两个文件
#pki/reqs/server.req
#pki/private/server.key



#签约S端证书
#第一个server表示S端，后面是取的名字
./easyrsa sign server server
#这里需要输入CA证书的PEM pass
#之后会生成S端证书
#pki/issued/server.crt



#创建Diffie-Hellman
./easyrsa gen-dh
#生成dh.pem文件
#pki/dh.pem
```

<br>

- **C端证书**

```sh
cd /etc/openvpn/easy-rsa/client/
#./easyrsa -h



#初始化
./easyrsa init-pki



#创建C端证书
./easyrsa gen-req client nopass
#这里生成两个文件
#pki/reqs/client.req
#pki/private/client.key



#在CA端导入C端证书
cd /etc/openvpn/easy-rsa/server
./easy-rsa import-req /etc/openvpn/easy-rsa/client/reqs/client.req client



#签约C端证书
#第一个client表示C端，第二个为定义的名字
./easyrsa sign client client
#这里需要输入CA证书的PEM pass
#之后会生成C端证书
#/etc/openvpn/easy-rsa/server/pki/issued/client.crt
#注意生成的位置，不要搞错了
```

<br>

- **梳理上面生成的文件**

```
server/pki/ca.crt
server/pki/dh.pem

server/pki/reqs/server.req
server/pki/reqs/client.req

server/pki/private/ca.key
server/pki/private/server.key

server/pki/issued/server.crt
server/pki/issued/client.crt

#client/pki/reqs/client.req
client/pki/private/client.key

```


<br/>
<br/>
<br/>



## 拷贝相应证书到openvpn目录下


```sh
#S端
cd /etc/openvpn/server

cp /etc/openvpn/easy-rsa/server/pki/ca.crt .
cp /etc/openvpn/easy-rsa/server/pki/private/server.key .
cp /etc/openvpn/easy-rsa/server/pki/issued/server.crt .
cp /etc/openvpn/easy-rsa/server/pki/dh.pem .



#C端
cd /etc/openvpn/client
cp /etc/openvpn/easy-rsa/server/pki/ca.crt .
cp /etc/openvpn/easy-rsa/client/pki/private/client.key .
cp /etc/openvpn/easy-rsa/server/pki/issued/client.crt .
```



<br/>
<br/>
<br/>



## 配置文件


在[openvpn GitHub](https://github.com/OpenVPN/openvpn/tree/master/sample)去下载对应配置文件，做相应的修改。


<br/>


### S端配置文件

一下只是我的栗子，详细信息请参考自己的项目。具体的每个选项描述，栗子文件里面有解释。

`vim server.conf`:

```
port 1194
proto udp
dev tun

ca /etc/openvpn/server/ca.crt
cert /etc/openvpn/server/server.crt
key /etc/openvpn/server/server.key
dh /etc/openvpn/server/dh.pem

#VPN CIDR
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt

#推送的S端的CIDR给C端路由
push "route 10.0.0.0 255.255.224.0"

#推送S端DNS
push "dhcp-option DNS 10.247.3.10"
push "dhcp-option DNS 114.114.114.114"

client-to-client
keepalive 20 120
cipher AES-256-CBC
persist-key
persist-tun
log         /dev/stdout
log-append  /dev/stdout

verb 3
explicit-exit-notify 1


#启用用户/密码进行登录需要添加的选项
#栗子文件里面没有这些信息
script-security 3
auth-user-pass-verify /etc/openvpn/checkpsw.sh via-env
#http://openvpn.se/files/other/checkpsw.sh
#去下载这个脚本
#client-cert-not-required
#此选项只使用用户密码，不使用证书
#注释它，使用证书和用户密码双重登录
username-as-common-name
```


<br/>
<br/>


### C端配置文件

`vim client.ovpn`:

```
client
dev tun

proto udp
remote addr port

resolv-retry infinite

nobind

persist-key
persist-tun


#此处我将CA证书和C端证书信息写入配置文件
#当然，也可下载证书在指定，但这麻烦了
<ca>
-----BEGIN CERTIFICATE-----
xxxxxxxxxxx
-----END CERTIFICATE-----
</ca>

<cert>
-----BEGIN CERTIFICATE-----
xxxxxxxxxxxxxxxxx
-----END CERTIFICATE-----
</cert>

<key>
-----BEGIN PRIVATE KEY-----
xxxxxxxxxxxxxxxx
-----END PRIVATE KEY-----
</key>


remote-cert-tls server
cipher AES-256-CBC
verb 3

#用户认证
script-security 3
auth-user-pass
#可将用户信息写入文件，用户密码各一行
```


<br/>
<br/>


### 另外几个配置

`vi checksw.sh`:

```sh
#!/bin/sh
###########################################################
# checkpsw.sh (C) 2004 Mathias Sundman <mathias@openvpn.se>
#
# This script will authenticate OpenVPN users against
# a plain text file. The passfile should simply contain
# one row per user with the username first followed by
# one or more space(s) or tab(s) and then the password.
  
PASSFILE="/etc/openvpn/psw-file"
LOG_FILE="/etc/openvpn/openvpn-password.log"
TIME_STAMP=`date "+%Y-%m-%d %T"`
  
###########################################################
  
if [ ! -r "${PASSFILE}" ]; then
  echo "${TIME_STAMP}: Could not open password file \"${PASSFILE}\" for reading." >> ${LOG_FILE}
  exit 1
fi
  
CORRECT_PASSWORD=`awk '!/^;/&&!/^#/&&$1=="'${username}'"{print $2;exit}' ${PASSFILE}`
  
if [ "${CORRECT_PASSWORD}" = "" ]; then
  echo "${TIME_STAMP}: User does not exist: username=\"${username}\", password=\"${password}\"." >> ${LOG_FILE}
  exit 1
fi
  
if [ "${password}" = "${CORRECT_PASSWORD}" ]; then
  echo "${TIME_STAMP}: Successful authentication: username=\"${username}\"." >> ${LOG_FILE}
  exit 0
fi
  
echo "${TIME_STAMP}: Incorrect password: username=\"${username}\", password=\"${password}\"." >> ${LOG_FILE}
exit 1
```

<br>

`vi psw-file`: 这为可登录的用户密码

直接往这个文件写入用户和密码即可，并不需要重启openvpn服务。

```
user1    pass-user1

#comment
user2   pass-user2
```

<br>

`vi start_openvpn.sh`: 启动脚本

```sh
#!/bin/bash

mkdir -p /dev/net

if [ ! -c /dev/net/tun ]; then
    mknod /dev/net/tun c 10 200
fi

echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf && sysctl -p

#此处一定要记得写iptables，否则后面连上了VPN也无法正常访问
#我也是找了好久才找到这个问题
#这个网段为openvpn里面定义的网段
iptables -t nat -A POSTROUTING -s 10.8.0.0/24  -j MASQUERADE

cd /etc/openvpn

#--daemon，放入后台
/sbin/openvpn --config /etc/openvpn/server/server.conf
```

<br>

由于我是运行在k8s集群容器内，所有还有几个文件:

- `Dockerfile`
- `.dockerignore`
- `k8s.yaml`: 由于它需要创建和使用系统资源，所以请使用特权容器运行



<br/>
<br/>

---

<br/>
<br/>




# 启动


- 启动S端
- 客户端连接
  - Windows客户端
  - Linux客户端
  - Mac客户端


启动之后应该就能正常访问了，如果不能正常访问，请查看你推送的CIDR和DNS，还有ipv4转发和iptables等。

<br>

```
#S端
#由于需要使用和创建系统资源，所以请用特权容器进行运行，不然会提示没有权限
/sbin/openvpn --config /etc/openvpn/server/server.conf



#C端

#Windows下载Openvpn GUI，制定客户端配置文件进行连接，之后输入用户名和面膜

#Linux下
#/sbin/openvpn --config /etc/openvpn/client/client.ovpn

#Mac下，下载对应Openvpn软件，指定配置文件进行连接
```












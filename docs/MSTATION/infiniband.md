# infiniband驱动安装

## 简介
InfiniBand架构是一种支持多并发链接的“转换线缆”技术，在这种技术中，每种链接都可以达到2.5Gbps的运行速度。这种架构在一个链接的时候速度是500MB/秒，四个链接的时候速度是2GB/秒，12个链接的时候速度可以达到6GB/秒。InfiniBand技术不是用于一般网络连接的，它的主要设计目的是针对服务器端的连接问题的。因此，InfiniBand技术将会被应用于服务器与服务器（比如复制，分布式工作等），服务器和存储设备（比如SAN和直接存储附件）以及服务器和网络之间（比如LAN， WANs和the Internet）的通信。

## 下载驱动
查看infiniband线缆型号
```
lspci -v | grep Mellanox

```
到[Mellanox](http://www.mellanox.com/page/software_overview_ib)官网下载相应的驱动程序

## 安装驱动
解压下载的驱动包 `tar -zxvf MLNX_XXX.tgz`

进入解压的文件夹中执行 `./mlnxofedinstall`

重启 `reboot`

## 配置驱动
添加和启动服务
```
service openibd start
chkconfig openibd on
service opensmd start
chkconfig opensmd on

```
查看HCA端口状态 ibstat

## IPoIB配置
在/etc/sysconfig/network-scripts/下ifcfg-ib0文件
```
CONNECTED_MODE=no
TYPE=InfiniBand
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ib0
DEVICE=ib0
ONBOOT=yes
IPV6_PRIVACY=no
IPADDR=10.0.0.1
NETMASK=255.255.255.0
BROADCAST=10.0.0.255
NETWORK=10.0.0.0	

```
重启网络接口ib0
```
ifdown ib0
ifup ib0

```
测试接口
```
ping 10.0.0.1	

```
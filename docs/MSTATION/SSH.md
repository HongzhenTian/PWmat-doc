# SSH免密码登陆

## 简介
SSH 为 Secure Shell 的缩写，由 IETF 的网络小组（Network Working Group）所制定；SSH 为建立在应用层基础上的安全协议。SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。
## 下载安装文件
下载ssh相关的软件包
```
yum install openssh

```

## 获取机器指纹
```
for i in ms1 ms2
do
  ssh-keyscan -t rsa ${i} >> /etc/ssh/ssh_known_hosts
done

```
得到的ssh_known_hosts进一步修改如下：
```
ms1,192.168.199.154 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDTyRcul0/h1XN5brsVUL0Dd...	

```

## 创建主机文件
在/etc/ssh/下设置shosts.equiv
```
ms1
ms2
...
	

```

## 配置SSH服务
配置sshd_config
```
HostbasedAuthentication yes
IgnoreRhosts no		

```
配置ssh_config
```
Host *
    HostbasedAuthentication yes
	EnableSSHKeysign yes

```
## 配置其他主机
将上面的四个文件复制到其他机器上面

重启ssh服务
```
service sshd restart	

```
# NFS服务配置

## 简介
NFS（Network File System）即网络文件系统，是FreeBSD支持的文件系统中的一种，它允许网络中的计算机之间通过TCP/IP网络共享资源。在NFS的应用中，本地NFS的客户端应用可以透明地读写位于远端NFS服务器上的文件，就像访问本地文件一样。
## 下载安装文件
下载nfs相关的软件包
```
yum install nfs-utils
yum install rpcbind

```

## 添加启动服务
启动rcpbind和nfs服务
```
systemctl enable rpcbind
systemctl start rpcbind
systemctl enable nfs
systecmctl start nfs

```

## 配置共享目录
创建/home/nfs目录 `mkdir -p /home/nfs/`

修改权限 `chmod 777 /home/nfs/`

修改/etc/exports文件
```
/home/nfs 10.0.0.0/24(insecure,rw,sync,no_root_squash)	

```

## 挂载共享目录
```
mount -t nfs -o nfsvers=3,vers=3 10.0.0.2:/home/nfs /home/nfs		

```

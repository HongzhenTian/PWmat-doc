# texlive安装

## 下载最新版TeX Live
```
http://tug.org/texlive/acquire-iso.html	
```

## 进入root账号，挂载镜像

```
mount -o loop texlive2018.iso /mnt/iso如果没有/mnt/iso目录，请手动建立mkdir -p /mnt/iso)

```						
							
## 安装
```
cd /mnt/iso
./install-tl

```

## 配置环境
```
    export MANPATH=${MANPATH}:/usr/local/texlive/2018/texmf-dist/doc/man
    export INFOPATH=${INFOPATH}:/usr/local/texlive/2018/texmf-dist/doc/info
    export PATH=/usr/local/texlive/2018/bin/x86_64-linux:${PATH}
    
```    							
## 卸载镜像

```
cd
umount /mnt/iso
```
    							
## 更新

 ```   
 tlmgr update --self
 tlmgr update --all
 ```
    							


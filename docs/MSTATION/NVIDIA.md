# NVIDIA驱动安装

## 简介
CentOS7默认会安装开源显卡驱动“Nouveau”，这个驱动会影响Nvidia官方驱动的安装。至于二者之间的关系，可以用下面一段话来解释。
```
During an interview, in Finland Linus Torvalds the man behind the 
exceptional idea of Linux and git source code management, gave 
his ‘Middle Finger Salute‘ to NVIDIA in frustration with the support 
provided by the company for the Linux platform.

Torvalds is depressed with the fact that NVIDIA is not supporting 
Linux, enough. It gets even more worse with the truth that NVIDIA 
is getting hot with every passing day in Android based mobile handset 
market which literally means that NVIDIA is not supporting Linux.

The outburst of anger and frustration was the result of the question 
asked by a Linux user. The question was ‘Optimus‘ feature of NVIDIA 
which lets the user to switch On/Off Graphics Processing Unit (GPU) 
to save power came late for Linux, as compared to other Operating 
Systems. NVIDIA was very much clear when asked about this, and clearly 
stated that NVIDIA is not going to support Linux to the point, Windows 
and Mac would get.

This issue of NVIDIA is not new and the users have been complaining 
for years regarding this. The Advanced Micro Devices (AMD) has tried 
to fill this with its own open source driver. NVIDIA refused to release 
Open Source driver saying that it can not make critical informations 
publicly available.

```
神仙打架的事情暂且不管，下面介绍如何安装Nvidia的驱动。
## 确定GPU型号
安装开发工具
```
# yum groupinstall "Development Tools"					 
# yum install kernel-devel kernel-headers dkms

```
查看GPU型号
```
# lspci -nn | grep VGA

```
类似输出
```
02:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP102 [GeForce GTX 1080 Ti] [10de:1b06] (rev a1)
03:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP102 [GeForce GTX 1080 Ti] [10de:1b06] (rev a1)
06:00.0 VGA compatible controller [0300]: ASPEED Technology, Inc. ASPEED Graphics Family [1a03:2000] (rev 30)
82:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP102 [GeForce GTX 1080 Ti] [10de:1b06] (rev a1)
83:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP102 [GeForce GTX 1080 Ti] [10de:1b06] (rev a1)	

```

## 屏蔽Nouveau驱动
在/etc/modprobe.d/建立blacklist.con
```
blacklist nouveau

```
创建“initramfs”文件
```
# mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak  
# dracut -v /boot/initramfs-$(uname -r).img $(uname -r)

```
重启reboot
## 安装Nvidia驱动
更改系统运行级别 init 3

执行下面的命令静默安装Nvidia驱动
```
sh NVIDIA-Linux-x86_64-384.98.run --ui=none --no-questions --accept-license --no-opengl-files

```
重启reboot
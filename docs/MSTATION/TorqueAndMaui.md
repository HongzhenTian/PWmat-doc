# 作业调度系统TORQUE和MAUI配置（单机版）

## 简介
Torque是批处理作业管理器，Maui是作业调度配置器，一般配合Torque优化任务调度。

## Torque编译安装
解压缩  `tar -zxvf torque-5.1.2-1448394813_f498aba.tar.gz`

进入目录 `cd torque-5.1.2-1448394813_f498aba`

配置 `./configure --prefix=/opt/torque/5.1.2/ --with-server-home=/opt/torque/5.1.2_spool/`

编译 `make`

安装 `make install`

配置环境变量，`vim /etc/profile.d/torque.sh`，编辑内容如下：
```
TORQUEROOT=/opt/torque/5.1.2
export PATH=$TORQUEROOT/bin:$TORQUEROOT/sbin:$PATH
export LD_LIBRARY_PATH=$TORQUEROOT/lib:$LD_LIBRARY_PATH
```
为了保证接下来的配置顺利进行，执行`source /etc/profile.d/torque.sh`


## Torque设置服务端
在解压缩的目录torque-5.1.2-1448394813_f498aba下

复制服务脚本
```
cp contib/init.d/trqauthd /etc/init.d/
cp contib/init.d/pbs_server /etc/init.d/
cp contib/init.d/pbs_mom /etc/init.d/

```
增加系统服务
```
chkconfig --add trqauthd
chkconfig --add pbs_server
chkconfig --add pbs_mom
```
设置开机启动
```
chkconfig trqauthd on
chkconfig pbs_server on
chkconfig pbs_mom on

```
启动服务 `service trqauthd start`

初始化数据库 `./torque.setup root`

编辑节点文件 `vim /opt/torque/5.1.2_spool/server_priv/nodes`
```
#server name, number of processes
mstation np=4	

```
启动服务 `service pbs_server restart`

创建队列
```
queue_name='test'
qmgr -c "create queue $queue_name"
qmgr -c "set queue $queue_name queue_type=execution"
qmgr -c "set queue $queue_name enabled=true"
qmgr -c "set queue $queue_name started=true"
qmgr -c "set queue $queue_name max_user_queuable=20"
qmgr -c "set queue $queue_name max_user_run=4"
qmgr -c "set queue $queue_name resources_default.walltime=10:00:00"
qmgr -c "set queue $queue_name resources_max.walltime=3600:00:00"
qmgr -c "set queue $queue_name priority=255"
qmgr -c "set server default_queue=$queue_name"
qmgr -c "set server scheduling=true"
qmgr -c "set server allow_node_submit=true"
qmgr -c "set server query_other_jobs=true"
qmgr -c "set server keep_completed=3600"	

```
编辑文件 `vim /opt/torque/5.1.2_spool/mom_priv/config`
```
$pbsserver mstation
$logevent 255	

```
启动服务 `service pbs_mom restart`

## 安装配置MAUI
解压缩 `tar -zxvf maui-3.3.tar.gz`

进入目录 `cd maui-3.3`

配置 `./configure --prefix=/opt/maui/3.3 --with-spooldir=/opt/maui/3.3_spool LDFLAGS="-L/opt/torque/5.1.2/lib"`

编译 `make`

安装 `make install`

配置环境变量，`vim /etc/profile.d/maui.sh`，编辑内容如下：
```
MAUIROOT=/opt/maui/3.3
export PATH=${MAUIROOT}/bin:${MAUIROOT}/sbin:$PATH
export LD_LIBRARY_PATH=${MAUIROOT}/lib:$LD_LIBRARY_PATH

```
加载MAUI环境`source /etc/profile.d/maui.sh`

编辑启动文件 `vim /etc/init.d/maui`
```
#!/bin/sh
#
# maui  This script will start and stop the MAUI Scheduler
#
# chkconfig: 345 85 85
# description: maui
#
ulimit -n 32768
# Source the library functions
. /etc/rc.d/init.d/functions

MAUI_PREFIX=/opt/maui/3.3

# let see how we were called
case "$1" in
        start)
                echo -n "Starting MAUI Scheduler: "
                daemon --user root $MAUI_PREFIX/sbin/maui
                echo
                ;;
        stop)
                echo -n "Shutting down MAUI Scheduler: "
                killproc maui
                echo
                ;;
        status)
                status maui
                ;;
        restart)
                $0 stop
                $0 start
                ;;
        *)
                echo "Usage: maui {start|stop|restart|status}"
                exit 1
esac	

```
添加执行权限 `chmod +x /etc/init.d/maui`

增加系统服务 `chkconfig --add maui`

添加开机启动 `chkconfig maui on`

启动服务 `/etc/init.d/maui start`
## 安装视频
安装视频讲解：[torque-install.mp4](https://pan.baidu.com/s/12P3C2Q1TZncPERFsH8THLg#list/path=%2F)

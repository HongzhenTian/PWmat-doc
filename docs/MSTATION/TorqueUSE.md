# 作业调度系统torque使用
torque作业调度系统主要是方便将任务放到计算机后台执行、管理。下面介绍如何使用torque来提交作业。

这里我们以在MSTATION上面提交任务来说明，下面是单机MSTATION上一个标准的作业提交脚本内容。

```
#PBS -N 512GaAs-RELAX      # 作业名称，这里是一个512原子GaAs的弛豫计算
#PBS -l nodes=1:ppn=4      # 作业资源申请，这里是申请一个节点，4个GPU（因为PWmat是将一个cpu和一个GPU进行了绑定）
#PBS -q test               # 提交的队列是test，MSTATION默认的队列都是test
#PBS -l walltime=24:00:00  # 作业申请最大时间是24小时
cd $PBS_O_WORKDIR          # 切换到工作目录，也就是当前提交作业脚本的目录
mpirun -np 4 PWmat         # 运行PWmat

```
提交作业：
```
qsub job.pbs               # 提交作业命令，job.pbs是脚本名字，这个名字可以随意命名，按照LINUX命名规则即可
99.mstation                # 99.mstation是作业ID，作业ID也可以简写为99

```
查看作业：
```
qstat                      # 查看作业命令
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
99.mstation                512GaAs-RELAX    pwmat           00:04:13 R test # R表示运行，C表示结束

```
删除作业：
```
qdel 99

```
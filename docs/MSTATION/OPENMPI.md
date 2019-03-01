# OPENMPI安装
```
1. wget www.pwmat.com/pwmat-resource/course-download/MSTATION/install.openmpi-2.1.0.zip
 ```
 `(如果无法通过命令下载，可以点击此处[install.openmpi-2.1.0.zip](http://www.pwmat.com/pwmat-resource/course-download/PWmat/install.openmpi-2.1.0.zip)链接下载到本地再上传到服务器。)

```

                  2. unzip install.openmpi-2.1.0.zip
---------------------------------
Archive:  install.openmpi-2.1.0.zip
  inflating: install.openmpi-2.1.0.run

3. sh install.openmpi-2.1.0.run
---------------------------------
Verifying archive integrity... All good.
Uncompressing install openmpi  100%
>>> Please input the full path where to install OPENMPI 2.1.0
>>> The default directory is [/xxx/xxx/openmpi/2.1.0/intel-cuda]
>>> The PATH(Press Enter to accept the default):此处回车将安装到默认的位置
>>> icc ... OK!
>>> ifort ... OK!
>>> nvcc ... OK!
>>> Start installing openmpi 2.1.0
......
libtool: warning: relinking 'liboshmem.la'
>>> Do you wish to clean and update the .bashrc?[Y/y/N/n][default:N]Y输出Y将自动更新.bashrc中的环境变量
>>> FINISH INSTALLING Open MPI 2.1.0 IN DIRECTORY: /xxx/xxx/openmpi/2.1.0/intel-cuda

4. bash

5. mpirun --version
--------------------------------
mpirun (Open MPI) 2.1.0

Report bugs to http://www.open-mpi.org/community/help/
```

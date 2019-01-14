# 基本输入输出
[示例下载](http://39.98.50.106/pwmat-resource/course-download/PWmat/tutorial_basic-io.zip)

将文件夹解压得到以下几个文件：

``` 
{
    unzip tutorial_basic-io.zip
    -------------------------------
    Archive:  tutorial_basic-io.zip
    creating: tutorial_basic-io/
    inflating: tutorial_basic-io/atom.config
    inflating: tutorial_basic-io/ONCV.PWM.Ga.UPF
    inflating: tutorial_basic-io/ONCV.PWM.As.UPF
    inflating: tutorial_basic-io/etot.input
    inflating: tutorial_basic-io/job.pbs
}
```

## atom.config---结构文件

atom.config是PWmat需要的结构文件，其格式如下：

``` 
{
    2                                        ! 原子总数
    Lattice                                  ! 格矢标签，必须是LATTICE，大小写均可
    0.000000      2.826575      2.826575     ! 第一个格矢，单位angstrom
    2.826575      0.000000      2.826575     ! 第二个格矢
    2.826575      2.826575      0.000000     ! 第三个格矢
    Position                                 ! 原子位置标签，必须是POSITION，大小写均可
    31    0.00    0.00    0.00  1  1  1      ! 31号元素的相对原子位置0.00 0.00 0.00；1 1 1表示原子可以在x、y、z三个方向上移动
    33    0.25    0.25    0.25  1  1  1      ! 33号元素的相对原子位置0.25 0.25 0.25；1 1 1表示原子可以在x、y、z三个方向上移动
}
```

## etot.input---计算控制文件
```
{
    1 1                         ! 波函数并行设置、K点并行设置，二者之积必须等于使用的GPU总数
    job = scf                   ! 计算类型设置，job为关键词，大小写均可；scf表示自洽计算，大小写均可
    in.psp1 = ONCV.PWM.As.UPF   ! 第一个赝势文件设置，in.psp1为关键词，大小写均可；ONCV.PWM.As.UPF为赝势文件名，必须和实际文件名一致
    in.psp2 = ONCV.PWM.Ga.UPF   ! 第二个赝势文件设置，赝势文件不必和atom.config对应
    in.atom = atom.config       ! 结构文件设置，in.atom是关键词，大小写均可；atom.config是文件名，必须和实际文件名一致
    mp_n123 =  5 5 5 0 0 0      ! K点设置，采用Monk-horst方法，mp_n123是关键词，大小写均可
}
```

## ONCV.PWM.Ga.UPF---Ga的赝势文件

## ONCV.PWM.As.UPF---As的赝势文件
赝势文件部分内容如下：
```
...
element="As"                                ! 赝势元素名称
pseudo_type="NC"                            ! 赝势类型，NC表示Norm-Conserving，模守恒赝势 US表示Ultra-Soft，超软赝势
relativistic="scalar"
is_ultrasoft="F"
is_paw="F"
is_coulomb="F"
has_so="F"
has_wfc="F"
has_gipaw="F"
core_correction="T"                         
functional="PBE"                            ! 生成赝势文件的泛函
z_valence="    5.00"                        ! 赝势的价电子数
total_psenergy="  -1.24422152418E+01"
wfc_cutoff="45.00"                          ! 赝势默认的截断能
rho_cutoff="   1.35700000000E+01"          
l_max="2"
...
```

## job.pbs---提交作业的脚本文件
关于pbs作业调度使用可以参考[PBS使用](http://localhost:3000/#/MSTATION/TorqueUSE)，job.pbs内容如下：
```
#PBS -N tutorial_basic-io   # 作业名
#PBS -l nodes=1:ppn=1       # PWmat将CPU和GPU进行绑定，所以ppn=1表示一个GPU
#PBS -l walltime=10:00:00   # 作业运行最大时间10h0min0sec
#PBS -q test                # 提交作业的队列test
cd $PBS_O_WORKDIR           # 切换到当前目录
mpirun -np 1 PWmat          # 运行PWmat
```
提交作业
```
qsub job.pbs
-------------------
1.mstation
```

## 基本输出文件
计算结束之后得到如下几个文件：

文件名|内容
--|:--:
REPORT|主要输出文件
OUT.KPT|K点坐标文件
OUT.SYMM|对称性文件
OUT.RHO|电荷密度二进制文件
OUT.WG|波函数二进制文件
OUT.EIGEN|本征值二进制文件
OUT.OCC|电子占据数文件
OUT.FERMI|费米能文件
OUT.VR|电子势场文件



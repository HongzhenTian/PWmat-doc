# 磁性计算
[示例下载](http://www.pwmat.com/pwmat-resource/course-download/PWmat/magnetic.zip)
#### spin=2（线性磁矩计算）
进行一些计算的时候，选择合适的初始磁性，有助于加快收敛（弛豫、自洽计算）。下面我们通过一个简单的例子来介绍。
atom.config
``` 
{
  2
   LATTICE
        8.00000000     0.00000000     0.00000000
        0.00000000     8.00000000     0.00000000
        0.00000000     0.00000000     8.00000000
   POSITIONS
    26     0.37500000     0.00000000     0.00000000 1 1 1
    26     0.62500000     0.00000000     0.00000000 1 1 1
   MAGNETIC
    26  3
    26  3	
}
```
etot.input
```
{
   1  4
   job = scf
   in.atom = atom.config
   in.psp1 = Fe.SG15.PBE.UPF
   mp_n123 = 7 7 7 1 1 1
   spin = 2	
}
```
**_etot.input中设置SPIN=2_**

**_atom.config中增加MAGNETIC标签，大于0表示自旋向上，小于0表示自旋向下，0表示无自旋；可以设置为小数（分数要写成小数形式）_**

**_对于多K点的情形可以尝试1 4这种K点并行的方式加快计算_**

计算结束后在REPORT文件最后的部分，可以看到收敛的总磁矩，如果需要得到收敛时每个原子的磁矩，需要在etot.input中
设置“CHARGE_DECOMP=T”。结果输出至OUT.QDIV文件中，OUT.QDIV包含原子总电荷和原子的磁矩。
```
{
   spin_up;dn;loc_diff  =      19.1277718164       12.8722281837        6.4478088069
}
```
注：SPIN=2时，可以使用LDA，PBE，HSE等泛函进行计算。另外SPIN=2的时候如果想固定磁矩，可以在etot.input中设置CONSTRAINT_MAG参数，
该参数可以固定每个原子的磁矩为自己想要设置的磁矩，实际上是在能量项添加一项能量惩戒项（energy penalty term）。

#### spin=22（自旋轨道计算）
在etot.input中设置spin=22可以进行自旋轨道耦合计算，如果进行HSE+SOC的计算，需要在etot.input中设置XCFUNCTIONAL=HSE和SPIN=22，
另外需要用专门的SOC赝势。
#### spin=222（自旋轨道+非线性磁矩）
对于非线性的情况，例子如下：
atom.config
```
{
   2
    LATTICE
         8.00000000     0.00000000     0.00000000
         0.00000000     8.00000000     0.00000000
         0.00000000     0.00000000     8.00000000
    POSITIONS
     26     0.37500000     0.00000000     0.00000000 1 1 1
     26     0.62500000     0.00000000     0.00000000 1 1 1
    MAGNETIC_XYZ
     26  0 0 3
     26  0 0 3		
}
```
etot.input
```
{
   4 1
   job = scf
   precision = double
   convergence = difficult
   in.atom = atom.config
   in.psp1 = Fe.SG15.PBE.SOC.1.0.UPF
   mp_n123 = 1 1 1 1 1 1 2
   spin = 222
   ecut = 60
   ecut2 = 240
   ecut2l = 240
   SCF_ITER0_1 =   10   4    3    0.0000     0.02500    1
   SCF_ITER0_2 =  190   4    3    1.0000     0.02500    1
   WG_ERROR  =   1.0E-5
   E_ERROR   =   0.0
   RHO_ERROR =   1.0E-6		
}
```
**_etot.input中设置SPIN=222_**

**_atom.config中增加MAGNETIC_**\_**_XYZ标签，大于0表示沿着x或者y或者z轴的正方向，小于0表示沿着x或者y或者z轴的负方向，0表示无自旋；可以设置为小数（分数要写成小数形式）；需要设置x、y、z三个方向的磁矩_**

**_在etot.input中设置MP_**\_**_N123 = 1 1 1 1 1 1 2，最后一个2是为了去掉对称性，这点很重要，否则会计算出错_**

**_在etot.input中设置IN.PSP1 = Fe.SG15.PBE.SOC.1.0.UPF，非线性计算需要使用SOC特殊的赝势_**

**_在etot.input中设置高精度保证收敛_**

**_在etot.input中设置"CHARGE_**\_**_DECOMP=T"与"SOM_**\_**_SPHERE_**\_**_RCUT"值，有的时候"SOM_**\_**_SPHERE_**\_**_RCUT"的默认值太大，可能导致OUT.ATOMSPIN输出存在问题。_**

在计算结束后输出收敛的总磁矩
```
{
    spin_xyz             = 0.41738784887748E+00  0.16722422891261E-01  0.64483160018184E+01		
}
```
另外，OUT.ATOMSPIN包含原子的各个方向上的磁矩，原子总的电荷数。


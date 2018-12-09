# IN.OCC文件设置
在计算中如果需要固定电子的占据，可以通过IN.OCC文件来进行设置。具体使用如下：
``` 
{
  SCF_ITER0_1 = NITER0_1, NLINE0, imth, icmix, dE, Fermi-Dirac
}
```
当Fermi-Dirac=0的时候会从指定的文件IN.OCC读取电子初始的占据态。IN.OCC的格式如下，
``` 
{
  OCC1k1 OCC2k1 OCC3k1 ... OCCMk1 第一个K点占据态
  OCC1k2 OCC2k2 OCC3k2 ... OCCMk2 第二个K点占据态
  ...
}
```
OCC1 OCC2 OCC3 ... OCCM表示电子占据数，0表示未占据，1表示全占据，部分占据0~1。注意OCC的M数要和NUM__BAND设置一致，K点数要和MP_N123产生的K点数一致（具体可以参考OUT.KPT）。

下面给出一个参考例子
etot.input中设置如下：
```
{
   1 1
   job = scf
   in.psp1 = O.SG15.PBE.UPF
   in.atom = atom.config1
   mp_n123 = 2 2 2 0 0 0
   NUM_BAND = 13
   SCF_ITER0_1 =    6   4    3    0.0000     0.02500    0
   SCF_ITER0_2 =   94   4    3    1.0000     0.02500    0
}
```
atom1.config文件如下：
```
{
      1
      LATTICE
            8.00000000     0.00000000     0.00000000
            0.00000000     8.00000000     0.00000000
            0.00000000     0.00000000     8.00000000
      POSITION
         8     0.00000000     0.00000000     0.00000000 1 1 1
}
```
IN.OCC(对于重复的OCC可以使用N*OCC的方式输入)
```
{
     0.25 3*0.13 9*0.0
     0.75 0.5 2*0.448 9*0.0
     0.75 0.55 2*0.54 9*0.0
     0.25 3*0.185 9*0.0
}
```

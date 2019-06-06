<font size=5><center>**自洽计算收敛性介绍**</center></font>
##### 查看能量收敛情况

<font size=3>无论是自洽SCF计算还是其他计算结束之后通过下面的命令查看REPORT中总能收敛情况，当然我只推荐在SCF计算的时候用该命令查看，对于优化计算或者分子动力学模拟或者TDDFT计算我们推荐
查看RELAXSTEPS文件或者MDSTEPS文件，查看总能的收敛情况：</font>
```
{
   grep "E_tot" REPORT
   --------------------------
    E_tot(eV)            = -.41268237180024E+03    -.4127E+03
    ...
    ...
    E_tot(eV)            = -.41266814301481E+03    -.2844E-04
    E_tot(eV)            = -.41266813461830E+03    0.8397E-05
    E_tot(eV)    = -.41266813461830E+03    0.8397E-05
    E_tot(Ryd)   = -.30330548713321E+02    0.3086E-06 
}
```
<font size=3>下面我们介绍一些跟自洽收敛相关的参数以及这些参数的设置方法：</font>
##### ①调节全局精度和收敛参数ACCURACY、CONVERGENCE
自洽计算有两个全局收敛参数ACCURACY和CONVERGENCE：
```
{
   ACCURACY = "NORM"（默认值），"HIGH"
   当ACCURACY=NORM，ECUT将会被设置为从赝势读取的默认值，ECUT2=2*ECUT,ECUT2L=ECUT2。当JOB=RELAX的时候，ECUT2=4*ECUT；
   当ACCURACY=HIGH，ECUT将被设置为1.2倍的默认值，ECUT2=4*ECUT,ECUT2L=4*ECUT2，P123=N123
   CONVERGENCE = "EASY"（默认值），"DIFFICULT"
   当CONVERGENCE设置为DIFFICULT的时候，E_ERROR，RHO_ERROR，WG_ERROR都会设置较高的精度。
}
```
##### ②调节截断能ECUT、ECUT2、ECUT2L
自洽计算收敛控制的一个重要参数是截断能ECUT，对于弛豫计算，适当增大ECUT、ECUT2、ECUT2L有助于弛豫的收敛性。对于一些比较难于收敛的体系，
推荐ECUT2=4\*ECUT，ECUT2L=ECUT2，有时候甚至可以设置 ECUT2=4\*ECUT，ECUT2L=4\*ECUT2：
```
{
   1 1
   job = scf
   in.atom = atom.config
   in.psp1 = ONCV.PWM.O.UPF
   mp_n123 = 6 6 6 0 0 0
   ECUT = 60
}
```
##### ③调节收敛精度RHO_ERROR E_ERROR WG_ERROR
自洽计算收敛精度由三个参数共同控制：RHO_ERROR（rho的收敛精度）, E_ERROR（能量的收敛精度）, WG_ERROR（波函数的收敛精度），三者任一达到即停止计算。
如果仅用一个参数控制计算终止，可以将另外两个参数设置为0。例如仅用E_ERROR控制计算的收敛：
```
{
   1 1
   job = scf
   in.atom = atom.config
   in.psp1 = ONCV.PWM.O.UPF
   mp_n123 = 6 6 6 0 0 0
   wg_error = 0
   rho_error = 0
   e_error = 1.0E-6	<--能量收敛精度	
}
```
提交任务，计算结束查看REPORT文件，在最后计算得到的总能部分有这样一行说明文字：
```
{
   ending_scf_reason = tol Etot_err  1.000000000000000E-006	<--SCF计算结束的原因是：达到了Etot_err达到了1.0E-6		
}
```
##### ④调节自洽计算的步数
自洽计算的步数主要由参数SCF_ITER0_XX控制，例如本例中的参数设置如下（可以在REPORT文件开头找到预设）
```
{
   SCF_ITER0_1 =    6   4    3    0.0000     0.02500    2 <--0.0表示进行非自洽计算
   SCF_ITER0_2 =   94   4    3    1.0000     0.02500    2 <--1.0表示进行自洽计算			束的原因是：达到了Etot_err达到了1.0E-6		
}
```
在自洽计算之前，会进行几步NONSCF计算来优化波函数，从而保证后面的SCF计算更快收敛。上面的的红色部分，已经对NONSCF 和SCF进行了标识。6和94都是最大步数。如果需要更多的迭代步数，可以修改这两个参数。
##### ⑤调节occupation占据展宽
自洽计算的occupation展宽主要有SCF_ITER0_XX中的dE和Guassian-Dirac控制，例如本例中的参数设 置如下（可以在REPORT文件开头找到预设）
```
{
    SCF_ITER0_1 =    6   4    3    0.0000     0.02500    2 <-- 0.025表示dE，2表示Guassian
    SCF_ITER0_2 =   94   4    3    1.0000     0.02500    2 			
}
```
在自洽计算中，dE和Gaussian-Dirac参数控制着每个轨道的占据。在费米能附近，半导体和绝缘体的占据分布要么是1，要么 是0，dE数值的变化影响较弱，默认值即可；但是对于金属体系，dE的作用非常明显，为了加快收敛，这里推荐设置0.1～0.2，同时 Guaasian-Dirac设置2、3、4

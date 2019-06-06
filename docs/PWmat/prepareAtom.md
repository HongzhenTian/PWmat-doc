<font size=5><center>**准备结构文件<font size=6>atom.config</font>**</center></font>

<font size=4>**<font size=5>atom.config</font>文件简介**</font>

atom.config包含了晶格格矢、原子位置以及初始的速度、受力、磁矩、固定方法晶格优化参数用于其他功能计算，具体形式可以参考“[基本输入和基本输出](https://hongzhentian.github.io/PWmat-doc/#/PWmat/basicInputAndOutput)”教程。通常的atom.config格式如下：
```
2                                      # 原子数目
Lattice                                # 晶格格矢标签
0.000000      2.826575      2.826575   # 第一个格矢
2.826575     -0.000000      2.826575   # 第二个格矢
2.826575      2.826575      0.000000   # 第三个格矢
Position                               # 原子位置标签
31  0.000    0.000    0.000  0  0  0   # 31：原子序数，0.0 0.0 0.0：原子分数坐标 0 0 0：原子是否沿着x、y、z方向移动
33  0.250    0.250    0.250  0  0  0
Magnetic                               # 初始磁矩（可选）
... ...
Velectiry                              # 初始速度（可选）
... ...
```
除了手动输入这些参数之外，我们还提供了一些格式转换小程序，用于将其他程序的晶格文件转换为atom.config，下面将分别介绍：

### ①poscar2config.x
poscar2config.x用于将VASP的POSCAR文件转换为atom.config文件，特别注意目前只支持分数格式的POSCAR转化。用法：
```
poscar2config.x < POSCAR
```

### ②cell2config.x
cell2config.x用于将castep的.cell文件转换为atom.config文件。用法：
```
cell2config.x < atom.cell
```

### ③pwscf2config.x
poscar2config.x用于将Quantum Espresso(PW)的input文件转换为atom.config文件。用法：
```
pwscf2config.x < atom.pwscf.in	
```

### ④xsf2config.x
poscar2config.x用于将Xcrysden的xsf周期性结构文件转换为atom.config文件。用法：
```
xsf2config.x < atom.xsf	
```


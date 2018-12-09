# 准备atom.config

## atom.config文件简介
[示例下载](http://www.pwmat.com/tutorial/tutorial_prepare_config.zip)

atom.config包含了晶格格矢、原子位置以及初始的速度、受力、磁矩、压力用于其他功能计算。通常的atom.config格式如下：
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
除了手动输入这些参数之外，我们还提供了一些格式转换小程序，用于将其他程序的晶格文件转换为atom.config。

## poscar2config.x
poscar2config.x用于将VASP的POSCAR文件转换为atom.config文件。用法：
```
poscar2config.x < POSCAR
```

## cell2config.x
cell2config.x用于将castep的.cell文件转换为atom.config文件。用法：
```
cell2config.x < atom.cell
```

## pwscf2config.x
poscar2config.x用于将Quantum Espresso(PW)的input文件转换为atom.config文件。用法：
```
pwscf2config.x < atom.pwscf.in	
```

## xsf2config.x
poscar2config.x用于将Xcrysden的xsf周期性结构文件转换为atom.config文件。用法：
```
xsf2config.x < atom.xsf	
```

#### [视频解说](http://www.pwmat.com/video/convert_to_config.mp4)
# 高对称点路径产生
本教程通过[sumo](http://www.pwmat.com/tarble/sumo.zip)产生高对称点路径，具体安装方法以及使用教程如下：

#### 安装
通过anaconda2安装sumo:
```
{
    pip install sumo
}
```
下载网页中的附件sumo.zip，将sumo.zip上传到服务器账号的根目录（例如，用户是XXX，将sumo.zip上传到/home/XXX/下即可）,解压sumo.zip
```
{
    unzip sumo.zip
}
```
#### 使用方法
```
{
    python /home/XXX/sumo/cli/kgen.py -c atom.config
}
```
产生的K点文件即保存在IN.KPT中。
#### 屏幕输出
```
{
    /home/XXX/anaconda2/lib/python2.7/site-packages/pymatgen/__init__.py:87:UserWarning:
    Pymatgen will drop Py2k support from v2019.1.1. Pls consult the documentation
    at https://www.pymatgen.org for more details.
    at https://www.pymatgen.org for more details.""")
    OUTPUT:POSCAR.pwmat
    Structure information:
        Space group number: 186
        International symbol: P6_3mc
        Lattice type: hexagonal
    k-point path:
        \Gamma -> A -> L -> M ->\Gamma -> K -> H -> A

    k-points:
        A: 0.0 0.0 0.5
        \Gamma: 0.0 0.0 0.0
        H:-0.333 0.667 0.5
        K:-0.333 0.667 0.0
        M: 0.0 0.5 0.0
        L: 0.0 0.5 0.5
        ...
        The Monkhorst Pack Grids:  11 11  6

}
```
#### 其他一些详细设置
设置寻找对称性的精度，比如下面的0.01
```
   {
       python /home/XXX/sumo/cli/kgen.py -c atom.config --symprec 0.01
   }
```
设置寻找对称性的精度，比如下面的0.01
```
{
    python /home/XXX/sumo/cli/kgen.py -c atom.config --density 60
}
```   
自定义高对称K点0 0 0-> 0.5 0 0
```
{
    python /home/XXX/sumo/cli/kgen.py -c atom.config --kpoints 0 0 0, 0.5 0 00
}
```
**_以上参数可以混合在一起使用_**
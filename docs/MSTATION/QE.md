# QE安装

## 加载intel编译环境
```
source /opt/intel/impi/4.1.1.036/bin64/mpivars.sh
source /opt/intel/composer_xe_2013_sp1.0.080/bin/compilervars.sh intel64
source /opt/intel/composer_xe_2013_sp1.0.080/mkl/bin/mklvars.sh intel64	

```

## 生成配置文件
解压进入根目录
```
./configure --prefix=/where/install/q-e MPIF90=mpiifort F90=mpiifort F77=mpiifort --enable-openmp --with-scalapack=intel	
```
之后会生成make.inc，如果需要增加其他的库，可以手动修改此文件。（注意，再6.0.0之前，该文件名是make.sys）

## 编译
```
make pw

```
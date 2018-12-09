# ONCVPSP安装

## 下载最新版ONCVPSP
```
http://www.mat-simresearch.com/		

```

## 解压
```
tar -zxvf oncvpsp-3.3.1.tar.gz

```

## 在ONCVPSP根目录创建make.inc

```
# System-dependent makefile options for ONCVPSP
# This must be carefully edited before executing "make" in src
#
# Copyright (c) 1989-2015 by D. R. Hamann, Mat-Sim Research LLC and Rutgers
# University

##### Edit the following lines to correspond to your compilers ####

F77        = ifort
F90        = ifort
CC         = icc
FCCPP      = fpp

FLINKER     = $(F90)

FCCPPFLAGS = -ansi -DLIBXC_VERSION=203  #This probably should not be changed

##### Edit the following optimization flags for your system ####

FFLAGS     = -c -O2 -funroll-loops
CFLAGS     = -O2
##### Edit the following LAPACK and BLAS library paths for your system ####

#LIBS =   -lmkl_intel_lp64  -lmkl_sequential -lmkl_core
LIBS =  -mkl=parallel

##### Edit the following for to use libxc if available #####

OBJS_LIBXC = exc_libxc_stub.o

# oncvpsp is compatible with libxc
# To build oncvpsp with libxc, uncomment 3 of the following lines and edit
# the paths to point to your libxc library and include directories
# make clean in src before rebuilding after changing this

##for libxc 2.1.0 and later use
#LIBS += -L/home/drh/abinit/fallbacks/exports/lib -lxcf90 -lxc

##for earlier releases use
LIBS += -L/opt/libxc/2.0.1/intel/lib -lxc
FFLAGS += -I/opt/libxc/2.0.1/intel/include
OBJS_LIBXC =|   functionals.o exc_libxc.o

```

## 编译

```
make	

```

 在src下面就会生成需要的赝势产生程序，具体使用可以参考doc文件夹

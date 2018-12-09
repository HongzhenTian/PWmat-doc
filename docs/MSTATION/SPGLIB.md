# SPGLIB安装

## 下载最新版SPGLIB
```
git clone https://github.com/atztogo/spglib.git	

```

## 默认位置安装
进入根目录（需要root权限）
```
mkdir _build
cd _build
cmake ..
make
make install (probably installed under /usr/local)	

```
## 自定义安装位置
进入根目录
```
mkdir _build
cd _build
cmake -DCMAKE_INSTALL_PREFIX="" ..
make
make DESTDIR=/where/to/install install	
```

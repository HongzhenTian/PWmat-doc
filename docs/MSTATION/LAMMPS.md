# LAMMPS安装

## 加载CUDA环境
```
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

```

## 加载intel编译环境
```
source /opt/intel/impi/4.1.1.036/bin64/mpivars.sh
source /opt/intel/composer_xe_2013_sp1.0.080/bin/compilervars.sh intel64
source /opt/intel/composer_xe_2013_sp1.0.080/mkl/bin/mklvars.sh intel64	

```

## 编译LAMMPS的GPU库
解压lammps安装包，进入lib/gpu
```
make -f Makefile.linux	
```

## 复制编译文件
进入src/MAKE/OPTIONS，
```
cp Makefile.intel_cpu_intelmpi ../	

```

## 编译LAMMPS
进入src，
```
make yes-asphere yes-kspace yes-gpu
make yes-asphere yes-class2 yes-kspace yes-manybody yes-misc yes-molecule
make yes-mpiio yes-opt yes-replica yes-rigid
make yes-user-omp yes-user-intel
make intel_cpu_intelmpi(如果出现错误，再次执行该命令)

```

## 运行LAMMPS
```
mpirun -np NCPU where/is/lmp_intel_cpu_intelmpi -sf gpu -pk gpu NGPU -i file.in -o file.out

```
## 配置job.pbs
```
#PBS -N lammps      
#PBS -l nodes=1:ppn=NGPU
#PBS -q test               
#PBS -l walltime=24:00:00

export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
source /opt/intel/impi/4.1.1.036/bin64/mpivars.sh
source /opt/intel/composer_xe_2013_sp1.0.080/bin/compilervars.sh intel64
source /opt/intel/composer_xe_2013_sp1.0.080/mkl/bin/mklvars.sh intel64

cd $PBS_O_WORKDIR

mpirun -np NCPU where/is/lmp_intel_cpu_intelmpi -sf gpu -pk gpu NGPU -i file.in -o file.out

```
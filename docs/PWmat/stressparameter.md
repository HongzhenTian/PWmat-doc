# STRESS参数设置
如果需要对晶格进行特殊优化，比如沿着固定方向优化晶格或者在外界压力之下优化晶格，在PWmat中可以通过设置atom.config中的STRESS_MASK和STRESS_EXTERNAL参数来实现。
#### STRESS_MASK
对晶格沿着第一个格矢方向优化，可以在atom.config设置
``` 
{
  STRESS_MASK
  1 0 0
  0 0 0
  0 0 0
}
```

对晶格沿着沿着第二个格矢方向优化，可以在atom.config设置
   ``` 
   {
     STRESS_MASK
     0 0 0
     0 1 0
     0 0 0
   }
   ```
 对晶格沿着沿着第三个格矢方向优化，可以在atom.config设置
 ``` 
 {
   STRESS_MASK
   0 0 0
   0 0 0
   0 0 1
 }
 ```
#### STRESS_EXTERNAL
```
{
   STRESS_EXTERNAL
   P1 0 0
   0 P2 0
   0 0 P3 
}
```

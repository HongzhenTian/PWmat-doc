# HSE计算
当XCFUNCTIONAL = HSE之后，交换关联泛函就设置为HSE06了。对于HSE计算，还有其他一些参数控制影响HSE计算的结果。下面对这些参数进行介绍
#### HSE_OMEGA
``` 
{
  screening parameter
}
```
#### HSE_ALPHA
```
{
   mixing parameter of the explicit Fock exchange
}
```
#### HSEMASK_PSP
```
{
   the parameter can provide an element specified HSE mixing parameter HSE_ALPHA，这个参数可以对不同的元素设置不同混合参数。
}
```
#### P123
```
{
   a small box FFT can be used to calculate the explicit FOCK exchange integral. 能够加速HSE计算。
}
```
#### HSE_DETAIL 控制HSE计算的一些细节
```
{
   HSE_MIX
   describe the Fock exchange kernel mixing parameter, recommend 1。
   MAX_SXP 
   maximum number of Fock exchange kernel mixing terms, default 1. One can set it 2 or 3, speeding up but more memorgy。
   TOLHSE_MIX 
   tolerance for the Fock exchange term mixing for HSE SCF iteration to stop. 影响HSE SCF收敛的条件，可以通过"grep update_sxp REPORT"来查看。
   HSE_DN 
   the number of SCF steps for each Fock exchange kernel update. HSE自洽计算之间优化charge的步数（默认6，推荐3~10）。对于很难收敛的体系，可以适当增加这个值，这样也会导致HSE计算很慢。
   HSE_PBE_SCF
   是否在初始计算HSE之前进行PBE计算优化charge和wavefunction。		
}
```
#### RELAX_HSE 对于进行HSE RELAX有作用
```
{
    NUM_LDA 
    the maximum number of relaxation steps for LDA/PBE preconditioner run。HSE优化之前的预优化。
    FACT_HSELDA 
    the prefactor to stop LDA/PBE relaxation。控制预优化的终止。
    LDA_PBE
    预优化的交换关联泛函形式1-LDA 2-PBE。		
}
```

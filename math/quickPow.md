# 快速幂

## 递推写法(二进制加速)

参数：x，n

返回：$$x^n$$

时间复杂度：$$O(\log n)$$

备注：将其中两行替换成注释并添加mod全局变量或参数，即为取模快速幂，返回$$x^n \mod mod$$

```c++
int inline fpow(int x,int n){
    int re=1;
    while(n){
        if(n&1)
            re*=x;//re=(x*re)%mod;
        x*=x;//x=(x*x)%mod;
        n>>=1;
    }
    return re;
}
```

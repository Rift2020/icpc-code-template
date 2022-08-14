# 卢卡斯定理

用于求解**特大组合数取模**的值

## 标准卢卡斯

若$$p$$是质数，则有

$$\binom{n}{m}\bmod p = \binom{\left\lfloor n/p \right\rfloor}{\left\lfloor m/p\right\rfloor}\cdot\binom{n\bmod p}{m\bmod p}\bmod p$$

------

代码中mod即公式里的p

输入：为`mod`赋值，预处理`init_fact(mod)`，单次调用`lucas(n,m)`

返回：`lucas(n,m)`返回$$\binom{n}{m}\bmod mod$$的值

时间复杂度：预处理$$O(p)$$，单次$$O(\log^2 n)$$

溢出警告：`init_fact`，`C`，`lucas`中的乘法部分最大都会达到$$p^2$$的数值，可以使用快速乘防止溢出

```c++
/**
 *    author:  Rift
 *    created: 2022.08.14  12:00
**/
ll fact[maxn],mod;
void init_fact(ll n){
    fact[0]=1;
    for(int i=1;i<=n;++i)
        fact[i]=fact[i-1]*i%mod;
}
ll exgcd(ll a,ll b,ll &x,ll &y){
    if(b==0){
        x=1;
        y=0;
        return a;
    }
    ll g=exgcd(b,a%b,x,y);
    ll t=x;
    x=y;
    y=t-a/b*y;
    return g;
}
ll C(ll n,ll m){
    if(m>n)return 0;
    ll fz=fact[n];
    ll fm=fact[m]*fact[n-m]%mod;
    ll fm_inv,y;
    exgcd(fm,mod,fm_inv,y);
    fm_inv=(fm_inv+mod)%mod;
    return fz*fm_inv%mod;
}
ll lucas(ll n,ll m){
	if(m==0)return 1;
	if(n<m)return 0;
	return lucas(n/mod,m/mod)*C(n%mod,m%mod)%mod;
}
```

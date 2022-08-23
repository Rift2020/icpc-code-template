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

## ex卢卡斯

求解$$\binom{n}{m}\bmod p$$，且$$p$$是合数的场合

### 求单次

时间复杂度(最坏)(存疑)(只多不少)：$$O(p+\log n\log p+\log^2_p)$$

define：设置`maxmod`为$$p$$的最大值

参数：调用`exlucas(n,m,p)`

返回：$$\binom{n}{m}\bmod p$$的值

溢出分析：(因为时间复杂度限制$$p$$的大小)一般不会溢出

```c++
/**
 *    author:  Rift
 *    created: 2022.08.22  14:54
**/
#define maxmod 1000005
ll fac[maxmod];

ll inline fpow(ll x,ll n);//快速幂
ll inline fpow(ll x,ll n,ll mod);//取模快速幂
ll mod_inverse(ll a,ll n);//无所谓exgcd或是快速幂实现
ll CRT(ll a[],ll n[],ll k);//标准CRT和exCRT都能胜任

ll inline vn(ll n,ll p){
    ll re=0;
	for(ll pp=p;pp<=n;pp*=p)
		re+=n/pp;
	return re;
}
void pre_cal(ll p,ll pk){
	fac[0]=fac[1]=1;
	for(int i=2;i<=pk;++i){
		if(i%p==0)fac[i]=fac[i-1];
		else fac[i]=fac[i-1]*i%pk;
	}
}
ll cal(ll n,ll p,ll pk){
	if(n==0)return 1;
	ll a;
	if(n>=pk)
		a=fac[pk];
	else
		a=1;
	a=fpow(a,n/pk,pk);
	a=a*fac[n%pk]%pk;
	return cal(n/p,p,pk)*a%pk;
}
ll exlucas(ll n,ll m,ll mod){
	ll p[30],k[30],cnt=0;
	for(ll i=2;i*i<=mod;++i){
		if(mod%i==0){
			p[++cnt]=i;
			k[cnt]=1;
            mod/=i;
			while(mod%i==0){
				mod/=i;
				k[cnt]++;
			}
		}
	}
	if(mod>1){
		p[++cnt]=mod;
		k[cnt]=1;
	}
    ll crt_a[cnt+1],crt_n[cnt+1];
	for(int i=1;i<=cnt;++i){
		ll pk=fpow(p[i],k[i]);
		pre_cal(p[i],pk);
		ll fz=cal(n,p[i],pk);
		ll fm=cal(m,p[i],pk)*cal(n-m,p[i],pk)%pk;
		ll fm_inv=mod_inverse(fm,pk);
		crt_a[i]=fz*fm_inv%pk*fpow(p[i],vn(n,p[i])-vn(m,p[i])-vn(n-m,p[i]),pk)%pk;
		crt_n[i]=pk;
	}
    ll re=CRT(crt_a,crt_n,cnt);
	return re;
}
```

### p固定，求多次

1.假设$$p$$的质因数分解为$$p=q_1^{k_1}q_2^{k_2}...q_{cnt}^{k_{cnt}}$$

时间复杂度(存疑)(只多不少)：

预处理：$$O(\sqrt p+\sum q_i^{k_i})$$，(如果你处理出p的质因数分解直接写代码里，可以去掉$$\sqrt p$$)

单次：$$O(cnt\log n+\sum\log q_i^{k_i})$$(一般来说cnt都相当小)

2.假如你不知道$$p$$的质因数分解：

时间复杂度(最坏)(存疑)(只多不少)：

预处理：$$O(p)$$

单次：$$O(\log p\log n+\log^2 p)$$

------

define：设置`maxfact`为$$q_i^{k_i}$$的最大值

参数：预处理`pre_exlucas(p)`，每次查询`exlucas(n,m)`

返回：$$\binom{n}{m}\bmod p$$的值

溢出分析：计算过程最大可能达到$$(\max \{q_i^{k_i}\})^2$$

```c++
/**
 *    author:  Rift
 *    created: 2022.08.23  12:02
**/
ll *fac;
ll fact[30][maxfact];

ll inline fpow(ll x,ll n);//快速幂
ll inline fpow(ll x,ll n,ll mod);//取模快速幂
ll mod_inverse(ll a,ll n);//无所谓exgcd或是快速幂实现
ll CRT(ll a[],ll n[],ll k);//标准CRT和exCRT都能胜任

ll inline vn(ll n,ll p){
    ll re=0;
	for(ll pp=p;pp<=n;pp*=p)
		re+=n/pp;
	return re;
}
void pre_cal(ll *fac,ll p,ll pk){
	fac[0]=fac[1]=1;
	for(int i=2;i<=pk;++i){
		if(i%p==0)fac[i]=fac[i-1];
		else fac[i]=fac[i-1]*i%pk;
	}
}
ll cal(ll n,ll p,ll pk){
	if(n==0)return 1;
	ll a;
	if(n>=pk)
		a=fac[pk];
	else
		a=1;
	a=fpow(a,n/pk,pk);
	a=a*fac[n%pk]%pk;
	return cal(n/p,p,pk)*a%pk;
}
ll p[30],k[30],cnt=0;
ll crt_a[30],crt_n[30];
void pre_exlucas(ll mod){
	for(ll i=2;i*i<=mod;++i){
		if(mod%i==0){
			p[++cnt]=i;
			k[cnt]=1;
            mod/=i;
			while(mod%i==0){
				mod/=i;
				k[cnt]++;
			}
		}
	}
	if(mod>1){
		p[++cnt]=mod;
		k[cnt]=1;
	}
	for(int i=1;i<=cnt;++i){
		crt_n[i]=fpow(p[i],k[i]);
		pre_cal(fact[i],p[i],crt_n[i]);
	}
}
ll exlucas(ll n,ll m){
	for(int i=1;i<=cnt;++i){
		ll pk=crt_n[i];
		fac=fact[i];
		ll fz=cal(n,p[i],pk);
		ll fm=cal(m,p[i],pk)*cal(n-m,p[i],pk)%pk;
		ll fm_inv=mod_inverse(fm,pk);
		crt_a[i]=fz*fm_inv%pk*fpow(p[i],vn(n,p[i])-vn(m,p[i])-vn(n-m,p[i]),pk)%pk;
	}
    ll re=CRT(crt_a,crt_n,cnt);
	return re;
}
```


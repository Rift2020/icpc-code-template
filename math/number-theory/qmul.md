# 快速乘

某些模意义下的乘法可能会爆数据范围

比如说$$(2e^{16}*3e^{17})\mod 5e^{17}$$的结果并不会超出long long，但中间乘法运算的时候long long爆了

于是诞生了一种方法来阻止中间乘法运算的时候爆数据范围

## 法一 __int128

输入：$$a,b,mod$$

返回：$$(a\times b)\mod mod$$

复杂度：$$O(1)$$

```c++
/**
 *    author:  Rift
 *    created: 2022.08.13  12:06
**/
ll mul(ll a,ll b,ll mod){
	return (ll)((__int128)a*b%mod);
}
```

## 法二 经典快速乘

如果有些连`__int128`也吃不下了，可以用此法，并把代码中`ll`改为`__int128`

原理：$$a\times b=a\times(b_kb_{k-1...}b_1)_2=a\times b_k\times 2^{k-1}+a\times b_{k-1}\times 2^{k-2}+...+a\times b_1\times 2^0$$

输入：$$a,b,mod$$

返回：$$(a\times b)\mod mod$$

复杂度：$$O(\log b)$$

```c++
ll mul(ll a,ll b,ll mod){
	if(b<0){
		a=-a;
		b=-b;//由于对负数进行右移运算是不确定的(依赖平台实现)，所以将b转化为正数
	}
	a%=mod;
	b%=mod;
	ll ans=0;
	while(b){
		if(b&1)
			ans=(ans+a)%mod;
		a=(a<<1)%mod;
		b>>=1;
	}
	return ans;
}
```


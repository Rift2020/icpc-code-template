# 分解质因数

题外话：假如你只是想知道一个数的质因数分解，Linux下可以直接命令行`factor <number>`

## 朴素写法

**这不是最优方法**

时间复杂度：$$O(\sqrt n)$$

参数：$$n$$

结果：p[1~cnt]中存有不同的质因数，k[1~cnt]中存有对应质因数的幂，即$$n=p_1^{k_1}p_2^{k_2}...p_{cnt}^{k_{cnt}}$$

```c++
/**
 *    author:  Rift
 *    created: 2022.08.23  12:11
**/
ll p[30],k[30],cnt=0;
void factor(ll n){
	for(ll i=2;i*i<=n;++i){
		if(n%i==0){
			p[++cnt]=i;
			k[cnt]=1;
			n/=i;
			while(n%i==0){
				n/=i;
				k[cnt]++;
			}
		}
	}
	if(n>1){
		p[++cnt]=n;
		k[cnt]=1;
	}
}
```


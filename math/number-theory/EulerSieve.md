# 欧拉筛

筛出1~n的所有素数

全局变量：prime存放1~n的素数、notprime[i]为1表示i不是素数，为0则是素数

参数：n

时间复杂度：$$O(n)$$

```C++
/**
 *    author:  Rift
 *    created: 2022.07.25  14:20
**/
vector<int> prime;
bool notPrime[maxm];
void EulerSieve(int n){
	notPrime[1]=true;
	for(int i=2;i<=n;++i){
		if(!notPrime[i])
			prime.push_back(i);
		for(const int &p:prime){
			if(p*i>n)break;
			notPrime[p*i]=true;
			if(i%p==0)break;
		}
	}
}
```

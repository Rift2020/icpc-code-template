# 欧拉函数

## 求单个欧拉函数值

参数：x

返回：$$\phi(x)$$

时间复杂度：$$O(\sqrt n)$$
```C++
/**
 *    author:  Rift
 *    created: 2022.07.30  17:47
**/
int Euler(int x){
	int ans=x;
	int sqrtx=sqrt(x+0.5);
	for(int i=2;i<=sqrtx;++i){
		if(x%i==0)
			ans=ans/i*(i-1);
		while(x%i==0)x/=i;
	}
	if(x!=1)ans=ans/x*(x-1);
	return ans;
}
```

## 欧拉筛求1~n的欧拉函数值

结果保存在euler数组内,会同时筛出1~n的素数

参数：n

时间复杂度：$$O(n)$$

```
/**
 *    author:  Rift
 *    created: 2022.08.01  00:05
**/
vector<int> prime;
bool notprime[maxn];
int euler[maxn];
void EulerSieve_euler(int n){
	notprime[1]=true;
	euler[1]=1;
	for(int i=2;i<=n;++i){
		if(notprime[i]==false){
			prime.push_back(i);
			euler[i]=i-1;
		}
		for(int p:prime){
			if(i*p>n)break;
			notprime[i*p]=true;
			if(i%p==0){
				euler[i*p]=euler[i]*p;
				break;
			}
			else
				euler[i*p]=euler[i]*(p-1);
		}
	}
}
```

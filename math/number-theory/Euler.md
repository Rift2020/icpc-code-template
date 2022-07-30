# 欧拉函数

求单个欧拉函数值

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

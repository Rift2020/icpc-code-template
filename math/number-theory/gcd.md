# gcd

## 求两个数的最大公约数

时间复杂度(最坏)：$$O(\log n)$$

C++14可以用`__gcd`，C++17开始引入`numeric`头文件，可以用`gcd` 

### 手写

参数：a，b

返回：gcd(a,b)

```c++
int gcd(int a,int b){
	if(b==0)return a;
	return gcd(b,a%b);
}
```

## 求多个数的最大公约数

设有$$n$$个数$$a_1,a_2,...,a_n$$，$$a_i\leq x$$，求这$$n$$个数的最大公约数

时间复杂度：$$O(n \log x)$$

```c++
/**
 *    author:  Rift
 *    created: 2022.08.09  18:17
**/
int g=__gcd(a[1],a[2]);
for(int i=3;i<=n;++i)
	g=__gcd(g,a[i]);
```


# 模逆元

## 求单个数的模逆元

$$ax\equiv 1\pmod n$$

### 快速幂

原理：费马小定理

要求：n是素数

时间复杂度：$$O(\log n)$$

```c++
mod=p;
ans=qpow(a,n-2);
```

其中，qpow为**取模快速幂**

### exgcd

原理：模逆元是一种线性同余方程

要求：$$\gcd(a,n)=1$$

时间复杂度：$$O(\log n)$$

```
int x,y;
exgcd(a,n,x,y);
ans=x;
```


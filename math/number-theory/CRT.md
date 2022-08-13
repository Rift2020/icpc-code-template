# 中国剩余定理 CRT
求解如下形式的一元线性同余方程组：

$$
\begin{cases}
x &\equiv a_1 \pmod {n_1} \\
x &\equiv a_2 \pmod {n_2} \\
  &\vdots \\
x &\equiv a_k \pmod {n_k} \\
\end{cases}
$$

## 标准CRT

**要求**：$$n_1, n_2, \cdots, n_k$$两两互质

输入：a[]，n[]，k($$a_i$$存放在a[i]即数组从索引1开始而不是0)

返回：对应方程组的解

复杂度：$$O(k\log (\prod n))$$

溢出警告：运算过程中最大会有$$a_i\times \prod n$$的数据，可能爆long long，可以使用快速乘避免

```c++
ll CRT(int a[],int n[],int k){
	ll ns=1;
	ll re=0;
	for(int i=1;i<=k;++i)ns*=n[i];
	for(int i=1;i<=k;++i){
		ll minv,y;
		ll m=ns/n[i];
		exgcd(m,n[i],minv,y);
		re=(re+(a[i]*m*minv)%ns)%ns;
	}
	return (re+ns)%ns;
}
```

## exCRT

**不**要求：$$n_1, n_2, \cdots, n_k$$ 两两互质

输入：a[]，n[]，k($$a_i$$存放在a[i]即数组从索引1开始而不是0)

返回：对应方程组的解，启用if行注释则会返回-1报告无解

复杂度：$$O(k\log (lcm(n_1,n_2,...,n_k)))$$

溢出警告：运算过程中最大会有$$2n_i\times lcm(n_1,n_2,...n_k)$$的数据，可能爆long long，可以使用快速乘避免

(存疑)：似乎exCRT完爆CRT

原理：

可以证明$$\begin{cases}
x &\equiv a_1 \pmod {n_1} \\
x &\equiv a_2 \pmod {n_2} \\
\end{cases}$$等价于$$x\equiv b\pmod m$$，其中$$b=n_1p+a_1,m=lcm(n_1,n_2)$$，其中$$p$$是不定方程$$m_1p-m_2q=a_2-a_1$$的一个特解   (若$$\gcd(m_1,m_2)$$不能整除$$a_2-a_1$$，无解)

依次合并所有方程即可

```c++
ll exCRT(long long a[], long long n[], int k){
    ll anow=a[1],nnow=n[1];
    for(int i=2;i<=k;++i){
        ll x,y,g;
        g=exgcd(nnow, n[i], x, y);
        //if((a[i]-anow)%g!=0)return -1;
        x=x*((a[i]-anow)/g);
        anow= x * nnow + anow;
        nnow=nnow/g*n[i];//nnow=lcm(nnow,n[i])
        anow= (anow % nnow + nnow) % nnow;
    }
    return (anow % nnow + nnow) % nnow;
}
```


# 费马小定理和欧拉定理

## 费马小定理

若$$p$$是素数，且$$a$$不是p的倍数，则$$a^{p-1}\equiv 1\pmod p$$

又，对于任意整数$$a$$，有$$a^p\equiv a\pmod p$$

## 欧拉定理

若$$\gcd(a,m)=1$$，则有$$a^{\phi(m)}\equiv 1 \pmod m$$，其中$$\phi$$是欧拉函数

## 扩展欧拉定理

用处：**降幂**

$$a^b\equiv\left\{
\begin{aligned}
a^{b\mod \phi(m)} ,&\ \gcd(a,m)=1  \\  
a^{(b\mod \phi(m))+\phi(m)}, &\gcd(a,m)\neq 1,b\geq \phi(m)
\end{aligned}  
\right.$$

请注意：如果$$\gcd(a,m)\neq 1,b<\phi (m)$$，则不可降幂

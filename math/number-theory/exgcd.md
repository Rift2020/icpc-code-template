# exgcd

用于求二元一次不定方程$$ax+by=\gcd(a,b)$$的一组特解$$x_0,x_1$$

时间复杂度(最坏)：$$O(\log n)$$

参数：a，b，x，y(x，y的初值无所谓)

返回：$$\gcd(a,b)$$，并令$$x=x_0,y=y_0$$

```c++
/**
 *    author:  Rift
 *    created: 2022.08.09  22:30
**/
int exgcd(int a,int b,int &x,int &y){
	if(b==0){
		x=1;
		y=0;
		return a;
	}
	int g=exgcd(b,a%b,x,y);
	int t=x;
	x=y;
	y=t-(a/b)*y;
	return g;
}
```

## 扩展应用

### 二元一次不定方程$$ax+by=c$$的解

根据贝祖定理，当且仅当$$\gcd(a,b)|c$$时，该方程有整数解

先求出$$ax+by=\gcd(a,b)$$的一组解$$x_0,y_0$$

令$$g=\gcd(a,b)$$

则$$ax+by=c$$的一组特解为$$x_1=\frac c g x_0,y_1 =\frac c g y_0$$

任意解为

​	$$\left\{
\begin{aligned}
x=x_1+b't   \\  
y=y_1+a't  
\end{aligned}  
\right.$$ 其中$$t\in Z, a'=\frac a g,b'=\frac b g$$

$$x$$的最小正整数特解为$$(x_1\mod b'+b')\mod b'$$

#### 另外

如果$$\gcd(a,b)=1,a\geq 0,b \geq 0$$，并且限制$$x,y$$都是自然数

设$$k=ab-a-b$$ (易知$$k$$必为奇数)

则$$ax+by$$一定能表示$$\{0\}\cup (k,+\infty)$$的整数，一定不能表示$$(-\infty,0)\cup \{k\}$$，对于$$(0,k)$$之间的整数，则有：如果能表示$$n$$，则一定不能表示$$k-n$$

### 线性同余方程$$ax\equiv c \pmod b$$

实际上，这个方程的解的情况与二元一次不定方程$$ax+by=c$$完全等价

另：当a和c是超大数，可以取模b读取

### 逆元

实际上，这与线性同余方程中，c=1的情况相同

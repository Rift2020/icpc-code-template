# 时间复杂度

## 符号

(渐进复杂度)

上下界符号(既$$\leq$$又$$\geq$$)：$$\Theta$$

上界符号($$\leq$$)：$$O$$

下界符号($$\geq$$)：$$\Omega$$

“小于”符号$$<$$：$$o$$

“大于”符号$$>$$：$$\omega$$

## 主定理

用以计算递归算法的复杂度：

如果一个算法将规模为$$n$$的问题分解为$$a$$个，每个子问题规模$$n/b$$，且需要$$f(n)$$的时间作其他事情(譬如分解问题或是合并子问题的解)
$$
T(n) = a T\left(\frac{n}{b}\right)＋f(n)\qquad \forall n > b
$$
则：
$$
T(n) = \begin{cases}\Theta(n^{\log_b a}) & f(n) = O(n^{\log_b a-\epsilon}) \\ \Theta(f(n)) & f(n) = \Omega(n^{\log_b a+\epsilon})\\ \Theta(n^{\log_b a}\log^{k+1} n) & f(n)=\Theta(n^{\log_b a}\log^k n),k\ge 0 \end{cases}
$$
对于第二种情况，还需要满足：存在某个常数$$c<1$$使得对所有足够大的$$n$$，都有$$af(n/b)\leq cf(n)$$

注意：主定理的三种情况并没有覆盖所有的情况，它们之间有“间隙”，如果情况恰好落在间隙里，则无法用主定理求解

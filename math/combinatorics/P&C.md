# 排列组合

## 排列

n个不同元素，取m个排成一列，所有排列的个数。记作$$P_n^m$$ (或记$$A_n^m$$)

$$A_n^m=n(n-1)...(n-m+1)=\frac{n!}{(n-m)!}$$

### 全排列

$$A_n^n=n!$$

## 组合

n个不同元素，取m个组成一个集合，所有集合的数量。记作$$C_n^m$$，更简洁的，记作$$\tbinom n m$$

$$C_n^m=\tbinom{n}{m}=\frac{P_n^m}{m!}=\frac{n!}{m!(n-m)!}$$

### 组合数性质

$$\binom{n}{m}=\binom{n-1}{m}+\binom{n-1}{m-1}\tag{3}$$

$$\binom{n}{0}+\binom{n}{1}+\cdots+\binom{n}{n}=\sum_{i=0}^n\binom{n}{i}=2^n\tag{4}$$



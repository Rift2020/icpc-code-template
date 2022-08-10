# 整除

自C++11或C99标准开始，整数除法为**向零取整**(直接舍去小数部分)

即有

```
5/2=2
-5/2=-2
```

下面两行宏函数，分别为**向上取整**的整除，**向下取整**的整除，即它们分别给出$$\lceil \frac a b\rceil$$，$$\lfloor \frac a b \rfloor$$

```
/**
 *    author:  Rift
 *    created: 2022.08.10  13:30
**/
#define divCeil(a,b) ((a<0||a%b==0)?a/b:a/b+1)
#define divFloor(a,b) ((a>0||a%b==0)?a/b:a/b-1)
```


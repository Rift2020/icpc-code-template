# (取模)快读

读取一个int范围的数

(添加注释行则变为取模读取)

```c++
/**
 *    author:  Rift
 *    created: 2022.08.10  19:59
**/
int inline read(){
    int s=0,f=1;
    char c=getchar();
    while(!(c<='9'&&c>='0')){
        if(c=='-')f=-1;
        c=getchar();
    }
    while(c<='9'&&c>='0'){
        s=(s<<1)+(s<<3);
        s+=c-'0';
		//s%=mod;
        c=getchar();
    }
    return f*s;
}
```


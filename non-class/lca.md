# 最近公共祖先

- 两点的最近公共祖先必定处在树上两点间的最短路上
- $$d(u,v)=dep_u+dep_v-2dep_{LCA(u,v)}$$,dep深度

```c++
/**
 *    author:  Rift
 *    modified: 2022.11.09  13:00
**/
#include<bits/stdc++.h>
#define maxn 500005
#define maxm 500005
using namespace std;
int n,m,s,a,b,fa[maxn][30],dep[maxn];
vector<int> v[maxn];
void ydfs(int x,int r){
    dep[x]=dep[r]+1;
    fa[x][0]=r;
    for(int i=1;;++i){
        if(fa[fa[x][i-1]][i-1]==0)break;
        fa[x][i]=fa[fa[x][i-1]][i-1];
    }
    for(int i:v[x]){
        if(i!=r)
            ydfs(i,x);
    }
}
int lca(int x,int y){

    if(dep[x]>dep[y])swap(x,y);//令y比x深
    int ddep=dep[y]-dep[x];
    for(int j=0;ddep;++j,(ddep>>=1))//令y上跳，直到dep[x]==dep[y]
		if(ddep&1)y=fa[y][j];
	if(x==y)return x;
    for(int i=29;i>=0;--i){//令x，y一起上跳
        if(fa[x][i]==0||fa[x][i]==fa[y][i])continue;
        x=fa[x][i];
        y=fa[y][i];
    }
    return fa[x][0];
}
int main(){
    //freopen("1.in","r",stdin);
    cin>>n>>m>>s;
    memset(fa,0,sizeof(fa));
    for(int i=1;i<n;++i){
        scanf("%d%d",&a,&b);
        v[a].push_back(b);
        v[b].push_back(a);
    }
    dep[s]=1;
    ydfs(s,0);
    for(int i=0;i<m;++i){
        scanf("%d%d",&a,&b);
        printf("%d\n",lca(a,b));
    }
    return 0;
}
```


# 差分约束

```c++
/**
 *    author:  Rift
 *    created: 2022.11.08  16:45
**/
//形如 x_v - x_u \leq w 方程组
#include<bits/stdc++.h>
#include <climits>
#define rep(i, a, b) for (int i = (a); i <= (b); ++i)
#define per(i, a, b) for (int i = (a); i >= (b); --i)
#define x first
#define y second
#define maxn 5005
using namespace std;
using ll = long long;
using pr = pair<int,int>;
int n,m,s=0;
vector<pr> g[maxn];
int dis[maxn],cnt[maxn];
bool inq[maxn];
queue<int> q;
bool spfa(){
    dis[s]=0;
    q.push(s);
    while(!q.empty()){
        int u=q.front();
        q.pop();
        inq[u]=false;
        for(auto i:g[u]){
            int v=i.x,w=i.y;
            if(dis[u]+w<dis[v]){
                cnt[v]=cnt[u]+1;
                dis[v]=dis[u]+w;
                if(cnt[v]>n)return false;//因为加了一个超级源点，但没有把n加1，所以此处是大于
                if(inq[v]==false){
                    q.push(v);
                    inq[v]=true;
                }
            }
        }
    }
    return true;
}

signed main(){
	ios::sync_with_stdio(false),cin.tie(nullptr);
	cin>>n>>m;
	for(int i=0;i<=n;++i)dis[i]=INT_MAX;
	for(int i=1;i<=n;++i)
		g[0].push_back({i,0});
	rep(i,1,m){
		int u,v,w;
		cin>>v>>u>>w;//!!!!! v,u,w !!!!!!!
		g[u].push_back({v,w});
	}
	if(spfa()==false){
		cout<<"NO"<<endl;
	}
	else{
		rep(i,1,n){
			cout<<dis[i]<<" ";
		}
		cout<<endl;
	}
	
	return 0;
}
```


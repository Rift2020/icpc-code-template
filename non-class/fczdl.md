# 分层最短路

```c++
/**
 *    author:  Rift
 *    created: 2022.11.08  12:33
 **/
/*
 * 分层图s->t，可以免费k条路
 * 第i层的点u映射为n*i+u，原始图在第0层
 * 因此共有k+1层，n*(k+1)个点
 */
#include<bits/stdc++.h>
#include <vector>
#define rep(i, a, b) for (int i = (a); i <= (b); ++i)
#define per(i, a, b) for (int i = (a); i >= (b); --i)
#define x first
#define y second
#define maxn 200005//n*k
using namespace std;
using ll = long long;
using pr = pair<int,int>;
int n,m,k,s,t;
vector<pr> g[maxn];//num,cost
int dist[maxn];
bool vis[maxn];
priority_queue<pr,vector<pr>,greater<pr> > pq;//cost,num
void connect(int u,int v,int w){
	for(int j=0;j<=k;++j){
		g[u+j*n].push_back({v+j*n,w});
		if(j!=k)
			g[j*n+u].push_back({(j+1)*n+v,0});
	}

}
void dij(){
	pq.push(pr{0,s});
	while(!pq.empty()){
		int u=pq.top().second;
		pq.pop();
		if(vis[u])continue;
		vis[u]=true;
		for(auto i:g[u]){
			int cost=i.y,v=i.x;
			if(dist[v]>dist[u]+cost){
				dist[v]=dist[u]+cost;
				pq.push(pr{dist[v],v});
			}
		}
	}
}
signed main(){
	ios::sync_with_stdio(false),cin.tie(nullptr);
	cin>>n>>m>>k;
	cin>>s>>t;
	for(int i=0;i<=n*(k+1);++i)dist[i]=INT_MAX;
	dist[s]=0;
	rep(i,1,m){
		int u,v,w;
		cin>>u>>v>>w;
		connect(u,v,w);
		connect(v,u,w);

	}

	dij();
	int ans=dist[t];
	
	for(int i=0;i<=k;++i)
		ans=min(ans,dist[t+i*n]);
	cout<<ans<<endl;

	return 0;
}
```


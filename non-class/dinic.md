# 最大流
dinic
$$O(n^2m)$$
亦可求二分图最大匹配$$O(m\sqrt n)$$
```
/**
 *    author:  Rift
 *    created: 2022.11.15  03:16
**/
#define maxn 205
#define maxm 10010//maxm 必须设为两倍


struct edge{
    int from,to,flow,cap;//from实际上用不到
};
struct dinic{
    int n,m,s,t;//n实际上用不到
    edge eg[maxm];//边，[0,n)索引存边
    vector<int> g[maxn];//g[u]存着点u为起点的边，值为eg的索引
    int dep[maxn];//残量网络中，点i的层数
	int cur[maxn];//用于当前弧优化(一次建层中，一条边增广过，那么就不可能被增广第二次)
    bool vis[maxn];

    queue<int> q,cl;

    void init(int n){
        for(int i=0;i<=n;++i)
            g[i].clear();
        m=0;
    }
    void add_edge(int u,int v,int cap){
        g[u].push_back(m);
        eg[m++]={u,v,0,cap};
        g[v].push_back(m);
        eg[m++]={v,u,0,0};

    }
    bool bfs(){//分层
        memset(vis,0,sizeof(vis));
        q=cl;
        q.push(s);
        dep[s]=0;
        vis[s]=1;
        while(!q.empty()){
            int u=q.front();
            q.pop();
            for(int i:g[u]){
                edge &e=eg[i];
                if(vis[e.to]||e.cap==e.flow)continue;
                vis[e.to]=1;
                dep[e.to]=dep[u]+1;
                q.push(e.to);
            }
        }
        return vis[t];
    }
    int dfs(int u,int left){//当前增广的节点，流到这个点的流量
        if(u==t||left==0)return left;
        int flow=0,f;
        for(int &i=cur[u];i<g[u].size();++i){//当前弧优化&多路增广
            edge &e=eg[g[u][i]];
            edge &fe=eg[g[u][i]^1];//反向边
            if(dep[u]+1!=dep[e.to])continue;
            f=dfs(e.to,min(left,e.cap-e.flow));
            if(f==0)continue;
            e.flow+=f;
            fe.flow-=f;
            flow+=f;
            left-=f;
            if(left==0)break;
        }
        return flow;
    }
    int MaxFlow(int s,int t){
        this->s=s;
        this->t=t;
        int flow=0;
        while(bfs()){
            memset(cur,0,sizeof(cur));
            flow+=dfs(s,INT_MAX);
        }
        return flow;
    }
};
```

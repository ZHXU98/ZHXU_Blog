## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_F_爬爬爬山_【dijkstra】

爬山是wlswls最喜欢的活动之一。  
在一个神奇的世界里，一共有n座山，m条路。wls初始有k点体力，在爬山的过程中，他所处的海拔每上升1m，体力会减1点，海拔每下降1m，体力会加一点。  
现在wls想从1号山走到n号山，在这个过程中，他的体力不能低于0，所以他可以事先花费一些费用请dls把某些山降低，将一座山降低l米需要花费  
l*l
的代价，且每座山的高度只能降低一次。因为wls现在就在1号山上，所以这座山的高度不可降低。wls从1号山到n号山的总代价为降低山的高度的总代价加上他走过的路的总长度。  
wls想知道最小的代价是多少。

脑子第一反应是能量守恒 迷wa 上白书板子写过了  
预处理下 然后跑 dijkstra n可以到100000 加堆  
(wls 居然吐槽好多队 dj得名字写得千奇百怪 其实我也不记得这么拼写啦 23333)

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 200000+5;
    
    int n,m;
    ll k;
    
    typedef pair<ll,int> P;
    vector<P> G[maxn];
    priority_queue<P,vector<P>,greater<P> > que;
    
    ll dis[maxn];
    ll h[maxn];
    
    void dj() {
    	memset(dis,0x3f,(n+3)*sizeof(ll));
    	dis[1]=0;
    	que.push(P(0,1));
    	while(!que.empty()) {
    		P tp=que.top();
    		que.pop();
    		int u=tp.second,v;
    		if(dis[u]<tp.first) continue;
    		for(int i=0; i<G[u].size(); i++) {
    			P tmp=G[u][i];
    			v=G[u][i].second;
    			if(dis[v]>dis[u]+tmp.first) {
    				dis[v]=dis[u]+tmp.first;
    				que.push(P(dis[v],v));
    			}
    		}
    	}
    	cout<<dis[n]<<endl;
    }
    
    int main() {
    	scanf("%d %d %lld",&n,&m,&k);
    	for(int i=0; i<n; i++) scanf("%lld",&h[i+1]);
    	k+=h[1];
    	int st,ed;
    	ll co;
    	for(int i=0; i<m; i++) {
    		scanf("%d %d %lld",&st,&ed,&co);
    
    		if(h[ed]>k) G[st].push_back(P(co+(h[ed]-k)*(h[ed]-k),ed));
    		else G[st].push_back(P(co,ed));
    
    		if(h[st]>k) G[ed].push_back(P(co+(h[st]-k)*(h[st]-k),st));
    		else G[ed].push_back(P(co,st));
    	}
    	dj();
    	return 0;
    }
    


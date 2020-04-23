## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_J_夺宝奇兵_【枚举+贪心】

wls 满足自己从村民哪里买回东西后比每个村民剩下得 都严格多

枚举wls买了K个 满足k个判断 少于k个 贪心得补全

INF开小了 ll 4个3f也是醉了 。。。。orz

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1000+5 ;
    
    int n,m;
    
    struct node {
    	int id,pos;
    	ll val;
    	bool operator < (const node &a) {
    		return val<a.val;
    	}
    	node() {}
    	node(int _id,int _pos,ll _val) {
    		id=_id,pos=_pos,val=_val;
    	}
    } a[maxn];
    
    vector<node> G[maxn];
    bool vis[maxn];
    
    int main() {
    	scanf("%d %d",&n,&m);
    	ll cost;
    	int id;
    	for(int i=0; i<m; i++) {
    		scanf("%lld %d",&cost,&id);
    		G[id].push_back(node(id,i,cost));
    		a[i]=node(id,i,cost);
    	}
    
    	sort(a,a+m);
    	for(int i=1; i<=n; i++) sort(G[i].begin(),G[i].end());
    	
    	ll ans=0x3f3f3f3f3f3f3f3f,tmp=0;
    	int cnt=0;
    	
    	for(int k=1; k<=m; k++) {
    		tmp=0,cnt=0;
    		memset(vis,0,(m+5)*sizeof(bool));
    		for(int i=1; i<=n; i++) {
    			if(G[i].size()>=k) {
    				int len=max(0,(int)G[i].size()-k+1);
    				cnt+=len;
    				for(int j=0; j<len; j++) {
    					tmp+=G[i][j].val;
    					vis[G[i][j].pos]=1;
    				}
    			}
    		}
    
    		if(cnt<k) {
    			for(int i=0;i<m;i++){
    				if(!vis[a[i].pos]){
    					tmp+=a[i].val;
    					cnt++;
    				}
    				if(cnt==k) break;
    			}
    		}
    		if(cnt==k) ans=min(ans,tmp);
    	}
    	cout<<ans<<endl;
    	return 0;
    }
    


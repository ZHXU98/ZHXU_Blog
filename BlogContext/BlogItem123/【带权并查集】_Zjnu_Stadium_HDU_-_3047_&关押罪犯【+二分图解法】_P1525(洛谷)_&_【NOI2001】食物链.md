## 【带权并查集】_Zjnu_Stadium_HDU_-_3047_&关押罪犯【+二分图解法】_P1525(洛谷)_&_【NOI2001】食物链

带权并查集 分析A，B之间的相对距离，可以得到rnk[fa] = rnk[A]+x-rnk[B]。 注意到这时，对于原来的A的树，只更新了fa跟结点的权值，
那么其它结点的更新在查找的那一步里面实行了。  
维护是相对距离

一开始 ab之间关系 a到b是s 在 fa fb 不一样是 我们可以当 a到fa b到fb 还有 a到b距离s 是向量 那么
rnk[fb]=rnk[a]-rnk[b]+s

种类并查集很多耶可以转到带权上orz

<http://acm.hdu.edu.cn/showproblem.php?pid=3047>  
Zjnu Stadium HDU - 3047 带权并查集板子题

板子

    
    
    #include <iostream>
    using namespace std;
    typedef long long ll;
    typedef pair<int,int> P;
    const int maxn=200000+10;
    ////const int INF = 0x3f3f3f3f;
    const int gxs = 3;
    int n,m;
    int pre[maxn],rnk[maxn];
    
    void init() {
    	for(int i=0; i<maxn; i++) pre[i]=i,rnk[i]=0;
    }
    //查找
    int finds(int x){
    	if(x==pre[x]) return x;
    	int t=pre[x];
    	pre[x]=finds(pre[x]);
    	rnk[x]+=rnk[t];
    	return pre[x];
    }
    
    //新建关系
    void unions(int x,int y,int s) {
    	int fx=finds(x),fy=finds(y);
    	if (fx!=fy) {
    		pre[fy]=fx;
    		rnk[fy]=rnk[x]-rnk[y]+s;//+mod;
    	//	rnk[fy]%=mod;
    	}
    }
    
    int main() {
    	int b,a,k;
    	ios::sync_with_stdio(false);
    	while(cin>>n>>m){
    		init();
    		int ans=0;
    		for(int i=0;i<m;i++){
    			cin>>a>>b>>k;
    			if(finds(a)==finds(b)){
    				if(rnk[a]+k!=rnk[b]) ans++;
    			}else{
    				unions(a,b,k);
    			}
    		}
    		cout<<ans<<endl;
    	} 
    	return 0;
    }
    

之后2题只是在带权并查集上 加了mod  
就比如食物链 a吃b b吃c c吃a 一样 0 表示同类 1 表示x吃y 2表示y吃x  
他们关系成环 所以知道 a b距离1 b c 距离又是 1 很显然 a c距离是2 这样 就是 c吃a 每次分析相对位置就好

    
    
    #include <iostream>
    using namespace std;
    typedef long long ll;
    typedef pair<int,int> P;
    const int maxn=200000+10;
    ////const int INF = 0x3f3f3f3f;
    //const long long mod = 1e9+7;
    const int gxs = 3;
    int n,m;
    const int mod=3;
    
    int pre[maxn],rnk[maxn];
    //查找 
    
    void init(){
        for(int i=0;i<maxn;i++) pre[i]=i,rnk[i]=0;
    }
    int finds(int x) 
    {
        if(x==pre[x]) return x;
        else 
        {
            int root=finds(pre[x]);     // 找到根节点
            rnk[x]+=rnk[pre[x]];       // 权值合并，更新
            rnk[x]%=mod;
            return pre[x]=root;        // 压缩路径
        }
    }
    
    //新建关系
    void unions(int x,int y,int m){
        int a=finds(x),b=finds(y);
        if(a!=b){
            pre[b]=a;
            rnk[b]=rnk[x]+m-rnk[y];
            rnk[b]+=gxs;
            rnk[b]%=gxs;
        }
    } 
    
    int main() {
        int b,a,k;
        ios::sync_with_stdio(false);
        cin>>n>>m;
            init();
            int ans=0;
            for(int i=0;i<m;i++){
                cin>>k>>a>>b;
                if((k==2&&a==b)||a>n||b>n||a<=0||b<=0) {
                    ans++;
                }
                else if(k==1){
                    if(finds(a)==finds(b)&&(rnk[a]!=rnk[b])) ans++;
                    else  unions(a,b,0);
                }
                else if(k==2){
                    if(finds(a)==finds(b)&&(rnk[a])!=(rnk[b]+2)%mod) ans++;
                    else  unions(a,b,1);
                }
            }
            cout<<ans<<endl;
        
        return 0;
    }
    

排序是必须的，我们要尽可能让危害大的罪犯在两个监狱里。  
那么，再结合敌人的敌人和自己在一个监狱的规律合并。  
当查找时发现其中两个罪犯不可避免地碰撞到一起时，只能将其输出并结束。  
带权 mod取2；  
wa 第一个点 就是没有冲突时输出 0 orz

    
    
    #include <iostream>
    #include <vector>
    #include <algorithm>
    using namespace std;
    typedef long long ll;
    typedef pair<int,int> P;
    const int maxn=200000+10;
    ////const int INF = 0x3f3f3f3f;
    const int gxs = 2;
    const int mod=2;
    int n,m;
    int pre[maxn],rnk[maxn];
    
    void init() {
    	for(int i=0; i<maxn; i++) pre[i]=i,rnk[i]=0;
    }
    //查找
    int finds(int x){
    	if(x==pre[x]) return x;
    	int t=pre[x];
    	pre[x]=finds(pre[x]);
    	rnk[x]+=rnk[t];
    	rnk[x]%=gxs;
    	return pre[x];
    }
    
    //新建关系
    void unions(int x,int y,int s) {
    	int fx=finds(x),fy=finds(y);
    	if (fx!=fy) {
    		pre[fy]=fx;
    		rnk[fy]=rnk[x]-rnk[y]+s+gxs;
    		rnk[fy]%=mod;
    	}
    }
    
    struct node{
    	int a,b;
    	ll val;
    	bool operator < (const node &a)const{
    		return a.val<val;
    	}
    };
    
    vector<node> que;
    
    int main() {
    	cin>>n>>m;
    	int a,b;
    	ll v;
    	for(int i=0;i<m;i++){
    		cin>>a>>b>>v;
    		que.push_back(node{a,b,v});
    	}
    	sort(que.begin(),que.end());
    	init();
    //	ll ans=que[m-1].val;
    	for(int i=0;i<m;i++){
    		a=que[i].a;
    		b=que[i].b;
    		v=que[i].val;
    		if(finds(a)==finds(b)){
    			if(rnk[a]==rnk[b]){
    				cout<<v<<endl;
    				return 0;
    			}
    		}else{
    			unions(a,b,1);
    		}
    	}
    	cout<<0<<endl;
    	return 0;
    }
    

接下来二分图解法 二分枚举最大边 看看能不能切二部份

    
    
    #include <iostream>
    #include <vector>
    #include <algorithm>
    #include <cstring>
    #include <queue>
    using namespace std;
    typedef long long ll;
    typedef pair<int,int> P;
    const int maxn=20000+10;
    ////const int INF = 0x3f3f3f3f;
    const int gxs = 2;
    const int mod=2;
    int n,m;
    
    struct node {
    	int to;
    	ll val;
    };
    
    vector<node> G[maxn];
    int col[maxn];
    
    void add_edge(int a,int b,ll v) {
    	G[a].push_back(node {b,v});
    	G[b].push_back(node {a,v});
    }
    int flag;
    bool ok(ll mid) {
    	queue <int> q;
    	for(int i=1; i<=n; i++)
    		if(!col[i]) {
    			q.push(i);
    			col[i]=1;
    			while(!q.empty()) {
    				int x=q.front();
    				q.pop();
    				for(int i=0; i<G[x].size(); i++)
    					if(G[x][i].val>=mid) {// 跟上面一样 不能二分图了 2人一个集合里面 出现的最小冲突 就是mid
    						if(!col[G[x][i].to]) {
    							q.push(G[x][i].to);
    							if(col[x]==1) col[G[x][i].to]=2;
    							else col[G[x][i].to]=1;
    						} 
    						if(col[G[x][i].to]==col[x])
    							return false;
    					}
    			}
    		}
    	return true;
    }
    
    int main() {
    	cin>>n>>m;
    	int a,b;
    	ll v;
    	ll l,r;
    	for(int i=0; i<m; i++) {
    		cin>>a>>b>>v;
    		add_edge(a,b,v);
    		r=max(r,v);
    	}
    	r++;
    	l=0;
    	ll ans=r-1;
    	while(r-l>1) {
    		ll mid=(l+r)>>1;
    	//	flag=0;
    		memset(col,0,(n+5)*sizeof(int));
    		if(ok(mid)) {
    			r=mid;
    		} else l=mid;
    	}
    	cout<<l;
    	return 0;
    }
    


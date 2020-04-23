## 2019_ACM-ICPC_西安邀请赛_A_C_D_L_M_(+签到题_题解)

## A

**Tasks**  
上来以为DP 结果直接贪也是楞了

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 1e5 + 5 ;
    
    int a[maxn];
    
    signed main() {
        fastio;
        int n, m;
        cin >> n >> m;
    
        for(int i = 1 ; i <= n; i++) {
            cin >> a[i];
        }
    
        int res = 0;
        int ans = 0;
        sort(a + 1, a + 1 + n);
        for(int i = 1; i <=n; i++) {
            if(res + a[i] <= m)
                res += a[i], ans++;
            else
                break;
        }
    
        cout << ans << endl;
    
        return 0;
    }
    

## C

** Angel’s Journey**  
画图计算公式 简单计算下

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 1e5 + 5 ;
    const double pi = acos(-1.0);
    
    double rdis(double x, double y, double a, double b) {
        return sqrt((x - a) * (x - a) + (y - b) * (y - b));
    }
    
    signed main() {
        //  fastio;
        int T;
        cin >> T;
        while(T--) {
            double rx, ry, r, x, y;
    
            cin >> rx >> ry >> r >> x >> y;
    
            if(x < rx - r || x > rx + r) {
                if(x < rx - r) {
                    double ans = rdis(rx - r, ry, x, y);
                    ans += pi / 2 * r;
                    printf("%.4lf\n", ans);
                } else {
                    double ans = rdis(rx + r, ry, x, y);
                    ans += pi / 2 * r;
                    printf("%.4lf\n", ans);
                }
            } else {
                double d = rdis(x, y, rx, ry);
                double qd = sqrt(d * d - r * r);
    
                double a1 = atan(abs(x-rx) / abs(ry-y));
                double a2 = atan(qd / r);
    
                double j = pi / 2 - a1 - a2;
    
                double xl = j * r;
    
             //   cout<<qd<<"    "<<a1<<"   "<<a2<<"   "<<j<<"  "<<xl<<endl;
    
                double ans = xl;
                ans += qd;
                ans += pi / 2 * r;
                printf("%.4lf\n", ans);
            }
        }
        return 0;
    }
    

## D

**Miku and Generals**  
把物品有关系 一开始dfs分好 变成一个必选其中一个的  
而孤立点 就是一半的01背包

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 1e6 + 5 ;
    const double pi = acos(-1.0);
    
    int n, m;
    int dp[maxn];
    int a[maxn];
    
    int head[3005], cnt;
    int nxt[10000], to[10000];
    
    struct node {
    	int v, w;
    } wu[205];
    
    void ade(int a, int b) {
    	to[++cnt] = b;
    	nxt[cnt] = head[a];
    	head[a] = cnt;
    }
    
    int s1,s2;
    int vis[maxn];
    
    void dfs(int x) {
    	if(vis[x]==1) {
    		if(s1==-1) s1=0;
    		s1+=a[x];
    	} else {
    		if(s2==-1) s2=0;
    		s2+=a[x];
    	}
    
    	for(int i = head[x]; i; i = nxt[i]) {
    		if(vis[to[i]]) continue;
    		vis[to[i]] = (vis[x] == 1 ? 2 : 1);
    		dfs(to[i]);
    	}
    }
    
    signed main() {
    //	fastio;
    	int T;
    	cin >> T;
    	while(T--) {
    		int sum=0;
    		memset(head,0,sizeof(head));
    		memset(vis,0,sizeof(vis));
    		memset(dp,0,sizeof(dp));
    		cnt=0;
    		cin >> n >> m;
    		for(int i = 1; i <= n; i++) {
    			cin >> a[i];
    			a[i]/=50;
    			sum+=a[i];
    		}
    
    		for(int i = 1; i <= m; i++) {
    			int u, v;
    			cin >> u >> v;
    			ade(u,v),ade(v,u);
    		}
    
    		int wus=0;
    	
    		for(int i=1; i<=n; i++) {
    			if(vis[i]) continue;
    			vis[i]=1;
    			s1=-1,s2=-1;
    			dfs(i);
    			wu[++wus]=node {s1,s2};
    		}
    	
    		for(int i=1; i<=wus; i++) {
    			for(int j=sum/2; j>=0; j--) {
    				if(wu[i].w!=-1) {
    					if(j>=wu[i].w&&j>=wu[i].v) {
    						dp[j]=max(dp[j-wu[i].v]+wu[i].v,dp[j-wu[i].w]+wu[i].w);
    					} else if(j>=wu[i].w) {
    						dp[j]=dp[j-wu[i].w]+wu[i].w;
    					} else if(j>=wu[i].v) {
    						dp[j]=dp[j-wu[i].v]+wu[i].v;
    					}
    				} else {
    					if(j>=wu[i].v)
    						dp[j]=max(dp[j],dp[j-wu[i].v]+wu[i].v);
    				}
    			}
    		}
    		cout<<(sum-dp[sum/2])*50<<endl;
    		
    	}
    	return 0;
    }
    
    

## L

**Swap**  
直接打表上去也压力不大。。才400ms

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    using namespace std;
    #define int long long
    
    const int maxn = 1e5 + 5 ;
    
    set<string> S;
    
    string s;
    int len;
    void dfs(int d) {
        if(d) {
            if(len % 2 == 0) {
                for(int i = 0; i < len / 2; i++) {
                    swap(s[i], s[i + len / 2 ]);
                }
              //  cout << s << endl;
                if(S.find(s) != S.end())
                    return ;
                S.insert(s);
                dfs(!d);
            }
            else{
                for(int i = 0; i < len / 2; i++) {
                    swap(s[i], s[i + len / 2 +1]);
                }
              //  cout << s << endl;
                if(S.find(s) != S.end())
                    return ;
                S.insert(s);
                dfs(!d);
            }
    
        } else {
            for(int i = 0; i < len - 1; i += 2) {
                swap(s[i], s[i + 1]);
            }
           // cout << s << endl;
            if(S.find(s) != S.end())
                return ;
            S.insert(s);
            dfs(!d);
        }
    }
    
    signed main() {
        fastio;
        int n=3;
       // while(n<20){
       cin>>n;
       if(n==1) cout<<1<<endl;
       else if(n==2) cout<<2<<endl;
       else{
    
            s="";
            for(int i = 1; i <= n; i++)
                s.push_back(i + 'a');
          //  cout << s << endl;
            len = n;
            dfs(0);
          //  cout<<n<<"     " << S.size()<<endl;
          cout<<S.size()<<endl;
            n++;
            S.clear();
       }
       // }
        return 0;
    }
    
    

## M

**Travel**  
数据水了 直接贪过去的也行。。。  
正解应该是二分 chk可达n点

    
    
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;
    typedef long long ll;
    
    const int maxn=1e5+10;
    
    struct node {
    	int to,dis;
    };
    bool vis[maxn];
    
    vector<node>v[maxn];
    int n,m,c,d,e;
    
    bool check(int x) {
    	queue<pair<int,int> > que;
    	que.push(make_pair(1,x*e));
    	while(!que.empty()){
    		pair<int,int> p=que.front();
    		que.pop(); 
    		if(p.first==n) return 1; 
    		if(vis[p.first]) continue ;
    		vis[p.first]=1;
    		for(int i=0;i<v[p.first].size();i++){
    			if(p.second>=1&&v[p.first][i].dis<=x*d){
    				que.push(make_pair(v[p.first][i].to,p.second-1));
    			}
    		}
    	}
    	return 0;
    }
    
    signed main() {
    	cin>>n>>m>>c>>d>>e;
    	memset(vis,0,sizeof(vis));
    	for(int i=0; i<m; i++) {
    		int x,y,z;
    		cin>>x>>y>>z;
    		v[x].push_back(node {y,z});
    		v[y].push_back(node {x,z});
    	}
    	
    	int l=0,r=1e5,mid;
    	while(r-l>1) {
    		memset(vis,0,sizeof(vis));
    		mid=(l+r)/2;
    		if(check(mid)) r=mid;
    		else l=mid;
    	}
    	cout<<r*c<<endl;
    	return 0;
    }
    

菜鸡我只能签到了


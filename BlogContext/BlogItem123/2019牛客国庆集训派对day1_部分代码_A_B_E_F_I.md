## 2019牛客国庆集训派对day1_部分代码_A_B_E_F_I

## A 全 1 子矩阵

暴力跑 注意 它全0也是不可以的 我醉了 wa了一发

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int N = 1e2 + 10;
    
    int n, m;
    char mp[N][N];
    
    int main() {
        while(~scanf("%d %d", &n, &m)) {
            int x1 = 100, x2 = -1, y1 = 100, y2 = -1;
            for(int i = 1; i <= n; i ++) {
                scanf("%s", mp[i] + 1);
                for(int j = 1; j <= m; j ++) {
                    if(mp[i][j] == '1') {
                        x1 = min(x1, i);
                        x2 = max(x2, i);
                        y1 = min(j, y1);
                        y2 = max(j, y2);
                    }
                }
            }
            if(x1 == 100 && x2 == -1 && y1 == 100 && y2 == -1) {
                cout << "No" << endl;
            } else {
                int f = 0;
                for(int i = 1; i <= n; i++) {
                    for(int j = 1; j <= m; j++) {
                        if(i >= x1 && i <= x2 && j >= y1 && j <= y2) {
                            if(mp[i][j] == '0') {
                                cout << "No" << endl;
                                f = 1;
                                break;
                            }
                        }
                        else {
                            if(mp[i][j] == '1') {
                                cout << "No" << endl;
                                f = 1;
                                break;
                            }
                        }
                        if(f == 1) break;
                    }
                    if(f == 1) break;
                }
                if(!f) cout << "Yes" << endl;
            }
        }
        return 0;
    }
    

## B 组合数

给出 n 和 k，求 min⁡{n!k!(n−k)!,1018}\min\\{\frac{n!}{k! (n - k)!},
10^{18}\\}min{k!(n−k)!n!​,1018} 的值。  
wdnmd 我既然分解阶乘质因子去搞。。。。。。。。。。。。。。  
然后 没有然后的 其实慢简单的 就是直接暴力for 上面分子大 / 分母小的  
直到某一次 大于1e18 就好了  
为什么非要开long double啊orz 我不开还过不去

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    #define int long double
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    const int inf = 0x3f3f3f3f;
    const long long mod = 1e9 + 7;
    const int maxn = 500000 + 5;
      
    int n, k, ans;
      
    int C(int n, int k){
        int res = 1;
        for(int i = 1, p = n; p >= max(n - k + 1, k + 1); i ++, p --) {
            res = res * p / i;
            if(res >= 1e18) return 0;
        }
        return res;
    }
      
    signed main() {
        while(cin >> n >> k) {
            int p = C(n, k);
            if(p == 0) printf("1000000000000000000\n");
            else cout << (long long)p << endl;
        }
        return 0;
    }
    

## E Numbers

刚开始都没有想到暴力。。。。 结果暴力试一试过了  
想想也对 数据100 个 做到不重复也不容易

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    int ans = 0;
    int len ;
    string s;
    bool vis[105];
    
    void dfs(int x) {
        if(x == len) {
            ans ++;
            return ;
        }
        int t = s[x] - '0';
       
        
        if(vis[t] == 0) {
            vis[t] = 1;
            dfs(x + 1);
            vis[t] = 0;
        } 
    	if(x < len - 1) {
    		if(t == 0) return ;
            int p = t * 10 + s[x + 1] - '0';
            if(vis[p] == 0) {
    	       	vis[p] = 1;
    	        dfs(x + 2);
    	        vis[p] = 0;
    		}
        }
    }
    
    
    int main() {
        while(cin >> s){
            len = s.size();
            ans = 0;
            dfs(0);
            cout << ans << endl;
        }
        return 0;
    }
    

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191001164407856.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
看图 直接搞 分4个块 和 4条线 就是一个等差的数列

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    const long long mod = 1e9 + 7;
    long long n, a, b, c, d;
    
    int main() {
    	while(cin >> n >> a >> b >> c >> d) {
    		long long ans = 0;
    		long long tp = n - 1;
    		//	cout << tp << endl;
    		tp = n * (n - 1) / 2 % mod;
    		ans = (ans + tp * a % mod * b % mod) % mod;
    		ans = (ans + tp * b % mod * c % mod) % mod;
    		ans = (ans + tp * c % mod * d % mod) % mod;
    		ans = (ans + tp * d % mod * a % mod) % mod;
    		ans = (ans + a * n % mod) % mod;
    		ans = (ans + b * n % mod) % mod;
    		ans = (ans + c * n % mod) % mod;
    		ans = (ans + 1 +d * n % mod) % mod;
    		cout << ans << endl;//"    " << mod - ans << endl;
    	}
    	return 0;
    }
    

## I 2019

聪明可可的题 几乎是原题。。。  
我们考虑点分治  
首先是 点分治 每个点到其他点取于2019 然后 t数组存放他们个数  
最后 要注意 自己到自己其实也是0…  
最后剪掉就好 记得除2 只能小的到大的

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    #define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f;
    const long long mod = 2019;
    const int maxn = 5e4+5;
    
    int n,m,k;
    
    int cnt,head[maxn];
    int to[maxn<<1],nxt[maxn<<1],dis[maxn<<1];
    
    int t[2050];
    
    void ade(int a,int b,int c) {
    	to[++cnt]=b;
    	nxt[cnt]=head[a];
    	dis[cnt]=c;
    	head[a]=cnt;
    }
    
    int siz[maxn],SIZ,dp[maxn],root;
    bool use[maxn];
    
    void get_root(int u,int fa) {
    	dp[u]=0,siz[u]=1;
    	for (int i=head[u]; i; i=nxt[i]) {
    		int v=to[i];
    		if (v==fa||use[v]) continue;
    		get_root(v,u);
    		dp[u]=max(dp[u],siz[v]);
    		siz[u]+=siz[v];
    	}
    	dp[u]=max(dp[u],SIZ-siz[u]);
    	if (dp[u]<dp[root]) root=u;
    }
    
    int d[maxn],tail;
    
    void get_dist(int u,int fa,int len) {
    	t[len % 2019]++;
    	d[++tail]=len % 2019;
    	siz[u]=1;
    	for(int i=head[u]; i; i=nxt[i]) {
    		int v=to[i];
    		if(v==fa||use[v]) continue ;
    		get_dist(v,u,(len+dis[i])%2019);
    		siz[u]+=siz[v];
    	}
    }
    
    int ans;
    
    int get_ans(int u,int len,int w) {
    	memset(t, 0, sizeof (t));
    	d[tail=0]=0;
    	get_dist(u,0,len);
    	int res=0;
    	for(int i = 1; i <= 1009; i ++) {
    		res += (t[i] * t[2019 - i] * 2);
    	}
    	res += (t[0] * t[0]);
    	return res;
    }
    
    void solve(int u) {
    	ans+=get_ans(u,0,0);
    	use[u]=1;
    	for(int i=head[u]; i; i=nxt[i]) {
    		int v=to[i];
    		if(use[v]) continue;
    		ans-=get_ans(v,dis[i],-1);
    		root=0,SIZ=siz[v],dp[0]=n;
    		get_root(v,u);
    		solve(root);
    	}
    }
    void init() {
    	cnt = 0;
    	memset(head, 0, sizeof(head));
    	memset(use, 0, sizeof use);
    	ans = 0;
    }
    signed main() {
    	fastio;
    	while(cin>>n) {
    		init();
    		for(int i=1; i<n; i++) {
    			int a,b,c;
    			cin>>a>>b>>c;
    			ade(a,b,c),ade(b,a,c);
    		}
    		dp[0]=SIZ=n,root=0;
    		get_root(1,0);
    		solve(root);
    		cout << (ans - n)/2<< endl;
    	}
    	return 0;
    }
    


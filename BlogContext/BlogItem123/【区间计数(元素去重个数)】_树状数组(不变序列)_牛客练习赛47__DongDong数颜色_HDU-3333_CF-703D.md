## 【区间计数(元素去重个数)】_树状数组(不变序列)_牛客练习赛47__DongDong数颜色_HDU-3333_CF-703D

## 牛客练习赛47 | DongDong数颜色

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190611091344975.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190611091403820.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
HH的项链 进阶版  
这里对一个子树包含的所有节点进行处理  
我们考虑先dfs建序处理成区间问题  
然后 跟HH项链一样 我们离线处理  
优先处理右区间在前的 不断更新 每个颜色下表位置 从而在权值线段树上统计个数  
虽然这里是树状数组写的  
(当然数据可能水了 如果是一个链的话 dfs 3e4 差不多就炸栈了 当然也可能是牛客栈区大)

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    const int maxn = 1e5 + 5;
    
    int n, m;
    int head[maxn], cnt;
    int nxt[maxn << 1], to[maxn << 1];
    
    void ade(int a, int b) {
        to[++cnt] = b;
        nxt[cnt] = head[a];
        head[a] = cnt;
    }
    
    int in[maxn], out[maxn], tot, pos[maxn];
    
    void dfs(int x, int pre) {
        in[x] = ++tot;
        pos[tot] = x;
        for(int i = head[x]; i; i = nxt[i]) {
            if(to[i] == pre)
                continue;
            dfs(to[i], x);
        }
        out[x] = tot;
    }
    
    int col[maxn], vis[maxn], ans[maxn];
    
    struct node {
        int l, r, id;
        bool operator < (const node & a) const {
            return r < a.r;
        }
    } que[maxn];
    
    int c[maxn];
    
    void add(int p, int x) {
        for(; p <= n; p += p & -p)
            c[p] += x;
    }
    
    int getsum(int p) {
        int ret = 0;
        for(; p; p -= p & -p)
            ret += c[p];
        return ret;
    }
    
    int main() {
        cin >> n >> m;
        for(int i = 1; i <= n; ++i)
            cin >> col[i];
    
        for(int i = 2, x, y; i <= n; ++i) {
            cin >> x >> y;
            ade(x, y), ade(y, x);
        }
        dfs(1, -1);
        for(int i = 1, x; i <= m; ++i) {
            cin >> x;
            que[i] = node {in[x], out[x], i};
        }
        sort(que + 1, que + 1 + m);
    
        for(int i = 1, p = 1; i <= m; ++i) {
            while(que[i].r >= p && p <= n) {
                int x = col[pos[p]];
                if(vis[x]) {
                    add(vis[x], -1);
                }
                add(p, 1);
                vis[x] = p;
                p++;
            }
            ans[que[i].id] = getsum(que[i].r) - getsum(que[i].l - 1);
        }
        for(int i = 1; i <= m; ++i) {
            printf("%d\n", ans[i]);
        }
        return 0;
    }
    

HDU 3333  
<http://acm.hdu.edu.cn/showproblem.php?pid=3333>

统计区间不同数据的和  
只要把 -1 +1操作 改成 -a[i] +a[i] 。。。。。

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e5 + 5;
    
    int n, m;
    
    ll c[maxn], a[maxn], ans[maxn];
    map<int, int> vis;
    
    int lowbit(int x) {
    	return x & (-x);
    }
    
    void add(int x, int y) {
    	for(; x <= n; x += lowbit(x))
    		c[x] += y;
    }
    
    ll getsum(int x) {
    	ll res = 0;
    	for(; x; x -= lowbit(x))
    		res += c[x];
    	return res;
    }
    
    struct node {
    	int l, r, id;
    	bool operator < (const node &a) const {
    		return r < a.r;
    	}
    } que[maxn];
    
    int main() {
    	int cas ;
    	cin >> cas;
    	while(cas --) {
    		cin >> n ;
    		vis.clear();
    		memset(c, 0, sizeof c);
    		for(int i = 1; i <= n; i ++)
    			cin >> a[i];
    
    		cin >> m;
    		for(int i = 1, l, r; i <= m; i ++) {
    			cin >> l >> r;
    			que[i] = node {l, r, i};
    		}
    
    		sort(que + 1, que + 1 + m);
    
    		for(int i = 1, p = 1; i <= m; i ++) {
    			while(que[i].r >= p) {
    				if(vis[a[p]]) {
    					add(vis[a[p]], -a[p]);
    					vis[a[p]] = p;
    				}
    				add(p, a[p]);
    				vis[a[p]] = p;
    				p ++;
    			}
    			ans[que[i].id] = getsum(que[i].r) - getsum(que[i].l - 1);
    		}
    
    		for(int i = 1; i <= m; i ++) {
    			cout << ans[i] << endl;
    		}
    	}
    	return 0;
    }
    

Mishka and Interesting sum CodeForces - 703D  
统计区间出现偶数次的异或和  
这道题 我们如果先搞一个 前缀异或和 是不是处理出了 所有技术次数据的异或值

那么我们考虑 类比上面的 我们可以统计 区间不同(只要一个)数据的异或和 那样 又一个奇数次 异或值被我们处理出来  
2次奇数次异或 只会剩下 第二次的数据 这就是偶数次(第一次统计不出来的)数据  
将2次异或和进行处理 我们就可以把偶数次的处理出来

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e6 + 5;
    
    int n, m;
    
    int c[maxn], a[maxn], sum[maxn], ans[maxn];
    unordered_map<int, int> vis;
    
    int lowbit(int x) {
    	return x & (-x);
    }
    
    void add(int x, int y) {
    	for(; x <= n; x += lowbit(x))
    		c[x] ^= y;
    }
    
    int getsum(int x) {
    	int res = 0;
    	for(; x; x -= lowbit(x))
    		res ^= c[x];
    	return res;
    }
    
    struct node {
    	int l, r, id;
    	bool operator < (node const & a) const {
    		return r < a.r;
    	}
    } que[maxn];
    
    int main() {
    //	ios::sync_with_stdio(false);cin.tie(0);
    	scanf("%d",&n); 
    	for(int i = 1; i <= n; i ++)
    		scanf("%d",&a[i]), sum[i] = sum[i - 1] ^ a[i];
    
    	cin >> m;
    	for(int i = 1, l, r; i <= m; i ++) {
    		scanf("%d %d",&l,&r);
    		que[i] = node {l, r, i};
    	}
    
    	sort(que + 1, que + 1 + m);
    
    	for(int i = 1, p = 1; i <= m; i ++) {
    		while(p <= n && que[i].r >= p) {
    			if(vis[a[p]]) {
    				add(vis[a[p]], a[p]);
    				vis[a[p]] = p;
    			}
    			add(p, a[p]);
    			vis[a[p]] = p;
    			p ++ ;
    		}
    		ans[que[i].id] = getsum(que[i].r) ^ getsum(que[i].l - 1) ^ (sum[que[i].r] ^ sum[que[i].l - 1]);
    	}
    
    	for(int i = 1; i <= m; i ++) {
    		printf("%d\n",ans[i]);
    	}
    	return 0;
    }
    


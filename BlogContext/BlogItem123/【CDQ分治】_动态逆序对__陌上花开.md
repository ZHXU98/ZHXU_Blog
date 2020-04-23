## 【CDQ分治】_动态逆序对__陌上花开

~~CDQ 太NB了orz 学不会~~ (2019-8-21更新 重学 算是理解了吧)

## [CQOI2011]动态逆序对

对于序列A，它的逆序对数定义为满足i<j，且Ai>Aj的数对(i,j)的个数。给1到n的一个排列，按照某种顺序依次删除m个元素，你的任务是在每次删除一个元素之前统计整个序列的逆序对数。

输入格式  
输入第一行包含两个整数n和m，即初始元素的个数和删除的元素个数。以下n行每行包含一个1到n之间的正整数，即初始排列。以下m行每行一个正整数，依次为每次删除的元素。

输出格式  
输出包含m行，依次为删除每个元素之前，逆序对的个数。

    
    
    #include <bits/stdc++.h>
    #define int long long
    typedef long long ll;
    using namespace std;
    const int maxn = 100000 + 10;
    const int mod = 1e9 + 7;
    const int INF = 0x3f3f3f3f;
    
    int n, m, k;
    struct node {
    	int pos, time, val;
    } a[maxn], tmp[maxn];
    int p[maxn], ans[maxn];
    
    bool cmp(const node &a, const node &b) {
    	return a.time > b.time;
    }
    
    int c[maxn];
    void add(int x, int y) {
    	while(x <= n) c[x] += y, x += (x & -x);
    }
    void cle(int x) {
    	while(x <= n) c[x] = 0, x += (x & -x);
    }
    int sum(int x) {
    	int res = 0;
    	while(x) res += c[x], x -= (x & -x);
    	return res;
    }
    
    void cdqpre(int l, int r) { // 右侧删得早 树状数组存 位置前面的数据 
    	if(l == r) return ;  // 要再被删之前 比位置前面出现的小 统计前面大于的个数 
    	int mid = l + r >> 1;
    	cdqpre(l, mid), cdqpre(mid + 1, r);
    	int i = l, j = mid + 1, k = l;
    	while(i <= mid && j <= r) {
    		if(a[i].pos < a[j].pos) {
    			add(a[i].val, 1);
    			tmp[k ++] = a[i ++]; 
    		} else {
    			ans[a[j].time] += sum(n) - sum(a[j].val);
    			tmp[k ++] = a[j ++];
    		}
    	}
    	while(i <= mid) tmp[k ++] = a[i ++];
    	while(j <= r) {
    		ans[a[j].time] += sum(n) - sum(a[j].val);
    		tmp[k ++] = a[j ++];
    	}
    	for(int i = l ; i <= mid; i ++) {
    		cle(a[i].val);
    		a[i] = tmp[i];
    	}
    	for(int j = mid + 1; j <= r; j ++) a[j] = tmp[j];
    }
    
    void cdqnxt(int l, int r) { // 同样右侧删得早 我们树状数组存后面位置的数据 
    	if(l == r) return ; // 要再被删之前 比后面位置出现大 统计录进去小的个数 
    	int mid = l + r >> 1;
    	cdqnxt(l, mid), cdqnxt(mid + 1, r);
    	int i = l, j = mid + 1, k = l;
    	while(i <= mid && j <= r) {
    		if(a[i].pos > a[j].pos) {
    			add(a[i].val, 1);
    			tmp[k ++] = a[i ++];
    		} else {
    			ans[a[j].time] += sum(a[j].val - 1);
    			tmp[k ++] = a[j ++];
    		}
    	}
    	while(i <= mid) tmp[k ++] = a[i ++];
    	while(j <= r) {
    		ans[a[j].time] += sum(a[j].val - 1);
    		tmp[k ++] = a[j ++];
    	}
    	for(int i = l; i <= mid; i ++) {
    		cle(a[i].val);
    		a[i] = tmp[i];
    	}
    	for(int j = mid + 1; j <= r; j ++) a[j] = tmp[j];
    }
    
    int ans0() {
    	int a0 = 0;
    	for(int i = n; i >= 1; i --) {
    		int x = a[i].val;
    		a0 += sum(a[i].val - 1);
    		add(x, 1);
    	}
    	memset(c, 0, sizeof(c));
    	return a0;
    }
    
    signed main() {
    	cin >> n >> m;
    	for(int i = 1; i <= n; i ++) {
    		cin >> a[i].val;
    		p[a[i].val] = i;
    		a[i].pos = i;
    		a[i].time = 50000 + 10;
    	}
    	for(int i = 1, t; i <= m; i ++) cin >> t, a[p[t]].time = i;
    	ans[0] = ans0();
    	sort(a + 1, a + 1 + n, cmp);
    	cdqpre(1, n);
    	sort(a + 1, a + 1 + n, cmp);
    	cdqnxt(1, n);
    	cout << ans[0] << endl;
    	for(int i = 1; i < m; i ++) {
    		ans[i] = ans[i - 1] - ans[i];
    		cout << ans[i] << endl;
    	}
    	return 0;
    }
    

## 三维偏序（陌上花开）

https://www.luogu.org/problem/P3810

    
    
    #include <bits/stdc++.h>
    //#define int long long
    typedef long long ll;
    using namespace std;
    const int maxn = 2e5 + 10;
    const int mod = 1e9 + 7;
    const int INF = 0x3f3f3f3f;
    
    int n, m, k;
    
    struct node {
    	int x, y, z, id;
    } a[maxn];
    int p[maxn], ans[maxn], b[maxn];
    
    bool cmp1(const node &a, const node &b) {
    	return a.x < b.x || (a.x == b.x && a.y < b.y) \
    	       || (a.x == b.x && a.y == b.y && a.z < b.z);
    }
    
    bool cmp2(const node &a, const node &b) {
    	return a.y < b.y || (a.y == b.y && a.z < b.z) \
    	       || (a.y == b.y && a.z == b.z && a.x < b.x);
    }
    
    int c[maxn];
    void add(int x, int y) {
    	while(x <= k) c[x] += y, x += (x & -x);
    }
    int sum(int x) {
    	int res = 0;
    	while(x) res += c[x], x -= (x & -x);
    	return res;
    }
    
    void cdq(int l, int r) {
    	if(l == r) return ;
    	int mid = l + r >> 1;
    	cdq(l, mid), cdq(mid + 1, r);
    	sort(a + l, a + r + 1, cmp2);
    	for(int i = l; i <= r; i ++) {
    		if(a[i].x <= mid) add(a[i].z, 1);
    		else b[a[i].id] += sum(a[i].z);
    	}
    	for(int i = l; i <= r; i ++) {
    		if(a[i].x <= mid) add(a[i].z, -1);
    	}
    }
    
    signed main() {
    	cin >> n >> k;
    	for(int i = 1; i <= n; i ++)
    		cin >> a[i].x >> a[i].y >> a[i].z, a[i].id = i;
    
    	sort(a + 1, a + 1 + n, cmp1);
    	for(int i = 1; i <= n; ) {
    		int j = 1 + i;
    		while(j <= n && a[j].x == a[i].x && \
    		        a[j].y == a[i].y && a[j].z == a[i].z) j ++;
    
    		while(i < j) {
    			p[a[i].id] = a[j - 1].id;
    			a[i].x = i;
    			i ++;
    		}
    	}
    	cdq(1, n);
    	for(int i = 1; i <= n; i ++) {
    		ans[b[p[a[i].id]]] ++;
    	}
    	for(int i = 0; i < n; i ++) cout << ans[i] << endl;
    	return 0;
    }
    


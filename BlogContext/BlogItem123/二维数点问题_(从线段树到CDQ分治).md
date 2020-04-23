## 二维数点问题_(从线段树到CDQ分治)

~~ps当然还有的二维树状数组 这里暂时不提~~

## star

<http://acm.hdu.edu.cn/showproblem.php?pid=1541>  
统计 x y 到 0 0 有多少星星  
排序 按x y 升序 排 前面只影响后面 离散化 树状数组 统计  
HDU 星星 这道题 算是简单题 数据范围也没有看 自己在胡诌离散化 处理大矩阵数据了  
找板子题 没有找到 以下代码 带离散化 可以处理的矩阵相当大了

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e5 + 10;
    
    int n, cnt;
    struct node {
    	int x, y;
    	bool operator < (const node& a) const {
    		if(x != a.x) return x < a.x;
    		else return y < a.y;
    	}
    } a[maxn];
    int c[maxn], b[maxn], ans[maxn];
    void add(int x, int y) {
    	for(; x <= n; x += (x & (-x))) c[x] += y;
    }
    
    int sum(int x) {
    	int res = 0;
    	for(; x; x -= (x & (-x))) res += c[x];
    	return res;
    }
    
    int main() {
    	while(cin >> n) {
    		memset(c, 0, sizeof c);
    		memset(ans, 0, sizeof ans);	
    		for(int i = 1; i <= n; i ++)
    			cin >> a[i].x >> a[i].y, b[i] = a[i].y;
    		sort(a + 1, a + 1 + n);
    		sort(b + 1, b + 1 + n);
    		int cnt = unique(b + 1, b + 1 + n) - b - 1;
    		int now;
    		for(int i = 1; i <= n; i ++) {
    			int pos = lower_bound(b + 1, b + 1 + cnt, a[i].y) - b;
    			now = sum(pos + 1);
    			ans[now] ++;
    			add(pos + 1, 1);
    		}
    		for(int i = 0; i < n; i ++) {
    			printf("%d\n", ans[i]);
    		}
    	}
    	return 0;
    }
    

## P2163 [SHOI2007]园丁的烦恼

本来想着二维数组 然后不对劲啊  
CDQ分治嚎啊  
如果你理解的CDQ 分治 这题并不难  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190821102755942.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 3e6 + 10;
    const int INF = 0x3f3f3f3f;
    typedef pair<int, int> P;
    
    int n, m, cnt;
    struct node {
    	int x, y, op, id;
    	bool operator < (const node &b) const {
    		return x == b.x ? (y == b.y ? op < b.op : y < b.y) : x < b.x;
    	}
    } que[maxn], tmp[maxn];
    int ans[maxn];
    void cdq(int L, int R) {
    	if(L == R) return ;
    	int mid = L + R >> 1;
    	cdq(L, mid), cdq(mid + 1, R);
    	int l = L, r = mid + 1, pos = L, tot = 0;
    	while(l <= mid && r <= R) {
    		if(que[l].y <= que[r].y)
    			tot += !que[l].op, tmp[pos ++] = que[l ++];
    		else
    			ans[que[r].id] += tot, tmp[pos ++] = que[r ++];
    	}
    	while(l <= mid) tmp[pos ++] = que[l ++];
    	while(r <= R) ans[que[r].id] += tot, tmp[pos ++] = que[r ++];
    	for(int i = L; i <= R; i ++) que[i] = tmp[i];
    }
    
    int main() {
    	scanf("%d %d", &n, &m);
    	for(int i = 1, x, y; i <= n; i ++) {
    		scanf("%d %d", &x, &y);
    		que[++ cnt] = node {x, y, 0, 0};
    	}
    	int tot = 0;
    	for(int i = 1, a, b, c, d; i <= m; i ++) {
    		scanf("%d %d %d %d", &a, &b, &c, &d);
    		que[++ cnt] = node {c, d, 1, ++ tot};
    		que[++ cnt] = node {c, b - 1, 1, ++ tot};
    		que[++ cnt] = node {a - 1, d, 1, ++ tot};
    		que[++ cnt] = node {a - 1, b - 1, 1, ++ tot};
    	}
    	sort(que + 1, que + 1 + cnt);
    	cdq(1, cnt);
    	for(int i = 1; i + 3 <= tot; i += 4)
    		printf("%d\n", ans[i] - ans[i + 1] - ans[i + 2] + ans[i + 3]);
    	return 0;
    }
    

## CDQ 陌上花开 动态逆序对

这里是三维偏序  
<https://blog.csdn.net/qq_40831340/article/details/99587234>

## P4390 [BOI2007]Mokia 摩基亚

依然是三维偏序 注意坐标 要加一 题目给的是 在格子中数量

    
    
    #include<bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 4e6 + 10;
    const int INF = 0x3f3f3f3f;
    typedef pair<int, int> P;
    
    int n, m, cnt;
    struct node { // 1是询问 ; 0 是需改
    	int x, y, z, op, id, val;
    } que[maxn], tmp[maxn];
    int ans[maxn], c[maxn];
    
    bool cmp(const node &a, const node &b) {
    	if(a.x != b.x) return a.x < b.x;
    	if(a.y != b.y) return a.y < b.y;
    	if(a.z != b.z) return a.z < b.z;
    	return a.op < b.op;
    }
    
    void add(int x, int y) {
    	for(; x <= n; x += (x & (-x))) c[x] += y;
    }
    
    void add(int x) {
    	for(; x <= n; x += (x & (-x))) c[x] = 0;
    }
    
    int sum(int x) {
    	int res = 0;
    	for(; x; x -= (x & (-x))) res += c[x];
    	return res;
    }
    
    void cdq(int L, int R) {
    	if(L == R) return ;
    	int mid = L + R >> 1;
    	cdq(L, mid), cdq(mid + 1, R);
    	int l = L, r = mid + 1, pos = L, tot = 0;
    	while(l <= mid && r <= R) {
    		if(que[l].y <= que[r].y) {
    			if(!que[l].op) add(que[l].z, que[l].val);
    			tmp[pos ++] = que[l ++];
    		} else {
    			if(que[r].op) ans[que[r].id] += sum(que[r].z);
    			tmp[pos ++] = que[r ++];
    		}
    	}
    	while(r <= R) {
    		if(que[r].op) ans[que[r].id] += sum(que[r].z);
    		tmp[pos ++] = que[r ++];
    	}
    	while(l <= mid) {
    		if(!que[l].op) add(que[l].z, que[l].val);
    		tmp[pos ++] = que[l ++];
    	}
    
    	for(int i = L; i <= R; i ++) {
    		if(i <= mid && !que[i].op) add(que[i].z);
    		que[i] = tmp[i];
    	}
    }
    
    int main() {
    	scanf("%d %d", &m, &n);
    	++ n;
    	int tot = 0, tim = 0;
    	int x1, y1, x2, y2, val, op;
    	while(scanf("%d", &op) && op != 3) {
    		if(op == 1) {
    			tim ++;
    			scanf("%d %d %d", &x1, &y1, &val);
    			que[++ cnt] = node {x1 + 1, y1 + 1, tim, 0, 0, val};
    		} else {
    			scanf("%d %d %d %d", &x1, &y1, &x2, &y2);
    			x2 += 1, y2 += 1;
    			que[++ cnt] = node {x2, y2, tim, 1, ++tot, 0};
    			que[++ cnt] = node {x2, y1, tim, 1, ++tot, 0};
    			que[++ cnt] = node {x1, y2, tim, 1, ++tot, 0};
    			que[++ cnt] = node {x1, y1, tim, 1, ++tot, 0};
    		}
    	}
    	n = tim;
    	sort(que + 1, que + 1 + cnt, cmp);
    	cdq(1, cnt);
    	for(int i = 1; i + 3 <= tot; i += 4)
    		printf("%d\n", ans[i] - ans[i + 1] - ans[i + 2] + ans[i + 3]);
    	return 0;
    }
    


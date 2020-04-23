##
【线段树】_CH4301_Can_you_answer_on_these_queries_III__2019南昌网络赛_I_Max_answer__Interval_GCD_CH4302

<https://www.acwing.com/problem/content/246/>

CH4301 Can you answer on these queries III  
这题改了好久啊

问 区间子段和 最大  
显然一开始我们分成 lmax rmax lrmax 和 sum 一开始想的还行  
后面查询就是蛋疼的开始

如同我注释的那个地方一样  
如果 我们直接返回 lrmax 我们就丢掉了一些信息  
这个区间内 我们只存储 lrmax 比如如果左区间递归到最底层 但是 你右区间 没有 可以说是在根节点下一层 但是这节点 是 2 而 左节点是 1
我们却只返回的最大值  
当然 我们这里不能返回和 因为这lrnax区间 题目要求是连续的 而你 lrmax 只维护了本区间 连续 和其他区间是不一定连续的

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    const int maxn = 5e5+10;
    int n, m;
    
    int sum[maxn << 2], lmax[maxn << 2], rmax[maxn << 2], lrmax[maxn << 2];
    
    void push_up(int rt) {
    	sum[rt] = sum[rt << 1] + sum[rt << 1 | 1];
    	lmax[rt] = max(lmax[rt << 1], sum[rt << 1] + lmax[rt << 1 | 1]);
    	rmax[rt] = max(rmax[rt << 1 | 1], sum[rt << 1 | 1] + rmax[rt << 1]);
    	lrmax[rt] = max( max(lrmax[rt << 1], lrmax[rt << 1 | 1]), rmax[rt << 1] + lmax[rt << 1 | 1]);
    }
    
    void update(int L, int l, int r, int rt, int c) {
    	if(l == r) {
    		lrmax[rt] = lmax[rt] = rmax[rt] = sum[rt] = c;
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) update(L, l, mid, rt << 1, c);
    	else update(L, mid + 1, r, rt << 1 | 1, c);
    	push_up(rt);
    }
    
    //int query(int L, int R, int l, int r, int rt) {
    //	if(L <= l && r <= R) {
    //		return lrmax[rt];
    //	}
    //	int mid = l + r >> 1;
    //	int res = -0x3f3f3f3f;
    //	if(L <= mid) res = max(res, query(L, R, l, mid, rt << 1));
    //	if(R > mid) res = max(res, query(L, R, mid + 1, r, rt << 1 | 1));
    //	return res;
    //}
    
    struct node {
    	int sum;
    	int lmax, rmax, lrmax;
    };
    
    node query(int L, int R, int l, int r, int rt) {
    	if(L <= l && r <= R) {
    		return node {sum[rt], lmax[rt], rmax[rt], lrmax[rt]};
    	}
    	//if(l == r) return node {sum[rt], lmax[rt], rmax[rt], lrmax[rt]};
    	int mid = l + r >> 1;
    	if (R <= mid) return query(L, R, l, mid, rt << 1);
    	else if (L > mid) return query(L, R, mid + 1, r, rt << 1 | 1);
    	else {
    		node res;
    		node re1 = query (L, R, l, mid, rt << 1);
    		node re2 = query (L, R, mid + 1, r, rt << 1 | 1);
    		res.sum = re1.sum + re2.sum;
    		res.lmax = max(re1.lmax, re1.sum + re2.lmax);
    		res.rmax = max(re2.rmax, re2.sum + re1.rmax);
    		res.lrmax = max( max(re1.lrmax, re2.lrmax), re1.rmax + re2.lmax);
    		return res;
    	}
    }
    
    int main() {
    	fastio;
    	cin >> n >> m;
    	int k;
    	for(int i = 1; i <= n; i ++) {
    		cin >> k;
    		update(i, 1, n, 1, k);
    	}
    	while(m--) {
    		int cmd, x ,y;
    		cin >> cmd >> x >> y;
    		if(cmd == 1) {
    			if(x > y) swap (x, y);
    			cout << query(x, y, 1, n, 1).lrmax << endl;
    		} else {
    			update(x, 1, n, 1, y);
    		}
    	}
    	return 0;
    }
    

**南昌网络赛**  
<https://blog.csdn.net/qq_40831340/article/details/89449486>  
I Max answer  
Alice has a magic array. She suggests that the value of a interval is equal to
the sum of the values in the interval, multiplied by the smallest value in the
interval.  
Now she is planning to find the max value of the intervals in her array. Can
you help her?

给你一个长度n的序列 选取一个数字 和(包含它连续的)区间(而且它必须是这个区间的最小值) 使a[i]*(这一区间的和) 最大  
首先单调栈 处理每个a[ i ] 管理区间(没有比它小的最大连续区间)  
然后 对 a[ i ] 分2种情况 正负来考虑

  1. 大于 0 的情况 显然是 它 管理的区间和 *a[ i ] 因为a[i]>0 同时它肯定是这区间最低点 该区间越长和越大
  2. 小于 0 的情况 不妨我们维护一个 前缀和 既然它是负数 我们 当前i 到R[ i ]前缀和最好越小越好 而L[ i ] 到 i得和越大越好

    
    
    #include<bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
    //#define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f ;
    const int maxn = 5e5 + 5 ;
    
    int n;
    ll a[maxn],sum[maxn]; 
    int L[maxn],R[maxn];
    
    struct node {
    	ll minsum,maxsum;
    } tr[maxn<<4];
    
    void pushup(int rt) {
    	tr[rt].minsum=min(tr[rt<<1].minsum,tr[rt<<1|1].minsum);
    	tr[rt].maxsum=max(tr[rt<<1].maxsum,tr[rt<<1|1].maxsum);
    }
    
    void build(int l,int r,int rt) {
    	if(l==r) {
    		tr[rt]={sum[l],sum[l]};
    		return ;
    	}
    	int mid=(l+r)>>1;
    	build(l,mid,rt<<1);
    	build(mid+1,r,rt<<1|1);
    	pushup(rt);
    }
    
    ll maxquery(int L,int R,int l,int r,int rt){
    	if(L<=l&&r<=R){
    		return tr[rt].maxsum;
    	}
    	int mid=(l+r)>>1;
    	ll res=-INF;
    	if(L<=mid) res=max(res,maxquery(L,R,l,mid,rt<<1));
    	if(R>mid) res=max(res,maxquery(L,R,mid+1,r,rt<<1|1));
    	return res;
    }
    
    ll minquery(int L,int R,int l,int r,int rt){
    	if(L<=l&&r<=R){
    		return tr[rt].minsum;
    	}
    	int mid=(l+r)>>1;
    	ll res=INF;
    	if(L<=mid) res=min(res,minquery(L,R,l,mid,rt<<1));
    	if(R>mid) res=min(res,minquery(L,R,mid+1,r,rt<<1|1));
    	return res;
    }
    
    void solLR() {
    	stack<int> s;
    	for(int i=1; i<=n; i++) {
    		while(!s.empty()&&a[s.top()]>=a[i]) s.pop();
    		L[i]=(s.empty())?0:s.top();
    		s.push(i);
    	}
    	while(!s.empty()) s.pop();
    	for(int i=n; i>=1; i--) {
    		while(!s.empty()&&a[s.top()]>=a[i]) s.pop();
    		R[i]=(s.empty())?(n+1):s.top();
    		s.push(i);
    	}
    }
    
    signed main() {
    	scanf("%lld",&n);
    	for(int i=1; i<=n; i++) scanf("%lld",&a[i]),sum[i]=sum[i-1]+a[i];
    	solLR();
    	build(1,n,1);
    	ll ans=-INF;
    	ll rmin,lmax;
    	for(int i=1; i<=n; i++) {
    		if(a[i]<0) {
    			rmin=minquery(i,R[i]-1,1,n,1);
    			lmax=maxquery(L[i]+1,i,1,n,1);
    			ans=max(ans,a[i]*(rmin-lmax));
    		} else {
    			ans=max(ans,a[i]*(sum[R[i]-1]-sum[L[i]]));
    		}
    	}
    	printf("%lld\n",ans);
    	return 0;
    }
    

**Interval GCD CH4302**

这题真实跪了 l > n 越界这情况也是绝了 看题解才改过这地方orz

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    
    const int maxn = 5e5+10;
    int n, m;
    int a[maxn],b[maxn],c[maxn];
    
    struct node {
    	int val;
    } tree[maxn << 2];
    
    int gcd(int a, int b) {
    	return b == 0 ? a : gcd (b, a%b);
    }
    
    void push_up(int rt) {
    	tree[rt].val = gcd(tree[rt << 1].val, tree[rt << 1 | 1].val);
    }
    
    void build(int l, int r, int rt) {
    	if(l == r) {
    		tree[rt].val = b[l];
    		return ;
    	}
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1);
    	build(mid + 1, r, rt << 1 | 1);
    	push_up(rt);
    }
    
    void update(int L, int l, int r, int rt, int c) {
    	if(L > n) return ; // wc 这里什么鬼啊 越界到什么一个位置了  
    //  这里 L > n 的话  最后一个点 差分 被 暴力改掉了  导致gcd错误 
    //  这bug真实难找orz 
    	if(l == r) {
    		tree[rt].val += c;
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) update(L, l, mid, rt << 1, c);
    	else update(L, mid + 1, r, rt << 1 | 1, c);
    	push_up(rt);
    }
    
    int query(int L, int R, int l, int r, int rt) {
    	if(L <= l && r <= R) {
    		return abs(tree[rt].val);
    	}
    	int mid = l + r >> 1;
    	int x = 0, y = 0;
    	if(L <= mid) x=query(L, R, l, mid, rt << 1);
    	if(R > mid) y=query(L, R, mid + 1, r, rt << 1 | 1);
    	return gcd(x, y);
    }
    
    int ask(int x) {
    	int res=0;
    	for(; x; x -= (-x) & x) {
    		res += c[x];
    	}
    	return res;
    }
    
    int add(int x, int y) {
    	for(; x <= n; x += (-x) & x ) {
    		c[x] += y;
    	}
    }
    
    signed main() {
    	fastio; 
    	cin >> n >> m;
    	for (int i = 1; i <= n; i ++ ) {
    		cin >> a[i];
    		b[i] = a[i] - a[i - 1];
    	}
    	build(1, n, 1);
    	string cmd;
    	int l, r, d;
    	for(int i = 1; i <= m ; i ++ ) {
    		cin >> cmd >> l >> r;
    		if(cmd[0] == 'C') {
    			cin >> d;
    			update(l, 1, n, 1, d);
    			update(r + 1, 1, n, 1, -d);
    			add(l, d);
    			add(r + 1, -d);
    		} else {
    			cout << gcd(a[l] + ask(l), query(l + 1, r, 1, n, 1)) << endl;
    		}
    	}
    	return 0;
    }
    


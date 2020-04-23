## 2019HDU杭电多校第三场_HDU_6606_Distribution_of_books_(DP+线段树)

> 题目要使得最大值最小？考虑二分答案 对于每次二分答案（假设为x），  
>  如何判定x能否满足分为k份的要求呢？考虑动态规划  
>  dp[i] = max(dp[j]) + 1; (sum[i] - sum[j] <= x)  
>  令dp[i]表示前i个数最多能分成几段, 则 如果直接dp，时间复杂度为n^2，显然会TLE!  
>  考虑用平衡树维护或者离散化后权值线段树维护，总体复杂度n*log(n)

考虑一个答案，然后dp[i]表示分配前i本书最多可以分给几个人，然后如果无法以i结尾就不更新dp数组  
然后把前缀和离散化，dp[i]=max(dp[j]+1),sum[i]-sum[j]<=mid ，那么就用sum[j]>=sum[i]-mid，  
二分出最小的离散化值，权值树状数组维护下标表示的sum[j]能得到的最大dp[j]。  
然后注意由于是要前i个完全分配，所以一定要有解的情况才更新dp[i]和权值树  
线段树优化DP orz chklonglong和 int 没有对应 然后我连样例都跑不出来

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 2e5 + 5;
    const int INF = 0x3f3f3f3f;
    const long long LINF = 0x3f3f3f3f3f3f3f3f;
    typedef pair<int, int> P;
    
    int cas, n, k;
    int a[maxn];
    ll sum[maxn], b[maxn], cnt;
    
    int tree[maxn << 2];
    void build(int l, int r, int rt) {
    	tree[rt] = 0;
    	if(l == r) return ;
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1);
    	build(mid + 1, r, rt << 1 | 1);
    }
    
    void updata(int L, int l, int r, int rt, int val) {
    	if(l == r) {
    		tree[rt] = max(val, tree[rt]);
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, l, mid, rt << 1, val);
    	else updata(L, mid + 1, r, rt << 1 | 1, val);
    	tree[rt] = max(tree[rt << 1], tree[rt << 1 | 1]);
    }
    
    int query(int L, int R, int l ,int r, int rt) {
    	if(L <= l && R >= r) return tree[rt];
    	int mid = l + r >> 1;
    	int res = 0;
    	if(L <= mid) res = query(L, R, l, mid, rt << 1);
    	if(R > mid) res = max(res, query(L, R, mid + 1, r, rt << 1 | 1));
    	return res;
    }
    
    int dp[maxn];
    bool chk(long long mid) {
    	build(1, cnt, 1);
    	int f = 0;
    	for(int i = 1; i <= n; i ++) dp[i] = 0;
    	for(int i = 1; i <= n; i ++) {
    		int pos = lower_bound(b + 1, b + 1 + cnt, b[sum[i]]-mid)-b; // sum[j] 的位置 pos - cnt
    		if(b[sum[i]] <= mid) dp[i] = 1; // 默认 刚开始更新不了同时单独一个符合的也是一个人的
    		int x = query(pos, cnt, 1, cnt, 1);
    		if(x) dp[i] = x + 1;  // 拿之前同时符合位置的 最大值 + 1
    		f = max(f, dp[i]);
    		updata(sum[i], 1, cnt, 1, dp[i]); // sum[i] 更新 如果sum[i] 这个位置之前有更大的不更新
    	}
    	return f >= k;
    }
    
    int main() {
    	scanf("%d", &cas);
    	while(cas --) {
    		scanf("%d %d", &n, &k);
    		for(int i = 1; i <= n; i ++)
    			scanf("%d", a + i), sum[i] = sum[i - 1] + a[i], b[i] = sum[i];
    		sort(b + 1, b + 1 + n);
    		cnt = unique(b + 1, b + 1 + n) - b - 1;
    		for(int i = 1; i <= n; i ++)
    			sum[i] = lower_bound(b + 1, b + 1 + cnt, sum[i]) - b;
    
    		ll l = -1e14-5, r = 1e14+5;
    		while(l < r) {
    			ll mid = l + r >> 1;
    			if(chk(mid)) r=mid;
    			else l=mid+1;
    		}
    		cout << l << endl;
    	}
    	return 0;
    }
    


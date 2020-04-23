## 2019HDU多校第六场_6635_Nonsense_Time_(LIS_记录路径)

    
    
    Problem Description
    You a given a permutation p1,p2,…,pn of size n. Initially, all elements in p are frozen. There will be n stages that these elements will become available one by one. On stage i, the element pki will become available.
    
    For each i, find the longest increasing subsequence among available elements after the first i stages.
     
    
    Input
    The first line of the input contains an integer T(1≤T≤3), denoting the number of test cases.
    
    In each test case, there is one integer n(1≤n≤50000) in the first line, denoting the size of permutation.
    
    In the second line, there are n distinct integers p1,p2,...,pn(1≤pi≤n), denoting the permutation.
    
    In the third line, there are n distinct integers k1,k2,...,kn(1≤ki≤n), describing each stage.
    
    It is guaranteed that p1,p2,...,pn and k1,k2,...,kn are generated randomly.
     
    
    Output
    For each test case, print a single line containing n integers, where the i-th integer denotes the length of the longest increasing subsequence among available elements after the first i stages.
     
    
    Sample Input
    1
    5
    2 5 3 1 4
    1 4 5 3 2
     
    
    Sample Output
    1 1 2 3 3
    

题意概  
一开始数组锁了 每次我们每次选择 一个位置解放 问最长上升长度

这样 其实 我们只要倒着来 每次锁一个数 看看他在不在 之前lis里面 在的重新求一次  
因为这题是随机数据 没有卡 暴力过了。。。、

以下 去掉vis 就是正常求LIS 的板子了

    
    
    #include <bits/stdc++.h>
    const int maxn = 5e4 + 5;
    using namespace std;
    int a[maxn], b[maxn], c[maxn], d[maxn];
    bool vis[maxn];
    int used[maxn], path[maxn], n;
    int LIS() { // d[]存LIS， a[]原数组
    	int len = 0;
    	for (int i = 1; i <= n; i ++) {
    		if(vis[a[i]]) continue; //
    		int it = lower_bound(d, d + len, a[i]) - d;
    		if (it == len) {
    			d[len++] = a[i];
    			path[i] = len;
    		} else {
    			d[it] = a[i];
    			path[i] = it + 1;
    		}
    	}
    	fill(used, used + n + 1, 0);
    	int tmp = len;
    	for (int i = n; i >= 1; --i) {
    		if(vis[a[i]]) continue;//
    		if (path[i] == tmp) used[i] = 1, tmp--;
    	}// 倒序打印也行 少个used
    	return len;
    }
    int main() {
        ios::sync_with_stdio(false);
        cin.tie(0), cout.tie(0);
        int T;
        scanf("%d", &T);
        while (T--) {
            scanf("%d", &n);
            fill(vis, vis+n+1, 0);
            for (int i = 1; i <= n; ++i) scanf("%d", &a[i]);
            for (int i = 1; i <= n; ++i) scanf("%d", &b[i]);
            c[n] = LIS();
            for (int i = n-1; i >= 1; --i) {
            	vis[a[b[i+1]]] = -1;
                if (used[a[b[i+1]]] == 0) c[i] = c[i+1];
                else c[i] = LIS();
            }
            for (int i = 1; i <= n; ++i) {
            	if (i > 1) printf(" ");
            	printf("%d", c[i]);
            }
            puts("");
        }
        return 0;
    }
    


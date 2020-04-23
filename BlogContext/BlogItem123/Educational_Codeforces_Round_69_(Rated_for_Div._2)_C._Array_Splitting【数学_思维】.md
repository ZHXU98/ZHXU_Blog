##
Educational_Codeforces_Round_69_(Rated_for_Div._2)_C._Array_Splitting【数学_思维】

~~待续  
之后写~~

代数化简  
ar1−al1+ar2−al2+......ark−alka_{r1} - a_{l1} + a_{r2} - a_{l2} +...... a_{rk}
- a_{lk}ar1​−al1​+ar2​−al2​+......ark​−alk​ 是我们要求得得 如何使他最小  
我们通过移项  
ar1−al2+ar2−al3+.......+ark−al1a_{r1} - a_{l2} + a_{r2} - a_{l3} + ....... +
a_{rk} - a_{l1}ar1​−al2​+ar2​−al3​+.......+ark​−al1​  
l2,r1l2 ,r1l2,r1 是相邻得 这就变更成了找 差分数组中 分为k段 需要k-1个断点  
最小值（正常来讲他们都是负数 相反数中得最大值） k - 1个部分 + 最小值 - 最大值

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    int cas, n, m , k;
    int a[300005];
    int b[300005];
    signed main() {
    	int i, j;
    	cin >> n >> k;
    	for (i = 0; i < n; i++)
    		cin >> a[i];
    	for (i = 0; i < n; i++)
    		b[i] = a[i] - a[i - 1];
    
    	sort(b, b + n - 1, greater<int>{});
    	int ans = a[n - 1] - a[0];
    	for (i = 0; i < k - 1; i++)
    		ans -= b[i];
    
    	cout << ans << endl;
    
        return 0;
    }
    


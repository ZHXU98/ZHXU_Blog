## Educational_Codeforces_Round_75_(Rated_for_Div._2)_D_-_Salary_Changing_【二分】

## D. Salary Changing

<https://codeforces.com/contest/1251/problem/D>  
题意： 让你在一堆l r 区间都选一个数据 求和 他们的和不大于k 找可能的最大中位数  
二分结果转判定  
自己一直wa的快哭了 直到看到2样例  
我的想法是 二分mid中位数 找到可行最大值  
在chk 函数中 我尝试 将R < mid 直接加他的L  
L大于mid的 我也是直接加他的 L  
包含mid的 我直接加的是mid 后来看到样例2 才知道直接错在哪里了  
全取mid 可能取超过k 我们只要 mid成为中位数就好 取到k就可以了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define A first
    #define B second
    const int maxn = 2e5 + 10;
    
    int cas, n;
    
    typedef pair<long long, long long> pll;
    
    pll a[maxn];
    long long k;
    
    bool chk(long long mid) {
        long long tmp = 0;
        int pos = n / 2 + 1;
        for(int i = 1; i <= n; i ++) {
            if(a[i].B >= mid && pos) {
                tmp += max(mid, a[i].A);
                pos --;
            } else
                tmp += a[i].A;
        }
        return tmp <= k && !pos;
    }
    
    int main() {
        cin >> cas;
        while(cas --) {
            cin >> n >> k;
            for(int i = 1; i <= n; i ++) {
                cin >> a[i].A >> a[i].B;
            }
            sort(a + 1, a + 1 + n, greater<pll>{});
            long long l = 0, r = 1e9 + 1;
            while(l < r) {
                long long mid = l + r + 1 >> 1;
                if(chk(mid))
                    l = mid;
                else
                    r = mid - 1;
            }
            cout << l << endl;
        }
        return 0;
    }
    
    


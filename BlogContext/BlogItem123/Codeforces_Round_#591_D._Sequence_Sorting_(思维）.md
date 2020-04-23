## Codeforces_Round_#591_D._Sequence_Sorting_(思维）

## D. Sequence Sorting

<https://codeforces.com/contest/1241/problem/D>  
给了你一个序列 对一类数据可全部放在前面or 最后面  
问最少次数让它 递增  
对于一类数据 把它放前面 or 放后面 肯定是因为它破坏了 当前某个连续序列的单调性 所以我们考虑找到一个最长的存在上升序列 把破坏它的 一类数 进行操作
就保证了最小

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 3e5 + 10;
    
    int cas, n;
    int a[maxn], b[maxn];
    int mis[maxn], mas[maxn];
    int dp[maxn];
    
    int main() {
        cin >> cas;
        while(cas --) {
            cin >> n;
            memset(mas, -1, (n + 5) * sizeof(int));
            memset(mis, 0x3f, (n + 5) * sizeof(int));
            for(int i = 1; i <= n; i ++) {
                cin >> a[i];
                b[i] = a[i];
                mis[a[i]] = min(i, mis[a[i]]);
                mas[a[i]] = max(i, mas[a[i]]);
            }
            sort(b + 1, b + 1 + n);
            int len = unique(b + 1, b + 1 + n) - b - 1;
            dp[1] = 1;
            int ans = 1;
            for(int i = 2; i <= len; i ++) {
           //     cout << b[i] << " " << b[i - 1]  << endl;
            //    cout << mis[b[i]] << "  " << mas[b[i - 1]] << endl;
                if(mis[b[i]] > mas[b[i - 1]]) dp[i] = dp[i - 1] + 1;
                else dp[i] = 1;
                ans = max(dp[i], ans);
             //   cout << ans << endl;
            }
            cout << len - ans << endl;
        }
        return 0;
    }
    


## Codeforces_Round_#597_(Div._2)_C._Constanze's_Machine_【数学】

## C. Constanze’s Machine

<https://codeforces.com/contest/1245/problem/C>  
题意 ：  
给了你一个string 这次有个人强行将其中的w 和 m 字符 改成了 uu 和 nn  
你的任务是这个序列原来是什么 只需要统计原来序列的可能方案数就好

如果遇到2个连续的 u∣∣vu||vu∣∣v 显然类似与斐波那契数列 dp[i]=dp[i−1]+dp[i−2]dp[i] = dp[i - 1] +
dp[i - 2]dp[i]=dp[i−1]+dp[i−2]  
加上改变的方案数 和 不改变的

    
    
    #include <bits/stdc++.h>
    using namespace std;
    #define int long long
    const int maxn = 2e5 + 5;
    const int mod = 1e9 + 7;
     
    string str;
    int ans = 0;
    int dp[maxn];
     
    signed main() {
        cin >> str;
        dp[0] = dp[1] = 1;
        if(str[0] == 'w' || str[0] == 'm') {
            cout << 0 << endl;
            return 0;
        }
        for(int i = 1; i < str.size(); i ++) {
            if(str[i] == 'w' || str[i] == 'm') {
                cout << 0 << endl;
                return 0;
            }
            if(str[i] == 'u' || str[i] == 'n') {
                if(str[i - 1] == str[i]) {
                    dp[i + 1] = (dp[i] + dp[i - 1]) % mod;
                } else dp[i + 1] = dp[i];
            } else dp[i + 1] = dp[i];
        }
        cout << dp[str.size()] << endl;
        return 0;
    }
    


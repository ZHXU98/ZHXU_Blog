## Educational_Codeforces_Round_70_(Rated_for_Div._2)

这教育场怎么这么难啊  
<https://codeforces.com/contest/1202>

## A. You Are Given Two Binary Strings…

给你01串 x+y∗2kx + y * 2 ^ kx+y∗2k 找到让他们结果的reverse 字典序最小  
二进制 * 2 ^ k 就是左移K位 让他们反转最小的话 肯定相办法让 y 能搞出的0 尽可能靠前 也就是 y 的最后一个1 应该和 我们末尾对齐之后
往前面找第一个1 这样就能让第一个0出现的尽可能早

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    char s1[maxn], s2[maxn];
     
    int main() {
        int cas;
        scanf("%d", &cas);
        while(cas --) {
            scanf("%s", s1 + 1);
            scanf("%s", s2 + 1);
            int len2 = strlen(s2 + 1);
            int len1 = strlen(s1 + 1);
            reverse(s1 + 1, s1 + len1 + 1);
            reverse(s2 + 1, s2 + len2 + 1);
    //        cout << (s2 + 1) << " " << (s1 + 1) << endl;
            int pos = 0, ans = 0;
            for(int i = 1; i <=len2 ; ++ i) {
                if(s2[i] == '1') {
                    pos = i;
                    break;
                }
            }
            for(int i = pos; i <= len1; ++ i) {
                if(s1[i] == '1') {
                    ans = i;
                    break;
                }
            }
            printf("%d\n", ans - pos);
        }
        return 0;
    }
    

## B. You Are Given a Decimal String…

这题读懂真不容易。。。。  
你只可以对这个东西 + x， + y 问你最少补几个字符搞出来(原序列最短是多少 - 给的残缺的)  
floyd 找这个每个状态的转移代价 如何 贪心 最小值往里加就好了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int INF = 0x3f3f3f3f;
    const int maxn = 2e6 + 5;
    char s[maxn];
     
    int dp[15][15];
    int len;
     
    int sol(int x, int y) {
        memset(dp, 0x3f, sizeof(dp));
        for(int i = 0; i < 10; ++ i)
            dp[i][(i + x) % 10] = dp[i][(i + y) % 10] = 1;
        for(int k = 0; k < 10; k ++)
            for(int i = 0; i < 10; i ++)
                for(int j = 0; j < 10; j ++)
                    dp[i][j] = min(dp[i][j], dp[i][k] + dp[k][j]);
        int ans = 0;
        for(int i = 2; i <= len; i ++) {
            if(dp[s[i - 1] - '0'][s[i] - '0'] == INF) return -1;
            ans += dp[s[i - 1] - '0'][s[i] - '0'] - 1;
        }
        return ans;
    }
     
    int main() {
        scanf("%s", s + 1);
        len = strlen(s + 1);
    //    cout << len << endl;
        for(int i = 0; i <= 9; i ++) {
            for(int j = 0; j <= 9; j ++) {
                printf("%d ",sol(i, j));
            }puts("");
        }
        return 0;
    }
    


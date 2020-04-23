## 【区间dp】_Dire_Wolf_HDU_-_5115_2014-北京ICPC

区间DP 算是裸板吧  
这次求的是最小值

题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=5115>;

题目大意：总共有N只狼排成一排，每只狼都有一个基础攻击力ai，以及被击杀后可给别的狼提供的攻击力bi（一只狼被攻击的话，它相邻的狼会为它提供额外的攻击力bi），你击杀一只狼都会减少与这只狼攻击力加上相邻的狼提供的额外的攻击力的和的生命值，问如果要将全部狼都击杀，你最少需要减少多少生命值。

枚举区间长度与起点，求出终点，然后枚举起点与终点之间的点。dp[i][j]为杀死i到j的狼所受到的伤害，然后枚举k作为最后一只被杀死的狼，此时受到的伤害为attack[k]+extra[i-1]+extra[j+1]，得到状态转移方程为：  
dp[i][j] = min(dp[i][j] , dp[i][k - 1] + dp[k + 1][j] + base[k] + extra[i - 1]
+ extra[j + 1]);  
以K为分割点。。

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 205;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1);
    
    int n,m,t;
    int dp[maxn][maxn];
    int attack[maxn];
    int extra[maxn];
    
    int main() {
        scanf("%d",&t);
        int cas=1;
        while(t--){
            scanf("%d",&n);
            for(int i=1;i<=n;i++) scanf("%d",&attack[i]);
            for(int i=1;i<=n;i++) scanf("%d",&extra[i]);
            attack[0]=attack[n+1]=extra[0]=extra[n+1]=0;
    
            for(int i=1;i<=n;i++){
                for(int j=i;j<=n;j++){
                    dp[i][j]=INF;
                }// i从1到n-len 这样每次用k分割更新左到右 区间最小值
            }// 
    
            for(int len=0;len<=n;len++){
                for(int i=1;i<=n-len;i++){
                    int j=i+len;
                    for(int k=i;k<=j;k++){
                        dp[i][j]=min(dp[i][j],dp[i][k-1]+dp[k+1][j]+attack[k]+extra[i-1]+extra[j+1]);
                    }
                }
            }
    
            printf("Case #%d: %d\n",cas++,dp[1][n]);
        }
        return 0;
    }


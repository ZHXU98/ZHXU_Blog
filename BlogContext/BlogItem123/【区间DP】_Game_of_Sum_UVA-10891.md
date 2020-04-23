## 【区间DP】_Game_of_Sum_UVA-10891

题目链接：  
[http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1832](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1832)

题意：  
有n个数字排成一条直线，然后有两个小伙伴来玩游戏， 每个小伙伴每次可以从两端（左或右）中的任意一端取走一个或若干个数（获得价值为取走数之和），
但是他取走的方式一定要让他在游戏结束时价值尽量的高，最头疼的是两个小伙伴都很聪明，所以每一轮两人都将按照对自己最有利的方法去取数字，请你算一下在游戏结束时，先取数的人价值与后取数人价值之差（不要求绝对值）。

预处理前缀和  
每次只能拿最优且在两端取连续个(包括只取一个)  
dp[i,j] 存此区间可取最大值  
在i~j中 找最小的 tmp ->>(dp[i~k] dp[k~j] I<=k&&k<=l )  
sum[I,j]-tmp 便是dp[i~j] 区间最大值

2*dp[1~n]-sum[n] 最大值减最小值

    
    
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
    
    int t,n,m;
    int dp[maxn][maxn];
    int sum[maxn];
    
    int main(){
        while(cin>>n&&n){
            memset(sum,0,sizeof(sum));
            memset(dp,0,sizeof(dp));
            for(int i=1;i<=n;i++) cin>>m,sum[i]=sum[i-1]+m;
    
            int tmp=INF;
    
            for(int len=0;len<=n;len++){
                for(int i=0;i<=n-len;i++){
                    int j=i+len;
                    tmp=INF;
                    for(int k=i;k<=j;k++) tmp=min(tmp,dp[i][k]);
                    for(int k=j;k>=i;k--) tmp=min(tmp,dp[k][j]);
                    dp[i][j]=sum[j]-sum[i-1]-tmp;
                }
            }
    
            cout<<-sum[n]+2*dp[1][n]<<endl;
        }
        return 0;
    } 


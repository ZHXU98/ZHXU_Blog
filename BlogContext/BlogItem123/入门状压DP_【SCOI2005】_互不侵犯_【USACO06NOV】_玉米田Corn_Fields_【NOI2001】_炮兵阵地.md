## 入门状压DP_【SCOI2005】_互不侵犯_【USACO06NOV】_玉米田Corn_Fields_【NOI2001】_炮兵阵地

先附上 <https://blog.csdn.net/qq_40831340/article/details/81502522>  
TSP问题  
Traveling by Stagecoach POJ - 2686 和 2018年小白月赛4 D-郊区春游题解

## [SCOI2005]互不侵犯

题目描述  
在N×N的棋盘里面放K个国王，使他们互不攻击，共有多少种摆放方案。国王能攻击到它上下左右，以及左上左下右上右下八个方向上附近的各一个格子，共8个格子。

输入输出格式  
输入格式：  
只有一行，包含两个数N，K （ 1 <=N <=9, 0 <= K <= N * N）

输出格式：  
所得的方案数

输入输出样例  
输入样例#1：  
3 2  
输出样例#1：  
16

二进制 先预处理出来不相邻的1 代表国王可以有的状态 就可以减少枚举次数  
同时记录下 这个状态用了多少国王

接下来从第二层开始 dp 首先确保 2层不冲突 就把 上层状态[ p ]+这次放多少 kins[ j ]本次状态可以放多少 不断加下去就好

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    #define int long long
    using namespace std;
    typedef long long ll;
    
    ll dp[15][(1<<9)+5][100];
    int n,m,k;
    int cnt,ans;
    int state[1<<11],king[1<<11];
    
    void init() {
        int tot=(1<<n)-1;
        for(int i=0; i<=tot; i++) {
            if(!((i<<1)&i)) {
                state[++cnt]=i;
                int res=i;
                while(res) {
                    king[cnt]+=res%2;
                    res>>=1;
                }
            }
        }
    }
    
    signed main() {
        fastio;
        cin>>n>>k;
        init();
        for(int i=1; i<=cnt; i++) dp[1][i][king[i]]=1;
        //	if(king[i]<=k) 
        
        for(int i=2;i<=n;i++){
            for(int j=1;j<=cnt;j++){
                for(int p=1;p<=cnt;p++){
                    if(state[j]&state[p]) continue;
                    if(state[j]&(state[p]<<1)) continue;
                    if((state[j]<<1)&state[p]) continue;
                    
                    for(int s=1;s<=k;s++){
                        if(king[j]+s>k) break;
                        dp[i][j][king[j]+s]+=dp[i-1][p][s];
                    }
                    
                }
            }
        }
        int ans=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=cnt;j++)
                ans+=dp[i][j][k]; 
        } 
        cout<<ans<<endl;
        return 0;
    }
    

## [USACO06NOV] 玉米田Corn Fields

农场主John新买了一块长方形的新牧场，这块牧场被划分成M行N列(1 ≤ M ≤ 12; 1 ≤ N ≤
12)，每一格都是一块正方形的土地。John打算在牧场上的某几格里种上美味的草，供他的奶牛们享用。

遗憾的是，有些土地相当贫瘠，不能用来种草。并且，奶牛们喜欢独占一块草地的感觉，于是John不会选择两块相邻的土地，也就是说，没有哪两块草地有公共边。

John想知道，如果不考虑草地的总块数，那么，一共有多少种种植方案可供他选择？（当然，把新牧场完全荒废也是一种方案）

输入输出格式  
输入格式：  
第一行：两个整数M和N，用空格隔开。

第2到第M+1行：每行包含N个用空格隔开的整数，描述了每块土地的状态。第i+1行描述了第i行的土地，所有整数均为0或1，是1的话，表示这块土地足够肥沃，0则表示这块土地不适合种草。

输出格式：  
一个整数，即牧场分配总方案数除以100,000,000的余数。

输入输出样例  
输入样例#1：  
2 3  
1 1 1  
0 1 0  
输出样例#1：  
9

本题与上题大体一样。。。。。。。都不要处理每层cows的个数 仅统计方法总数

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    #define int long long
    using namespace std;
    typedef long long ll;
    
    const long long mod = 1e8;
    
    int n,m,ans;
    
    int mp[15];
    int dp[15][((1<<12)+5)];
    int state[(1<<12)+5],cows[(1<<12)+5];
    int cnt;
    
    void init() {
        int tot=(1<<m)-1;
        for(int i=0; i<=tot; i++) {
            if((i<<1)&i) continue;
            int res=i;
            state[++cnt]=i;
            while(res) {
                cows[cnt]+=res%2;
                res>>=1;
            }
        }
    }
    
    signed main() {
        fastio;
    //	freopen("a.in","r",stdin);
        cin>>n>>m;
        
        for(int i=1;i<=n;i++){
            int res=0,k;
            for(int j=m-1;j>=0;j--){
                cin>>k;
                mp[i]|=(k<<j);
            }
        }
        
        init();
        for(int i=1;i<=cnt;i++)
            if((mp[1]|state[i])!=mp[1]) continue;
            else dp[1][state[i]]=1;
    
        for(int i=2;i<=n;i++){
            for(int j=1;j<=cnt;j++){
                if((mp[i]|state[j])!=mp[i]) continue;
                for(int p=1;p<=cnt;p++){
                    if((mp[i-1]|state[p])!=mp[i-1]) continue;
                    if(state[p]&state[j]) continue;
                    dp[i][state[j]]=(dp[i][state[j]]+dp[i-1][state[p]])%mod;
                }
            }
        }
        
        for(int i=0;i<(1<<m);i++) ans=(ans+dp[n][i])%mod;
        cout<<ans<<endl;
        return 0;
    }
    

## [NOI2001]炮兵阵地

题目描述  
司令部的将军们打算在N _M的网格地图上部署他们的炮兵部队。一个N_ M的地图由N行M列组成，地图的每一格可能是山地（用“H”
表示），也可能是平原（用“P”表示），如下图。在每一格平原地形上最多可以布置一支炮兵部队（山地上不能够部署炮兵部队）；一支炮兵部队在地图上的攻击范围如图中黑色区域所示：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190508130632183.jpg)

如果在地图中的灰色所标识的平原上部署一支炮兵部队，则图中的黑色的网格表示它能够攻击到的区域：沿横向左右各两格，沿纵向上下各两格。图上其它白色网格均攻击不到。从图上可见炮兵的攻击范围不受地形的影响。
现在，将军们规划如何部署炮兵部队，在防止误伤的前提下（保证任何两支炮兵部队之间不能互相攻击，即任何一支炮兵部队都不在其他支炮兵部队的攻击范围内），在整个地图区域内最多能够摆放多少我军的炮兵部队。

输入输出格式  
输入格式：  
第一行包含两个由空格分割开的正整数，分别表示N和M；

接下来的N行，每一行含有连续的M个字符（‘P’或者‘H’），中间没有空格。按顺序表示地图中每一行的数据。N≤100；M≤10。

输出格式：  
仅一行，包含一个整数K，表示最多能摆放的炮兵部队的数量。

输入输出样例  
输入样例#1：  
5 4  
PHPP  
PPHH  
PPPP  
PHPP  
PHHP  
输出样例#1：  
6

这题相比上2道题 多了要处理的 层数  
所以 我们选择 开3位 第一位层数 第二位 前一层的状态 第三位 当前层的状态  
先处理 1 2 层 将炮车数 初始化  
接下来 像前2道题一样 不断枚举每层的状态 和前一层 前2层的状态 判断合法  
最后只要在 最后一层 所有状态中找最大值就好  
(也可以在在dp过程中接着ans=max(ans,dp[ i ][ p ][ j ]));

    
    
    #include <bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0)
    #define int long long
    using namespace std;
    typedef long long ll;
    
    const long long mod = 1e8;
    
    int n,m,ans;
    
    int mp[105];
    int dp[105][((1<<6)+5)][(1<<6)+5];
    int state[(1<<6)+5],pcs[(1<<6)+5];
    int cnt;
    
    void init() {
        int tot=(1<<m)-1;
        for(int i=0; i<=tot; i++) {
            if((i<<1)&i) continue;
            if((i<<2)&i) continue;
            state[++cnt]=i;
            int res=i;
            while(res) {
                pcs[cnt]+=res%2;
                res>>=1;
            }
            if(!(mp[1]&state[cnt])) dp[1][0][cnt]=pcs[cnt];
        }
    
        for(int i=1; i<=cnt; i++) {
            for(int j=1; j<=cnt; j++) {
                if(state[i]&state[j]) continue;
                if(mp[2]&state[j]) continue;
                dp[2][i][j]=max(dp[2][i][j],dp[1][0][i]+pcs[j]);
            }
        }
    }
    
    signed main() {
        fastio;
    //	freopen("a.in","r",stdin);
        cin>>n>>m;
        string str;
        for(int i=1; i<=n; i++) {
            cin>>str;
            int res=0;
            for(int j=0; j<m; j++) {
                if(str[j]=='H') res=res<<1|1;
                else res<<=1;
            }
            mp[i]=res;
        }
        init();
        // wa 这密密麻麻的continue啊 让我ac吧 233333
        for(int i=3; i<=n; i++) {
            for(int j=1; j<=cnt; j++) {
    
                if(mp[i]&state[j]) continue;
    
                for(int p=1; p<=cnt; p++) {
    
                    if(mp[i-1]&state[p]) continue;
                    if(state[j]&state[p]) continue;
    
                    for(int k=1; k<=cnt; k++) {
    
                        if(mp[i-2]&state[k]) continue;
                        if(state[k]&state[j]) continue;
                        if(state[k]&state[p]) continue;
    
                        dp[i][p][j]=max(dp[i-1][k][p]+pcs[j],dp[i][p][j]);
                    }
                }
            }
        }
    
        for(int i=1; i<=cnt; i++)
            for(int j=1; j<=cnt; j++)
                ans=max(ans,dp[n][i][j]);
    
        cout<<ans<<endl;
        return 0;
    }
    


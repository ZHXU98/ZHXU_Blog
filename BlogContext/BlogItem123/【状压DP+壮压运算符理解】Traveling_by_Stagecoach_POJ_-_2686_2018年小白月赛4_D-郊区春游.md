## 【状压DP+壮压运算符理解】Traveling_by_Stagecoach_POJ_-_2686_2018年小白月赛4_D-郊区春游

少量位运算使用理解

    
    
    第 i 位为从右往左从0开始数
    如果要设置 n 的第 i 位为1，n=（n|（1<<i）；                              
    如果要设置 n 的第 i 位为0，n=（n &（~（1<<i））；
    
    & 按位与
    如果两个相应的二进制位都为1，则该位的结果值为1，否则为0
    l 按位或
    两个相应的二进制位中只要有一个为1，该位的结果值为1
    ^  按位异或
    若参加运算的两个二进制位值相同则为0，否则为1
    ~ 取反
    ~是一元运算符，用来对一个二进制数按位取\反，即将0变1，将1变0
    << 左移
    用来将一个数的各二进制位全部左移N位，右补0
    （ >> 右移 
    将一个数的各二进制位右移N位，移到右端 的低位被舍弃，对于无符号数，高位补0

题干

有一个人从某个城市要到另一个城市（点数<=30）  
然后有n个马车票，相邻的两个城市走的话要消耗掉一个马车票。  
花费的时间呢，是马车票上有个速率值，用边/速率是花的时间。  
求最后这个人花费的最短时间是多少

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <queue>
    #include <stack>
    using namespace std;
    typedef long long ll;
    typedef unsigned long long ull;
    
    const int maxn=35;
    const int INF=1e9;
    double dp[1<<10][maxn];
    int mp[maxn][maxn];
    int d[maxn];
    
    
    int main(){
        int t,n,m,src,des;
        while(cin>>t>>n>>m>>src>>des&&(t+n+m+src+des)){
            memset(mp,-1,sizeof(mp));
            for(int i=0;i<t;i++) cin>>d[i];
            for(int i=1;i<=m;i++) {
                int st,ed,dis;
                cin>>st>>ed>>dis;
                if(mp[st][ed]==-1||mp[st][ed]>dis){
                    mp[st][ed]=dis;
                    mp[ed][st]=dis;
                } 
            }
            for(int i=0;i<(1<<9);i++){
                for(int j=0;j<maxn;j++) dp[i][j]=INF;
            }
            dp[0][src]=0;
            double ans=INF;
            for(int i=0;i<(1<<t);i++){//
                for(int u=1;u<=n;u++){
                    for(int k=0;k<t;k++){
                        if(!(i&(1<<k))){//取票不冲突重复
                            for(int j=1;j<=n;j++){
                                if(mp[u][j]!=-1)dp[i|(1<<k)][j]=min(dp[i|(1<<k)][j],dp[i][u]+1.0*mp[u][j]/d[k]);
                            }
                        }
                    }
                }
                ans=min(ans,dp[i][des]);
            }
            if(ans==INF) cout<<"Impossible\n";
            else printf("%.3f\n",ans);//poj %lf wa飞。。。 看discuss 才知道poj坑人。。
        }
        return 0;
    }
    
    

链接：<https://www.nowcoder.com/acm/contest/134/D>  
来源：牛客网

时间限制：C/C++ 1秒，其他语言2秒  
空间限制：C/C++ 262144K，其他语言524288K  
64bit IO Format: %lld

题目描述

今天春天铁子的班上组织了一场春游，在铁子的城市里有n个郊区和m条无向道路，第i条道路连接郊区Ai和Bi，路费是Ci。经过铁子和顺溜的提议，他们决定去其中的R个郊区玩耍（不考虑玩耍的顺序），但是由于他们的班费紧张，所以需要找到一条旅游路线使得他们的花费最少，假设他们制定的旅游路线为V1,
V2 ,V3 …
VR，那么他们的总花费为从V1到V2的花费加上V2到V3的花费依次类推，注意从铁子班上到V1的花费和从VR到铁子班上的花费是不需要考虑的，因为这两段花费由学校报销而且我们也不打算告诉你铁子学校的位置。  
输入描述:  
第一行三个整数n, m, R(2 ≤ n ≤ 200, 1 ≤ m ≤ 5000, 2 ≤ R ≤ min(n,
15))。第二行R个整数表示需要去玩耍的郊区编号。以下m行每行Ai, Bi, Ci(1 ≤ Ai, Bi ≤ n, Ai ≠ Bi, Ci ≤
10000)保证不存在重边。  
输出描述:  
输出一行表示最小的花费

示例1

输入  
4 6 3  
2 3 4  
1 2 4  
2 3 3  
4 3 1  
1 4 1  
4 2 2  
3 1 6  
输出  
3

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <cmath>
    #include <algorithm>
    #include <queue>
    #include <vector>
    using namespace std;
    
    const int maxn = 200 + 5;
    const int INF = 0x3f3f3f3f;
    
    int rs[maxn];
    int mp[maxn][maxn]; 
    int dp[1<<15+2][20];
    int n,m,x,r;
    int st,ed,dis;
    
    int main() {
        while(cin>>n>>m>>r){
            memset(mp,INF,sizeof(mp));
            for(int i=0;i<r;i++) cin>>rs[i];
            for(int i=0;i<m;i++){
                cin>>st>>ed>>dis;
                if(mp[st][ed]>dis){
                    mp[st][ed]=dis;
                    mp[ed][st]=dis;
                }
            }
    
            for(int k=1;k<=n;k++){
                for(int i=1;i<=n;i++){
                    for(int j=1;j<=n;j++){
                        if(mp[i][j]>mp[i][k]+mp[k][j]) mp[i][j]=mp[i][k]+mp[k][j];
                    }
                }
            }
            memset(dp,INF,sizeof(dp));
            for(int i=0;i<r;i++) dp[1<<i][i]=0;// 自己到自己得是0.。。我第一次把第一层全变为0了
        for(int s=1;s<(1<<r);s++)
            for(int i=0;i<r;i++){
                if(!(s&(1<<i)))continue ;// rs第i城市在S中存在 借助的中间点
                for(int j=0;j<r;j++){   
                    if(s&(1<<j))continue ;//  rs第j城市在S不存在 当前要推的点
                    dp[s|(1<<j)][j]=min(dp[s|(1<<j)][j],dp[s][i]+mp[rs[i]][rs[j]]);
                }
            }
            int ans=INF;
            for(int i=0;i<r;i++){
                ans=min(ans,dp[(1<<r)-1][i]);
            }
            cout<<ans<<endl;
        } 
        return 0;
    }


## 【二维费用背包】FATE_HDU_-_2159_(三维+二维优化_版)

acm.hdu.edu.cn/showproblem.php?pid=2159

>
> 题意：现在玩游戏欲升级，升级需要经验值n，杀怪可以赚经验值，但是会扣忍耐度，游戏中有k种怪，数目都无限多。现在玩家还有m点忍耐度，问能否在最多杀s个怪的情况下升级，若能则输出剩余的最大忍耐度。  
>  思路：  
>
> 1、此题有两个约束，一个是忍耐度，一个是最多杀的怪物数。抽象来说意味着对于每件物品，具有两种不同的费用。此即为二维背包的模型！而二维费用背包模型最常见的形式便是：物品总个数有上限限制，如此题的最多杀s个怪。而关于二维背包的处理，无非是增加一个状态维度即可，转移方程类比着选或不选第i个物品的思路写出即可。  
>  2、此题怪物数目无限多，也即这是个完全背包问题。关于完全背包问题，存在O(N*V)时间复杂度的算法（N为物品总数，V为背包容量）， 实现方式即为  
>  f[i,j]=max(f[i-1,j],f[i,j-v[i]]+w[i]) 其中循环变量i遍历物品种类，j正向遍历0~V  
>  注意递推式与01背包有些许不同：  
>  （1）max的第二项是f[i,...]，是i！
>
> （2）j在01背包时是逆序循环的，如今变成了顺序循环。  
>  这样的目的是，该子结果就是已经考虑过重复选第i项的结果了！另外这个i和j如果需要是可以交换循环顺序的。
>
> 当然依然可以空间优化，去除物品种类那一维。而值得注意的是此题需要算的是如果可以升级需输出能剩余的最大忍耐度，故循环时可以将忍耐度那一维放在最外层循环
    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    #include <map>
    #include <queue>
    #include <set>
    #include <stack>
    using namespace std;
    typedef long long ll;
    
    const int maxn_m = 100 + 5;
    const int maxn_n = 100 + 5;
    const int maxn = 2000000+5;
    const int mod = 1e9 + 7;
    const int INF = 0x3f3f3f3f;
    const double PI = acos(-1.0);
    
    int dp[110][110][110];// 种类 耐力 杀敌个数 
    int val[110],w[110];// 经验 耐力 
    int main ()
    {
        int n,k,m,s,ans=INF;
        while(scanf("%d%d%d%d",&n,&m,&k,&s)!=EOF){
            memset(dp,0,sizeof(dp));
            for(int i=1;i<=k;i++)scanf("%d%d",w+i,val+i);
            
            for(int i=1;i<=k;i++)
             for(int j=1;j<=m;j++)// 这里的 把每层都传下去 丢了val[i] 前的wa也没有啥解释的 
              for(int t=1;t<=s;t++){
              	dp[i][j][t]=(j<val[i])?dp[i-1][j][t]:max(dp[i][j-val[i]][t-1]+w[i],dp[i-1][j][t]);
    		   	ans=(dp[i][j][t]>=n)?min(ans,j):ans;
    		  }
                            
            if(dp[k][m][s]<n) printf("-1\n");       
            else printf("%d\n",m-ans);
        	ans=INF;
        }
        return 0;
    }

外带一个 二维优化版

dp[i][k]表示在i忍耐度下杀k只怪所得到的最多经验；

dp[i][k]=max(dp[i][k],dp[i-a[j].w][k-1]+a[j].v)

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    #include <map>
    #include <queue>
    #include <set>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 105;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int dp[maxn][maxn],a[maxn],b[maxn];
    int n,m,k,s;
    
    int main(){
    	while(cin>>n>>m>>k>>s){
    		for(int i=0;i<k;i++) cin>>a[i]>>b[i];
    		memset(dp,0,sizeof(dp));
    		int ans=INF;
    		for(int i=0;i<k;i++){
    			for(int j=b[i];j<=m;j++){
    				for(int t=1;t<=s;t++){
    					dp[j][t]=max(dp[j][t],dp[j-b[i]][t-1]+a[i]);
    					if(dp[j][t]>=n){
    						ans=min(ans,j);
    					}
    				}
    			}
    		}
    		if(ans>m) cout<<-1<<endl;
    		else cout<<m-ans<<endl;
    	}
    	return 0;
    } 


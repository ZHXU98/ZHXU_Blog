## 【DP】_Cow_Exhibition_POJ_-_2184_经典好题

poj.org/problem?id=2184

> 奶牛想证明他们是聪明而风趣的。为此，贝西筹备了一个奶牛博览会，她已经对N头奶 牛进行了面试，确定了每头奶牛的智商和情商。
> 贝西有权选择让哪些奶牛参加展览。由于负的智商或情商会造成负面效果，所以贝西不
> 希望出展奶牛的智商之和小于零，或情商之和小于零。满足这两个条件下，她希望出展奶牛 的智商与情商之和越大越好，请帮助贝西求出这个最大值
>
> Input
>
> 第一行：一个整数N，表示奶牛的数量， 1 ≤ N ≤ 100 第二行到第N + 1行：第i + 1行有两个用空格分开的整数：
> Si和Fi，分别表示第i头奶牛的智商和情商， -1000 ≤ Si ≤ 1000， -1000 ≤ Fi ≤ 1000
>
> Output
>
> 第一行：单个整数，表示情商与智商和的最大值。贝西可以不让任何奶牛参加展览，如果这样应该输出 0

这题算是我更深入学习DP的一个开端吧 orz

<https://blog.csdn.net/keysona/article/details/45751903>[
](https://blog.csdn.net/keysona/article/details/45751903)思路是从这篇博客看来的

因为只有选取和不选取的2个状态 所以dfs 爆tmd搜 大概率应该是tle了

不过只有2状态这也提示了 01背包

首先是可以用IQ或EQ作为背包容量 同时加上 100*1000 使之全部为正

然后 以100*1000 为正负分界点 开始类似于01背包的 DP 但是这里不同的是 正负的处理

还有我们通常01背包都是for j=MAXV->weight[i] 从后向前更新

不过这道题 就选择将初始化 -INF 认为-INF为未被物品覆盖的点 将100*1000 认为是零点

这样 凡减去背包容量到 0点或不是-INF的点 将他的val 更新选取最大的

另外 这道题非常重要的 便是更新 正数的时候for j=MAXV->weight[i]

因为正数时 减去这weight 状态是之前来的 如果从 weight[i]->MAXV 就会导致 变成完全背包

负数时 从0开始 因为它的状态是从较大的数来的 因为weight是负的

如果从 MAXV+weight[i] 开始 就又会变成上面所说的完全背包

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    #include <map>
    #include <queue>
    #include <set>
    #include <stack>
    #include <list> 
    using namespace std;
    typedef long long ll;
    
    const int maxn = 105;
    const int maxv = 100*1000*2+5;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int IQ[maxn],EQ[maxn]; 
    int dp[maxv];
    
    int main(){
    	int n;
    	cin>>n;
    	for(int i=0;i<n;i++) cin>>IQ[i]>>EQ[i];
    	memset(dp,-INF,sizeof(dp));
    	dp[100*1000]=0;
    	for(int i=0;i<n;i++){
    		if(IQ[i]>=0){
    			for(int j=maxv-5;j>=IQ[i];j--)
    				if(dp[j-IQ[i]]>-INF) dp[j]=max(dp[j],dp[j-IQ[i]]+EQ[i]);
    		}
    		else{
    			for(int j=0;j<=maxv+IQ[i]-5;j++)
    				if(dp[j-IQ[i]]>-INF) dp[j]=max(dp[j],dp[j-IQ[i]]+EQ[i]);
    		}
    	}
    	int ans=0;
    	for(int i=100*1000;i<=maxv-5;i++){
    		if(dp[i]>0) ans=max(ans,i-100*1000+dp[i]); 
    	} 
    	cout<<ans<<endl;
    	return 0;
    } 

最后 for 找 EQ+IQ 最大数据

做完这题收获蛮大的 数据处理 和负数这些状态


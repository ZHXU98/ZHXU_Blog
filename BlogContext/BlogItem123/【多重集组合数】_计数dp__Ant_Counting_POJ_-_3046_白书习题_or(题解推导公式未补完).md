## 【多重集组合数】_计数dp__Ant_Counting_POJ_-_3046_白书习题_or(题解推导公式未补完)

poj.org/problem?id=3046

题意：

给出T种数字(蚂蚁)//同数字 同种类 统计时 1 1 2 和 1 2 1 是一样的

每种各有N[i]个 然后用这些数字构成一些序列， 长度在S 到 B 内 组合总数

直接上白书 的多重集组合数 除了用滚动数组减内存

其实我觉得这题比较难理解 书上那个公式具体怎么算的了

dp[ i + 1 ][ j ] =dp[ i + 1 ][ j - 1] + dp[ i ][ j ] - dp[ i ][ j - 1 -ai ]

    
    
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
    
    const int maxn = 1000+5 ;
    const int maxv = 100*1000+5;
    const int mod = 1000000 ;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int dp[2][maxv];
    int used[maxn];
    
    int main(){
    	std::ios::sync_with_stdio(false);
    	int T,A,S,B,x,ans=0;
    	cin>>T>>A>>S>>B;
    	for(int i=1;i<=A;i++){
    		cin>>x;
    		used[x]++;
    	}
    	dp[0][0]=dp[1][0]=1;
    	for(int i=1;i<=T;i++)
    		for(int j=1;j<=B;j++)
    			if(j-used[i]-1>=0)dp[i%2][j]=(dp[(i-1)%2][j]+dp[i%2][j-1]-dp[(i-1)%2][j-used[i]-1]+mod)%mod;
    			else dp[i%2][j]=(dp[(i-1)%2][j]+dp[i%2][j-1])%mod;
    	for(int i=S;i<=B;i++)
    		ans=(ans+dp[T%2][i])%mod;
    	cout<<ans<<endl;
    	return 0;
    } 

之后补上那个 公式 怎么推导

未更完


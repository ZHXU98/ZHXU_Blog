## 【区间DP】_Cheapest_Palindrome_poj-3280_回文串dp

poj.org/problem?id=3280

> 题意：给出一个由m中字母组成的长度为n的串，给出m种字母添加和删除花费的代价，求让给出的串变成回文串的代价。
>
> 分析：我们知道求添加最少的字母让其回文是经典dp问题，转化成LCS求解。这个是一个很明显的区间dp  
>  我们定义dp [ i ] [ j ] 为区间 i 到 j 变成回文的最小代价。  
>  那么对于d[ i ] [ j ]有三种情况  
>  首先：对于一个串如果s[ i ]==s [ j ]，那么dp [ i ] [ j ]=dp[ i+1 ] [ j -1 ] ; //
> 已经对应构成回文串  
>  其次：如果dp [ i+1 ] [ j ] 改 i 变成回文串，那么dp [ i ] [ j ]=dp [ i +1 ] [ j ]+min（add
> [ i ]，del [ i ]）；  
>  最后，如果dp [ i ] [ j-1 ]改 j 变成回文串，那么dp [ i ] [ j ]=dp[ i ] [j-1 ] + min（add [
> j ]，del [ j ]）；
    
    
    ​#include <iostream>
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
    
    const int maxn = 2*1e3+10;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    int v[300];
    int dp[maxn][maxn];
    string str;
    int n,m,x,y;
    char ch;
    
    int main(){
    	cin>>n>>m;
    	cin>>str;
    	memset(v,INF,sizeof(v));
    	for(int i=0;i<n;i++){
    		cin>>ch>>x>>y;
    		v[int(ch)]=min(x,y);
    	}
    	for(int j=1;j<m;j++){
    		for(int i=j-1;i>=0;i--){
    			if(str[i]==str[j]) dp[i][j]=dp[i+1][j-1];
    			else dp[i][j]=min(dp[i+1][j]+v[str[i]],dp[i][j-1]+v[str[j]]);
    		}
    	}
    	cout<<dp[0][m-1]<<endl;
    	
    	return 0;
    } ​


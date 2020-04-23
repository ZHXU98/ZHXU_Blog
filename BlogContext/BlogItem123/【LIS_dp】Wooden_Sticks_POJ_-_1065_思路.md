## 【LIS_dp】Wooden_Sticks_POJ_-_1065_思路

<http://poj.org/problem?id=1065>

> 题意： 锯木机 开机首先要了1分钟 之后据木头 如果木头的长宽均小于等于上一块 就不需要重启 不然重启又花费一分钟
>
> 问最短 花费时间
>
> 思路：看了题解才明白可以转LIS 最长上升子序列
>
> 首先 按照长度 由大到小排序 相同时 按照宽度由大到小排序
>
> 鸽笼原理 形成LIS长度的 递减序列 // 难以想象
    
    
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
    
    const int maxn =5000+5;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    struct node{
    	int h,w;
    	bool operator<(const node &a) const {return (h==a.h&&w>a.w)||h>a.h;}
    }sticks[maxn];
    
    int dp[maxn],a[maxn];
    
    int main(){
    	int t,n;
    	cin>>t;
    	while(t--){
    		memset(dp,INF,sizeof(dp));
    		cin>>n;
    		for(int i=0;i<n;i++) cin>>sticks[i].h>>sticks[i].w;
    		sort(sticks,sticks+n);
    		for(int i=0;i<n;i++) a[i]=sticks[i].w;
    		for(int i=0;i<n;i++)
    			*lower_bound(dp,dp+n,a[i])=a[i];
    	//	for(int i=0;i<n;i++) cout<<a[i]<<"  ";cout<<endl;	
    		printf("%d\n",lower_bound(dp,dp+n,INF)-dp);
    	}
    	return 0;
    } 


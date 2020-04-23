## 【完全背包dp】Space_Elevator_POJ_-_2392

poj.org/problem?id=2392

>
> 有一群奶牛想到太空去，他们有k中类型的石头，每一类石头高h，石头能达到的高度c，以及它的数量a，在做背包前需要对石块能到达的最大高度（a）进行排序，而且每种砖块都有一个限制条件，就是说以该种砖块结束的最大高度H不能超过某个高度，不同砖块的高度不同。求最高的高度是多少。

自己以前写 就是暴力把每个高度用for k->c 塞进数组里面 然后彻底当01背包写 还有书上在dp里层多写个for k->c 一样的道理

记得以前在poj上遇到会超时的题 当时就见到用used的了 不过写这题的时候并没有记住orz

现在想想多个 used数组 同样都是转01背包带个个数限制 不过更快了 现在算是彻底记下了

    
    
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
    
    const int maxn = 400+5;
    const int maxv = 400*100+5;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    
    bool dp[maxv];
    int used[maxv]; // 比起再写一个 for k->c 显然用这个节约了时间 
    
    struct node{
    	int h,a,c;
    	bool operator < (const node&n)const {
            return a<n.a;
        }
    }a[maxn];
    
    int main(){
    	std::ios::sync_with_stdio(false);
    	int n,maxh=0;
    	cin>>n;
    	for(int i=0;i<n;i++){
    		cin>>a[i].h>>a[i].a>>a[i].c;
    		maxh=max(maxh,a[i].a);
    	}
    	sort(a,a+n);// 按 高度限制有小到大排序 
    	memset(dp,0,sizeof(dp));
    	dp[0]=1;
    	for(int i=0;i<n;i++){
    		memset(used,0,sizeof(used));// 此后多重背包问题求解  
    		for(int j=a[i].h;j<=a[i].a;j++){
    			if(!dp[j]&&dp[j-a[i].h]&&used[j-a[i].h]<a[i].c){
    				used[j]=used[j-a[i].h]+1;
    				dp[j]=1;
    			}
    		}
    	}
    	for(int i=maxh;i>=0;i--)
    		if(dp[i]){
    			cout<<i<<endl;
    			break;
    		}
    	return 0;
    } 

越做dp越发觉 自己的IQ不足 orz


## 【树形DP_思维】_CF_551_(Div._2)_1153D_-_43_Serval_and_Rooted_Tree

<https://codeforces.com/contest/1153/problem/D>  
不算太容易想的dp  
如果我们知道有一个 子树有m个叶子 ，将K，K-1，K-2，…，K-M + 1分到这些叶子，  
max 节点的节点就是 k ；  
min 节点的值为 k - m + 1，对于 min 节点 我们希望这个值尽可能大，因此我们希望m尽可能小 ；  
这里 m 就是 dp [ i ] ;

现在，dp [ 树的每个叶子 ] = 1;  
对于一个节点 i ，它max值只能是 i 为根的子树中叶子节点的数值中排名第 dp[i] 大的数值。  
所以 对于min节点 只能 是子节点dp和  
而max 节点是 子节点dp最小值

dfs 以下列方式填充数组：

  1. if 节点的属性为“max”，则指定dp [node] = 其子节点的dp的最小值;
  2. else dp [node] = 其子节点的dp值之和。

答案是 叶子节点和 - dp [ 1 ] + 1，其中dp [ 1 ] 是 root；

    
    
    #include <bits/stdc++.h>
    #define fir first
    #define sec second
    #define fastio ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
    using namespace std;
    typedef long long ll;
    const int maxn = 3e5+5;
    const int INF = 0x3f3f3f3f;
    int m,n,k;
    int a[maxn],dp[maxn];
    int head[maxn],nxt[maxn],to[maxn],cnt=1;
    
    void ade(int a,int b){
    	to[++cnt]=b;
    	nxt[cnt]=head[a];
    	head[a]=cnt;
    }
    
    void dfs(int x){
    	dp[x]=a[x]?INF:0;
    	if(head[x]==0) k++,dp[x]=1;
    	for(int i=head[x];i;i=nxt[i]){
    		dfs(to[i]);
    		if(a[x]) dp[x]=min(dp[x],dp[to[i]]);
    		else dp[x]+=dp[to[i]];
    	}
    }
    
    int main(){
    	fastio;
    	cin>>n;
    	for(int i=1;i<=n;i++) cin>>a[i];
    	for(int i=2;i<=n;i++) cin>>m,ade(m,i);
    	dfs(1);
    //	for(int i=1;i<=n;i++) cout<<dp[i]<<"  ";cout<<endl;
    	cout<<k-dp[1]+1<<endl;
    	return 0; 
    }
    


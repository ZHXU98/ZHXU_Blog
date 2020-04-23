## CCPC-Wannafly_Winter_Camp_Day1_(Div2,_onsite)_E_流流流动_(树形DP)

orz

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190408165730795.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)

其实看懂题 他和没有上司的舞会题一样 orz  
最后建立0为树根 有些点x3 之后 在n外面 所以是孤立的 所以用0连接全部块  
同时数组开大点 防止越界

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn=2000+5;
    typedef long long ll;
    int n,f[maxn],d[maxn];
    int pre[maxn];
    int dp[maxn][5];
    int head[maxn],nxt[maxn<<1],to[maxn<<1],cnt;
    
    void ade(int a,int b) {
    	to[++cnt]=b;
    	nxt[cnt]=head[a];
    	head[a]=cnt;
    }
    
    void init(){
    	for(int i=0;i<maxn;i++) pre[i]=i;
    }
    
    int find(int x) {
    	return pre[x]==x?x:pre[x]=find(pre[x]);
    }
    
    void unions(int x,int y) {
    	int fx=find(x),fy=find(y);
    	if(fx!=fy) pre[fx]=fy;
    }
    
    void dfs(int u,int fa) {
    	dp[u][0]=0;
    	dp[u][1]=f[u];
    	for(int i=head[u]; i; i=nxt[i]) {
    		int v=to[i];
    		if(to[i]!=fa) {
    			dfs(v,u);
    			dp[u][0]+=max(dp[v][0],dp[v][1]);
    			dp[u][1]+=max(dp[v][0],dp[v][1]-d[min(u,v)]);
    		}
    	}
    }
    
    int main() {
    	init();
    	scanf("%d",&n);
    	for(int i=1; i<=n; i++) scanf("%d",&f[i]);
    	for(int i=1; i<=n; i++) scanf("%d",&d[i]);
    	for(int i=2; i<=n; i++) {
    		if(i&1) { 
    			ade(i,3*i+1),ade(i*3+1,i);
    			unions(3*i+1,i);
    		} else { 
    			ade(i,i/2),ade(i/2,i);
    			unions(i,i/2);
    		}
    	}
    	for(int i=1; i<=n; i++) {
    		if(pre[i]==i) ade(0,i),ade(i,0);
    	}
    	dfs(0,-1);
    	printf("%d\n",max(dp[0][0],dp[0][1]));
    	return 0;
    }
    


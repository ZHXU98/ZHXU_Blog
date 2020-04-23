## 【线段树】_Assign_the_task_HDU_-_3974_dfs建序_树上的操作

<https://vjudge.net/problem/22741/origin>  
依旧是 树上对各个结点子树的操作  
原题翻译：  
有一家公司有N个员工(从1到N)，公司里每个员工都有一个直接的老板(除了整个公司的领导)。如果你是某人的直接老板，那个人就是你的下属，他的所有下属也都是你的下属。如果你是没有人的老板，那么你就没有下属，没有直接老板的员工就是整个公司的领导，也就是说N个员工构成了一棵树。公司通常把一些任务分配给一些员工来完成，当一项任务分配给某个人时，他/她会把它分配给他/她的所有下属，换句话说，这个人和他/她的所有下属在同一时间接受了一项任务。此外，每当员工收到一个任务，他/她将停止当前任务(如果他/她有)，并开始新的任务。在公司将某些任务分配给某个员工后，编写一个程序来帮助找出某个员工当前的任务。

第一行包含单个正整数T(T<=10)，表示测试用例的数量。对于每个测试用例：第一行包含一个整数N(N≤50，000)，它是雇员的数目。下面的N-1行分别包含两个整数u和v，这意味着雇员v是雇员u的直接老板(1<=u，v<=N)。下一行包含一个整数M(M≤50，000)。下面的M行分别包含一条消息，“Cx”表示对员工x的当前任务的查询，“Tx
y”表示公司将任务y分配给员工x。(1<=x<=N，0<=y<=10^9)

此时 这题与poj上 apple tree 单点更新不一样了 任务更新到某个结点 就要让他的员工都被覆盖次任务 线段树的区间更新 补上pushdown 即可

某些 题目没有给出根节点 只需要知道跟入度为0 用vis把儿子结点全标记 然后for暴力查就好

    
    
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
    
    const int maxe = 1e6+5;
    const int maxv = 50000+100;
    const int maxn = 50000+100;
    const int mod = 1000000 ;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    const int cx[]={0,0,1,-1};
    const int cy[]={1,-1,0,0};
    
    int t,n,m,st,ed;
    int tree[maxn<<2],add[maxn<<2];
    int nxt[maxn],head[maxn],ver[maxn];
    int cnt,id;
    bool vis[maxn];
    
    struct node{
    	int l,r;
    }point[maxn];
    
    void add_e(int u,int v){
    	nxt[++cnt]=head[u];
    	ver[cnt]=v;
    	head[u]=cnt;
    }
    
    void dfs(int x){
    	point[x].l=++id;
    	for(int i=head[x];i!=-1;i=nxt[i]){
    		dfs(ver[i]);
    	}
    	point[x].r=id;
    }
    
    void pushdown(int rt){
    	add[rt<<1]=add[rt<<1|1]=add[rt];
    	tree[rt]=tree[rt<<1]=tree[rt<<1|1]=add[rt];
    	add[rt]=-1;
    }
    
    void updata(int L,int R,int l,int r,int rt,int c){
    	if(L<=l&&R>=r){
    		add[rt]=c;
    		tree[rt]=c;
    		return ;
    	}
    	if(add[rt]!=-1) pushdown(rt);
    	int mid=(l+r)>>1;
    	if(L<=mid) updata(L,R,l,mid,rt<<1,c);
    	if(R>mid) updata(L,R,mid+1,r,rt<<1|1,c);
    }
    
    int query(int L,int l,int r,int rt){
    	if(l==r){
    		return tree[rt];
    	}
    	if(add[rt]!=-1) pushdown(rt);
    	int mid=(l+r)>>1;
    	if(L<=mid) query(L,l,mid,rt<<1);
    	else query(L,mid+1,r,rt<<1|1);
    }
    
    
    int main(){
    	scanf("%d",&t);
    	int cas=1;
    	while(t--){
    		printf("Case #%d:\n",cas++);
    		cnt=-1;id=0;
    		memset(vis,0,sizeof(vis));
    		memset(nxt,-1,sizeof(nxt));
    		memset(head,-1,sizeof(head));
    		memset(tree,-1,sizeof(tree));
    		memset(add,-1,sizeof(add));
    		scanf("%d",&n);
    		for(int i=1;i<n;i++){
    			scanf("%d %d",&st,&ed);
    			vis[st]=1;
    			add_e(ed,st);
    		}
    		for(int i=1;i<=n;i++)
    			if(!vis[i]) dfs(i);
    		char ch;
    		scanf("%d",&m);
    		for(int i=0;i<m;i++){
    			scanf(" %c %d",&ch,&st);
    			if(ch=='C'){
    			//	cout<<point[st].l<<"  "<<point[st].r<<endl;
    				printf("%d\n",query(point[st].l,1,n,1));
    			} 
    			else{
    				scanf("%d",&ed);
    			//	cout<<point[st].l<<"  "<<point[st].r<<endl;
    				updata(point[st].l,point[st].r,1,n,1,ed);
    			}
    		}
    	}
        return 0;
    }
    


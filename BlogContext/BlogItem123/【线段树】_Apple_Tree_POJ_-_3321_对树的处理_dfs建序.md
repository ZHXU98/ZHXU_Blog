## 【线段树】_Apple_Tree_POJ_-_3321_对树的处理_dfs建序

<http://poj.org/problem?id=3321>  
题意 给你一颗树 Q是查询某个结点带他子树 的和 C 是改变树上结点 原来是0 变为1 原来1变为0

这样只需要 与或运算 ^ 就好了 而且单点更新 区间查询 水题  
这道题重点是DFS对树建序 这样就可以更方便维护树和子树了  
这里我DFS建序维护的是 结点应该管理的区间左右结点的序号

    
    
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
    const int maxv = 100000+100;
    const int maxn = 100000+100;
    const int mod = 1000000 ;
    const int INF = 0x3f3f3f3f;
    const double PI=acos(-1.0);
    const int cx[]={0,0,1,-1};
    const int cy[]={1,-1,0,0};
    
    int n,m,id=1,st,ed;
    
    int tree[maxv<<4];
    struct node{
    	int l,r;
    }point[maxn];// 结点管理的左右区间
    
    int head[maxn],v[maxn],nxt[maxn],cnt;
    void add(int u,int vi){
    	nxt[++cnt]=head[u];
    	v[cnt]=vi;
    	head[u]=cnt;
    }
    
    void dfs(int x){
    	point[x].l=++id;
    	for(int i=head[x];i!=-1;i=nxt[i]){
    		dfs(v[i]);
    	}
    	point[x].r=id;
    }
    
    void build(int l,int r,int rt){
    	if(l==r){
    		tree[rt]=1;
    		return ;
    	}
    	int mid=(l+r)>>1;
    	if(l<=mid) build(l,mid,rt<<1);
    	if(mid<r) build(mid+1,r,rt<<1|1);
    	tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    
    void updata(int L,int l,int r,int rt){
    	if(l==r){
    		tree[rt]^=1;
    		return ;
    	}
    	int mid=(l+r)>>1;
    	if(mid>=L) updata(L,l,mid,rt<<1);
    	else updata(L,mid+1,r,rt<<1|1);
    	tree[rt]=tree[rt<<1]+tree[rt<<1|1]; 
    }
    
    int query(int L,int R,int l,int r,int rt){
    //	cout<<L<<"  "<<R<<"   "<<l<<"   "<<r<<"   "<<rt<<endl; 
    	if(L<=l&&R>=r){
    		return tree[rt];
    	}
    	int res=0;
    	int mid=(l+r)>>1;
    	if(L<=mid) res+=query(L,R,l,mid,rt<<1);
    	if(R>mid) res+=query(L,R,mid+1,r,rt<<1|1);
    	return res;
    }
    
    int main(){
    	while(~scanf("%d",&n)){
    		memset(head,-1,sizeof(head));
    		memset(v,0,sizeof(v));
    		cnt=-1;
    		id=0;
    		memset(nxt,-1,sizeof(nxt));
    		memset(point,0,sizeof(point)); 
    		for(int i=1;i<n;i++){
    			scanf("%d %d",&st,&ed);
    			add(st,ed);
    		}
    		dfs(1);
    //		for(int i=1;i<=10;i++){
    //			cout<<point[i].l<<"   "<<point[i].r<<endl;
    //		}
    		build(1,n,1);
    		scanf("%d",&m);
    		char ch;
    		for(int i=0;i<m;i++){
    			getchar();
    			scanf("%c %d",&ch,&st);
    			if(ch=='Q'){
    				printf("%d\n",query(point[st].l,point[st].r,1,n,1));
    			}
    			else{
    				updata(point[st].l,1,n,1);
    			}
    		}
    		
    	}
        return 0;
    }
    


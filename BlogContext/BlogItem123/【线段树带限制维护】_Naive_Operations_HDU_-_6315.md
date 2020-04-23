## 【线段树带限制维护】_Naive_Operations_HDU_-_6315

<http://acm.hdu.edu.cn/showproblem.php?pid=6315>

给了一堆分母  
add l r 区间每个值加1  
query lr 查询 l r 每个数据 val/b 向下取正

第一次写了线段树 访问每个点。。。n^2 也是醉了  
算是技巧题 第一次做还真是不知道可以 这样减低复杂度

另外试了一下 scanf cin 一个800ms 一个2800ms

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <queue>
    #include <stack>
    #include <map> 
    using namespace std;
    typedef long long ll;
    
    const int maxn=100000+5;
    int b[maxn];
    
    struct node{
        int add;
        int ci;
        int val;
    }tree[maxn<<2];
    
    
    void build(int l,int r,int rt){
        if(l==r){
            tree[rt].add=0;
            tree[rt].val=0;
            tree[rt].ci=b[l];
            return ;
        }
        int m=(l+r)>>1;
        build(l,m,rt<<1);
        build(m+1,r,rt<<1|1);
    
        tree[rt].ci=min(tree[rt<<1].ci,tree[rt<<1|1].ci);
        tree[rt].add=0;
        tree[rt].val=0;
    }
    
    void pushdown(int rt){
        tree[rt<<1].ci+=tree[rt].add;
        tree[rt<<1|1].ci+=tree[rt].add;
    
        tree[rt<<1].add+=tree[rt].add;
        tree[rt<<1|1].add+=tree[rt].add;
    
        tree[rt].add=0;
    }
    
    void update(int L,int R,int l,int r,int rt,bool ok){
        if(L<=l&&R>=r){
            if(ok){//ok防止下一次访问 重复减去
                tree[rt].ci--;
                tree[rt].add--;
            }
            if(tree[rt].ci>0) return;
            if(l==r){
                if(tree[rt].ci==0){
                    tree[rt].val++;
                    tree[rt].ci=b[l];
                    tree[rt].add=0;
                    return ;
                }
            }
            ok=0;
        }
        if(tree[rt].add!=0) pushdown(rt);//不断推 直到cishu==0 更新树
        int m=(l+r)>>1;
        if(L<=m) update(L,R,l,m,rt<<1,ok);
        if(R>m) update(L,R,m+1,r,rt<<1|1,ok);
    
        tree[rt].ci=min(tree[rt<<1].ci,tree[rt<<1|1].ci);// 推回去很重要
        tree[rt].val=tree[rt<<1].val+tree[rt<<1|1].val;// 第一次写 忘记维护回去 cishu了
    }
    
    int query(int L,int R,int l,int r,int rt){
        if(L<=l&&R>=r){
            return tree[rt].val;
        }
        int m=(l+r)>>1;
        int res=0;
        if(L<=m) res+=query(L,R,l,m,rt<<1);
        if(R>m)  res+=query(L,R,m+1,r,rt<<1|1);
        return res;
    }
    
    int main(){
        int n,m,l,r;
        char cmd[10];
        while(cin>>n>>m){
            for(int i=1;i<=n;i++){
                scanf("%d",&b[i]);
            }
        //  memset(tree,0,sizeof(tree));
            build(1,n,1);
            for(int i=1;i<=m;i++){
                scanf("%s %d%d",cmd,&l,&r);
                if(cmd[0]=='a'){
                    update(l,r,1,n,1,1);
                }
                else{
                    printf("%d\n",query(l,r,1,n,1));
                }
            }
        }
        return 0;
    }
    


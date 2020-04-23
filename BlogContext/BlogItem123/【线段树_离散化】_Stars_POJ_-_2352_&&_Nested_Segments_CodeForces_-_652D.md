## 【线段树_离散化】_Stars_POJ_-_2352_&&_Nested_Segments_CodeForces_-_652D

友情链接 <https://blog.csdn.net/weixin_42754600/article/details/81940760>

Stars POJ - 2352  
<http://poj.org/problem?id=2352>  
题意 给了一堆星星坐标 （x，y）数他左下有多少星星  
输入 时注意 是按一层一层来的 先把第一层 由x从小到大排完 进入下一个高点的y层  
5  
1 1  
5 1  
7 1  
3 3  
5 5  
输出  
1  
2  
1  
1  
0  
左下角那个块区有几个星星 同时按升序输出左下块区同样数量的有几个  
ps从0开始 最高n-1数量 因为不包括自己

可以考虑线段树 输入时 查询1~x点可区间和 之后 x位置+1  
因为是一层一层输入的 同时是x升序 所以这样就可以访问到每个点左下角有几个  
就跟 堆玩了一层方块 进入下一层 然后每次数 左下包括左边 能有几个一样 数据较小  
下一题数据量大点 不过方法还是一样的

    
    
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
    
    const int maxn=15000+5;
    const ll mod=1e9+9;
    int x,y;
    int tree[(32000+10)<<2];
    int ans[maxn];
    
    void update(int L,int l,int r,int rt){
        if(l==r){
            tree[rt]++; //同下一个注释
            return ;
        }
        int m=(l+r)>>1;
        if(L<=m) update(L,l,m,rt<<1);
        else update(L,m+1,r,rt<<1|1);
    
        tree[rt]++;// 其实 只要在这函数第一行写个这就ok了
    }
    
    int query(int L,int R,int l,int r,int rt){
        if(L<=l&&R>=r){
            return tree[rt];
        }
        int m=(l+r)>>1;
        int ans=0;
        if(L<=m) ans+=query(L,R,l,m,rt<<1);
        if(R>m) ans+=query(L,R,m+1,r,rt<<1|1);
        return ans;
    }
    
    int main(){
        int n;
        while(cin>>n){
            memset(ans,0,sizeof(ans));
            memset(tree,0,sizeof(tree));
            for(int i=1;i<=n;i++){
                cin>>x>>y;
                x++;
                ans[query(1,x,1,32000+5,1)]++;// 写个32000会wa 开大点就好了
                update(x,1,32000+5,1);
            }
            for(int i=0;i<n;i++){
                cout<<ans[i]<<endl;
            }
        }
        return 0;
    }

Nested Segments CodeForces - 652D  
<http://codeforces.com/problemset/problem/652/D>

这道题同上一题 不过由于 x y范围较大 -1e9 到1e9 离散化 orz

    
    
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
    
    const int maxn=2e5+5;
    
    struct node{
        int x,y;
        int id;// 方便ans数组输出。。
        int num;
    }G[maxn];
    
    bool cmp1(node a,node b){
        return a.y<b.y;
    }
    
    bool cmp2(node a,node b){
        if(a.x==b.x) return a.y<b.y;
        return a.x>b.x;
    }
    
    int ans[maxn];
    
    int tree[maxn<<2];
    
    
    void updata(int L,int l,int r,int rt){
        if(l==r){
            tree[rt]++;
            return;
        }
        int m=(l+r)>>1;
        if(L<=m) updata(L,l,m,rt<<1);
        else updata(L,m+1,r,rt<<1|1);
    
        tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    
    int query(int L,int R,int l,int r,int rt){
        if(L<=l&&R>=r){
            return tree[rt];
        }
        int m=(l+r)>>1;
        int ans=0;
        if(L<=m) ans+=query(L,R,l,m,rt<<1);
        if(R>m) ans+=query(L,R,m+1,r,rt<<1|1);
        return ans;
    }
    
    int main(){
        int n,x,y;
        while(~scanf("%d",&n)){
            memset(tree,0,sizeof(tree));
            memset(ans,0,sizeof(ans));
        //  build(1,n,1);
            for(int i=1;i<=n;i++){
                scanf("%d %d",&G[i].x,&G[i].y);
                G[i].id=i;
            }
    
            sort(G+1,G+1+n,cmp1);
            for(int i=1;i<=n;i++) G[i].y=i;// 书上离散化 把重复的 反复到几个固定的数据 
            //但是这题 书上那个 又二分什么的 lower_bound得自己写 
            // 结构体 这里我不怎么会套二分查 写的太少
            //不过友情链接出了一个解决办法 开个新数组搞 处理了
            sort(G+1,G+1+n,cmp2);
            for(int i=1;i<=n;i++){
                ans[G[i].id]=query(1,G[i].y,1,maxn,1);
                updata(G[i].y,1,maxn,1);
            }
            for(int i=1;i<=n;i++)printf("%d\n",ans[i]);
        }
        return 0;
    }


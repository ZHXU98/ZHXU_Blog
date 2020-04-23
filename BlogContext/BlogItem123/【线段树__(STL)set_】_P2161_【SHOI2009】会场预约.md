## 【线段树__(STL)set_】_P2161_【SHOI2009】会场预约

题目描述

PP大厦有一间空的礼堂，可以为企业或者单位提供会议场地。这些会议中的大多数都需要连续几天的时间（个别的可能只需要一天），不过场地只有一个，所以不同的会议的时间申请不能够冲突。也就是说，前一个会议的结束日期必须在后一个会议的开始日期之前。所以，如果要接受一个新的场地预约申请，就必须拒绝掉与这个申请相冲突的预约。
一般来说，如果PP大厦方面事先已经接受了一个会场预约，例如从10日到15日，就不会在接受与之相冲突的预约，例如从12日到17日。不过，有时出于经济利益，PP大厦方面有时会为了接受一个新的会场预约，而拒绝掉一个甚至几个之前预订的预约。
于是，礼堂管理员QQ的笔记本上笔记本上经常记录着这样的信息：
本题中为方便起见，所有的日期都用一个整数表示。例如，如果一个为期10天的会议从“90日”开始到“99日”，那么下一个会议最早只能在“100日”开始。
最近，这个业务的工作量与日俱增，礼堂的管理员QQ希望参加SHTSC的你替他设计一套计算机系统，方便他的工作。这个系统应当能执行下面两个操作：
A操作：有一个新的预约是从“start日”到“end日”，并且拒绝掉所有与它相冲突的预约。执行这个操作的时候，你的系统应当返回为了这个新预约而拒绝掉的预约个数，以方便QQ与自己的记录相校对。
B操作：请你的系统返回当前的仍然有效的预约的总数。

输入输出格式  
输入格式：  
输入文件的第一行是一个整数n，表示你的系统将接受的操作总数。 接下去n行每行表示一个操作。每一行的格式为下面两者之一： “A start
end”表示一个A操作； “B”表示一个B操作。

输出格式：  
输出文件有n行，每行一次对应一个输入。表示你的系统对于该操作的返回值。

输入输出样例  
输入样例#1：  
6  
A 10 15  
A 17 19  
A 12 17  
A 90 99  
A 11 12  
B  
输出样例#1：  
0  
0  
2  
0  
1  
2  
说明  
N< = 200000

1< = Start End < = 100000

线段树解法

把不同 任务当作颜色看待  
线段树维护这区间是上面颜色管理 同时我们认为 这一区间超过一种颜色 用 -1 标记 0 表示没有被覆盖 大于0表示这一区间有第几个出现的任务颜色管理

在pushup 时 有

  1. if (tree[rt<<1] == tree[rt<<1|1]) tree[rt] = tree[rt<<1];
  2. else if (!tree[rt<<1] || !tree[rt<<1|1]) tree[rt] = tree[rt<<1] + tree[rt<<1|1];// 有个子树是0的话 直接拿另一个标记
  3. else tree[rt] = -1; // 区间颜色不同时 -1

所以对于A操作，我们只需要在线段树上查询询问的区间[x,y]，并在查询的时候在全局开数组，记录一下有哪几个颜色（记得处理过的颜色标记一下，因为颜色只会出现一次，不需要也不能重复删除）。  
会重复删也是因为lazy 0 是无色 都是 0的时候补下推 导致一些区间地下没有更新 所以vis去个重 防止减多

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 5, N = 1e5;
    
    int x[maxn], y[maxn];
    int que[maxn], tail,cnt;
    bool vis[maxn];
    int n, sum = 0;
    
    int tree[maxn<<2], lazy[maxn<<2];
    
    void pushup(int rt) {
        if (tree[rt<<1] == tree[rt<<1|1]) tree[rt] = tree[rt<<1];
        else if (!tree[rt<<1] || !tree[rt<<1|1]) tree[rt] = tree[rt<<1] + tree[rt<<1|1];
        else tree[rt] = -1;
    }
    
    void pushdown(int rt) {
        if (lazy[rt]) {
            tree[rt<<1] = tree[rt<<1|1] = lazy[rt];
            lazy[rt<<1] = lazy[rt<<1|1] = lazy[rt];
            lazy[rt] = 0;
        }
    }
    
    void query(int L, int R, int l, int r, int rt) {
        if (L <= l && r <= R && (~tree[rt])) { 
            if(tree[rt]) que[++tail] = tree[rt];
            return;
        }
        pushdown(rt);
        int mid=(l+r)>>1;
        if(L<=mid) query(L,R,l,mid,rt<<1);
        if(mid<R) query(L,R,mid+1,r,rt<<1|1);
        pushup(rt);
    }
    
    void update(int L, int R, int l, int r, int rt, int k) {
        if (L <= l && r<= R) {
            tree[rt] = lazy[rt] = k;
            return;
        }
        pushdown(rt);
        int mid=(l+r)>>1;
        if(L<=mid) update(L,R,l,mid,rt<<1,k);
        if(mid<R) update(L,R,mid+1,r,rt<<1|1,k);
        pushup(rt);
    }
    
    
    signed main() {
        cin>>n;
        string cmd;
        for (int i = 1; i <= n; i++) {
            cin>>cmd;
            if (cmd[0] == 'A') {
                cin>>x[i]>>y[i];
                tail = 0;
                query(x[i], y[i], 1, N, 1);
                cnt = 0;
                for (int j = 1; j <= tail; j++)
                    if (!vis[que[j]]) { 
                        int s = x[que[j]], t = y[que[j]];
                        update(s,t,1,N,1,0);
                        vis[que[j]] = true;
                        cnt++;
                    }
                update(x[i],y[i],1,N,1,i);
                cout<<cnt<<endl;
                sum = sum - cnt + 1;
            }
            else
                cout<<sum<<endl;
        }
        return 0;
    }
    

orz stl解法

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e5 + 5, N = 1e5;
    
    struct node{
        int l,r;
        bool operator < (const node &x) const{
            return r<x.l;
        }
    };
    
    int n;
    set<node> s;
    
    int main(){
        cin>>n;
        while(n--){
            string op;
            cin>>op;
            if(op=="A"){
               	int l,r,ans=0;
               	cin>>l>>r;
                node x=(node){l,r};
                set<node>::iterator i=s.find(x);
                while(i!=s.end()){
                    ans++;
                    s.erase(i);
                    i=s.find(x);
                }
                s.insert(x);
                cout<<ans<<endl;
            }
            else cout<<s.size()<<endl;
        }
        return 0;
    }
    


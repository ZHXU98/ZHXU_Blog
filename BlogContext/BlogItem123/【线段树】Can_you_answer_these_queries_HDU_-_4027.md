## 【线段树】Can_you_answer_these_queries_HDU_-_4027

A lot of battleships of evil are arranged in a line before the battle. Our
commander decides to use our secret weapon to eliminate the battleships. Each
of the battleships can be marked a value of endurance. For every attack of our
secret weapon, it could decrease the endurance of a consecutive part of
battleships by make their endurance to the square root of it original value of
endurance. During the series of attack of our secret weapon, the commander
wants to evaluate the effect of the weapon, so he asks you for help.  
You are asked to answer the queries that the sum of the endurance of a
consecutive part of the battleship line.

Notice that the square root operation should be rounded down to integer.  
Input  
The input contains several test cases, terminated by EOF.  
For each test case, the first line contains a single integer N, denoting there
are N battleships of evil in a line. (1 <= N <= 100000)  
The second line contains N integers Ei, indicating the endurance value of each
battleship from the beginning of the line to the end. You can assume that the
sum of all endurance value is less than 2 63.  
The next line contains an integer M, denoting the number of actions and
queries. (1 <= M <= 100000)  
For the following M lines, each line contains three integers T, X and Y. The
T=0 denoting the action of the secret weapon, which will decrease the
endurance value of the battleships between the X-th and Y-th battleship,
inclusive. The T=1 denoting the query of the commander which ask for the sum
of the endurance value of the battleship between X-th and Y-th, inclusive.  
Output  
For each test case, print the case number at the first line. Then print one
line for each query. And remember follow a blank line after each test case.  
Sample Input  
10  
1 2 3 4 5 6 7 8 9 10  
5  
0 1 10  
1 1 10  
1 1 5  
0 5 8  
1 4 8  
Sample Output  
Case #1:  
19  
7  
6

2^31次方开个10几次就没有了 。。。 所以当区间长度和他被管理的区间节点一样 就跳过 不然被卡数据超时。。。

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <string>
    #include <queue>
    #include <stack>
    #include <set>
    #include <list>
    using namespace std;
    typedef long long ll;
    
    const int maxn=100050;
    ll n,k;
    
    ll tree[maxn<<2];
    
    void pushup(int rt){
        tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    } 
    
    void build(int l,int r,int rt){
        if(l==r){
            scanf("%lld",&tree[rt]);
            return ;
        }
        int m=(l+r)>>1;
        build(l,m,rt<<1);
        build(m+1,r,rt<<1|1);
        pushup(rt);
    }
    
    void update(int L,int R,int l,int r,int rt){
        if(L<=l&&r<=R&&tree[rt]==(ll)(r-l+1)) return;//////////
        if(l==r){
            tree[rt]=sqrt(1.0*tree[rt]);
            return;
        }
        int m=(l+r)>>1;
        if(L<=m) update(L,R,l,m,rt<<1);
        if(R>m)  update(L,R,m+1,r,rt<<1|1);
        pushup(rt);
    }
    
    ll query(int L,int R,int l,int r,int rt){
        if(L<=l&&r<=R){
            return tree[rt];
        }
        int m=(l+r)>>1;
        ll res=0;
        if(L<=m) res+=query(L,R,l,m,rt<<1);
        if(R>m) res+=query(L,R,m+1,r,rt<<1|1);
        return res;
    }
    
    int main(){
        int t=1;
        while(cin>>n){
            build(1,n,1);
            cin>>k;cout<<"Case #"<<t<<":"<<endl; 
            for(int i=0;i<k;i++){
                int cmd,l,r;
                cin>>cmd>>l>>r;
            if(r<l){
                swap(r,l);
            }
                if(cmd==0){
                    update(l,r,1,n,1);
                }
                if(cmd==1){
                    printf("%lld\n",query(l,r,1,n,1));
                } 
            }
            t++;
            cout<<endl;
        }
        return 0;
    }
    
    


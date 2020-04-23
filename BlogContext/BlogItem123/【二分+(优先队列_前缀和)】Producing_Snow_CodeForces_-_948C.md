## 【二分+(优先队列_前缀和)】Producing_Snow_CodeForces_-_948C

题干
给定长度n(n<=1e5)，第一行v[i]表示表示第i堆雪的体积，第二行t[i]表示第1~i天的雪将要消融的体积，一堆雪如果消融到体积为0则消失，求每天消融的雪的体积。  
首先是优先队列的…..

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <algorithm>
    #include <cmath>
    #include <vector>
    #include <map>
    #include <queue>
    #include <stack>
    using namespace std;
    typedef long long ll;
    
    const int INF=0x3f3f3f3f;
    const int maxn=1e5+5;
    int n,m,c1,c2,v,q;
    ll T[maxn],V[maxn],ST[maxn];
    
    int main(){
        ST[0]=0;
        while(cin>>n){
            for(int i=1;i<=n;i++) cin>>V[i];
            for(int i=1;i<=n;i++) cin>>T[i];
            for(int i=1;i<=n;i++) ST[i]=ST[i-1]+T[i];
    
            priority_queue<ll,vector<ll>,greater<ll> > Q;
    
            for(int i=1;i<=n;i++){
                Q.push(V[i]+ST[i-1]);// 每次都加上 T的前缀和 让ST[i-1]消除 i前天的多的温度 
                ll res=T[i]*Q.size();// 这个时候还存在的雪堆默认都够消融 乘温度  接着进入while中去找不够的 
                while(!Q.empty()&&Q.top()<=ST[i]){//找雪不够融的天 pop掉 并且 那天多加的 减去 
                    res+=(Q.top()-ST[i]);//  这里top是本来的-ST[i] 变成我们多加上的 减去即可             
                    Q.pop();
                }
                if(i!=1) cout<<" ";
                cout<<res;
            }cout<<endl; 
        }
        return 0;
    }

接下来是 练习手写二分 其实可以lower_bound

    
    
    #include <iostream>
    #include <cstdio>
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
    #include <bitset>
    using namespace std;
    typedef long long ll;
    
    const int maxn=1e5+10;
    
    ll V[maxn],T[maxn],ST[maxn];
    ll Q[maxn],RE[maxn];
    int n;
    
    int main(){
        ST[0]=0;
        V[0]=0;
        T[0]=0;
        while(cin>>n){
            memset(Q,0,sizeof(Q));
            memset(RE,0,sizeof(RE));
            for(int i=1;i<=n;i++) cin>>V[i];
            for(int i=1;i<=n;i++) cin>>T[i];
            for(int i=1;i<=n+10;i++) ST[i]=ST[i-1]+T[i];
    
            for(int i=1;i<=n;i++){
            //  Q[i]++;
                ll L=i-1,R=n+10;
                while(R-L>1){//白书还是耐用。。。。。
                    ll m=(R+L)>>1;
                //  cout<<L<<"  "<<R<<" "<<m<<endl;
                    if(V[i]<=(ST[m]-ST[i-1])) R=m;
                    else L=m;
                }
            //  cout<<"  DAS "<<R<<endl;
                Q[i]++,Q[R]--;//第一次使用神奇操作 来记录那天可以有多少堆
                RE[R]+=(V[i]-(ST[R-1]-ST[i-1])); 
            }
        //  for(int i=0;i<=n;i++) 
            for(int i=1;i<=n;i++){
                Q[i]=Q[i-1]+Q[i];
                if(i!=1) cout<<" ";
                cout<<Q[i]*T[i]+RE[i];
            }cout<<endl;
        }
        return 0;
    }
    


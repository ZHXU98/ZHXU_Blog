## 【思维_前缀和】Molly's_Chemicals_CodeForces_-_776C

<http://codeforces.com/problemset/problem/776/C>  
题意：给n个数和数k，求这n个数里面有多少段的和sum满足sum == k^i (i=0, 1, 2…)。

思路：暴力的话公式是sum[i] - sum[j] = k^t,但是肯定超时，转化一下，变成sum[j] = sum[i] -
k^t，那么可以用map类型的数组记录到每一个前缀sum[i]时满足上式，即sum[i]-k的个数。

如果sum[i]-K^x可以在map出现 相当于在数列中分块 意味到sum[i]到sum[I-pos]==k^x 这区间可以出现k^x

    
    
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
    
    const int maxn=1e5+10;
    map<ll,int> mp;
    ll sum[maxn];
    
    int main(){
        ll n,m,k;
        int t;
        cin>>n>>k;
        sum[0]=0;
        for(int i=1;i<=n;i++){
            cin>>m;
            sum[i]=sum[i-1]+m;
        } 
        ll tp=1;
        ll ans=0,res=0;
        mp[0]=1;
        for(int i=1;i<=n;i++){
            tp=1;
            while(tp<1e15){
                ans+=mp[sum[i]-tp];
                tp*=k;
                if(k==1) break; 
                if(k==-1&&tp==1) break; //tp等于是为了先找一下 有没有-1的数据
            }                          //然后到1直接break;
            mp[sum[i]]++;
        }
        cout<<ans<<endl;
        return 0;
    }

不知道为什么 上面这份占内存严重。。  
另外下面一份同学的。。。

    
    
    #include <iostream>
    #include <cstdio>
    #include <cstring>
    #include <map>
    #include <cstdlib>
    typedef long long LL;
    using namespace std;
    
    int a[100000+50];
    map<LL, int> mp;
    
    int main(){ 
        int n, k;
        while(~scanf("%d%d", &n, &k)) {
            for(int i = 0; i <n; i++)
                scanf("%d", &a[i]);
            LL p = 1;
            LL ans = 0;
           while(abs(p) < 1e15) {
                mp.clear();
                mp[0] = 1;
                LL sum = 0;
                for(int i = 0; i < n; i++) {
                    sum += a[i];
                    ans += mp[sum-p];
                    mp[sum]++;
                }
                p *= k;
                if(p == 1) break;
           }
           printf("%lld\n", ans);
        }
        return 0;
    }
    


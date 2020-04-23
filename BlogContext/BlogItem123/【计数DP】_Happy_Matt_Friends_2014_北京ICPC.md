## 【计数DP】_Happy_Matt_Friends_2014_北京ICPC

<http://acm.hdu.edu.cn/showproblem.php?pid=5119>

菜啊  
计数DP没有看出来  
没有看出来。。。。

H - Happy Matt Friends  
HDU - 5119  
Matt has N friends. They are playing a game together.

Each of Matt’s friends has a magic number. In the game, Matt selects some
(could be zero) of his friends. If the xor (exclusive-or) sum of the selected
friends’magic numbers is no less than M , Matt wins.

Matt wants to know the number of ways to win.  
InputThe first line contains only one integer T , which indicates the number
of test cases.

For each test case, the first line contains two integers N, M (1 ≤ N ≤ 40, 0 ≤
M ≤ 10 6).

In the second line, there are N integers ki (0 ≤ k i ≤ 10 6), indicating the
i-th friend’s magic number. OutputFor each test case, output a single line
“Case #x: y”, where x is the case number (starting from 1) and y indicates the
number of ways where Matt can win. Sample Input  
2  
3 2  
1 2 3  
3 3  
1 2 3  
Sample Output  
Case #1: 4  
Case #2: 2

Hint  
In the ﬁrst sample, Matt can win by selecting:  
friend with number 1 and friend with number 2. The xor sum is 3.  
friend with number 1 and friend with number 3. The xor sum is 2.  
friend with number 2. The xor sum is 2.  
friend with number 3. The xor sum is 3. Hence, the answer is 4.

    
    
    #include <iostream>
    #include <cstdio>
    #include <cmath>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 1<<20;
    const int mod = 1e9+7;
    const int INF = 0x3f3f3f3f;
    
    // 计数dp 
    // 1e6 20长度 2进制。。 
    // 1024*1024 = 1e8+1e3； 
    // 因为这个性质 a^b=c  c^b=a;
    
    // 所以 直接背包计数就可以了 dp[i][j] = dp[i - 1][j] + dp[i - 1][j ^ a[i]]
    
    
    
    int t,n,m;
    ll dp[3][1<<20];
    int num[45];
    
    int main(){
        scanf("%d",&t);
        int cas=1;
        while(t--){
            scanf("%d %d",&n,&m);
            for(int i=1;i<=n;i++) scanf("%d",&num[i]);
            memset(dp,0,sizeof(dp));
            dp[0][0]=1;
            for(int i=1;i<=n;i++){
                for(int j=0;j<maxn-5;j++){
                    dp[i%2][j]=max(dp[(i)%2][j],dp[(i-1)%2][j^num[i]]+dp[(i-1)%2][j]);
                }
            }
            ll ans=0;
            for(int i=m;i<maxn-5;i++){
                ans+=dp[n%2][i];
            }
            printf("Case #%d: %lld\n",cas++,ans);
        }
        return 0;
    } 


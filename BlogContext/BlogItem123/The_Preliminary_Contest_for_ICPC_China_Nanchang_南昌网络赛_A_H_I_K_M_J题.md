## The_Preliminary_Contest_for_ICPC_China_Nanchang_南昌网络赛_A_H_I_K_M_J题

我补过 J题拉 orz 交了3次 重写了一次orz  
~~真是太快乐(自闭)拉~~  
连接 <https://blog.csdn.net/qq_40831340/article/details/90739880>

> A PERFECT NUMBER PROBLEM  
>  Write a program to output the first 55 perfect numbers. A perfect number is
> defined to be a positive integer where the sum of its positive integer
> divisors excluding the number itself equals the number.  
>  For example: 1+ 2 + 3 = 61+2+3=6, and 66 is the first perfect number.  
>  There is no input for this problem.

百度完美数可得。。。。

    
    
    int main(){
    	cout<<"6"<<endl;
    	cout<<"28"<<endl;
    	cout<<"496"<<endl;
    	cout<<"8128"<<endl;
    	cout<<"33550336"<<endl;
    	return 0;
    }
    

> M Subsequence

给母串 和一些子串 问 字串是否在母串出现过 (可以不连续) 2018百度之星网络赛第一题 2019 牛客小白月赛12 最后一题 贴过来就过 。。。

其实就是开了个26*maxn 的数组 倒着扫 存每层下一个字母出现位置 就可以 o(m) 的确定一个串存不存在了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e5+5;
     
    char mp[maxn];
    char sp[maxn];
    int pos[30][maxn];
     
    int main() {
        scanf("%s",mp);
        int len=strlen(mp);
        for(int i=len-1; i>=0; i--) {
            for(int j=27; j>=1; j--) {
                pos[j][i]=pos[j][i+1];
            }
            pos[mp[i]-'a'+1][i]=i+1;
     
        }
        int n;
        scanf("%d",&n);
        for(int i=1; i<=n; i++) {
            scanf("%s",sp);
            len=strlen(sp);
            int j=0;
            int nxt=0;
            while(j<len) {
                nxt=pos[sp[j]-'a'+1][nxt];
                if(nxt==0) {
                    cout<<"NO\n";
                    break;
                }
                if(j==len-1) {
                    cout<<"YES\n";
                    break;
                }
                j++;
            }
        }
        return 0;
    }
    

> H Coloring Game  
>  David has a white board with 2 \times N2×N grids.He decides to paint some
> grids black with his brush.He always starts at the top left corner and ends
> at the bottom right corner, where grids should be black ultimately.  
>  Each time he can move his brush up(↑), down(↓), left(←), right(→), left
> up(↖), left down(↙), right up(↗), right down (↘) to the next grid.  
>  For a grid visited before,the color is still black. Otherwise it changes
> from white to black.  
>  David wants you to compute the number of different color schemes for a
> given board. Two color schemes are considered different if and only if the
> color of at least one corresponding position is different.

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190422134814982.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
给你一个2x n 个网格 这个人左上走到右下 走过的格子涂黑 它可以走周围8个方向  
因为它最后是到右下的 我们可以认为 这个2x n 除了第一层(左上一定黑) 和 最后一层（右下一定黑）  
其他层 只有3个状态 10 01 11 所以 是3^（n-2）个状态 + 第一层和最后一次的4个状态  
ans = 4*3^（n-2）;

ps 1列 特判；

    
    
    #include<bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
    //#define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f ;
    const int maxn = 5e5 + 5 ;
    const long long mod=1000000007;
    
    long long int quick_mod(long long int a,long long int b) {
    	long long int ans=1;
    	a%=mod;
    	while(b) {
    		if(b&1) {
    			ans=ans*a%mod;
    			b--;
    		}
    		b>>=1;
    		a=a*a%mod;
    	}
    	return ans;
    }
    
    int main() {
    	ll n;
    	cin>>n;
    	if(n==1) cout<<1<<endl;
    	else cout<<4*quick_mod(3,n-2)%mod<<endl;
    	return 0;
    }
    

> I Max answer  
>  Alice has a magic array. She suggests that the value of a interval is equal
> to the sum of the values in the interval, multiplied by the smallest value
> in the interval.  
>  Now she is planning to find the max value of the intervals in her array.
> Can you help her?

给你一个长度n的序列 选取一个数字 和(包含它连续的)区间(而且它必须是这个区间的最小值) 使a[i]*(这一区间的和) 最大  
首先单调栈 处理每个a[ i ] 管理区间(没有比它小的最大连续区间)  
然后 对 a[ i ] 分2种情况 正负来考虑

  1. 大于 0 的情况 显然是 它 管理的区间和*a[ i ] 因为a[i]>0 同时它肯定是这区间最低点 该区间越长和越大
  2. 小于 0 的情况 不妨我们维护一个 前缀和 既然它是负数 我们 当前i 到R[ i ]前缀和最好越小越好 而L[ i ] 到 i得和越大越好

    
    
    #include<bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
    //#define int long long
    using namespace std;
    typedef long long ll;
    const long long INF = 0x3f3f3f3f3f3f3f3f ;
    const int maxn = 5e5 + 5 ;
    
    int n;
    ll a[maxn],sum[maxn]; 
    int L[maxn],R[maxn];
    
    struct node {
    	ll minsum,maxsum;
    } tr[maxn<<4];
    
    void pushup(int rt) {
    	tr[rt].minsum=min(tr[rt<<1].minsum,tr[rt<<1|1].minsum);
    	tr[rt].maxsum=max(tr[rt<<1].maxsum,tr[rt<<1|1].maxsum);
    }
    
    void build(int l,int r,int rt) {
    	if(l==r) {
    		tr[rt]={sum[l],sum[l]};
    		return ;
    	}
    	int mid=(l+r)>>1;
    	build(l,mid,rt<<1);
    	build(mid+1,r,rt<<1|1);
    	pushup(rt);
    }
    
    ll maxquery(int L,int R,int l,int r,int rt){
    	if(L<=l&&r<=R){
    		return tr[rt].maxsum;
    	}
    	int mid=(l+r)>>1;
    	ll res=-INF;
    	if(L<=mid) res=max(res,maxquery(L,R,l,mid,rt<<1));
    	if(R>mid) res=max(res,maxquery(L,R,mid+1,r,rt<<1|1));
    	return res;
    }
    
    ll minquery(int L,int R,int l,int r,int rt){
    	if(L<=l&&r<=R){
    		return tr[rt].minsum;
    	}
    	int mid=(l+r)>>1;
    	ll res=INF;
    	if(L<=mid) res=min(res,minquery(L,R,l,mid,rt<<1));
    	if(R>mid) res=min(res,minquery(L,R,mid+1,r,rt<<1|1));
    	return res;
    }
    
    void solLR() {
    	stack<int> s;
    	for(int i=1; i<=n; i++) {
    		while(!s.empty()&&a[s.top()]>=a[i]) s.pop();
    		L[i]=(s.empty())?0:s.top();
    		s.push(i);
    	}
    	while(!s.empty()) s.pop();
    	for(int i=n; i>=1; i--) {
    		while(!s.empty()&&a[s.top()]>=a[i]) s.pop();
    		R[i]=(s.empty())?(n+1):s.top();
    		s.push(i);
    	}
    }
    
    signed main() {
    	scanf("%lld",&n);
    	for(int i=1; i<=n; i++) scanf("%lld",&a[i]),sum[i]=sum[i-1]+a[i];
    	solLR();
    	build(1,n,1);
    	ll ans=-INF;
    	ll rmin,lmax;
    	for(int i=1; i<=n; i++) {
    		if(a[i]<0) {
    			rmin=minquery(i,R[i]-1,1,n,1);
    			lmax=maxquery(L[i]+1,i,1,n,1);
    			ans=max(ans,a[i]*(rmin-lmax));
    		} else {
    			ans=max(ans,a[i]*(sum[R[i]-1]-sum[L[i]]));
    		}
    	}
    	printf("%lld\n",ans);
    	return 0;
    }
    

> K MORE XOR  
>  Given a sequence of n numbers and three functions.  
>  Define a function f(l,r)f(l,r) which returns \oplus a[x]⊕a[x] (l \le x \le
> rl≤x≤r). The \oplus⊕ represents exclusive OR.  
>  Define a function g(l,r)g(l,r) which returns \oplus f(x,y)(l \le x \le y
> \le r)⊕f(x,y)(l≤x≤y≤r).  
>  Define a function w(l,r)w(l,r) which returns \oplus g(x,y)(l \le x \le y
> \le r)⊕g(x,y)(l≤x≤y≤r).  
>  You are also given a number of xor-queries. A xor-query is a pair (i, ji,j)
> (1 \le i \le j \le n1≤i≤j≤n). For each xor-query (i, j)(i,j), you have to
> answer the result of function w(l,r)w(l,r).

让你求异或得前缀和的前缀和的前缀和 orz 规律题 异或 出现2次的数字 显然是0 这里搞贡献度就好  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190422134149740.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)发现
4 组一循环 当长度是 1的时候 只取这个段的每4个数的第一个 orz 其他的看表

暴力直接for超时了 这里选择维护一个前缀异或和的数组

    
    
    #include<bits/stdc++.h>
    #define fastio ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
    using namespace std;
    typedef long long ll;
    const int INF = 0x3f3f3f3f ;
    const int maxn = 1e5 + 5 ;
    
    int a[maxn];
    
    int main() {
    	int t,n,l,r,res,len,q;
    	scanf("%d",&t);
    	while(t--) {
    		scanf("%d",&n);
    		for(int i=1; i<=n; i++) scanf("%d",&a[i]);
    		for(int i=4; i<=n; i++) a[i]^=a[i-4];
    		scanf("%d",&q);
    		while(q--) {
    			scanf("%d %d",&l,&r);
    			len = r - l + 1 ;
    			if(len%4==0) printf("0\n");
    			else if(len % 4 == 1) {
    				if(l>=4) res=a[r]^a[l-4];
    				else res=a[r];
    				printf("%d\n",res);
    			} else if(len % 4 == 2) {
    				if(l>=4) res=a[r]^a[l-4];
    				else res=a[r];
    				l++,r--;
    				if(l>=4) res=res^a[r]^a[l-4];
    				else res=res^a[r];
    				printf("%d\n",res);
    
    			} else if(len % 4 == 3) {
    				l++,r--;
    				if(l>=4) res=a[r]^a[l-4];
    				else res=a[r];
    				printf("%d\n",res);
    			}
    		}
    	}
    	return 0;
    }
    


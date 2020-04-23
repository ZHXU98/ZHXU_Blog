## 2019HDU多校第三场_G_Find_the_answer_(二分树状数组)

二分 树状数组 离散化  
这题 wa了好多发 最后发现二分从0开始就好了  
问了一圈人 就我二分乱搞  
题意 就是 给了你 N 长度序列 你选前 I 个 他们和必须小于 M 你可以让其中某些数字变成0  
让他们 最后和小于 M （前I个 不包括I）

所以 我考虑 离散化 + 树状数组存对应位置 + 1 和 树状数组维护他们的和  
这样一来 就存在单调性 可二分了  
二分树状数组 M - A[I] 大小  
如果大于 我们 把大于位置之后个数的直接输出就好  
然后 在把这个数据补充到树状数组中 对应位置 +1

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 2e5 + 5;
    
    long long c[maxn];
    int num[maxn];
    int a[maxn], b[maxn], ct[maxn];
    int n, m, t, cpt;
    
    long long queval(int x){
        long long res = 0;
        for(; x; x -= x&(-x)) res += c[x];
        return res;
    }
    
    void add(int x, int y){
        for(;x <= n + 1; x += x & (-x)) c[x] += y;
    }
    
    void addd(int x, int y){
        for(;x <= n + 1; x += x & (-x)) num[x] += y;
    }
    
    int quenum(int x){
        int res = 0;
        for(; x; x -= x&(-x)) res += num[x];
        return res;
    }
    int que(int x) {
    	//cout << queval(x) - queval(x - 1) << endl;
    	cout << quenum(n) << "   " << quenum(x) << endl ;
    }
    
    bool chk(int mid) {
    //	cout << mid << "  sum " << queval(mid) << endl;
    	return queval(mid) <= m - cpt; 
    }
    
    signed main(){
    	scanf("%d", &t);
    	while(t --) {
    		scanf("%d %d", &n, &m);
    		memset(c, 0, (n + 5) * sizeof(long long)) ;
    		memset(num, 0, (n + 5) * sizeof(int)) ;
    		for(int i = 1; i <= n; i ++) scanf("%d", &a[i]), b[i] = a[i] , ct[i] = 0;
    		sort(b + 1, b + 1 + n);
    		for(int i = 1; i <= n; i ++) {
    			int pos = lower_bound(b + 1, b + 1 + n, a[i]) - b;
    			if(ct[pos] == 0) ct[pos] = pos;
    			int rt = pos;
    			pos = ct[pos];
    			ct[rt] = pos + 1;
    			cpt = a[i];
    		//	cout << pos << endl;	
    			int l = 0, r = n + 1;
    			while(l < r) {
    				int mid = l + r + 1 >> 1;
    				if(chk(mid)) l = mid;
    				else r = mid - 1;
    			} 
    			//cout << l << " p    " ;
    			printf("%d", quenum(n) - quenum(l)); 
    		//	que(l);
     		
     			add(pos, a[i]);
    			//que(pos);
    			addd(pos, 1);
    			printf(" ");
    		} puts("");
    	}
    	return 0;
    }
    

心疼自己 wa 好几发 如果 不是从0 就会出现错误orz  
样例  
5 5  
4 4 4 4 4


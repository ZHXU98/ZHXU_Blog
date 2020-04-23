## 2019_牛客暑假多校第八场_A_All-one_Matrices

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190819114352471.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
第3次了 关于最大01矩阵的  
这次找 尽可能大 不相互包含的  
寻找策略是 下一层1的长度 不等于我当前这层长度  
剩下的依然是 单调栈维护1矩阵 左右到哪里

    
    
    #include<bits/stdc++.h>
    using namespace std;
    const int maxn = 3000 + 10;
    
    int n, m;
    int a[maxn][maxn], dp[maxn][maxn];
    int vis[maxn][maxn];
    int l[maxn], r[maxn], sum[maxn][maxn];
    int que[maxn], head;
    
    signed main() {
    	scanf("%d %d", &n, &m);
    	for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j ++) {
                scanf("%1d", &a[i][j]);
                sum[i][j] = sum[i][j - 1] + a[i][j];
                if(a[i][j] == 1) dp[i][j] = dp[i - 1][j] + 1;
    			else dp[i][j] = 0;
            }
        }
        
    	int ans = 0;
    	for(int i = 1; i <= n; i ++) {
    		head = 0; que[++head] = 0;
    		for(int j = 1; j <= m; j ++) {
    			while(head > 0 && dp[i][j] <= dp[i][que[head]]) head--;
    			l[j] = que[head] + 1;
    			que[++head] = j;
    		}
    		head = 0; que[++head] = m + 1;
    		for(int j = m; j >= 1; j --) {
    			while(head > 0 && dp[i][j] <= dp[i][que[head]]) head--;
    			r[j] = que[head] - 1;
    			que[++head] = j;
    		}
    
    		for(int j = 1; j <= m; j ++) {
    			if(dp[i][j] == 0) continue;
    			if(sum[i][r[j]] - sum[i][l[j] - 1] != r[j] - l[j] + 1) continue;
    			if(vis[l[j]][r[j]] == i) continue; //标记K层
                if(sum[i + 1][r[j]] - sum[i + 1][l[j] - 1] == r[j] - l[j] + 1) continue;
    			ans ++;
    			vis[l[j]][r[j]] = i;
    		}
    	}
        printf("%d\n", ans);
    	return 0;
    }
    


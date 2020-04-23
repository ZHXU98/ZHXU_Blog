## 2019杭电多校第五场_permutation_1_(全排列)

##### 给你n个数 1到n 你全排列相邻差序列 有字典序第k大差序列的 输出

数据到1e4 大于 8 的直接暴力 n 后面 1 ~ n-1 的第k-1排列 就是解  
然后 1 到 8 打表处理

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    const int maxn = 1e6 + 6;
    
    struct node {
    	int num[10];
    	char st[10];
    	bool operator < (const node &a) const {
    		return strcmp(st, a.st) < 0;
    	}
    } a[10][maxn];
    int b[30];
    
    void init() {
    	for(int k = 2; k <= 8; k ++) {
    		for(int i = 1; i <= k; i ++) b[i] = i;
    		int cnt = 1;
    		do {
    			for(int i = 1; i <= k; i ++) {
    				a[k][cnt].num[i] = b[i];
    				if(i != 1) a[k][cnt].st[i-2] = b[i] - b[i-1] + '*';
    			}
    			a[k][cnt].st[k-1] = '\0';
    			cnt ++;
    		} while(next_permutation(b + 1, b + k + 1));
    		sort(a[k] + 1,a[k] + cnt);
    	}
    }
    
    signed main() {
    	init();
    	int cas, n, k;
    	cin >> cas;
    	while(cas --) {
    		cin >> n >> k;
    		if(n > 8) {
    			b[1] = n;
    			for(int i = 2; i <= n; i ++) b[i] = i - 1;
    			while(--k) next_permutation(b + 1, b + 1 + n);
    			for(int i = 1; i <= n; i ++) {
    				if(i != 1) cout << " ";
    				cout << b[i];
    			}
    			puts("");
    		} else {
    			for(int i = 1; i <= n; i ++) {
    				if(i != 1) cout << " ";
    				cout << a[n][k].num[i];
    			}
    			puts("");
    		}
    	}
    	return 0;
    }
    


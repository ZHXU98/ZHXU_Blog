## 2019_牛客多校第九场_E_All_men_are_brothers_(并查集__数学)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190815193932686.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
每4个 互相不能是朋友  
考虑并查集维护  
我们正着数 有点难 正好 我们朋友关系 是一个一个加进去的 这样就可以 每次减去我们加入这2个集合产生的冲突 + 剩下集合贡献出的2个  
如果每次在数就超时了 而且 我们组合数 减去的 是 所有大于2的集合 贡献2个元素的量 所以开一个变量 存下 加的时候 把这2个要加入集合去掉 合并完并查集
把题面合并 就o1 处理组合数  
题解搞得平方没有看懂 以上好理解点感觉

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 10;
    const int mod = 1;
    #define int long long
    
    int pre[maxn], siz[maxn];
    int c[maxn][5];
    
    void init() {
    	for(int i = 1; i < maxn; i ++) pre[i] = i, siz[i] = 1;
    	c[1][0] = c[1][1] = 1;
        for(int i = 2; i < maxn; i ++){
            c[i][0] = 1;
            for(int j = 1; j <= 4; j ++){
                c[i][j] = c[i-1][j] + c[i-1][j-1];
            }
        }
    }
    
    int finds(int x) {
    	return x == pre[x] ? x : pre[x] = finds(pre[x]);
    }
    
    void unions(int x, int y) {
    	x = finds(x), y = finds(y);
    	pre[x] = y,siz[y] += siz[x];
    }
    
    signed main() {
    	int n, m;
    	cin >> n >> m;
    	init();
    	int ans = c[n][4];
    	cout << ans << endl;
    	int ps = 0;
    	for(int i = 1, a, b; i <= m; i ++) {
    		cin >> a >> b;
    		if(finds(a) == finds(b)) {
    			cout << ans << endl;
    		} else {
    			int t1 = 0;
    			if(siz[finds(a)] >= 2) t1 = c[siz[finds(a)]][2];
    			int t2 = 0;
    			if(siz[finds(b)] >= 2) t2 = c[siz[finds(b)]][2];
    			
    			int ss = siz[finds(a)] + siz[finds(b)];
    			int ks = siz[finds(a)] * siz[finds(b)];
    			int ts = ps + t1 + t2;
    		//	cout << "    " << ts << endl;
    			int tmp = ks * (c[n - ss][2] + ts); 
    		//	cout << tmp << endl;
    			ans -= tmp;
    			cout << ans << endl;
    			ps = ps - c[ss][2] + t1 + t2;
    			unions(a, b);
    		}
    	}
    	return 0;
    }
    


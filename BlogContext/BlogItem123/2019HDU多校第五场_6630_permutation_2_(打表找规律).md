## 2019HDU多校第五场_6630_permutation_2_(打表找规律)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190806145441851.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
以下是我打的表。。。。。![在这里插入图片描述](https://img-
blog.csdnimg.cn/20190806145455906.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080614550026.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
眼瞎了 别笑orz  
我们发现 第一行 除了最后一个就是解。。。。。  
然后 l == 1 or r == 1 去错开一行 r - l + 1 就是要的解

    
    
    #include<bits/stdc++.h>
    #define N 1000010
    using namespace std;
    const int mod = 998244353;
    int mp[15][15];
    const int maxn = 1e5 + 5;
    int a[maxn];
    
    int main() {
    //	int n;
    //	cin >> n;
    //	int a[12];
    //	for(int i = 1; i <= n; i ++) {
    //		a[i] = i;
    //	}
    //
    //	for(int x = 1; x <= n; x ++) {
    //		for(int y = x + 1; y <= n; y ++) {
    //			int ans = 0;
    //			do {
    //				int f = 1;
    //				if(a[1] == x && a[n] == y) {
    //					for(int z = 2; z <= n; z ++) {
    //						if(abs(a[z] - a[z - 1]) > 2)
    //							{f = 0; break;}
    //					}
    //					if(f) ans ++;
    //				}
    //			}while(next_permutation(a + 1, a + 1 + n));
    //			mp[x][y] = mp[y][x] = ans;
    //		}
    //	}
    //	cout << "    ";
    //	for(int i = 1; i <= n; i ++)
    //		cout << "  "<< i << " ";
    //	cout << endl;
    //	cout << endl;
    //	for(int i = 1; i <= n; i ++) {
    //		cout << i << "     ";
    //		for(int j = 1; j <= n; j ++) {
    //			cout << mp[i][j] << "   ";
    //		}cout << endl;
    //	}
    	int t;
    	cin >> t;
    	int n, l, r;
    
    	a[0] = 0;
    	a[1] = 1;
    	a[2] = 1;
    	a[3] = 1;
    	for(int i = 4; i < maxn; i ++) {
    		a[i] = (a[i - 1] + a[i - 3]) % mod ;
    	}
    //	for(int i = 1; i <= 15; i ++) cout << a[i] << " " ; cout << endl;
    	while(t --) {
    		cin >> n >> l >> r;
    		if(l != 1) l++;
    		if(r != n) r--;
    			cout << a[r -  l + 1] << endl;
    		
    	}
    	return 0;
    }
    


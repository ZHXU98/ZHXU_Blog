## 2018_ICPC_沈阳_C_Insertion_Sort_(数学推导规律)

常见的数学规律 要不开跟 平方 做差 求和 要不就是 位置关系 作差 作和 差分  
打表 之后 就是考验眼力和脑子能不能转的时候了  
  
现有一段函数，要求输入一个数组A和一个k，进行一次题目给出的冒泡模仿插入 进行k次。  
问给你三个数，n，k，mod，你在1-n的全排列中，有多少个序列运行这个函数之后其最长上升子序列的长度大于等于(n-1)，最后的结果对mod取模。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820191950631.png)  
然后 我们上下做差 能看出是等差数列  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820192238144.png)  
首项是 i∗i!i * i!i∗i!，然后差是 2∗i!2 * i!2∗i!  
那么 对应第 k 列 n 行 来说 它就是 k!+k! +k!+ 等差数列的和  
这样开始化简公式  
矩阵 对角线 是 K！ 我们差 也是从这里开始的 之后就是细心推了 //看上图  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190820192953782.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
要注意 相对位置 该加对加好  
不要少去mod 有负数 加成正的在处理

    
    
    #include<bits/stdc++.h>
    using namespace std;
    #define int long long
    typedef long long ll;
     
    int n, m , mod;
     
    signed main() {
    	int cas;
    	int tt = 1;
    	cin >> cas;
    	while(cas --) {
    		cin >> n >> m >> mod;
    		int k = 1;
    		if(m >= n - 1) m = n;
    		for(int i = 2; i <= m; i ++) {
    			k *= i % mod;
    			k %= mod;
    		}
    		cout << "Case #" << tt ++ << ": " ;
    		if(m >= n - 1) cout << k << endl;
    		else cout << (k)*(n * n % mod - (m + 1) * n % mod + m + 1 + mod) % mod << endl;
    	}
    	return 0;
    }
    

后面有遇到一次 用了个不太一样的做法  
差的首相是 后面的阶乘 - 前一项 直接for过去了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e3 + 5;
    const int INF = 0x3f3f3f3f;
    long long n, m, k;
    long long mod;
    int cas;
    
    
    signed main() {
        int tt = 1;
        cin >> cas;
        while(cas --) {
            cin >> n >> m >> mod;
            long long ans = 1;
            for(int i = 1; i <= n; i ++) {
                ans = ans * i % mod;
            }
            if(m >= n - 1) {
                cout << "Case #" << tt ++ << ": ";
                cout << ans << endl;
            } else {
                ans = 1;
                long long c = 2;
                long long d = 0;
                for(int i = 1; i <= m; i ++) {
                    ans = ans * i % mod;
                    c = c * i % mod;
                }
                d = (ans *(m + 1) - ans + mod) % mod;
                for(int i = 1; i <= n - m; i ++) {
                    ans = (ans + d) % mod;;
                    d = (d + c) % mod;
                }
                cout << "Case #" << tt ++ << ": ";
                cout << ans << endl;
            }
        }
        return 0;
    }
    
    


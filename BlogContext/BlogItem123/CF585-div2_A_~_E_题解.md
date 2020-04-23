## CF585-div2_A_~_E_题解

[CF585-div2](https://codeforces.com/contest/1215)

## A. Yellow Cards

A题没有啥好说的 模拟就完事了 我好菜啊 这题10分钟不出  
最少必然是尽可能分到上界 最多当然是优先罚 容量少的啦

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 1e5 + 5;
    const double eps = 1e-4;
     
    int a1, a2, k1, k2, n; 
     
    int main() {
    	cin >> a1 >> a2 >> k1 >> k2;
    	cin >> n;
    	
    	int mi1 = a1 * (k1 - 1);
    	int mi2 = a2 * (k2 - 1);
    	
    	if(n > mi1 + mi2) {
    		cout << n - mi1 - mi2 << " ";
    	} else cout << 0 << " ";
    	int ans = 0;
    	if(k1 < k2) {
    		int cq1 = n / k1;
    		if(cq1 > a1) {
    			ans = a1 + (n  - a1 * k1) / k2;
    		}
    		else ans = cq1;
    	} else {
    		int cq2 = n / k2;
    		if(cq2 > a2) {
    			ans = a2 + (n - a2 * k2) / k1;
    		}
    		else ans = cq2;
    	}
    	cout << ans << endl;
    	return 0;
    }
    

## B. The Number of Products

这题是我菜了 orz 真的菜 过CD 不过Borz  
第一思路是DP  
不过 我们首先想 正数 他直能是前一个是正数 +1，  
负数依然是不变  
负数 他肯定是遇到负数 * 前面正数 + 1；  
正数 负数的数量交换  
这样 试一试就找到dp转移了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    typedef long long ll;
    const int maxn = 2e5 + 5;
     
    int n;
    int a[maxn];
    int d1[maxn], d2[maxn];
     
    int main() {
    	scanf("%d", &n);
    	for(int i = 1; i <= n; i ++) scanf("%d", a + i);
    	ll ans1 = 0, ans2 = 0;
    	for(int i = 1; i <= n; i ++) {
    		if(a[i] > 0) {
    			d1[i] = d1[i - 1] + 1;
    			d2[i] = d2[i - 1];
    		} else {
    			d1[i] = d2[i - 1];
    			d2[i] = d1[i - 1] + 1;
    		}
    		ans1 += d1[i];
    		ans2 += d2[i];
    	}
    	cout << ans2 << " " << ans1 << endl;
    	return 0;
    } 
    

## C. Swap Letters

模拟题 虽然一开始第一反应是最小编辑距离 醉了  
找到上下不同 从后面换 后面没有就考虑多走一步 我们上下直接换 这样就能把样例1过了  
然后我也没有想到直接过了

    
    
    #include <bits/stdc++.h>
    using namespace std;
     
    const int maxn = 2e5 + 5;
     
    int n;
    char s1[maxn], s2[maxn];
    set<int > pa, pb;
    queue<pair<int, int> > ans;
     
    int main() {
    	scanf("%d", &n);
    	scanf("%s %s", s1 + 1, s2 + 1);
    	int as = 0, bs = 0;
    	for(int i = 1; i <= n; i ++) {
    		if(s1[i] == 'a') as++;
    		else bs ++;
    		if(s2[i] == 'a') as++;
    		else bs ++;
    		if(s1[i] != s2[i]) {
    			if(s2[i] == 'a') {
    				pa.insert(i);
    			} else {
    				pb.insert(i);
    			}
    		}
    	}
    	if(as%2 || bs % 2) {
    		puts("-1");
    	} else {
    		for(int i = 1, p; i <= n; i ++) {
    			if(s1[i] != s2[i]) {
    				if(s1[i] == 'a') {
    					if(pb.upper_bound(i) != pb.end()) {
    						p = *pb.upper_bound(i);
    						pb.erase(p);
    						ans.push(make_pair(i, p));
    //						cout << i << " " << p << endl;
    						swap(s1[i], s2[p]);
    					}
    					else {
    						ans.push(make_pair(i, i));
    //						cout << i << " " << i << endl;
    						swap(s1[i], s2[i]);
    						pa.erase(i), pb.insert(i);
    						i --;
    					}
    				} else {
    					if(pa.upper_bound(i) != pa.end()) {
    						p = *pa.upper_bound(i);
    						pa.erase(p);
    						ans.push(make_pair(i, p));
    //						cout << i << " " << p << endl;
    						swap(s1[i], s2[p]);
    					}
    					else {
    						ans.push(make_pair(i, i));
    //						cout << i << " " << i << endl;
    						swap(s1[i], s2[i]);
    						pb.erase(i), pa.insert(i);
    						i --;
    					}
    				}
    			}
    		}
    		printf("%d\n", ans.size());
    		while(!ans.empty()) {
    			printf("%d %d\n", ans.front().first, ans.front().second);
    			ans.pop();
    		}
    	}
    	return 0;
    }
    

## D. Ticket Game

这题 分类讨论下  
我们M想win 必须 特别极端去选 0 9  
如果这样 都能让左右和一样 那么S赢

第一情况就是 操作一样 左右直接比较和  
第二情况就是 左操作 > 右边 （cl - cr） / 2 就是可以做的操作 * 9 看下和  
第三情况就是 左操作 < 右边 （cr - cl） / 2 就是可以做的操作 * 9 看下和 对比极端能不能平就好  
问号 可以先肖掉一样多一堆  
然后M在数量多一侧 上一个9 S就同侧补0 s一直逼近  
这样分这些情况模拟就好

    
    
    #include <bits/stdc++.h>
    using namespace std;
    
    const int maxn = 2e5 + 5;
    
    int n;
    char s[maxn];
    
    int main() {
    	scanf("%d", &n);
    	scanf("%s", s + 1);
    	int cl, cr, s1, s2;
    	cl = cr = s1 = s2 = 0;
    	for(int i = 1; i <= n; i ++) {
    		if(s[i] == '?' && i <= n / 2) cl ++;
    		else if(s[i] == '?') cr ++; 
    		else if(i <= n / 2) s1 += s[i] - '0';
    		else s2 += s[i] - '0';
    	}
    	
    	if(s1 == s2) {
    		if(cl == cr) cout << "Bicarp" << endl;
    		else cout << "Monocarp" << endl;
    	} else {
    		if(cl > cr) {
    			cl -= cr; cl /= 2; cl *= 9;
    			if(s1 + cl == s2)  cout << "Bicarp" << endl;
    			else cout << "Monocarp" << endl;
    		} else {
    			cr -= cl; cr /= 2; cr *= 9;
    			if(s1 == s2 + cr)  cout << "Bicarp" << endl;
    			else cout << "Monocarp" << endl;
    		}
    	}
    	return 0;
    }
    

~~E 题没有看懂 困了 先睡了 未完待续 明天补上~~  
E题 链接  
<https://blog.csdn.net/qq_40831340/article/details/101041551>


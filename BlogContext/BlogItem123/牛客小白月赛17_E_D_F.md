## 牛客小白月赛17_E_D_F

F 题 方程不好直接解 二分求答案 (有单调性)  
D 题 数据太少 而且是静态的 直接先暴力所有的 输入完一个一个对就好

## E 题

链接：https://ac.nowcoder.com/acm/contest/1085/E  
来源：牛客网

题目描述  
小sun最近为了应付考试，正在复习图论，他现在学到了图的遍历，觉得太简单了，于是他想到了一个更加复杂的问题：

无向图有n个点，从点1开始遍历，但是规定：按照每次“走两步”的方式来遍历整个图。可以发现按照每次走两步的方法，不一定能够遍历整个图，所以现在小sun想问你，最少加几条边，可以完整的遍历整个图。

输入描述:  
第一行两个整数n，m代表图的点数和边数。

接下来m行，每行两个整数u，v代表u，v有边相连（无向边）  
输出描述:  
输出一行，代表最少要添加的边数。  
示例1

输入  
5 4  
1 2  
2 3  
3 4  
4 5  
输出  
1  
示例2

输入  
5 5  
1 2  
2 3  
3 4  
4 5  
1 5  
输出  
0  
备注:  
数据范围：

3≤n≤1e5, 2≤m≤1e5  
1≤u,v≤n  
图中点的编号从1到n。

走两步的意思：

比如现在有两条边：(1,2),(2,3)，从1开始走，只能走到1或者3。

(1->2->3),(1->2->)

题解：  
很容易发现，如果存在奇环，那么这个奇环所在的连通块所有的点都可以遍历到。

那么这个图如果存在奇数环，只需把这个奇数环所在连通块与其它连通块连接就可以了，答案是连通块个数-1.

如果不存在奇数环，那么可以将奇数个连通块连在一起构成一个奇数环，再与其它连通块连接，答案为连通块个数。

怎么求有多少个连通块，是否存在奇数环，可以用图染色的方法，如果存在奇数环，那么就存在相邻两个节点颜色一样，如果是偶数环，就不存在相邻两个节点颜色一样。

> 作者：sd197555  
>  链接：https://ac.nowcoder.com/discuss/258571?type=101&order=time&pos=&page=1  
>  来源：牛客网  
>  考察这个遍历的性质，发现如果图存在奇环，那么跟当前奇环联通的所有点全部可以遍历到。  
>
> 首先讨论图有多少个联通块，在这些联通块中，若存在一个包含奇环的联通块，那么将当前的联通块与所有的不包含奇环的联通块相连就行了，答案个数为不包含奇环的联通块个数。  
>  若所有的联通块都不包含奇环，那么可以用若干条边在k个（k为奇数）联通块之间构造一条奇环，然后再把其他的联通块跟当前的奇环相连，答案为联通块的个数。
    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    typedef long long ll;
    int n, m, k;
    int cas;
    
    int head[maxn], cnt;
    int to[maxn << 1], nxt[maxn << 1];
    
    void ade(int a, int b) {
    	to[++ cnt] = b;
    	nxt[cnt] = head[a], head[a] = cnt;
    }
    
    int dfn[maxn], ans = 0;
    bool v[maxn];
    void dfs(int u, int f, int yuan) {
    	dfn[u] = f;
    	for(int i = head[u]; i; i = nxt[i]) {
    		if(!dfn[to[i]]) dfs(to[i], f <= 1 ? 2 : 1, yuan);
    		else if(f == dfn[to[i]]) {
    			if(v[yuan]) continue;
    			else ans ++;
    			v[yuan] = 1;	
    		}
    	}
    }
    
    signed main() {
    	scanf("%d %d", &n, &m);
    	for(int i = 1, a, b; i <= m; i ++) {
    		scanf("%d %d", &a, &b);
    		ade(a, b), ade(b, a);
    	}
    	int coun = 0;
    	for(int i = 1; i <= n;  i++) {
    		if(!dfn[i]) {
    			dfs(i, 0, i);
    			coun ++;
    		}
    	}
    	if(ans) cout << coun - ans << endl;
    	else cout << coun << endl;
    	return 0;
    }
    


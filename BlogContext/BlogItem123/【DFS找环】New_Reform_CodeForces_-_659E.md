## 【DFS找环】New_Reform_CodeForces_-_659E

Berland has n cities connected by m bidirectional roads. No road connects a
city to itself, and each pair of cities is connected by no more than one road.
It is not guaranteed that you can get from any city to any other one, using
only the existing roads.  
The President of Berland decided to make changes to the road system and
instructed the Ministry of Transport to make this reform. Now, each road
should be unidirectional (only lead from one city to another).  
In order not to cause great resentment among residents, the reform needs to be
conducted so that there can be as few separate cities as possible. A city is
considered separate, if no road leads into it, while it is allowed to have
roads leading from this city.  
Help the Ministry of Transport to find the minimum possible number of separate
cities after the reform.  
Input  
The first line of the input contains two positive integers, n and m — the
number of the cities and the number of roads in Berland (2 ≤ n ≤ 100 000, 1 ≤
m ≤ 100 000).  
Next m lines contain the descriptions of the roads: the i-th road is
determined by two distinct integers xi, yi (1 ≤ xi, yi ≤ n, xi ≠ yi), where xi
and yi are the numbers of the cities connected by the i-th road.  
It is guaranteed that there is no more than one road between each pair of
cities, but it is not guaranteed that from any city you can get to any other
one, using only roads.  
Output  
Print a single integer — the minimum number of separated cities after the
reform.  
Examples  
Input  
4 3  
2 1  
1 3  
4 3  
Output  
1  
Input  
5 5  
2 1  
1 3  
2 3  
2 5  
4 3  
Output  
0  
Input  
6 5  
1 2  
2 3  
4 5  
4 6  
5 6  
Output  
1

一个连通图 跑环 有环便是0 无环便是0

dfs 搜 只要搜到 标记过的便一定成环

    
    
    #include<iostream>
    #include<cstdio>
    #include<cstring>
    #include <cmath>
    #include <map>
    #include <set>
    #include <vector>
    #include <queue>
    #include <algorithm>
    using namespace std;
    typedef long long ll;
    
    const int maxn = 100000 + 5;
    const int INF = 0x3f3f3f3f;
    int n, m;
    
    vector<int> G[maxn];
    bool vis[maxn];
    int ans, res;
    
    void dfs(int nx, int pre) {
        if (vis[nx]) {
            res = 0;
            return;
        }
        vis[nx] = 1;
        for (int i = 0; i < G[nx].size(); i++) {
            if (G[nx][i]!=pre) {
                dfs(G[nx][i], nx);
            }
        }
    }
    
    int main() {
        int st, ed;
        while (cin >> n >> m) {
            for (int i = 0; i < m; i++) {
                cin >> st >> ed;
                G[st].push_back(ed);
                G[ed].push_back(st);
            }
    
             ans = 0,res;
    
            for (int i = 1; i <= n; i++) {
                if (!vis[i]) {
                    res = 1;
                    dfs(i, 0);
                    ans += res;
                }
            }
            cout << ans << endl;
        }
        return 0;
    }
    


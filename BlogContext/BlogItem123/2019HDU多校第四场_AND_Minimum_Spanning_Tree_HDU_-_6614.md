## 2019HDU多校第四场_AND_Minimum_Spanning_Tree_HDU_-_6614

一个思路题 画了好多 才看出来 有点菜 orz

### AND Minimum Spanning Tree

##### 题面

You are given a complete graph with N vertices, numbered from 1 to N.  
The weight of the edge between vertex x and vertex y (1<=x, y<=N, x!=y) is
simply the bitwise AND of x and y. Now you are to find minimum spanning tree
of this graph.  
给了你1 到 n 边权是 2点 & 的值  
很快知道 只有 1 和 0 但是 一开始没有思考找最小 只是构造出来 wa了一发 然后脑子开始不转了 花了点时间 想到从低开始 遇到第一个二进制 0 与之前
的 100… 数据 看看能不能对其  
如果没有0不然就向上找 + 1… 能不能再n以下 就过了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 2e6 + 5;
    
    int main() {
        int t, n;
        scanf("%d", &t);
        while(t -- ) {
            scanf("%d", &n);
            
            int ejws = 0;
            int m = n;
            while(m) {
                ejws ++;
                m/=2;
            } 
            
            int q1s = 0;
            q1s = (1 << ejws) - 1;
            
            int ans = 0;
            
            if(n == q1s) ans = 1;
            printf("%d\n", ans);
            
            for(int i = 2; i <= n; i ++) {
                int ejws = 0;
                int m = i;
                int f = 0;
                while(m) {
                    if(m % 2 == 0) {
                        printf("%d", 1 << ejws);
                        f = 1;
                        break;
                    }
                    m /= 2;
                    ejws ++;
                }
                if(!f) {
                    ;if(i + 1 <= n) printf("%d", i + 1); 
                    else printf("%d", 1); 
                }
                if(i == n) puts("");
                else printf(" ");
            }
        }
        return 0;
    }
    


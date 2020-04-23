## HihoCoder_1632_Secret_Poems_北京2017_ICPC

<https://vjudge.net/problem/HihoCoder-1632>  
北京这题真的是 模拟到爆炸

    
    
    #include<bits/stdc++.h>
    #define ll long long
    using namespace std;
    const int maxn = 1e3 + 5;
    
    char mp[maxn][maxn];
    char id[maxn * maxn];
    char ans[maxn][maxn];
    int n, m;
    
    bool vis[maxn][maxn];
    int tot = 0;
    void dfs(int x, int y, int op, int f) {
     //   cout << x << " " << y << "  " << op << endl;
        id[++tot] = mp[x][y];
        if(x == n && y == n)
            return ;
        if(op == 0) {
            if(!f) {
                if(y == n)
                    dfs(x + 1, y, 3, 1);
                else
                    dfs(x, y + 1, 0, 1);
            } else {
                if(x == 1) dfs(x + 1, y - 1, 1, 1);
                if(x == n) dfs(x - 1, y + 1, 2, 1);
            }
        }
        if(op == 1) {
            if(x == n) dfs(x, y + 1, 0, 1);
            else  if(y == 1) dfs(x + 1, y, 3, 1);
            else dfs(x + 1, y - 1, 1, 0);
        }
        if(op == 2) {
            if(y == n) dfs(x + 1, y, 3, 1);
            else if(x == 1) dfs(x, y + 1, 0, 1);
            else dfs(x - 1, y + 1, 2, 0);
        }
        if(op == 3) {
            if(y == 1) dfs(x - 1, y + 1, 2, 0);
            if(y == n) dfs(x + 1, y - 1, 1, 0);
        }
    }
    
    void dfs2(int x, int y, int cnt, int op) {
        ans[x][y] = id[cnt];
        vis[x][y] = 1 ;
        if(cnt != n * n) {
            if(op == 0) {
                if(y + 1 <= n && !vis[x][y + 1])
                    dfs2(x, y + 1, cnt + 1, 0);
                else
                    dfs2(x + 1, y, cnt + 1, 1);
            }
            if(op == 1) {
                if(x + 1 <= n && !vis[x + 1][y])
                    dfs2(x + 1, y, cnt + 1, 1);
                else
                    dfs2(x, y - 1, cnt + 1, 2);
            }
            if(op == 2) {
                if(y - 1 >= 1 && !vis[x][y - 1])
                    dfs2(x, y - 1, cnt + 1, 2);
                else
                    dfs2(x - 1, y, cnt + 1, 3);
            }
            if(op == 3) {
                if(x - 1 >= 1 && !vis[x - 1][y])
                    dfs2(x - 1, y, cnt  + 1, 3);
                else
                    dfs2(x, y + 1, cnt + 1, 0);
            }
        }
    }
    
    int main() {
        while(scanf("%d", &n) != EOF) {
            tot = 0;
            memset(vis, 0, sizeof(vis));
            for(int i = 1; i <= n; i ++)
                scanf("%s", mp[i] + 1);
            dfs(1, 1, 0, 0);
            dfs2(1, 1, 1, 0);
            for(int i = 1; i <= n; i ++) {
                for(int j = 1; j <= n; j++) {
                    printf("%c", ans[i][j]);
                }
                puts("");
            }
        }
        return 0;
    }
    


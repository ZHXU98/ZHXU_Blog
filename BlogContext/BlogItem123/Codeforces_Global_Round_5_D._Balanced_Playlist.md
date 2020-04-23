## Codeforces_Global_Round_5_D._Balanced_Playlist

## D. Balanced Playlist

<https://codeforces.com/contest/1237/problem/D>  
Every morning, you choose a track. The playlist then starts playing from this
track in its usual cyclic fashion. At any moment, you remember the maximum
coolness x of already played tracks. Once you hear that a track with coolness
strictly less than x2 (no rounding) starts playing, you turn off the music
immediately to keep yourself in a good mood.  
他要听歌 从i开始 到 j 结束 这个区间的最大值/2 < a[j] 这个值 他就可以听这个歌  
我们慢慢就可以想到 j这个位置 是单调往后的 没有必要每次都找  
比如 5 2 9 18 24 2  
我们 5开始 他到2 就停了 但是 2开始 可以一直到24  
之后每个数据 除了 最后一个 都可以到 24  
所以这题就变成维护一个最远端了  
i一直往后 到 j 最大值/2 < a[j] 就可以放入 j

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 3e5 + 5;
    const int INF = 0x3f3f3f3f;
    int n, m;
    
    int tree[maxn << 2];
    int a[maxn];
    
    inline void push_up(int rt) {
        tree[rt] = max(tree[rt << 1], tree[rt << 1 | 1]);
    }
    
    void build(int l, int r, int rt) {
        if(l == r) {
            tree[rt] = a[l];
            return ;
        }
        int mid = l + r >> 1;
        if(l <= mid)
            build(l, mid, rt << 1);
        if(r > mid)
            build(mid + 1, r, rt << 1 | 1);
        push_up(rt);
    }
    
    int quemax(int L, int R, int l, int r, int rt) {
        if(L <= l && R >= r)
            return tree[rt];
        int mid = l + r >> 1;
        int res = -1;
        if(L <= mid)
            res = max(res, quemax(L, R, l, mid, rt << 1));
        if(R > mid)
            res = max(res, quemax(L, R, mid + 1, r, rt << 1 | 1));
        return res;
    }
    
    signed main() {
        cin >> n;
        for(int i = 1; i <= n; i ++) cin >> a[i];
        for(int i = n + 1; i <= 2 * n; i ++) a[i] = a[i - n];
        for(int i = 2 * n + 1; i <= 3 * n; i ++) a[i] = a[i - n];
        n *= 3;
        build(1, n, 1);
        int j = 2;
        for(int i = 1; i <= n/3; i ++) {
            for(; j <= 3 * n; j ++) {
                if(quemax(i, j, 1, n, 1) > a[j] * 2) break;
            }
            if(j - i > n/3*2) cout << -1 << " ";
            else cout << j - i << " ";
        }
        return 0;
    }
    
    


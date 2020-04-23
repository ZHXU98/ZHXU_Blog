## 【扫描线】_Atlantis_POJ_-_1151__HDU_-_1255_覆盖的面积__POJ_-_1177_Pictur

poj 1151  
扫描线 面积并

    
    
    #include <iostream>
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    #define db double
    
    const int maxn = 2005;
    
    struct node {
    	double lx,rx,y;
    	int s;
    	node() {}
    	node(db _lx, db _rx, db _y, int _s) {
    		lx=_lx, rx=_rx, y=_y, s=_s;
    	}
    	bool operator < (const node &S) const {
    		return y<S.y;
    	}
    } lines[maxn];
    
    db X[maxn];
    
    db sum[maxn<<2],flag[maxn<<2];
    
    void push_up(int rt, int l, int r) {
    	if(flag[rt])
    		sum[rt] = X [ r+1] - X[ l];
    	else if(l == r)
    		sum[rt] = 0;
    	else
    		sum[rt] = sum[rt << 1] + sum[rt << 1|1];
    
    }
    
    void update(int L, int R, int l, int r, int rt, int c) {
    	if(L <= l && r <= R) {
    		flag[rt]+=c;
    		push_up(rt, l, r);
    		return ;
    	}
    	int mid = (l + r) >> 1;
    //	if(R <= mid) update(L, R, l, mid, rt << 1, c);
    //	else if(L > mid) update(L, R, mid + 1, r, rt << 1|1, c);
    //	else {
    //		update(L, R, l, mid, rt << 1, c);
    //		update(L, R, mid + 1, r, rt << 1|1, c);
    //	}
    	if(R <= mid) update(L, R, l, mid, rt << 1, c);
    	if(L > mid) update(L, R, mid + 1, r, rt << 1|1, c);
    	push_up(rt, l, r);
    }
    
    int main() {
    	int cas=1;
    	int n;
    
    	while(cin >> n, n) {
    		int cnt = 0;
    		memset(sum, 0, sizeof sum);
    		memset(flag, 0, sizeof flag);
    		for(int i = 1; i <= n; i ++ ) {
    			double x1, x2, y1, y2;
    			cin >> x1 >> y1 >> x2 >> y2;
    			X[ ++cnt ] = x1;
    			lines[ cnt ] = node(x1, x2, y1, 1);
    			X[ ++cnt ]= x2;
    			lines[ cnt ] = node(x1, x2, y2, -1);
    		}
    		sort(X + 1, X + 1 + cnt);
    		sort(lines + 1, lines + 1 + cnt);
    		int pos = unique(X + 1, X + 1 + cnt) - X;
    		db ans = 0;
    
    		for(int i = 1; i < cnt; i ++ ) {
    			int l = lower_bound(X + 1, X + pos, lines[ i ].lx) - X;
    			int r = lower_bound(X + 1, X + pos, lines[ i ].rx) - X - 1;
    			update(l, r, 1, cnt, 1, lines[i].s); // cnt这里剪不剪1都行。。 反正多开没有事情 
    			ans += sum[ 1 ] * (lines[ i+1 ].y - lines[ i ].y);
    		}
    		cout << "Test case #" << cas++ << endl;
    		cout << "Total explored area: ";
    		printf("%.2lf\n\n",ans);
    	}
    	return 0;
    }
    

**覆盖的面积 HDU - 1255**  
面积交

    
    
    #include <iostream>
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    #define db double
    const int maxn = 2005;
    
    struct node {
    	double lx,rx,y;
    	int s;
    	node() {}
    	node(db _lx, db _rx, db _y, int _s) {
    		lx=_lx, rx=_rx, y=_y, s=_s;
    	}
    	bool operator < (const node &S) const {
    		return y<S.y;
    	}
    } lines[maxn];
    
    db X[maxn];
    
    db sum[maxn<<2],flag[maxn<<2];
    db sum2[maxn<<2];
    
    void push_up(int rt, int l, int r) {
    	if(flag[rt] >= 2) {
    		sum[rt] = sum2[rt] = X[r + 1] - X[l];
    	} else if(flag[rt] == 1) {
    		sum[rt] = X[r + 1] - X[l];
    		if(l == r) sum2[rt] = 0;
    		else sum2[rt] = sum[rt << 1] + sum[rt << 1|1];
    	} else {
    		if(l == r) sum2[rt] = sum[rt] = 0;
    		else {
    			sum[rt] = sum[rt << 1] + sum[rt << 1|1];
    			sum2[rt] = sum2[rt << 1] + sum2[rt <<1|1];
    		}
    	}
    }
    
    void update(int L, int R, int l, int r, int rt, int c) {
    	if(L <= l && r <= R) {
    		flag[rt]+=c;
    		push_up(rt, l, r);
    		return ;
    	}
    	int mid = (l + r) >> 1;
    	if(L <= mid) update(L, R, l, mid, rt << 1, c);
    	if(R > mid) update(L, R, mid + 1, r, rt << 1|1, c);
    	push_up(rt, l, r);
    }
    
    int main() {
    	int cas;
    	int n;
    	cin >> cas;
    	while(cas --) {
    		cin >> n;
    		int cnt = 0;
    		memset(sum, 0, sizeof sum);
    		memset(flag, 0, sizeof flag);
    		memset(sum2, 0 ,sizeof sum2);
    		for(int i = 1; i <= n; i ++ ) {
    			double x1, x2, y1, y2;
    			cin >> x1 >> y1 >> x2 >> y2;
    			X[ ++cnt ] = x1;
    			lines[ cnt ] = node(x1, x2, y1, 1);
    			X[ ++cnt ]= x2;
    			lines[ cnt ] = node(x1, x2, y2, -1);
    		}
    		sort(X + 1, X + 1 + cnt);
    		sort(lines + 1, lines + 1 + cnt);
    		int pos = unique(X + 1, X + 1 + cnt) - X;
    		db ans = 0;
    
    		for(int i = 1; i < cnt; i ++ ) {
    			int l = lower_bound(X + 1, X + pos, lines[ i ].lx) - X;
    			int r = lower_bound(X + 1, X + pos, lines[ i ].rx) - X - 1;
    			update(l, r, 1, cnt, 1, lines[i].s);
    			ans += sum2[ 1 ] * (lines[ i+1 ].y - lines[ i ].y);
    		}
    		printf("%.2lf\n",ans);
    	}
    	return 0;
    }
    

poj 1177 周长并

    
    
    #include <iostream>
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    #define db double
    
    const int maxn = 1e4 + 5;
    
    struct node {
        double lx, rx, y;
        int s;
        node() {}
        node(db _lx, db _rx, db _y, int _s) {
            lx = _lx, rx = _rx, y = _y, s = _s;
        }
        bool operator < (const node &S) const {
            return y < S.y;
        }
    } lines[3][maxn];
    
    db X[2][maxn];
    db sum[maxn << 2], flag[maxn << 2];
    
    void push_up(int l, int r, int rt, int p) {
        if(flag[rt])
            sum[rt] = X[p][r + 1] - X[p][l];
        else if(l == r)
            sum[rt] = 0;
        else
            sum[rt] = sum[rt << 1] + sum[rt << 1 | 1];
    }
    
    void update(int L, int R, int l, int r, int rt, int v, int p) {
        if(L <= l && r <= R) {
            flag[rt] += v;
            push_up(l, r, rt, p);
            return;
        }
    
        int mid = l + r >> 1;
        if(L <= mid)
            update(L, R, l, mid, rt << 1, v, p);
        if(R > mid)
            update(L, R, mid + 1, r, rt << 1 | 1, v, p);
        push_up(l, r, rt, p);
    }
    
    int main() {
        int n;
        while(cin >> n) {
            for(int i = 1; i <= n; i ++) {
                double x1, x2, y1, y2;
                cin >> x1 >> y1 >> x2 >> y2;
                X[0][i] = x1, X[0][i + n] = x2;
                X[1][i] = y1, X[1][i + n] = y2;
                lines[0][i] = node(x1, x2, y1, 1);
                lines[0][i + n] = node(x1, x2, y2, -1);
                lines[1][i] = node(y1, y2, x1, 1);
                lines[1][i + n] = node(y1, y2, x2, -1);
            }
            n <<= 1;
            int m[5];
            sort(X[0] + 1, X[0] + 1 + n);
            m[0] = unique(X[0] + 1, X[0] + 1 + n) - X[0] - 1;
            sort(X[1] + 1, X[1] + 1 + n);
            m[1] = unique(X[1] + 1, X[1] + 1 + n) - X[1] - 1;
            sort(lines[0] + 1, lines[0] + 1 + n);
            sort(lines[1] + 1, lines[1] + 1 + n);
    
            int ans = 0;
    
            for(int j = 0; j < 2; j++) {
                int t = 0, last = 0;
                memset(sum, 0, sizeof sum);
                memset(flag, 0, sizeof flag);
                for(int i = 1; i <= n; i ++) {
                    int l = lower_bound(X[j] + 1, X[j] + m[j], lines[j][i].lx) - X[j];
                    int r = lower_bound(X[j] + 1, X[j] + m[j], lines[j][i].rx) - X[j] - 1;
                    update(l, r, 1, m[j], 1, lines[j][i].s, j);
                    t += abs(last - sum[1]);
                    last = sum[1];
                }
                ans += t;
            }
            cout << ans << endl;
        }
        return 0;
    }
    
    

窗内得星星

    
    
    #include <iostream>
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    
    const int maxn = 2e4 + 5;
    
    struct node {
    	int x, ly, ry;
    	int s;
    	node() {}
    	node(int _x, int _ly, int _ry, int _s) {
    		x = _x, ly = _ly, ry = _ry, s = _s;
    	}
    	bool operator < (const node &S) const {
    		return x < S.x || (x == S.x && s > S.s);
    	}
    } line[maxn];
    
    int Y[maxn];
    int sum[maxn << 2], flag[maxn << 2];
    
    void push_up(int rt) {
    	sum[rt] = max(sum[rt << 1], sum[rt << 1 | 1]);
    } 
    
    void push_down(int l, int r, int rt) { 
    	if(flag[rt] == 0) return;
    	sum[rt] += flag[rt];
    	if(l != r) {      
    		flag[rt << 1] += flag[rt];
    		flag[rt << 1 | 1] += flag[rt];
    	}
    	flag[rt] = 0;
    }
    
    void updata(int L, int R, int l, int r, int rt, int v) {
    	if(L <= l && r <= R) {
    		flag[rt] += v;
    		return;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid)
    		updata(L, R, l, mid, rt << 1, v);
    	if(R > mid)
    		updata(L, R, mid + 1, r, rt << 1 | 1, v);
    	push_down(l, mid, rt << 1);
    	push_down(mid + 1, r, rt << 1 | 1);
    	push_up(rt);
    }
    
    int main() {
    	int n, w, h;
    	int cas;
    	cin >> cas;
    	while(cas --) {
    		memset(sum, 0, sizeof(sum));
    		memset(flag, 0, sizeof(flag));
    		cin >> n >> w >> h;
    		for(int i = 1, x, y, c; i <= n; i ++) {
    			cin >> x >> y >> c;
    			line[i] = node(x, y, y + h - 1, c);
    			line[i + n] = node(x + w - 1, y, y + h - 1, -c);
    			Y[i] = y + h - 1;
    			Y[i + n] = y;
    		}
    		n <<= 1;
    		sort(Y + 1, Y + 1 + n);
    		int tot = unique(Y + 1, Y + 1 + n) - Y - 1;
    		sort(line + 1, line + 1 + n);
    		int ans = 0;
    		for(int i = 1; i <= n; i ++) {
    			int l = lower_bound(Y + 1, Y + 1 + tot, line[i].ly) - Y;
    			int r = lower_bound(Y + 1, Y + 1 + tot, line[i].ry) - Y;
    			updata(l, r, 1, tot, 1, line[i].s);
    			ans = max(ans, sum[1]);
    		}
    		cout << ans << endl;
    	}
    	return 0;
    }
    

图上的理解 <https://www.luogu.org/problemnew/solution/P1856>


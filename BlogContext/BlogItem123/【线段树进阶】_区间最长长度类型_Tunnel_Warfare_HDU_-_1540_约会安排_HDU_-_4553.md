## 【线段树进阶】_区间最长长度类型_Tunnel_Warfare_HDU_-_1540_约会安排_HDU_-_4553

## Tunnel Warfare HDU - 1540

<https://vjudge.net/problem/HDU-1540>  
这题 找最长连续空间 一开始默认都连续  
D 破坏一个点 断开  
Q 查询 包含X 最长的区间长度  
R 倒着恢复破坏的点

区间最长长度的题 只要理解了 只有3个状态  
1.左端连续  
2.右端连续  
3.中间连续  
一般 区间长度题就容易处理了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    
    int lmas[maxn << 2], rmas[maxn << 2], mas[maxn << 2];
    
    void build(int l, int r, int rt) {
    	mas[rt] = lmas[rt] = rmas[rt] = r - l + 1;
    	if(l == r) return ;
    	int mid = l + r >> 1;
    	build(l, mid, rt << 1), build(mid + 1, r, rt << 1 | 1);
    }
    
    void push_up(int rt, int len) {
    	lmas[rt] = lmas[rt << 1];
    	rmas[rt] = rmas[rt << 1 | 1];
    	mas[rt] = max(mas[rt << 1], mas[rt << 1 | 1]);
    	mas[rt] = max(mas[rt], lmas[rt << 1 | 1] + rmas[rt << 1]);
    	if(rmas[rt] == len / 2) rmas[rt] += rmas[rt << 1];
    	if(lmas[rt] == len - len / 2) lmas[rt] += lmas[rt << 1 | 1];
    }
    
    void updata(int L, int l, int r, int rt, int val) {
    	if(l == r) {
    		mas[rt] = lmas[rt] = rmas[rt] = val;
    		return ;
    	}
    	int mid = l + r >> 1;
    	if(L <= mid) updata(L, l, mid, rt << 1, val);
    	else updata(L, mid + 1, r, rt << 1 | 1, val);
    	push_up(rt, r - l + 1);
    }
    
    int query(int L, int l, int r, int rt) {
    	if(mas[rt] == 0 || mas[rt] == l - r + 1) return mas[rt];
    	int mid = l + r >> 1;
    	if(L <= mid) {
    		if(L >= mid - rmas[rt << 1] + 1) return rmas[rt << 1] + lmas[rt << 1 | 1];
    		else return query(L, l, mid, rt << 1);
    	} else {
    		if(L <= mid + lmas[rt << 1 | 1]) return rmas[rt << 1] + lmas[rt << 1 | 1];
    		else return query(L, mid + 1, r, rt << 1 | 1);
    	}
    }
    
    int main() {
    	int n, m;
    	while(cin >> n >> m) {
    		build(1, n, 1);
    		stack<int> sta;
    		char op[5];
    		int x;
    		for(int i = 1; i <= m; i ++) {
    			cin >> op;
    			if(op[0] != 'R') cin >> x;
    			if(op[0] == 'R') {
    				x = sta.top();
    				sta.pop();
    				updata(x, 1, n, 1, 1);
    			} else if(op[0] == 'D') {
    				updata(x, 1, n, 1, 0);
    				sta.push(x);
    			} else if(op[0] == 'Q') {
    				cout << query(x, 1, n, 1) << endl;
    			}
    		}
    	}
    	return 0;
    }
    

## 约会安排 HDU - 4553

NS约你去开房 你肯定推了DS的约定  
你要尽可能陪NS 所以有NS不冲突的情况下  
如果DS妨碍 就推掉DS的约定  
如果只是DS约你打游戏 NS不冲突的情况下 再看看DS冲不冲突 选择一个时间更新  
要注意的是 NS的时间更新是可以到DS树上的 DS之间的约定只妨碍DS  
这样就实现的NS 优先处理了

    
    
    #include <bits/stdc++.h>
    using namespace std;
    const int maxn = 1e5 + 5;
    
    int T, n, m, CAS;
    
    struct SegTree {
        int lmas[maxn << 2], rmas[maxn << 2], mas[maxn << 2], add[maxn << 2];
    
        void push_up(int rt, int len) {
            lmas[rt] = lmas[rt << 1];
            rmas[rt] = rmas[rt << 1 | 1];
            mas[rt] = max(mas[rt << 1], mas[rt << 1 | 1]);
            mas[rt] = max(mas[rt], lmas[rt << 1 | 1] + rmas[rt << 1]);
            if(rmas[rt] == len / 2)
                rmas[rt] += rmas[rt << 1];
            if(lmas[rt] == len - len / 2)
                lmas[rt] += lmas[rt << 1 | 1];
        }
    
        void build(int l, int r, int rt) {
            mas[rt] = lmas[rt] = rmas[rt] = r - l + 1;
            add[rt] = -1;
            if(l == r)
                return ;
            int mid = l + r >> 1;
            build(l, mid, rt << 1);
            build(mid + 1, r, rt << 1 | 1);
        }
    
        void push_down(int rt, int len) {
            if(add[rt] == -1)
                return ;
            lmas[rt << 1] = add[rt] * (len - len / 2);
            lmas[rt << 1 | 1] = add[rt] * (len / 2);
    
            rmas[rt << 1] = add[rt] * (len - len / 2);
            rmas[rt << 1 | 1] = add[rt] * (len / 2);
    
            mas[rt << 1] = add[rt] * (len - len / 2);
            mas[rt << 1 | 1] = add[rt] * (len / 2);
    
            add[rt << 1] = add[rt << 1 | 1] = add[rt];
            add[rt] = -1;
        }
    
        void updata(int L, int R, int l, int r, int rt, int val) {
            if(L <= l && R >= r) {
                mas[rt] = lmas[rt] = rmas[rt] = val * (r - l + 1);
                add[rt] = val;
                return ;
            }
            push_down(rt, r - l + 1);
            int mid = l + r >> 1;
            if(L <= mid)
                updata(L, R, l, mid, rt << 1, val);
            if(R > mid)
                updata(L, R, mid + 1, r, rt << 1 | 1, val);
            push_up(rt, r - l + 1);
        }
    
        int query(int l, int r, int rt, int k) {
            if(l == r)
                return l;
            int mid = l + r >> 1;
            push_down(rt, r - l + 1);
            if(mas[rt << 1] >= k)
                return query(l, mid, rt << 1, k);
            if(rmas[rt << 1] + lmas[rt << 1 | 1] >= k)
                return mid - rmas[rt << 1] + 1;
            return query(mid + 1, r, rt << 1 | 1, k);
        }
    
    //    int pid(int L, int l, int r, int rt) {
    //        if(l == r) return mas[rt];
    //        int mid = l + r >> 1;
    //        push_down(rt, r - l + 1);
    //        if(L <= mid) return pid(L, l, mid, rt << 1);
    //        else return pid(L, mid + 1, r, rt << 1 | 1);
    //    }
    
    }NS, DS;
    
    int main() {
        cin >> CAS;
        string op;
        int k, l, r, pos;
        for(int cas = 1; cas <= CAS; cas ++) {
            cin >> n >> m;
            DS.build(1, n, 1);
            NS.build(1, n, 1);
            cout << "Case " << cas << ":" << endl;
            while(m --) {
                cin >> op;
                if(op[0] == 'D') {
                    cin >> k;
                    pos = DS.query(1, n, 1, k);
                    //cout << ans << endl;
                    if(pos + k - 1 > n)
                        cout << "fly with yourself" << endl;
                    else {
                        DS.updata(pos, pos + k - 1, 1, n, 1, 0);
                        cout << pos << ",let's fly" << endl;
                    }
                } else if(op[0] == 'N') {
                    cin >> k;
                    pos = DS.query(1, n, 1, k);
                    //cout << ans << endl;
                    if(pos + k - 1 <= n) {
                        DS.updata(pos, pos + k - 1, 1, n, 1, 0);
                        NS.updata(pos, pos + k - 1, 1, n, 1, 0);
                        cout << pos << ",don't put my gezi" << endl;
                    } else {
                        pos = NS.query(1, n, 1, k);
                        if(pos + k <= n) {
                            NS.updata(pos, pos + k - 1, 1, n, 1, 0);
                            DS.updata(pos, pos + k - 1, 1, n, 1, 0);
                            cout << pos << ",don't put my gezi" << endl;
                        } else
                            cout << "wait for me" << endl;
                    }
                } else if(op[0] == 'S') {
                    cout << "I am the hope of chinese chengxuyuan!!" << endl;
                    cin >> l >> r;
                    NS.updata(l, r, 1, n, 1, 1);
                    DS.updata(l, r, 1, n, 1, 1);
                }
    //            for(int i = 1; i <= n; i ++) {
    //                cout << NS.pid(i, 1, n, 1) << " ";
    //            }cout << endl;
    //            for(int i = 1; i <= n; i ++) {
    //                cout << DS.pid(i, 1, n, 1) << " ";
    //            }cout << endl;
            }
        }
        return 0;
    }
    


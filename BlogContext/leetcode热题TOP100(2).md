# leetcode热题TOP100(2)

## [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```c
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* fast = head, *slow = head;
        while(fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow) return 1;
        }
        return 0;
    }
};
```

## [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

既然快慢指针可以找到入环点， 不妨我们画个图 慢指针dis*2 == 快指针 我们发现最后里入点的距离其实就是炳长

```c
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head, *slow = head;
        while(fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow) break;
        }
        if(fast && fast->next) {
            fast = head;
            while(fast) {
                if(fast == slow) return fast;
                fast = fast->next;
                slow = slow->next;
            }
        }
        return NULL;
    }
};
```

## [146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

有个hash 表存 val 和节点对应位置， 删除head->next 时 只要把存的key对应的hash 去除就行

```c
class LRUCache {
public:
    struct node{
        int key;
        node* pre, *next;
        node(int k) { key = k; pre = next = NULL;}
    };
    int maxcap, tot;
    node *head, *tail;
    unordered_map<int, pair<int, node*>> dic;
    
    LRUCache(int capacity) {
        tot = 0;
        maxcap = capacity;
        node* dummy1 = new node(-1);
        node* dummy2 = new node(-1);
        head = dummy1, tail = dummy2;
        head->next = tail;
        tail->pre = head;
    }

    void del(node* now) {
        now->pre->next = now->next;
        now->next->pre = now->pre;
    }

    void tail_add(node *now) {
        now->next = tail;
        now->pre = tail->pre;
        tail->pre->next = now;
        tail->pre = now;
    }
    
    int get(int key) {
        if(!dic.count(key)) return -1;
        node* now = dic[key].second;
        del(now);
        tail_add(now);
        return dic[key].first;
    }
    
    void put(int key, int value) {
        if(dic.count(key)) {
            dic[key].first = value;
            node* now = dic[key].second;
            del(now);
            tail_add(now);
        } else {
            if(tot == maxcap) {
                node* now = head->next;
                dic.erase(now->key);
                del(now);
                delete(now);
                tot --;
            }
            node *now = new node(key);
            dic[key] = make_pair(value, now);
            tail_add(now);
            tot ++;
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

链表快排可以说是稳定的， 一般考虑节点指向改变， swap val 代价通常较高

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        QuickSort(head, NULL);
        return head;
    }
    void QuickSort(ListNode* l, ListNode* r) {
        if (l == r) return;
        ListNode* cur = l; // 基元
        int key = l->val;
        ListNode* p = l->next;
        while(p != r) {
            if (p->val < key) {
                cur = cur->next; // 将比基元小的值，依次交换到前面
                swap(p->val, cur->val);
            }
            p = p->next;
        }
        swap(l->val, cur->val);
        QuickSort(l, cur);
        QuickSort(cur->next, r);
    }
};
```

改变指针指向的

```c
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        return quickSortList(head);
    }

    ListNode* get_tail(ListNode* head) {
        while(head->next != NULL) head = head->next;
        return head;
    }

    ListNode* quickSortList(ListNode* head) {
        if(head == NULL || head->next == NULL) return head;
        
        ListNode* left = new ListNode(-1);
        ListNode* mid = new ListNode(-1);
        ListNode* right = new ListNode(-1);
        ListNode* ltail = left;
        ListNode* mtail = mid;
        ListNode* rtail = right;
        
        int k = head->val;
        for(ListNode* p = head; p != NULL; p = p->next) {
            if(p->val < k) ltail->next = p, ltail = ltail->next;
            else if(p->val == k) mtail->next = p, mtail = mtail->next;
            else rtail->next = p, rtail = rtail->next;
        }
        
        ltail->next = NULL, mtail->next = NULL, rtail->next = NULL;
        
        left->next = quickSortList(left->next);
        right->next = quickSortList(right->next);
        
        get_tail(left)->next = mid->next;
        get_tail(left)->next = right->next;
        return left->next;
    }
};
```

## [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

```c
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int fu = 1, zh = 1, res = -0x3f3f3f3f;
        for(auto i : nums) {
            if(i < 0) {
                int t = zh;
                zh = fu;
                fu = t;
            }
            zh = max(zh * i, i);
            fu = min(fu * i, i);
            res = max(zh, res);
        }
        return res;
    }
};
```



## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)

辅助栈 存最小就行

```c
class MinStack {
public:
    stack<int> mins;
    stack<int> s;
    MinStack() {}
    void push(int x) {
        if(!mins.empty()) mins.push(min(x, mins.top()));
        else mins.push(x);
        s.push(x);
    }
    void pop() {mins.pop();s.pop();}
    int top() { return s.top();}
    int getMin() {return mins.top();}
};

```

## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

a + b + c + b == b + c + a + c;

```c
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(!headA || !headB) return NULL;
        ListNode* p1 = headA, *p2 = headB;
        while(p1 != p2) {
            if(!p1) p1 = headB;
            else p1 = p1->next;
            if(!p2) p2 = headA;
            else p2 = p2->next;
        }
        return p1;
    }
};
```

## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

摩尔投票

```c
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int f = 0, res;
        for(auto i : nums) {
            if(f == 0) res = i;
            if(res != i) f --;
            else f++;
        } 
        return res;
    }
};
```

## [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

记录前一天的 每次对比继承就行

```c
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        int premax = 0, nowmax = 0;
        for(int i = 0; i < n; i ++) {
            int tmp = nowmax;
            nowmax = max(premax + nums[i], nowmax);
            premax = tmp;
        }
        return max(nowmax, premax);
    }
};
```

## [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

裸DFS

```c
class Solution {
public:
    void dfs(int x, int y, int n, int m, vector<vector<char>>& mp) {
        if(x < 0 || y < 0 || x >= n || y >= m) return ;
        if(mp[x][y] != '1') return ;
        mp[x][y] = '2';
        dfs(x + 1, y, n, m, mp);
        dfs(x - 1, y, n, m, mp);
        dfs(x, y + 1, n, m, mp);
        dfs(x, y - 1, n, m, mp);
    }
    int numIslands(vector<vector<char>>& grid) {
        if(grid.size() == 0) return 0;
        int n = grid.size(), m = grid[0].size();
        int res = 0;
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(grid[i][j] == '1') {
                    res ++;
                    dfs(i, j, n, m, grid);
                }
            }
        }
        return res;
    }
};
```

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

递归 + 迭代

```c
class Solution {
public:
    ListNode* sol(ListNode* head) {
        if(!head || !head->next) return head;
        ListNode* t = sol(head->next);
        head->next->next = head;
        head->next = NULL;
        return t;
    }
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL, *cur = head;
        while(cur) {
            ListNode* t = cur->next;
            cur->next = pre;
            pre = cur;
            cur = t;
        }
        return sol(head);
    }
};
```

## [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

dfs判环, 扫过的标记-1 就行

```c
class Solution {
public:

    bool dfs(int x, vector<vector<int>>& G, vector<int>& vis) {
        if(vis[x] == 1) return 1;
        if(vis[x] == -1) return 0;
        int res = 0;
        vis[x] = 1;
        for(int i = 0; i < G[x].size(); i ++) {
            res |= dfs(G[x][i], G, vis);
        }
        vis[x] = -1;
        return res;
    }

    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int> > G(numCourses, vector<int>(0));
        vector<int> vis(numCourses, 0);
        for(int i = 0; i < prerequisites.size(); ++ i) 
            G[prerequisites[i][0]].push_back(prerequisites[i][1]);
        for(int i = 0; i < numCourses; ++ i) 
            if(!vis[i]) if(dfs(i, G, vis)) return 0;
        return 1;
    }
};
```

## [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

Trie 模板

```c
class Trie {
public:
    /** Initialize your data structure here. */
    Trie* next[26];
    bool ok;
    Trie() {memset(next, 0, sizeof next), ok = 0;}
    /** Inserts a word into the trie. */
    void insert(string word) {
        Trie* root = this;
        for(int i = 0; i < word.size(); i ++) {
            if(root->next[word[i] - 'a']) root = root->next[word[i] -'a'];
            else root = root->next[word[i] - 'a'] = new Trie();
        }
        root->ok = 1;
    }
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trie* root = this;
        for(int i = 0; i < word.size(); i ++) {
            if(root->next[word[i] - 'a']) root = root->next[word[i] - 'a'];
            else return 0;
        }
        return root->ok;
    }
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trie* root = this;
        for(int i = 0; i < prefix.size(); i ++) {
            if(root->next[prefix[i] - 'a']) root = root->next[prefix[i] - 'a'];
            else return 0;
        }
        return 1;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

## [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

快排K大

```c
class Solution {
public:

    int qsort(vector<int> nums, int l, int r, int k) {
        if(l == r) return nums[l];
        int v = nums[l];
        int p = l;
        for(int i = l + 1; i <= r; i ++) {
            if(nums[i] < v) {
                p ++;
                swap(nums[i],nums[p]);
            }
        }
        swap(nums[l], nums[p]);
        if(p == k) return nums[p];
        else if(k < p) return qsort(nums, l, p - 1, k);
        else return qsort(nums, p + 1, r, k);
    }

    int findKthLargest(vector<int>& nums, int k) {
        return qsort(nums , 0, nums.size() - 1, nums.size() - k);
    }
};
```

## [221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

选 i - 1,  j - 1, (i - 1, j - 1) 的最小值 + 1 作为答案

```c
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int n = matrix.size();
        if(n == 0) return 0;
        int m = matrix[0].size();
        vector<int> dp(m, 0);
        int res = 0;
        for(int i = 0, pre = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                int t = dp[j];
                if(matrix[i][j] == '1') {
                    if(j > 0) dp[j] = min(dp[j - 1], min(dp[j], pre)) + 1;
                    else dp[j] = 1;
                }
                else dp[j] = 0;
                res = max(dp[j], res);
                pre = t;
            }
        }
        return res * res;
    }
};
```

## [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)

直接dfs交换

```c
class Solution {
public:
    void dfs(TreeNode* root) {
        if(!root) return ;
        TreeNode* t = root->right;
        root->right = root->left;
        root->left = t;
        dfs(root->left), dfs(root->right);
    }
    TreeNode* invertTree(TreeNode* root) {
        dfs(root);
        return root;
    }
};
```

## [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

o1 空间的 我服了 反转后面链表? 然后指针对着跑....

```c
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        stack<ListNode*> s;
        ListNode* p = head;
        while(p) s.push(p), p = p->next;
        while(!s.empty()) 
            if(s.top()->val != head->val) return 0; 
            else s.pop(), head = head->next;
        return 1;
    }
};
```

## [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

找到某个左树or右树有pq 就return 这个点(1),  同时有 我们最低点直接返回(root);

```c
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root) return NULL;
        if(root == p || root == q) return root;
        TreeNode* l = lowestCommonAncestor(root->left, p, q);
        TreeNode* r = lowestCommonAncestor(root->right, p, q);
        if(l && r) return root;
        if(l) return l;
        if(r) return r;
        return NULL;
    }
};
```

## [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)

```c
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> pre(n, 1);
        for(int i = 0; i < nums.size(); ++ i) {
            if(i == 0) pre[i] = nums[i];
            else pre[i] = nums[i] * pre[i - 1];
        }
        int r = 1;
        for(int i = (int)nums.size() - 1; i >= 0; i --) {
            int t = nums[i];
            if(i != 0) nums[i] = pre[i - 1] * r;
            else nums[i] = r;
            r = t * r;
        }
        return nums;
    }
};
```

## [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

模板

```c
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> que;
        vector<int> res;
        for(int i = 0; i < k; i ++) {
            while(!que.empty() && nums[que.back()] <= nums[i]) que.pop_back();
            que.push_back(i);
        }
        res.push_back(nums[que.front()]);
        for(int i = k; i < nums.size(); i ++) {
            if(i - que.front() >= k) que.pop_front();
            while(!que.empty() && nums[que.back()] <= nums[i]) que.pop_back();
            que.push_back(i);
            res.push_back(nums[que.front()]);
        }
        return res;
    }
};
```

## [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

$n + m$的算法 还能二分醉了

```c
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int row = 0;
        if(matrix.size() == 0) return 0;
        int col = matrix[0].size() - 1;
        while (row < matrix.size() && col >= 0) {
            if (matrix[row][col] > target) {
                col --;
            } else if (matrix[row][col] < target) {
                row ++;
            } else { 
                return true;
            }
        }
        return false;
    }
};
```

## [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

dfs 好像可以拉格朗日输处理 但是我不会 公式不记得了

```c
class Solution {
public:
    void dfs(int x, int p, unordered_set<int>& v, int& ans) {
        if(p == 1) {
            if(v.count(x)) ans = 0;
            return ;
        }
        for(auto i: v) {
            if(x - i >= 0) 
                dfs(x - i, p - 1, v, ans);
        }
    }
    int numSquares(int n) {
        unordered_set<int> v;
        for(int i = 1; i * i <= n; i ++) v.insert(i * i);
        int res = 5;
        for(int i = 1; i <= 4; i ++) {
            dfs(n, i, v, res);
            if(res != 5) return i;
        }
        return res;
    }
};
```

## [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

直接位移非0数据到p位置 p ++ 就行

```c
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int p = 0;
        for(int i = 0; i < nums.size(); i ++) {
            if(nums[i]) nums[p ++] = nums[i];
        }
        for(int i = p; i < nums.size(); i ++) nums[i] = 0;
    }
};
```

## [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

直接二分吧

```c
class Solution {
public:

    int chk(int x, vector<int>& a) {
        int res = 0;
        for(auto i : a) {
            if(i <= x) res ++;
        }
        return res;
    }

    int findDuplicate(vector<int>& nums) {
        int l = 1, r = nums.size();
        while(l < r) {
            int mid = l + r >> 1;
            if(chk(mid, nums) > mid) r = mid;
            else l = mid + 1; 
        }
        return l;
    }
};
```

## [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

```c
class Codec {
public:
    string rserialize(TreeNode* root, string &str) {
        if (root == NULL) {
            str += "#,";
        } else {
            str += to_string(root->val) + ",";
            str = rserialize(root->left, str);
            str = rserialize(root->right, str);
        }
        return str;
    }
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string ans;
        return rserialize(root, ans);
    }

    TreeNode* rdeserialize(TreeNode* root, vector<string>& vals, int &p) {
        if (vals[p] == "#") {
            return NULL;
        } else {
            root = new TreeNode(stoi(vals[p]));
            root->left = rdeserialize(root->left, vals, ++p);
            root->right = rdeserialize(root->left, vals, ++p);
        }
        return root;
    }
    
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        TreeNode* root = NULL;
        string tmp = "";
        vector<string> vals;
        for(int i = 0; i < data.size(); i ++) {
            if(data[i] == ',') vals.push_back(tmp), tmp="";
            else tmp = tmp + data[i];
        }
        vals.push_back(tmp);
        int p = 0;
        root = rdeserialize(root, vals, p);
        return root;
    }
};
```

## [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

LIS板子

```c
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 0x3f3f3f3f);
        for(int i = 0; i < nums.size(); i ++) 
            *lower_bound(dp.begin(), dp.end(), nums[i]) = nums[i];
        return lower_bound(dp.begin(), dp.end(), 0x3f3f3f3f) - dp.begin(); 
    }
};
```

## [301. 删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/)

去重, 应该有更好的办法

```c
class Solution {
public:

    bool chk(string& s){
        int cnt = 0;
        for(char i:s){
            if(i == '(')
                cnt++;
            if(i == ')'){
                cnt--;
                if(cnt < 0) return false;
            }
        }
        return cnt == 0;
    }

    void dfs(int l, int r, int p, string t, string& s, vector<string>& res) {
        if(l + r > s.size() - p) return;
        if(!l && !r && p == s.size()) {
            if(chk(t)) res.push_back(t);
            return ;
        }
        if(l && s[p] == '(') dfs(l - 1, r, p + 1, t, s, res);
        if(r && s[p] == ')') dfs(l, r - 1, p + 1, t, s, res);
        t += s[p];
        dfs(l, r, p + 1, t, s, res);
        
    }
    vector<string> removeInvalidParentheses(string s) {
        vector<string> res;
        int l = 0, r = 0;
        for(char i:s) {
            if(i == '(') l ++;
            if(i == ')') {
                if(l > 0) l --;
                else r ++;
            }
        }
        //cout << l << " " << r << endl;
        dfs(l, r, 0, "", s, res);
        sort(res.begin(), res.end());
        res.erase(unique(res.begin(), res.end()), res.end()); 
        return res;
    }
};
```

## [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

0 表示不持股；
1 表示持股；
2 表示处在冷冻期。
dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][2] - prices[i]);
dp[i][2] = dp[i - 1][0];

```c
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 0) return 0;
        vector<vector<int> > dp(prices.size(), vector<int>(3, 0));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        dp[0][2] = 0;
        for(int i = 1; i < prices.size(); i ++) {
            dp[i][0] = max(dp[i - 1][1] + prices[i], dp[i - 1][0]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][2] - prices[i]);
            dp[i][2] = dp[i - 1][0];
        }
        return max(dp[prices.size() - 1][0], dp[prices.size() - 1][2]);
    }
};
```

## [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)

迷的不行, 我特判过不去

```c
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(), 1);
        nums.push_back(1);
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for(int l = 2; l < n; l ++) {
            for(int i = 0; i < n - l; i ++) {
                int j = i + l;
                for(int k = i + 1; k < j; k ++) {
                    dp[i][j] = max(dp[i][j], \
                        dp[i][k]+dp[k][j]+nums[i]*nums[k]*nums[j]);
                }
            }
        }
        return dp[0][n - 1];
    }
};
```

## [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

树形dp 悬于不选2个状态乱搞

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    pair<int, int> dfs(TreeNode* root) {
        if(root == NULL) return {0, 0};
        auto p1 = dfs(root->left);
        auto p2 = dfs(root->right);
        pair<int, int> res = {0, 0};
        res.first = max(p1.first, p1.second) + max(p2.first, p2.second);// 不偷
        res.second = p1.first + p2.first + root->val; // 偷
        return res;
    }

    int rob(TreeNode* root) {
        auto res = dfs(root);
        return max(res.first, res.second);
    }
};
```

## [338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

dp[当前]  = 最低位1消去后 + 1

```c
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> ans(num + 1, 0);
        // cout << (7 &(-7)) << endl;
        for (int i = 1; i <= num; ++i)
            ans[i] = ans[i - (i & (-i))] + 1;
        return ans;
    }
};
```

## [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

hash 标记后面 最小堆维护最大K值

```c
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        for(auto i : nums) mp[i] ++;
        typedef pair<int, int> P;
        priority_queue<P, vector<P>, greater<P> > heap;
        for(auto i : mp) {
            heap.push(make_pair(i.second, i.first));
            if(heap.size() > k) heap.pop();
        }
        vector<int> res;
        while(!heap.empty()) res.push_back(heap.top().second), heap.pop();
        return res;
    }
};
```

## [394. 字符串解码](https://leetcode-cn.com/problems/decode-string/)

模拟栈

```c
class Solution {
public:
    typedef pair<int,string> PIS;
    string repeat(const string& str, int times) {
        string retString = "";
        for(int i = 0; i < times; ++i) retString += str;
        return retString;
    }
    string decodeString(string& s) {
        int repeatTims = 0;
        string res = "";
        vector<PIS> vecStack;  
        for(auto i : s) {
            if('0'<=i && i<='9') repeatTims = (repeatTims*10)+(i-'0');  //计算字符串需要重复的次数
            else if(i == '[') {
                vecStack.push_back({repeatTims,res}); // 存储之前已经有的res
                res = "";
                repeatTims = 0;
            }
            else if(i == ']') {
                PIS tmp = vecStack[vecStack.size()-1];
                vecStack.pop_back();
                res = tmp.second + (tmp.first==0 ? "" : repeat(res, tmp.first));
            }
            else res += i;
        }
        return res;
    }
};
```

## [399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/)

太恶心了不想做  先处理图 然后dfs 直到找到 a -> b return ;

## [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

n^2 排序之后按高度有多少个插入

```c
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), [](const vector<int>& a, const vector<int>& b){
            return a[0] > b[0] || (a[0] == b[0] && a[1] < b[1]);
        });
        vector<vector<int>> res;
        for(auto i : people) {
            res.insert(res.begin() + i[1], i);
        }
        return res;
    }
};
```

## [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

01背包

```c
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for(auto i : nums) sum += i;
        if(sum & 1) return 0;
        vector<bool> dp(sum/2 + 10, 0);
        dp[0] = 1;
        for(auto i : nums) 
            for(int j = sum/2; j >= i; j --)
                dp[j] = dp[j - i] || dp[j];
        return dp[sum/2];
    }
};
```

## [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

dfs每个点 跑路径$(但是路径方向必须是向下的只能从父节点到子节点)$

```c
class Solution {
public:
    void dfs2(TreeNode *root, int sum, int k, int &ans){
        if(root == NULL) return;
        if(sum + root->val == k) ans ++;
        dfs2(root->left, sum + root->val, k, ans);
        dfs2(root->right, sum + root->val, k, ans);
    }

    void dfs(TreeNode* root, int k, int& ans) {
        if(root == NULL) return ;
        dfs2(root, 0, k, ans);
        dfs(root->left, k, ans);
        dfs(root->right, k, ans);
    }

    int pathSum(TreeNode* root, int sum) {
        int ans = 0;
        return dfs(root, sum, ans), ans;
    }
};
```

## [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

和之前的滑动窗口一样

```c
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int need[256] = {0}, cnt[256] = {0};
        int ok = 0, f = 0;
        for(int i = 0; i < p.size(); i ++) need[p[i] - '\0'] ++;
        for(int i = 0; i < 256; i ++) if(need[i]) ok ++;
        int l = 0, r = 0;
        vector<int> res;
        while(r < s.size()) {
            if(need[s[r] - '\0']) {
                cnt[s[r] - '\0'] ++;
                if(cnt[s[r] - '\0'] == need[s[r] - '\0'])
                    f ++;
            }
            r ++;
            while(f == ok) {
                if(r - l == p.size()) res.push_back(l);
                if(need[s[l] - '\0']) {
                    cnt[s[l] - '\0'] --;
                    if(cnt[s[l] - '\0'] < need[s[l] - '\0']) f --;
                }
                l ++;
            }
        }
        return res;
    }
};
```

## [461. 汉明距离](https://leetcode-cn.com/problems/hamming-distance/)

二进制模就行了

```c
class Solution {
public:
    int hammingDistance(int x, int y) {
        int ans = 0;
        for(int i = 0; i < 32; i ++) {
            if((x >> i & 1) != (y >> i & 1)) ans ++;
            cout << ans << endl;
        }
        return ans;
    }
};
```

## [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

注意0+ - 也算方案

```c
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        if(nums.size() == 0 || S > 1000 || S < -1000) return 0;
        vector<vector<int>> dp(2005, vector<int>(2005, 0));
        // +0 -0 也算是方案数
        dp[0][nums[0] + 1000] = 1;
        dp[0][-nums[0] + 1000] += 1;

        for (int i = 1; i < nums.size(); i++) {
            for (int sum = -1000; sum <= 1000; sum++) {
                if (dp[i - 1][sum + 1000] > 0) {
                    dp[i][sum + nums[i] + 1000] += dp[i - 1][sum + 1000];
                    dp[i][sum - nums[i] + 1000] += dp[i - 1][sum + 1000];
                }
            }
        }
        return dp[nums.size() - 1][S+1000];
    }
};
```

## [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    void dfs(TreeNode* root, int &sum) {
        if(root == NULL) return ;
        dfs(root->right, sum);
        int t = root->val;
        root->val += sum;
        sum += t;
        dfs(root->left, sum);
    }
    TreeNode* convertBST(TreeNode* root) {
        int sum = 0;
        dfs(root, sum);
        return root;
    }
};
```

## [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

我以为是权值呢 结果是边数...醉了

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    int diameterOfBinaryTree(TreeNode* root) { 
        int ans = 1;
        return dfs(root, ans), ans - 1;
    }
    int dfs(TreeNode* root, int &ans) {
        if(!root) return 0;
        int lm = max(dfs(root->left, ans), 0);
        int rm = max(dfs(root->right, ans), 0);
        int lmr = lm + rm + 1;
        ans = max(lmr, ans);
        return max(lm, rm) + 1;
    }
};
```

## [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

常见题型 考虑 k - sum 是否出现过

```c
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        int sum = 0, ans = 0;
        hash[0] = 1;
        for(int i = 0; i < nums.size(); i ++) {
            sum += nums[i];
            ans += hash[k-sum];
            hash[-sum] += 1;
        }
        return ans;
    }
};
```

## [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

for去找上升点 下降点 就是答案

```c
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        stack <int> s;
        int l = nums.size(), r = 0;
        for (int i = 0; i < nums.size(); i++) {
            while (!s.empty() && nums[s.top()] > nums[i])
                l = min(l, s.top()), s.pop();
            s.push(i);
        }
        while(s.size()) s.pop();
        for (int i = nums.size() - 1; i >= 0; i--) {
            while (!s.empty() && nums[s.top()] < nums[i])
                r = max(r, s.top()), s.pop();
            s.push(i);
        }
        return r - l > 0 ? r - l + 1 : 0;
    }
};
```

## [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

```c
class Solution {
public:
    TreeNode* dfs(TreeNode*p1, TreeNode* p2) {
        if(!p1 && !p2) return NULL;
        if(p1 && p2) {
            p1->val += p2->val;
            p1->left = dfs(p1->left, p2->left);
            p1->right = dfs(p1->right, p2->right);
        } else {
            if(!p1) p1 = p2;
        }
        return p1;
    }
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        return dfs(t1, t2);
    }
};
```

## [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

```c
class Solution {
public:
    int countSubstrings(string s) {
        string t = "@";
        for(auto i : s) t += "#", t += i;
        t += "#%";
        int id = 0, mx = 0;
        vector<int> p(t.size(), 0);
        for(int j = 1; j < t.size() - 1; j ++) {
            p[j] = mx > j ? min(p[id * 2 - j], mx - j) : 1;
            while(t[j + p[j]] == t[j - p[j]]) p[j] ++;
            if(mx < p[j] + j) {
                mx = p[j] + j;
                id = j;
            }
        }
        int res = 0;
        for(auto i : p) res += i/2;
        return res;
    }
};
```

## [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

拿栈 栈中剩下的就找不到了

````c
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        vector<int> ans(T.size());
        stack<int> s;
        for (int i = 0; i < T.size(); ++ i) {
            while (!s.empty() && T[i] > T[s.top()]) ans[s.top()] = i - s.top(), s.pop() ;
            s.push(i);
        }
        while(!s.empty()) ans[s.top()] = 0, s.pop();
        return ans;
    }
};
````


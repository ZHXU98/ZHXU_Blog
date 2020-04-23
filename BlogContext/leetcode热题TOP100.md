# leetcode热题TOP100(1)

~~起因 好久没有刷算法题了 结果面试的时候居然写不出丢脸 top100看了看几天就能刷完，热热手吧 ACM白打了~~ 
~~做题有感ACM真白打了~~

~~做完公开 争取5天写完, 剑指offer也会做的~~

~~这里是前49题，后50题下一篇~~

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

```c
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> v;
        for(int i = 0; i < nums.size(); ++ i) {
            if(v.count(target - nums[i])) 
                return {i, v[target - nums[i]]};
            v[nums[i]] = i;
        }
        return {};
    }
};
```

vis 标记 直接找之前出现过差值

## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

模拟加法 大端法直接+就行

```c
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *res = new ListNode(-1), *tail = res;
        ListNode* p1 = l1, *p2 = l2;
        int y = 0;
        while(p1 || p2 || y) {
            int sum = y;
            if(p1) sum += p1->val, p1 = p1->next;
            if(p2) sum += p2->val, p2 = p2->next;
            tail->next = new ListNode(sum % 10);
            tail = tail->next;
            y = sum / 10; 
        }
        ListNode *tmp = res->next;
        delete res;
        return tmp;
    }
};
```

## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

```c
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int v[256]; memset(v, -1, sizeof v);
        int tail = 0, ans = 0;
        for(int i = 0; i < s.size(); ++ i) {
            if(~v[s[i] - '\0']) tail = max(v[s[i] - '\0'] + 1, tail), v[s[i] - '\0'] = i;
            else v[s[i] - '\0'] = i;
            ans = max(ans, i - tail + 1);
        }
        return ans;
    }
};
```

ps 没有人说只有小写字母, 反正RE了一发 干脆 - \0 吧

## [4. 寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

$log(n + m)$的复杂度, 脑子不够用了 第四题就卡壳醉了

$n+m$我倒是有, 两个指着谁小谁先走 特判奇偶(无额外空间)

```c
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();
        int k = (n + m) >> 1;
        int kpre = k - 1;
        bool f = ((n + m) % 2 == 0);
        int p1 = 0, p2 = 0;
        int pren = 0, sumn = 0;
        while(p1 < nums1.size() || p2 < nums2.size()) {
            int v1 = p1 < nums1.size() ? nums1[p1] : 0x3f3f3f3f;
            int v2 = p2 < nums2.size() ? nums2[p2] : 0x3f3f3f3f;
            if(v1 < v2) {
                if(p1 + p2 == kpre) pren = nums1[p1];
                if(p1 + p2 == k) {
                    sumn = nums1[p1];
                    if(f) return (pren + sumn) * 0.5;
                    else return sumn;
                }
                p1 ++;
            } else {
                if(p1 + p2 == kpre) pren = nums2[p2];
                if(p1 + p2 == k) {
                    sumn = nums2[p2];
                    if(f) return (pren + sumn) * 0.5;
                    else return sumn;
                }
                p2 ++;
            }
        }
        return -1;
    }
};
```

```c
log(n+m)的解法 每次抛出不合适的前k小
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();
        int k = (n + m + 1) >> 1;
        if((n + m) % 2 == 1) return getKnum(nums1, 0, n - 1, nums2, 0, m - 1, k);
        else return 
            (getKnum(nums1, 0, n - 1, nums2, 0, m - 1, k) + \
            getKnum(nums1, 0, n - 1, nums2, 0, m - 1, k + 1)) *0.5;
        return 0;
    }

    int getKnum(vector<int> &nums1, int s1, int e1, vector<int>& nums2, int s2, int e2, int k) {
        int len1 = e1 - s1 + 1;
        int len2 = e2 - s2 + 1;
        if(len1 > len2) return getKnum(nums2, s2, e2, nums1, s1, e1, k);
        if(len1 == 0) return nums2[s2 + k - 1];
        if(k == 1) return min(nums1[s1], nums2[s2]);
        int i = s1 + min(len1, k / 2) - 1;
        int j = s2 + min(len2, k / 2) - 1;
        if(nums1[i] > nums2[j]) return getKnum(nums1, s1, e1, nums2, j + 1, e2, k - (j - s2 + 1));
        else return getKnum(nums1, i + 1, e1, nums2, s2, e2, k - (i - s1 + 1));
    }
};
```

## [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```c
class Solution {
public:
    string longestPalindrome(string s) {
        return manacher(s);
    }
    string manacher(string& s) {
        string t = "$";
        for(int i = 0; i < s.size(); i ++) t += "#", t += s[i];
        t += "#@";
        int n = t.size();
        vector<int> p(n, 0);
        int id = 0, mx = 0;// 中心 最远右边界
        int maxlen = -1;
        int index = 0;
        for(int j = 1; j <= n - 1; ++ j) {
            p[j] = mx > j ? min(p[id * 2 - j], mx - j) : 1;
            // 最远大于当前位置 用之前的代替当前p[j]去算
            // 超过了 从1暴力跑
            while(t[j + p[j]] == t[j - p[j]]) p[j] ++;
            if(mx < p[j] + j) {
                mx = p[j] + j;
                id = j;
            }
            if(maxlen < p[j] - 1){
                maxlen = p[j] - 1;
                index = j;
            }
        }
        int start = (index - maxlen)/2;
        return s.substr(start, maxlen);
    }
};
```

## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

如果 $p[j + 1]$ 不是通配符 ' * ' ，则$f[i] [j]$是真，当且仅当$s[i]$可以和$p[j]$匹配，且$f[i+1] [j+1]$是真；
如果$p[j+1]$是通配符 ' * '，则下面的情况只要有一种满足，$f[i] [j]$就是真；

- $f[i] [j+2]$是真；
- $s[i]$可以和$p[j]$匹配，且$f[i+1] [j]$是真;

```c
class Solution {
public:
    vector<vector<int>> dp;
    int lens, lenp;
    bool isMatch(string s, string p) {
        lens = s.size();
        lenp = p.size();
        dp = vector<vector<int>>(lens + 1, vector<int>(lenp + 1, -1));
        return sol(s, p, 0, 0);
    }
    
    bool sol(string &s, string &p, int x, int y) {
        if(dp[x][y] != -1) return dp[x][y];
        if(y == lenp) return dp[x][y] = x == lens;
        bool match = (s[x] == p[y] || p[y] == '.') && x < lens;// 越界
        bool tmp;
        if(p[y + 1] == '*' && y + 1 < lenp) {
            tmp = ( (match && sol(s, p, x + 1, y) ) || sol(s, p, x, y + 2));// 多匹配||0匹配
        } else {
            tmp = match && sol(s, p, x + 1, y + 1);
        }
        return tmp;
    }
};
```

## [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

两个指针缩短点的板子 不然下一个不可能比当前大

```c
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size() - 1;
        int ans = 0;
        while(r > l) {
            ans = max(ans, min(height[l], height[r]) * (r - l));
            if(height[l] < height[r]) l ++;
            else r --;
        }
        return ans;
    }
};
```

## [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

此题重点是去重 sort之后 暴力去找==0的点, 此时要注意连续的相同数字要跳过

```c
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int> > res;
        sort(nums.begin(), nums.end());
        for(int i = 0; i < n; i ++) {
            int l = i + 1, r = n - 1;
            if(i > 0 && nums[i] == nums[i - 1]) continue; 
            if(nums[i] > 0) break;
            while(l < r) {
                int t = nums[i] + nums[l] + nums[r];
                if(t > 0) r --;
                else if(t < 0) l ++;
                else {
                    res.push_back({nums[i], nums[l], nums[r]});
                    while(r > l && nums[r] == nums[r - 1]) r --;
                    while(r > l && nums[l] == nums[l + 1]) l ++;
                    l ++, r--;
                }
            } 
        }
        return res;
    }
};
```

## [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

裸DFS

```c
class Solution {
public:

    map<char, string> v;
    void init() {
        v['2'] = "abc";
        v['3'] = "def";
        v['4'] = "ghi";
        v['5'] = "jkl";
        v['6'] = "mno";
        v['7'] = "pqrs";
        v['8'] = "tuv";
        v['9'] = "wxyz";
    }

    void dfs(string s, int x, int l, string &t, vector<string>& res) {
        if(x == l) res.push_back(t);
        //cout << s[x] << endl;
        //cout << v[s[x]].size() << endl;
        for(int i = 0; i < v[s[x]].size(); i ++) {
            t[x] = v[s[x]][i];
            dfs(s, x + 1, l, t, res);
        }
    }

    vector<string> letterCombinations(string digits) {
        vector<string> res; 
        if(digits.size() == 0) return res;
        init();
        string t(digits.size(), '0');
        dfs(digits, 0, digits.size(), t, res);
        return res;
    }
};
```

## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

常见应用 双指针

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *tail = head, *front = head;
        int t = n;
        while(t --) front = front->next;
        ListNode *pre = NULL;
        while(front) front = front->next, pre = tail, tail = tail->next;
        if(pre == NULL) return tail->next;
        pre->next = tail->next;
        return head;
    }
};
```

## [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

stack的应用吧 顺便说一句 [ 的ascill 码是91 ] 是93 中间个了个 \ 我是醉了

```c
class Solution {
public:
    bool chk(char s, char k) {
        return (s == ']' && k == '[') || (s == '}' && k == '{') || (s == ')' && k == '(');
    }

    bool isValid(string s) {
        stack<char> st;
        for(int i = 0; i < s.size(); i ++) {
            if(s[i] == '(' || s[i] == '[' || s[i] == '{') {
                st.push(s[i]);
            } else {
                if(!st.empty() && chk(s[i], st.top())) st.pop();
                else return 0;
            }
        }
        return st.empty();
    }
};
```

## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

细节注意下就行

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(-1), *tmp = head;
        while(l1 || l2) {
            if(l1 && l2) {
                if(l1->val > l2->val) tmp->next = l2, l2 = l2->next;
                else tmp->next = l1, l1 = l1->next;
                tmp = tmp->next;
            } else if(l1) {
                tmp->next = l1;
                l1 = l1->next;
                tmp = tmp->next;
            } else {
                tmp->next = l2;
                l2 = l2->next;
                tmp = tmp->next;

            }
        }
        tmp = head->next;
        delete head;
        return tmp;
    }
};
```

## [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

裸dfs

```c
class Solution {
public:
    void dfs(int l, int r, int p, int n, string &s, vector<string>& res) {
        if(p == n*2) res.push_back(s);
        if(l < n) s[p] = '(', dfs(l + 1, r, p + 1, n, s, res);
        if(r < l) s[p] = ')', dfs(l, r + 1, p + 1, n, s, res);
    }
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        string s(n * 2, '0');
        dfs(0, 0, 0, n, s, res);
        return res;
    }
};
```

## [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

原来优先队列的匿名函数重载得这样写

其次 关于内存泄漏, 外卖还可以考虑吧头部假节点变成临时局部变量 ListNode p(-1);

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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        auto cmp = [](ListNode *a, ListNode *b) {return a->val > b->val;}; 
        priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> heap(cmp);
        for(int i = 0; i < lists.size(); i ++) 
            if(lists[i] != NULL) heap.push(lists[i]);
        ListNode* head = new ListNode(-1), *tail = head;
        while(!heap.empty()){
            tail->next = heap.top(); heap.pop();
            tail = tail->next;
            if(tail->next) heap.push(tail->next);
        }
        tail = head->next;
        delete head;
        return tail;
    }
};
```

## [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

STL next_permutation 底层也算是这样实现的

```c
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        //next_permutation(nums.begin(), nums.end());
        int i = (int)nums.size() - 2;
        while(i >= 0 && nums[i + 1] <= nums[i]) i --;
        // 1 2 3 4 9 8 7
        if(i >= 0) {
            int j = nums.size() - 1;
            while(j >= 0 && nums[j] <= nums[i]) j --;
            swap(nums[i], nums[j]);
        }
        int j = nums.size() - 1;
        i ++;
        while(i < j) {
            swap(nums[i], nums[j]);
            i ++, j --;
        }
    }
};
```

## [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

正着找一遍, 反正找一遍 防丢 拿栈也行吧

```c
class Solution {
public:
    int longestValidParentheses(string s) {
        int l = 0, r = 0, p = 0, pre = -1;
        int ans = 0;
        while(p < s.size()) {
            if(s[p] == '(') l ++;
            else r ++;
            if(r > l) l = r = 0, pre = p;
            if(l == r) ans = max(ans, p - pre);
            p ++;
        }
        p = s.size() - 1;
        pre = p + 1;
        l = 0, r = 0;
        while(p >= 0) {
            if(s[p] == '(') l ++;
            else r ++;
            if(r < l) l = r = 0, pre = p;
            if(l == r) ans = max(ans, pre - p);
            p --;
        }
        return ans;
    }
};
```

## [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

第一次二分 用第一个位置找旋转位置就行 然后注意不要忘记选择右边 最后一个点 应该是$<=$ 不然会露掉最后一个位置是target的情况

```c
class Solution {
public:
    int find_o(int l, int r, vector<int>& a) {
        if(a[l] < a[r]) return 0;
        while(l < r) {
            int mid = l + r >> 1;
            if(a[0] > a[mid]) r = mid;
            else l = mid + 1;
        }
        return l;
    }

    int search(vector<int>& nums, int target) {
        if(nums.size() == 0) return -1;
        int r = nums.size() - 1;
        int o = find_o(0, r, nums);
        int t;
        if(nums[r] >= target) {
            t = lower_bound(nums.begin() + o, nums.end(), target) - nums.begin();
            if(nums[t] == target) return t;
        }
        else {
            t = lower_bound(nums.begin(), nums.begin() + o, target) - nums.begin();
            if(nums[t] == target) return t;
        } 
        return -1;
    }
};
```

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

特判空 和 l是不是最后位置没有找到

```c
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l = -1, r = -1;
        if(nums.size() == 0) return {-1, -1};
        l = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
        if(l == nums.size()) return {-1, -1};
        if(nums[l] != target ) return {-1, -1};
        else {
            r = upper_bound(nums.begin(), nums.end(), target) - nums.begin();
            return {l, r - 1};
        }
    }
};
```

## [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

去重的一般方法, 如果出现了一个数据只能用一次 如 40 题 我们 `if(i > p && s[i] == s[i - 1]) continue;` 避免使用多次同样数据重复

这里我们只要不重复使用之前就行

```c
class Solution {
public:

    void dfs(int sum, int p, int n,vector<int>& s, vector<int>& t, vector<vector<int> >& res) {
        if(sum > n) return;
        if(sum == n) {
            res.push_back(t);
            return ;
        }
        for(int i = p; i < s.size(); i ++) {
            //if(i > p && s[i] == s[i - 1]) continue; 40 题
            t.push_back(s[i]);
            dfs(sum + s[i], i, n, s, t, res);
            t.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> t;
        vector<vector<int> > res;
        dfs(0, 0, target, candidates, t, res);
        return res;
    }
};
```

## [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

跑左右2点最大可以多少, 然后每次选最小值 - 高度看啊可能存多少

```c
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size(), res = 0;
        vector<int> L(n, 0), R(n, 0);
        for(int i = 0; i < n; i ++) L[i] = i != 0 ?  max(L[i - 1], height[i]) : height[i];
        for(int i = n - 1; i >= 0; i --) R[i] = i != n - 1 ? max(R[i + 1], height[i]) : height[i];
        for(int i = 0; i < n; i ++) {
            //cout << L[i] << " " << R[i] << endl;
            res += max(min(R[i], L[i]) - height[i], 0);
        }
        return res;
    }
};
```

## [46. 全排列](https://leetcode-cn.com/problems/permutations/)

懒得写的 记住next_permutation 是找第一个比他字典序小的就停止, 记得使用前排序

```c
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int> > res; 
        do{
            res.push_back(nums);
        }while(next_permutation(nums.begin(), nums.end()));
        return res;
    }
};
```

## [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

下标是真的不好处理 虽然很明显可以看出是$(x, y)$==>$(y, n - x)$的关系

但是 如何选择反转点不重不漏得考虑 $(i<n/2)and(j<n -i)$ 画图看出

```c
class Solution {
public:

    void swap4(int n ,int x, int y, vector<vector<int> >& mp) {
        // y, n - x + 1
        int tmp = mp[x][y], pre;
       // cout << x << " " << y << endl;
        for(int i = 0; i < 4; i ++) {
            int nx = y, ny = n - x;
           // cout << nx << " " << ny << "|||" << x << " " << y << endl;
            pre = mp[nx][ny];
            mp[nx][ny] = tmp;
            x = nx, y = ny;
            tmp = pre;
        }
    }

    void rotate(vector<vector<int> >& matrix) {
        int n = matrix.size();
        for(int i = 0; i < n/2; i ++) {
            for(int j = i; j < n - i - 1; j ++) {
                swap4(n - 1, i, j, matrix);
            }
        }
    }
};
```

## [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

排序后 可map(hash)去重

```c
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, int> id;
        int cnt = 0;
        vector<vector<string> > res;
        for(int i = 0; i < strs.size(); ++ i) {
            string s = strs[i];
            sort(s.begin(), s.end());
            if(id.count(s)) res[id[s]].push_back(strs[i]);
            else{
                id[s] = cnt ++;
                vector<string> tmp;
                tmp.push_back(strs[i]);
                res.push_back(tmp);
            } 
        }
        return res;
    }
};
```

## [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

尝试分治做的 值得注意的是 lrsub 里面 i = mid 而不是 i = mid - 1 最后我们一定是在最小无穷里面更新答案 所以mid - 1< 0 的话 会少更新答案 例如(1, 2) 答案输出2

```c
class Solution {
public:

    int lrsub(vector<int>& a, int l, int mid, int r) {
        int lm = -0x3f3f3f3f, rm = -0x3f3f3f3f, sum = 0;
        for(int i = mid; i >= l; i --) sum += a[i], lm = max(lm, sum);
        sum = 0;
        for(int i = mid + 1; i <= r; i ++) sum += a[i], rm = max(rm, sum);
        return lm + rm;
    }

    int dfs(vector<int>& a, int l, int r) {
        if(l == r) return a[l];
        int mid = l + r >> 1;
        int lm = dfs(a, l, mid);
        int rm = dfs(a, mid + 1, r);
        int lrm = lrsub(a, l, mid, r);
        return max(lm, max(rm, lrm));
    }

    int maxSubArray(vector<int>& nums) {
        return dfs(nums, 0, nums.size() - 1);
    }
};
```

## [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

定义最远位置p 选择nums[i] + i 远 还是 p 远 最后比n - 1大就行

```c
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int p = 0;
        for(int i = 0; i < nums.size(); i ++) {
            if(p >= i) p = max(nums[i] + i, p);
        }
        return p >= nums.size() - 1;
    }
};
```

## [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

写的过于复杂

排序之后直接做就行

```c
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int> > res;
        if(intervals.size() == 0) return res;
        typedef pair<int, int> P;
        sort(intervals.begin(), intervals.end());
        P p = make_pair(intervals[0][0], intervals[0][1]);
        if(intervals.size() == 1) {
            res.push_back({p.first, p.second});
            return res;
        }
        for(int i = 1; i < intervals.size(); i ++) {
            P tp = make_pair(intervals[i][0], intervals[i][1]);
            if(tp.first <= p.second) {
                p.second = max(tp.second, p.second);
                if(i == intervals.size() - 1) res.push_back({p.first, p.second});
            }
            else {
                res.push_back({p.first, p.second});
                p = tp;
                if(i == intervals.size() - 1) res.push_back({p.first, p.second});
            }
        }
        return res;
    }
};
```

简化版:

```c
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int> > res;
        if(intervals.size() == 0) return res;
        sort(intervals.begin(), intervals.end());
        for(int i = 0; i < intervals.size(); i ++) {
            int l = intervals[i][0], r = intervals[i][1];
            if(res.empty() || res.back()[1] < l) res.push_back({l, r});
            else res.back()[1] = max(res.back()[1], r);
        }
        return res;
    }
};
```

## [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

很容易看出来公式 $ C_{m+n-2}^{m-1}$

dp 也是最简单的地推:

```c
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int> > dp(n, vector<int>(m, 0));
        dp[0][0] = 1;
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                if(i > 0) dp[i][j] += dp[i - 1][j];
                if(j > 0) dp[i][j] += dp[i][j - 1];
            }
        }
        return dp[n - 1][m - 1];
    }
};
```

## [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

第一反应是BFS 结果可以dp 醉了

```c
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        typedef pair<int, int> P;
        typedef pair<int, P> PP;
        const int cx[] = {0, 1};
        const int cy[] = {1, 0};
        int n = grid.size(), m = grid[0].size();
        int ans = 0x3f3f3f3f;
        priority_queue<PP> heap;
        vector<vector<bool> > vis(n, vector<bool>(m, 0));
        heap.push(make_pair(-grid[0][0],make_pair(0, 0)));
        while(!heap.empty()) {
            PP pp= heap.top(); heap.pop();
            P p = pp.second;
            if(vis[p.first][p.second]) continue;
            vis[p.first][p.second] = 1;
            if(p.first == n - 1 && p.second == m - 1) ans = min(-pp.first, ans);
            for(int i = 0; i < 2; i ++) {
                int nx = p.first + cx[i], ny = p.second + cy[i];
                if(nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
                heap.push(make_pair(-(-pp.first + grid[nx][ny]), make_pair(nx, ny))); 
            }
        }
        return ans;
    }
};
```

dp 无额外空间的解法:

```c
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < m; j ++) {
                int tmp = grid[i][j];
                grid[i][j] = 0x3f3f3f3f;
                if(i > 0) grid[i][j] = min(grid[i][j], grid[i - 1][j] + tmp);
                if(j > 0) grid[i][j] = min(grid[i][j], grid[i][j - 1] + tmp);
                if(i == 0 && j == 0) grid[i][j] = tmp;
            }
        }
        return grid[n - 1][m - 1];
    }
};
```

## [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

裸斐波那契

```c
class Solution {
public:
    int climbStairs(int n) {
        // 斐波那契
        int pre = 0, now = 1, ans = 0;
        for(int i = 1; i <= n; i ++) {
            ans = pre + now;
            pre = now, now = ans;
        }
        return (int)ans;
    }
};
```

## [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

经典的编辑距离问题。

状态表示：f[i,j] 表示将 word1 的前 i 个字符变成 word2 的前 j 个字符，最少需要进行多少次操作。
状态转移，一共有四种情况：

- 将 word1[i]删除或在 word2[j]后面添加 word1[i]，则其操作次数等于 f[i−1,j]+1;
- 将 word2[j]删除或在 word1[i]后面添加 word2[j]，则其操作次数等于 f[i,j−1]+1;
- 如果 word1[i]=word2[j]，则其操作次数等于 f[i−1,j−1];
- 如果 word1[i]≠word2[j]，则其操作次数等于 f[i−1,j−1]+1；
  时间复杂度分析：状态数 O(n2)，状态转移复杂度是 O(1)，所以总时间复杂度是 O(n2);

```c
class Solution {
public:
    vector<vector<int>> dp;
    int len1, len2;
    int minDistance(string word1, string word2) {
        len1 = word1.size(), len2 = word2.size();
        dp = vector<vector<int>>(len1 + 1, vector<int>(len2 + 1));
        for(int i = 0; i <= len1; i ++) dp[i][0] = i;
        for(int i = 0; i <= len2; i ++) dp[0][i] = i;

        for(int i = 1; i <= len1; i ++) {
            for(int j = 1; j <= len2; j ++) {
                if(word1[i - 1] != word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = dp[i - 1][j - 1];
                }

                dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j]);
                dp[i][j] = min(dp[i][j - 1] + 1, dp[i][j]);

            }
        }
        return dp[len1][len2];
    }
};
```

## [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

强行怼扫一遍 扫2边不好吗orz

两个指针 一个指0 一个指2 位置

```c
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        for(int i = 0; i <= r; i ++) {
            if(nums[i] == 0) swap(nums[l], nums[i]), ++ l;
            if(nums[i] == 2) swap(nums[i], nums[r]), i --, r --;
        }
    }
};
```

## [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

滑动窗口问题, need数组标记需求, ok表示需要多少字符, f表示现在满足多少
满足后 考虑l 移动 不断找最小

```c
class Solution {
public:
    string minWindow(string s, string t) {
        int need[256] = {0}, cnt[256] = {0};
        int ok = 0, f = 0;
        for(int i = 0; i < t.size(); i ++) need[t[i] - '\0'] ++;
        for(int i = 0; i < 256; i ++) if(need[i]) ok ++;
        int l = 0, r = 0;
        int ansl = -1, ansr = -1, ans = s.size() + 1;

        while(r < s.size()) {
            if(need[s[r] - '\0']) {
                cnt[s[r] - '\0'] ++;
                if(cnt[s[r] - '\0'] == need[s[r] - '\0'])
                    f ++;
            }
            r ++;
            while(f == ok) {
                if(r - l < ans) {
                    ans = r - l;
                    ansl = l;
                }
                if(need[s[l] - '\0']) {
                    cnt[s[l] - '\0'] --;
                    if(cnt[s[l] - '\0'] < need[s[l] - '\0']) f --;
                }
                l ++;
            }
        }
        return ans != s.size() + 1 ? s.substr(ansl, ans) : "";
    }
};
```

## [78. 子集](https://leetcode-cn.com/problems/subsets/)

dfs 二进制枚举 也可以直接二进制枚举

```c
class Solution {
public:
    void dfs(int p, vector<int>& nums, vector<int> &t, vector<vector<int>>& res) {
        if(p == nums.size()) {
            res.push_back(t);
            return ;
        }
        dfs(p + 1, nums, t, res);
        t.push_back(nums[p]);
        dfs(p + 1, nums, t, res);
        t.pop_back();
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> t;
        dfs(0, nums, t, res);
        return res;
    }
};
```

## [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

裸dfs

```c
class Solution {
public:

    bool dfs(int x, int y, int p, vector<vector<char>>& board, string& word, vector<vector<bool>>& vis) {
        if(p == word.size()) return 1;
        if(x < 0 || y < 0 || x >= board.size() || y >= board[0].size()) return 0;
        if(vis[x][y]) return 0;
        vis[x][y] = 1;
        bool res = 0;
        if(board[x][y] == word[p]) {
            res |= \
            dfs(x + 1, y, p + 1, board, word, vis) || \
            dfs(x, y + 1, p + 1, board, word, vis) || \
            dfs(x - 1, y, p + 1, board, word, vis) || \
            dfs(x, y - 1, p + 1, board, word, vis) ;
        }
        vis[x][y] = 0;
        return res;
    }

    bool exist(vector<vector<char>>& board, string word) {
        bool ans = 0;
        vector<vector<bool>> vis(board.size(), vector<bool>(board[0].size(), 0));
        for(int i = 0; i < board.size(); i ++) {
            for(int j = 0; j < board[0].size(); j ++) {
                ans |= dfs(i, j, 0, board, word, vis);
            }
        }
        return ans;
    }
};
```

## [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

记录最左端 最右端最小值 位置 单调栈处理 (最小值是当前限制距离的条件)

```c
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        int n = heights.size();
        vector<int> l(n), r(n);
        for(int i = 0; i < n; i ++) {
            while(!s.empty() && heights[s.top()] >= heights[i] ) s.pop();
            s.empty() ? l[i] = -1 : l[i] = s.top();
            s.push(i);
        }
        while(s.size()) s.pop();
        for(int i = n - 1; i >= 0; i --) {
            while(!s.empty() && heights[s.top()] >= heights[i] ) s.pop();
            s.empty() ? r[i] = n : r[i] = s.top();
            s.push(i);
        }
        int ans = 0;
        for(int i = 0; i < n; i ++) {
        //    cout << l[i] << " " << r[i] << endl;
            ans = max(ans, (r[i] - l[i] - 1) * heights[i]);
        }
        return ans;
    } 
};
```

## [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

引入上面单调栈的题, 就一样了

```c
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        int n = heights.size();
        vector<int> l(n), r(n);
        for(int i = 0; i < n; i ++) {
            while(!s.empty() && heights[s.top()] >= heights[i] ) s.pop();
            s.empty() ? l[i] = -1 : l[i] = s.top();
            s.push(i);
        }
        while(s.size()) s.pop();
        for(int i = n - 1; i >= 0; i --) {
            while(!s.empty() && heights[s.top()] >= heights[i] ) s.pop();
            s.empty() ? r[i] = n : r[i] = s.top();
            s.push(i);
        }
        int ans = 0;
        for(int i = 0; i < n; i ++) {
            ans = max(ans, (r[i] - l[i] - 1) * heights[i]);
        }
        return ans;
    }
    int maximalRectangle(vector<vector<char>>& matrix) {
        if(matrix.size() == 0) return 0;
        vector<int> heights(matrix[0].size(), 0);
        int res = 0;
        for(int i = 0; i < matrix.size(); i ++) {
            for(int j = 0; j < matrix[0].size(); j ++) {
                if(i == 0) heights[j] = matrix[i][j] == '1';
                else heights[j] = matrix[i][j] == '1' ? heights[j] + 1 : 0;
            }
            res = max(res, largestRectangleArea(heights));
        }
        return res;
    }
};
```

## [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

只要改变res.push_back 的位置就行(不同遍历顺序)

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
    void dfs(TreeNode* root, vector<int>& res) {
        if(root == NULL) return ;
        dfs(root->left, res);
        res.push_back(root->val);
        dfs(root->right, res);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        dfs(root, res);
        return res;
    }
};
```

## [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

分为左右子树的情况, 只有一个 和 空节点节点的数量状态为1 其他通过这个转移

```c
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1; dp[1] = 1;
        for(int i = 2; i <= n; i ++) {
            for(int j = 0; j < i; j ++) {
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }
        return dp[n];
    }
};
```

## [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

中序遍历的 非递归写法

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
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> s;
        long inorder = LONG_MIN;
        while(!s.empty() || root != NULL) {
            while(root != NULL) {
                s.push(root);
                root = root->left;
            }
            root = s.top(); s.pop();
            if(root->val <= inorder) return false;
            inorder = root->val;
            root = root->right;
        }
        return true;
    }
};
```

## [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

dfs遍历子树 从根开始 居然有空树醉了额

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
    bool dfs(TreeNode* t1, TreeNode* t2) {
        if(!t1 && !t2) return true;
        if(!t1 || !t2 || t1->val != t2->val) return false;
        return dfs(t1->left, t2->right) && dfs(t1->right, t2->left);
    }
    bool isSymmetric(TreeNode* root) {
        return dfs(root, root);
    }
};
```

## [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

控制每次访问下一层的个数 一开始记录的队列大小 就是一层的个数

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(root == NULL) return {};
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            vector<int> t;
            int n = que.size();
            for(int i = 0; i < n; i ++) {
                TreeNode* p = que.front(); que.pop();
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
                t.push_back(p->val);
            }
            res.push_back(t);
        }
        return res;
    }
};
```

## [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

裸DFS

```c
class Solution {
public:
    int dfs(TreeNode* root) {
        if(root == NULL) return 0;
        return max(dfs(root->left), dfs(root->right)) + 1;
    }
    int maxDepth(TreeNode* root) {
        return dfs(root);
    }
};
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

从画图出发, 找到mid点 递归左右

```c
class Solution {
public:
    TreeNode* build(vector<int>& preorder, int p1, int p2, vector<int>& inorder, int q1, int q2) {
        if(p1 > p2 || q1 > q2) return NULL;
        int p = 0;
        while(preorder[p1] != inorder[q1 + p]) p ++;
        TreeNode* root = new TreeNode(preorder[p1]);
        root->left = build(preorder, p1 + 1, p1 + p, inorder, q1, q1 + p - 1);
        root->right = build(preorder, p1 + p + 1, p2, inorder, q1 + p + 1, q2);
        // 每次输出前序遍历的 p1 就是后续遍历 根左右吗...
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return build(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1);
    }
};
```

## [114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)

说真的没有看懂， 反正是全部到右子树上成链表， 我们把所有有左子树的本身有右子树的考虑直接移植到右头 然后左子树头也直接一到root->right上 不断反复

```c
class Solution {
public:
    void flatten(TreeNode* root) {
        while(root) {
            if(root->left == NULL) root = root->right;
            else {
                TreeNode* p = root->left;
                while(p->right) p = p->right;
                p->right = root->right;
                root->right = root->left;
                root->left = NULL;
                root = root->right;
            }
        }
    }
};
```

## [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

挑战也提到过生存周期这个东西， 最小价格生产周期肯定高于之后的

```c
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int mi = 0x3f3f3f3f;
        int res = 0;
        for(int i = 0; i < prices.size(); i ++) {
            mi = min(mi, prices[i]);
            res = max(res, prices[i] - mi);
        }
        return res;
    }
};
```

## [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

注释没有任何意义， 除非我们没有考虑去0的情况

dfs左子树 右子树 3个状态轮番选择

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
    int dfs(TreeNode* root, int &ans) {
        if(!root) return 0;
        int lm = max(dfs(root->left, ans), 0);
        int rm = max(dfs(root->right, ans), 0);
        int lmr = lm + rm + root->val;
        //ans = max(max(lm, ans), max(rm, root->val));
        ans = max(lmr, ans);
        return max(lm, rm) + root->val;
    }
    int maxPathSum(TreeNode* root) {
        int ans = -0x3f3f3f3f;
        return dfs(root, ans), ans;
    }
};
```

## [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

hash 标记出现位， i - 1没有的话 便是起点 复杂度 最多$O(2n)$

```c
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> s;
        for(auto i : nums) s.insert(i);
        int ans = 0, tmp = 0;
        for(auto i : s) {
            if(s.count(i - 1)) continue;
            else {
                int t = i;
                tmp = 0;
                cout << i << endl;
                while(s.count(t)) {
                    t ++;
                    tmp ++;
                }
                ans = max(ans, tmp);
            }
        }
        return ans;
    }
};
```

## [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

如果是2个， 先亦或一遍 看看出来那个数据 找一个 二进制为1的 按它（01）分2组， 就变成这个题了

```c
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for(auto i: nums) res ^= i;
        return res;
    }
};
```

## [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

bfs 我看有个题解8ms 优先遍历的字典 看起来是优先字典可能不多， 我这里选择的是长度， 以为数据不会太长， 字典会出现很多无用 (优化挺玄学的 除了vis必要 其他的可以不加)

```c
class Solution {
public:
    bool chk(int st, int ed, vector<string>& w, string& s) {
        string t = s.substr(st, ed - st + 1);
        for(auto i : w) if(i == t) return 1;
        return 0;
    }
    bool bfs(string& s, vector<string>& w) {
        auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) {return a.second < b.second || (a.second == b.second && a.first < b.first);};
        priority_queue<pair<int, int>, vector<pair<int, int> >, decltype(cmp) > que(cmp);
        vector<bool> vis(s.size(), 0);
        for(int i = 0; i < s.size() ; ++ i) 
            if(chk(0, i, w, s)) que.push(make_pair(0, i));
        while(!que.empty()) {
            pair<int, int> p = que.top(); que.pop();
            //cout << p.second << " " << p.first << endl;
            if(vis[p.second]) continue;
            vis[p.second] = 1;
            if(chk(p.first, p.second, w, s)) {
                if(p.second == s.size() - 1) return 1;
                for(int i = p.second + 1; i < s.size(); i ++)
                    if(chk(p.second + 1, i, w, s)) que.push(make_pair(p.second + 1, i));
            }
        }
        return 0;
    }
    bool wordBreak(string s, vector<string>& wordDict) {
        return bfs(s, wordDict);
    }
};
```
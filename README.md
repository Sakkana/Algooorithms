# Algorithms

加油刷题！

---

## 基础算法

### <a href="https://leetcode.cn/problems/binary-search/description/">704.二分查找</a>

yxc 做法 \
可以维护区间序列，查找头尾。 \
这种做法 l 或者 r 不断等于 mid，最终必将收敛到一个 l == r 的 mid。\
最终只需要判断这个元素是否为目标元素。\
需要注意取 mid 时的死循环情况。
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (nums[mid] > target) {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        return nums[l] == target ? l : -1;
    }
};
```

传统做法 \
取 [l, r]，因此 l，r 都有意义，因此循环条件用 l <= r，\
且更新时都更新为合法的 mid，即需要 +1 或者 -1。\
不存在死循环情况，每一轮区间端点都会更新。\
不支持相同的数字区间找头尾。
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l <= r) {
            auto mid = (l + r) >> 1;
            if (nums[mid] > target) {
                r = mid - 1;
            } else if (nums[mid] < target) {
                l = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
};
```

取 [l, r)，右端点不合法，因此循环条件为 l < r，\
右端点同时也更新为不合法的 mid，因此 r = mid。\
左端点可以取到，因此要更新成合法的 mid，即 mid + 1。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size();
        while (l < r) {
            auto mid = (l + r) >> 1;
            if (nums[mid] > target) {
                r = mid;
            } else if (nums[mid] < target) {
                l = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
};
```


## 双指针

### <a href="https://leetcode.cn/problems/remove-element/description/">27. 移除元素</a>

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int p1 = 0, p2 = 0;
        int len = nums.size();
        while (p2 < len) {
            if (nums[p2] != val) {
                nums[p1] = nums[p2];
                ++ p1;
            }
            ++ p2;
        }
        return p1;
    }
};
```

由于不要求顺序，题解提供了这种优化时间的做法。
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int p1 = 0, p2 = nums.size();
        while (p1 != p2) {
            if (nums[p1] == val) {
                nums[p1] = nums[p2 - 1];
                -- p2;
            } else {
                ++ p1;
            }
        }
        return p1;
    }
};
```


### <a href="https://leetcode.cn/problems/remove-duplicates-from-sorted-array/description/">26.删除有序数组中的重复项</a>

自己第一次是这么做的，但是时空复杂度有点高。\
没有看到有序数组这个条件，其实可以不用哈希表的。\
因为有序数组的话，相同元素一定是连着的。
```c++
class Solution {
    map<int, bool> mp;
public:
    int removeDuplicates(vector<int>& nums) {
        int l = 0, r = 0;
        int len = nums.size();
        while (r < len) {
            if (!mp[nums[r]]) {
                mp[nums[r]] = true;
                nums[l] = nums[r];
                ++ l;
            }
            ++ r;
        }
        return l;
    }
};
```
后来又重新写了一遍。
虽然过了，但是说实话挺丑陋的。
```c++
class Solution {
    map<int, bool> mp;
public:
    int removeDuplicates(vector<int>& nums) {
        int l = 0, r = 0;
        int len = nums.size();
        while (r < len) {
            while (r < len && nums[l] == nums[r]) {
                ++ r;
            }
            if (l < len) {
                ++ l;
            }
            if (r < len) {
                nums[l] = nums[r];
            }
        }
        return l;
    }
};
```
官解这个确实挺优雅的。
```c++
class Solution {
    map<int, bool> mp;
public:
    int removeDuplicates(vector<int>& nums) {
        int l = 0, r = l + 1;
        int len = nums.size();
        while (r < len) {
            if (nums[l] != nums[r]) {
                if (r - l > 1) {
                    nums[l + 1] = nums[r];
                }
                ++ l;
            }
            ++ r;
        }
        return l + 1;
    }
};
```

### <a href="https://leetcode.cn/problems/squares-of-a-sorted-array/">977. 有序数组的平方</a>
连 easy 题都要看题解了...
任重而道远啊。
```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        vector<int> res;
        while (l <= r) {
            int l_pow = nums[l] * nums[l];
            int r_pow = nums[r] * nums[r];
            if a(l_pow > r_pow) {
                res.push_back(l_pow);
                ++ l;
            } else {
                res.push_back(r_pow);
                -- r;
            }
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

## 滑动窗口
其实滑动窗口也有双指针的特性，因为有个窗口前沿和窗口后沿。

### <a href="https://leetcode.cn/problems/minimum-size-subarray-sum/">209.长度最小的子数组</a>

其实这道题大二的时候做过，但是在餐馆里等烧烤，喝了点啤酒。\
当时刚转到cs一切都很陌生。\
没想到一年过去了，忘得也差不多了。

这里的条件有三个：
1. 长度最小
2. 连续
3. sum >= targe

因此，可以将窗口定义为满足上述条件的连续子序列。\
最核心的是如何更新窗口。\
首先，右沿要一直向右移动，直到出现一个满足条件的窗口。\
此时，左沿要向左移动，将窗口长度尽量缩短并且满足 sum >= target。\
一旦找到了最小的那个左沿（即再移动 sum < target），停止左沿移动。\
那么，当前就找到了一个局部的最优解，与全局最优解作比较。\
下一次，再次移动右沿，直到满足上述三个条件，周而复始。

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        int res = 0x3f3f3f3f;
        int sum = 0;
        for (int l = 0, r = 0; r < n; ++ r) {
            sum += nums[r];
            while (sum >= target) {
                res = min(res, r - l + 1);
                sum -= nums[l ++];
            }
        }
        return res == 0x3f3f3f3f ? 0 : res;
    }
};
```

这个用**二分**也可以做。\
既然是连续子序列的和，那么使用前缀和可以到达 O(1) 计算。\
我们要的是区间 sum([l, r]) >= target，那么就是针对 l 找到一个下标 x，满足前缀和 s[x] >= s[l - 1] + target。\
此时可以使用二分查找来锁定这个 x，因为前缀和是单调上升的。

```c++
class Solution {
    const int N = 1e5 + 10;
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        if (nums.size() == 1) {
            return nums[0] >= target ? 1 : 0;
        }

        int len = nums.size();
        vector<int> s(len + 1, 0);
        for (int i = 1; i <= len; ++ i) {
            s[i] = s[i - 1] + nums[i - 1];
        }

        int res = 0x3f3f3f3f;
        for (int i = 1; i <= len; ++ i) {
            int sum = s[i - 1] + target;
            auto end = lower_bound(s.begin(), s.end(), sum);
            if (end != s.end())
                res = min(res, static_cast<int>(end - s.begin()) - (i - 1));
        }
        return res == 0x3f3f3f3f ? 0 : res;
    }
};
```


### <a href="https://leetcode.cn/problems/fruit-into-baskets/description/">904. 水果成篮</a>

首先抓住条件：`一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。`\
这意味着，这里隐含了一个窗口，该窗口内部至多有两种元素。\
因此窗口维护的是：最长的且由1个或2个重复元素组成的最长序列。

```c++
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        int len = fruits.size();
        if (len == 1) {
            return 1;
        }

        unordered_map<int, int> mp;
        int l = 0, r = 0;
        int res = 0;
        while (r < len) {
            ++ mp[fruits[r]];
            while (mp.size() > 2) {
                auto pos = mp.find(fruits[l]);
                -- pos->second;
                if (!pos->second) {
                    mp.erase(pos);
                }
                ++ l;
            }
            res = max(res, r - l + 1);
            ++ r;
        }
        return res;
    }
};
```

### <a href="https://leetcode.cn/problems/minimum-window-substring/description/">76. 最小覆盖子串</a>

```c++

class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> mp_s, mp_t;
        auto check = [&]() {
            for (const auto it : mp_t) {
                if (mp_s[it.first] < it.second) {
                    return false;
                }
            }
            return true;
        };

        for (const auto it : t) {
            ++ mp_t[it];
        }

        int min_len = 0x3f3f3f3f, st = -1;
        int len = s.size();
        int l = 0, r = -1;
        while (r < len) {
            ++ mp_s[s[++ r]];
            while (l <= r && check()) {
                if (r - l + 1 < min_len) {
                    min_len = r - l + 1;
                    st = l;
                }
                -- mp_s[s[l ++]];
            }
            cout << l << " " << r  << endl;
        }
        return st == -1 ? string{} : s.substr(st, min_len);
    }
};
```







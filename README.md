# Algorithms

加油刷题！

---

## 基础算法
<a href="https://leetcode.cn/problems/binary-search/description/">704.二分查找</a>

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

<a href="https://leetcode.cn/problems/remove-element/description/">27. 移除元素</a>
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


<a href="https://leetcode.cn/problems/remove-duplicates-from-sorted-array/description/">26. 删除有序数组中的重复项</a>
自己第一次是这么做的，但是时空复杂度有点高。\
没有看到有序数组这个条件，其实可以不用哈希表的。
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
```c++

```


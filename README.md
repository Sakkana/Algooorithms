# Algorithms

加油刷题！

---

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


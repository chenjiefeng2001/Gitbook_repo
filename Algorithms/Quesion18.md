# Quesion18 4个数的和

## 题目描述

Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

0 <= a, b, c, d < n
a, b, c, and d are distinct.
nums[a] + nums[b] + nums[c] + nums[d] == target
You may return the answer in any order.

 

Example 1:

Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
Example 2:

Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
 

Constraints:

1 <= nums.length <= 200
-109 <= nums[i] <= 109
-109 <= target <= 109

## 题目思路

本题的思路：首先对整个数组进行排序，后续用双指针的方式来进行遍历，选择两个元素num[i]和num[j]作为基准元素。然后一个指针指向num[i+1]，另一个指针指向数组的末尾。
通过第二层循环，找到当前四个元素的和，判断与target的关系，移动指针以逼近目标值。

代码如下所示：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        int n = nums.size();
        if (n < 4) {
            return result;
        }
        sort(nums.begin(), nums.end());

        for (int i = 0; i < n - 3; ++i) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue; // 跳过重复的元素
            }
            for (int j = i + 1; j < n - 2; ++j) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue; // 跳过重复的元素
                }
                int left = j + 1, right = n - 1;
                while (left < right) {
                    long long current_sum = static_cast<long long>(nums[i]) + nums[j] + nums[left] + nums[right];
                    if (current_sum == target) {
                        result.push_back({nums[i], nums[j], nums[left], nums[right]});
                        ++left;
                        --right;
                        while (left < right && nums[left] == nums[left - 1]) {
                            ++left; // 跳过重复的元素
                        }
                        while (left < right && nums[right] == nums[right + 1]) {
                            --right; // 跳过重复的元素
                        }
                    } else if (current_sum < target) {
                        ++left;
                    } else {
                        --right;
                    }
                }
            }
        }
        return result;
    }
};
```

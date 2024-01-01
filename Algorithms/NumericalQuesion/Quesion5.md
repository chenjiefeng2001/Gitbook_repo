# 5.最长回文子字符串问题

> ## 题目描述
>
> Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.
>
> The overall run time complexity should be `O(log (m+n))`.

### 例子

**Example 1:**

<pre><strong>Input:</strong> nums1 = [1,3], nums2 = [2]
<strong>Output:</strong> 2.00000
<strong>Explanation:</strong> merged array = [1,2,3] and median is 2.
</pre>

**Example 2:**

<pre><strong>Input:</strong> nums1 = [1,2], nums2 = [3,4]
<strong>Output:</strong> 2.50000
<strong>Explanation:</strong> merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.</pre>

### 条件

* nums1.length == m
* nums2.length == n
* 0 <= m <= 1000
* 0 <= n <= 1000
* 1 <= m + n <= 2000
* $-10 ^6  \leq nums1[i], nums2[i] \leq 10 ^6 $

### 解题思路

分类讨论，字符串长度分为奇数和偶数。若为空字符串，则返回空，当为奇数时恰好

```C++
class Solution {
public:
    std::string longestPalindrome(std::string s) {
        if (s.empty()) return "";
      
        int start = 0, end = 0;
        for (int i = 0; i < s.size(); i++) {
            auto [left1, right1] = expandAroundCenter(s, i, i);     // 奇数长度的回文
            auto [left2, right2] = expandAroundCenter(s, i, i + 1); // 偶数长度的回文

            if (right1 - left1 > end - start) {
                start = left1;
                end = right1;
            }
            if (right2 - left2 > end - start) {
                start = left2;
                end = right2;
            }
        }
        return s.substr(start, end - start + 1);
    }

private:
    std::pair<int, int> expandAroundCenter(const std::string& s, int left, int right) {
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            --left;
            ++right;
        }
        return {left + 1, right - 1};
    }
};
```

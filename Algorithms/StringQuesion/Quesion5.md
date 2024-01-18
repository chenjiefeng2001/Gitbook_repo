# Question 5 最长回文字符串
## 题目描述
> 给定一个字符串，返回最长的回文子字符串

## 题目思路，
首先确定时间复杂度，这里遍历字符串的操作至少要$O(n)$，然后比较在i位置的左边的字符串是否等于右边的字符串，若一开始就不相等，则继续遍历，若相等则继续扩大字符串的比较，直到出现不等为止，用一个变量统计回文字符串的长度，遍历完成之后取最大的那个数。
代码如下所示：
```C++
class Solution {
public:
    std::string longestPalindrome(std::string s) {
        if (s.empty()) return "";//判断是否为空字符串，是的话则返回“”
        int start = 0, end = 0;//初始化开始和结尾
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
    std::pair<int, int> expandAroundCenter(const std::string& s, int left, int right) {//此处用于扩大判断范围，用于检测该回文字符串的左右是否还是回文字符串
        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            --left;
            ++right;
        }
        return {left + 1, right - 1};
    }
};
```


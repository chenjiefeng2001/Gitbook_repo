# Quesion10 正则表达式匹配

## 题目描述

给定一个输入字符串和一个模式，实现正则表达式匹配，支持 和 其中：`s``p``'.'``'*'`

* `'.'`匹配任何单个字符。
* `'*'`匹配零个或多个前一个元素。

匹配应涵盖**整个**输入字符串（而不是部分）。

---

**约束：**

* `1 <= s.length <= 20`
* `1 <= p.length <= 20`
* `s`仅包含小写英文字母。
* `p`仅包含小写英文字母 、 和 。`'.'``'*'`
* 保证角色每次出场，都会有一个之前的有效角色进行匹配。`'*'`

---

## 题目思路

构建正则表达式匹配的DP方程。首先检测是否为空字符串，随后匹配头字符以确定子字符串在字符串中的位置，最后找出所有字符串之后根据符号匹配对应的字符串。

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size(), n = p.size();//define the size of the string and the pattern's  size
        // dp[i][j] means s[0..i-1] matches p[0..j-1]
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        // empty string matches empty pattern
        dp[0][0] = true;
        // empty string matches pattern like a*b*c*
        for (int j = 2; j <= n; ++j) {
            dp[0][j] = p[j - 1] == '*' && dp[0][j - 2];
        }
        // dp[i][j] = dp[i-1][j-1] if s[i-1] == p[j-1] || p[j-1] == '.'
        // dp[i][j] = dp[i][j-2] if p[j-1] == '*' and matches empty string
        // dp[i][j] = dp[i-1][j] if p[j-1] == '*' and matches multiple chars
        for (int i = 1; i <= m; ++i) {
            char sc = s[i - 1];
            for (int j = 1; j <= n; ++j) {
                char pc = p[j - 1];
                if (sc == pc || pc == '.') {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (pc == '*') {
                    if (j >= 2) {
                        char ppc = p[j - 2];
                        if (ppc == sc || ppc == '.') {
                            dp[i][j] = dp[i - 1][j] || dp[i][j - 2];
                        } else {
                            dp[i][j] = dp[i][j - 2];
                        }
                    }
                }
            }
        }
        return dp[m][n];
    }
};
```

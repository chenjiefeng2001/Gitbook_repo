# Quesion14 最长公共前缀

## 题目描述

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string .""
 
---

Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"
Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
 
---
Constraints:

1 <= strs.length <= 200
0 <= strs[i].length <= 200
strs[i] consists of only lowercase English letters.

## 题目思路
构建一个二维数组，然后将字符串从头开始遍历。最后找出共同的最长子字符串
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        //比较每个向量中的对应字母是否相等？
        if (strs.empty()) {
        return "";
    }

    for (int i = 0; i < strs[0].length(); i++) {
        char current_char = strs[0][i];
        for (int j = 1; j < strs.size(); j++) {
            if (i >= strs[j].length() || strs[j][i] != current_char) {
                return strs[0].substr(0, i);
            }
        }
    }

    return strs[0];
    }
};

```


# Quesion20 有效括号对

## 题目描述
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:

Input: s = "()"
Output: true
Example 2:

Input: s = "()[]{}"
Output: true
Example 3:

Input: s = "(]"
Output: false

## 题目思路

使用堆栈来匹配括号，遍历整个字符串，如果遇到括号则推入堆栈内，当组成一对括号则弹出,最后检查堆里面是否是空的，是的话则返回假，否则返回真

代码如下所示：
```cpp
class Solution {
public:
    bool isValid(string s) {
        // 用堆栈匹配方法来匹配括号，如果匹配不到则返回false
        stack<char> st;
        unordered_map<char, char> mapping = {{')', '('}, {']', '['}, {'}', '{'}};

        for (char ch : s) {
            if (ch == '(' || ch == '[' || ch == '{') {
                st.push(ch);
            } else {
                if (st.empty() || st.top() != mapping[ch]) {
                    return false;
                }
                st.pop();
            }
        }

        return st.empty(); // 如果栈为空，说明所有括号都匹配成功
    }
    
};

```

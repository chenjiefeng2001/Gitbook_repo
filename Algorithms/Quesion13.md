# Quesion13 罗马数字转为整数

## 题目描述

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

## 题目思路
跟数字转罗马数字来说，这个实在是太简单了，也是直接打表，而且跟短了
```cpp
class Solution {
public:
    int romanToInt(string s) {
            int res = 0;
    map<char, int> romanMap = {{'M', 1000}, {'D', 500}, {'C', 100}, {'L', 50}, {'X', 10}, {'V', 5}, {'I', 1}};//确定每个字符对应的数字

    for (int i = 0; i < s.length(); i++) {//遍历整个字符串，根据表来换算每个位置的数字，最后返回加和
        int current = romanMap[s[i]];
        int next = (i + 1 < s.length()) ? romanMap[s[i + 1]] : 0;

        if (current < next) {
            res += (next - current);
            i++;  // Skip the next character since it has already been considered.
        } else {
            res += current;
        }
    }

    return res;
    }
};
```


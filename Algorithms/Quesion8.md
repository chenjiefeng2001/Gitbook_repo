# Quesion8 字符串转整数
## 题目描述
Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer (similar to C/C++'s atoi function).
The algorithm for myAtoi(string s) is as follows:
Read in and ignore any leading whitespace.
Check if the next character (if not already at the end of the string) is '-' or '+'. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
Convert these digits into an integer (i.e. "123" -> 123, "0032" -> 32). If no digits were read, then the integer is 0. Change the sign as necessary (from step 2).
If the integer is out of the 32-bit signed integer range [-231, 231 - 1], then clamp the integer so that it remains in the range. Specifically, integers less than -231 should be clamped to -231, and integers greater than 231 - 1 should be clamped to 231 - 1.
Return the integer as the final result.
## 题目思路
首先能确定的是不能使用atoi方式来进行转换。遍历字符串操作至少需要$O(n)$操作，先将字符串通过ascii码表转换为数字，然后压入数组，最后将数组转为数字即可完成，这里还需要处理前导空格和符号，
```C++
class Solution {
public:
    int myAtoi(string s) {
    int i = 0;
    int sign = 1;
    long long num = 0;//初始化

    // 处理前导空格
    while (i < s.length() && s[i] == ' ') {
        i++;
    }

    // 处理符号
    if (i < s.length() && (s[i] == '-' || s[i] == '+')) {
        sign = (s[i++] == '-') ? -1 : 1;
    }

    // 处理数字
    while (i < s.length() && std::isdigit(s[i])) {
        num = num * 10 + (s[i++] - '0');

        if (num * sign < INT_MIN) {
            return INT_MIN;
        } else if (num * sign > INT_MAX) {
            return INT_MAX;
        }
    }

    return num * sign;//采用+-1来判断是否需要加上符号
    }
};
```

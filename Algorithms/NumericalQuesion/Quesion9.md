# Quesion9 回文编号
##题目描述
Given an integer , return if is a xtruex
palindrome, and otherwise.false

## 题目思路
这题和之前的回文字符串异曲同工，可以直接把数字当作字符串进行处理，在进行判断的时候需要排除负数和零结尾的正数均不符合条件。记得判断是否为奇数。
```C++
class Solution {
public:
    bool isPalindrome(int x) {
         // 负数和以0结尾的正数不可能是回文数
    if (x < 0 || (x % 10 == 0 && x != 0)) {
        return false;
    }

    int reversed = 0;

    // 反转一半的数字
    while (x > reversed) {
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }

    // 当数字长度为奇数时，可以通过 reversed / 10 去掉中间的数字
    return x == reversed || x == reversed / 10;
    }
};
``` 

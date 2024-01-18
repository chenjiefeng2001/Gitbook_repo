# Quesion 6 之字形变换
## 题目描述·
he string is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)"PAYPALISHIRING"
P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"
Write the code that will take a string and make this conversion given a number of rows:
string convert(string s, int numRows);
### 题目思路
由题可知，首先得找到该变换的DP方程，因为已经给出了具体的变换格式，可以通过结果倒推DP方程：遍历给出的字符串，首先是a[1][1]到a[1][n]，然后再斜向上到a[n][1],接着继续
代码如下所示：
```C++
class Solution {
public:
    string convert(string s, int numRows) {
       if (numRows == 1 || numRows >= s.length()) {//首先确定是否为单行的Z字形变换，是的话直接返回该字符串
        return s;
    }

    vector<string> result(numRows, "");   //返回的结果
    int index = 0, step = 1;  //用这个去计数需要放哪个位置的字符

    for (char c : s) {//遍历操作，然后按照统计量在对应的位置放置字符
        result[index] += c;
        if (index == 0) {
            step = 1;
        } else if (index == numRows - 1) {
            step = -1;
        }
        index += step;
    }

    string convertedString;
    for (const string& row : result) {
        convertedString += row;
    }//将之字形变换后的字符串再转为和原有字符串格式一致的字符串

    return convertedString;
    }
};
```

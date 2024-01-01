# 1.两数和问题

> ## 问题描述
>
> Given an array of integers `nums` and an integer `target`, return  *indices of the two numbers such that they add up to `target`* .
>
> You may assume that each input would have  ***exactly* one solution** , and you may not use the *same* element twice.
>
> You can return the answer in any order.

例子：

---

**Example 1:**

<pre><strong>Input:</strong> nums = [2,7,11,15], target = 9
<strong>Output:</strong> [0,1]
<strong>Explanation:</strong> Because nums[0] + nums[1] == 9, we return [0, 1].
</pre>

**Example 2:**

<pre><strong>Input:</strong> nums = [3,2,4], target = 6
<strong>Output:</strong> [1,2]
</pre>

**Example 3:**

<pre><strong>Input:</strong> nums = [3,3], target = 6
<strong>Output:</strong> [0,1]
</pre>

---

**Constraints:					**

* $2 \leq nums.length \leq 10^4$
* -10 ^9 \leq nums[i] \leq 10 ^9 $
* $-10^9 \leq target \leq 10^9$
* **Only one valid answer exists.**

## 代码思路：

首先可以从题目中知道该算法的时间复杂度平均情况下为$O(n)$然后使用hashMap可以将查找开销优化到$O(1)$。因为都是查找工作，不需要进行修改删除操作，因此使用HashMap是一个很明智的选择。

---

代码如下所示：

```C++
#include <unordered_map>
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hashMap;		//构建HashMap
        for (int i =0 ; i < nums.size(); ++i){	//遍历整个数组
            if (hashMap.count(target - nums[i])) {
            return {i, hashMap[target - nums[i]]};
            }
                hashMap[nums[i]] = i;
            }
                return {}; 		//if not found, return empty
        }   
};
```

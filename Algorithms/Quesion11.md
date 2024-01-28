# Quesion11  最大水箱问题

## 题目描述

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

## 代码思路
此题使用二分节点法查找，从中间开始使用两个指针遍历左右两边，比较每个泳池的容量，并取最大值。最后返回该最大值作为问题的解。当然，你也可以使用递归的方法去找最大的泳池面积

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        //set the two nodes
        int begin_node=0;//确定初始节点
        int end_node=height.size()-1;
        int res=0;
        while(begin_node<end_node)
        {
            res=height[begin_node]<height[end_node]?
                std::max(res,(end_node-begin_node)*height[begin_node++]):
                std::max(res,(end_node-begin_node)*height[end_node--]);
        }
        return res;

    }
};

```

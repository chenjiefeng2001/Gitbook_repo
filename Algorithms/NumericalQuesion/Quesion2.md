# 2.两数相加问题

> ## 问题描述
>
> You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in  **reverse order** , and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
>
> You may assume the two numbers do not contain any leading zero, except the number 0 itself.
>
> **Example 1:**
>
> ![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

---

### 例子

<pre><strong>Input:</strong> l1 = [2,4,3], l2 = [5,6,4]
<strong>Output:</strong> [7,0,8]
<strong>Explanation:</strong> 342 + 465 = 807.
</pre>

**Example 2:**

<pre><strong>Input:</strong> l1 = [0], l2 = [0]
<strong>Output:</strong> [0]
</pre>

**Example 3:**

<pre><strong>Input:</strong> l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
<strong>Output:</strong> [8,9,9,9,0,0,0,1]</pre>

---

#### 条件


* The number of nodes in each linked list is in the range `[1, 100]`.
* `0 <= Node.val <= 9`
* It is guaranteed that the list represents a number that does not have leading zeros.

### 题目思路

这题很简单，即合并两个链表并作加法，若等于10则进一位。采用双指针遍历的方式来进行合并

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0);	//定义初始节点
        ListNode* cur = dummy;   
        int carry = 0;         //初始化进位
        while (l1 || l2 || carry) {	//循环遍历比较两个链表的大小，并且作加和，随后每个节点都向前移动
            int i1 = l1 ? l1->val : 0;
            int i2 = l2 ? l2->val :						 0;
            int add = i1 + i2 + carry;   
            carry = add >= 10 ? 1 : 0;   
            add = add >= 10 ? add - 10 : add;   
            cur->next = new ListNode(add);   
            cur = cur->next;   
            if (l1)   
            l1 = l1->next;   
            if (l2)   
            l2 = l2->next;   
            }   
            return dummy->next;
    }
};
```

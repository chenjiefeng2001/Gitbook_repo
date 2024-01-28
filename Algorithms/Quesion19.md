# Quesion19 从末尾移除第N个节点

## 题目描述

Given the head of a linked list, remove the nth node from the end of the list and return its head.

 

Example 1:


Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Example 2:

Input: head = [1], n = 1
Output: []
Example 3:

Input: head = [1,2], n = 1
Output: [1]
 

Constraints:

The number of nodes in the list is sz.
1 <= sz <= 30
0 <= Node.val <= 100
1 <= n <= sz

## 代码思路
本题的代码思路不难，主要难点在于如何操作，即移除headp和endp，即可完成移除操作

```cpp
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *p=head;
        int count=0;
        while(p)
        {
          p=p->next;
          count++;
        }
        int c=count-n;
        if(c==0)
        {
          head=head->next;
          return head;
        }
        ListNode *q=head;
        p=NULL;
        for(int i=0;i<c;i++)
        {
          p=q;
          q=q->next;
          }
        p->next=q->next;
        return head;

        
    }
};
  
```

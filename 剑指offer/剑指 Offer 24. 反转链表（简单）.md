# 剑指 Offer 24. 反转链表（简单）

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

示例:

    输入: 1->2->3->4->5->NULL
    输出: 5->4->3->2->1->NULL

限制：

    0 <= 节点个数 <= 5000

### 头插法
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        return headInsert(head);
    }

    ListNode* headInsert(ListNode* head){
        if(head == NULL || head->next == NULL){return head;}
        ListNode* suffix = head->next;
        ListNode* cur = head;     
        cur->next = NULL;
        while(suffix != NULL){
            ListNode* suffix_next = suffix->next;
            suffix->next = cur;
            cur = suffix;
            suffix = suffix_next;
        }
        return cur;
    }
};
```
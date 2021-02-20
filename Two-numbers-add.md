# 链表两数相加

https://leetcode-cn.com/problems/add-two-numbers/

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。


## 模拟
设置进位，考虑最后时刻是否有进位，考虑某个链表是否为空

```c++
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
        ListNode* head = new ListNode(0);
        ListNode* cur = head;
        int carry = 0;
        int temp = 0;
        while(l1 || l2) {
            if(l1 && l2) {
                temp = l1->val + l2->val + carry;  
                l1 = l1->next;
                l2 = l2->next;  
            }
            else {
                l1 = l1? l1:l2;
                l2 = nullptr;
                temp = l1->val + carry;
                l1 = l1->next;
            }

            carry = temp/10;
            temp = temp%10;
            ListNode* node = new ListNode(temp);
            cur->next = node;
            cur = node;
        }
        if(carry != 0) {
            ListNode* node = new ListNode(1);
            cur->next = node;
        }

        return head->next;
    }
}

```
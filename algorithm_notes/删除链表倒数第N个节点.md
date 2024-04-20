# 删除链表倒数第N个节点

## 解题思路：

当要删除链表某个节点时，需要知道被删除节点的前一个节点，本题需要删除倒数第N个节点那么就需要找到倒数第N+1个节点的位置。

可以使用快慢指针找到第N+1个节点的位置：

- 首先快慢指针都指向头节点，然后快指针先走N步

- 然后快慢指针同时走，当快指针刚好指到尾节点时慢指针便指向了倒数第N+1个节点

然后就可以指向删除操作了。



## 边界条件：

- 如果链表节点个数小于N时，不存在倒数第N个节点，直接返回头节点

- 如果链表节点个数等于N时，删除的是头节点，直接返回头节点的下一个节点。



## 代码：

```c
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
// 快慢指针，找到倒数第 N+1 个链表节点
//
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head == nullptr) return nullptr;
        ListNode* fast = head ;
        ListNode* slow = head ;
        int count = 0 ;
        while (fast){
            ++count ;
            fast = fast->next ;
        }
        if(count < n) return head ;
        if(count == n) return head->next ;
        fast = head ;
        for(int i = 0 ; i < n; i++){
            fast = fast->next ;
        }
        while(fast->next){
            fast = fast->next ;
            slow = slow->next ;
        }
        ListNode* q = slow->next ;
        slow->next = slow->next->next ;
        q->next = nullptr ;
        return head ;
    }
};
```



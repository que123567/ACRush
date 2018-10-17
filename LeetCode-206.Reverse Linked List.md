Reverse a singly linked list.

Example:

    Input: 1->2->3->4->5->NULL
    Output: 5->4->3->2->1->NULL

Follow up:

A linked list can be reversed either iteratively or recursively. Could you implement both?

### Approach 1 Runtime 12ms Beat 13.8%
- 下班后留在公司写了一会，写得乱七八糟，~~(╯﹏╰)b，被我分成了三种情况，简直路人代码。。

```js
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
    //nullptr || length==1
    if (head == nullptr || head->next == nullptr) {
        return head;
    }
    //length ==2
    ListNode *pre = head;
    ListNode *cur = head->next;
    if (cur->next == nullptr) {
        cur->next = pre;
        pre->next = nullptr;
        return cur;
    }
    //length > 2
    bool first = true;
    while (cur->next != nullptr) {
        ListNode *tmpNext = cur->next;
        cur->next = pre;
        if (first) {
            pre->next = nullptr;
            first = false;
        }
        pre = cur;
        cur = tmpNext;
        if (cur->next == nullptr) {
            cur->next = pre;
            break;
        }
    }
    return cur;
    }
};
```

### Approach 2 Runtime 4ms Beat 100%
- 同1，依旧是iteration版，但这个版本逻辑清晰多了

```js
ListNode *reverseNode_2(ListNode *head) {
    if (head == nullptr || head->next == nullptr) {
        return head;
    }
    ListNode *next = head->next;
    head->next = nullptr;
    ListNode *tmp;
    while (next != nullptr) {
        tmp = next;
        next = next->next;
        tmp->next = head;
        head = tmp;
    }
    return head;
}
```

### Approah 3 这个是recursion版，

```js
ListNode *reverseNode_3(ListNode *head) {
    if (NULL == head || NULL == head->next) return head;

    struct ListNode *p = head->next;
    head->next = NULL;
    struct ListNode *newhead = reverseNode_3(p);
    p->next = head;

    return newhead;
}
```


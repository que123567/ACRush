Given a linked list, rotate the list to the right by *k* places, where *k* is non-negative.

Example 1:

    Input: 1->2->3->4->5->NULL, k = 2
    Output: 4->5->1->2->3->NULL
    Explanation:
    rotate 1 steps to the right: 5->1->2->3->4->NULL
    rotate 2 steps to the right: 4->5->1->2->3->NULL

Example 2:

    Input: 0->1->2->NULL, k = 4
    Output: 2->0->1->NULL
    Explanation:
    rotate 1 steps to the right: 2->0->1->NULL
    rotate 2 steps to the right: 1->2->0->NULL
    rotate 3 steps to the right: 0->1->2->NULL,
    rotate 4 steps to the right: 2->0->1->NULL

### Approach 1: 
- 头尾相连组成循环链表，然后根据k的大小一直迭代。

### Runtime:8ms Beat98.98%

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
 ListNode *rotateRight(ListNode *head, int k) {
    if (head == nullptr || k == 0) {
        return head;
    }
    auto tail = head;
    //to the end
    int size = 1;
    while (tail->next != nullptr) {
        tail = tail->next;
        size++;
    }
    k %= size;
    //link tail with head
    if (tail != nullptr) {
        tail->next = head;
    }
    auto tmpHead = head;
    //size-k:in reverse order
    for (int i = 0; i < (size - k); ++i) {
        tmpHead = tmpHead->next;
    }
    while (--size > 0) {
        tmpHead = tmpHead->next;
    }
    //to the end
    auto resHead = tmpHead->next;
    //reset cicrle-linkedList to sigle-linkedList
    tmpHead->next = nullptr;
    return resHead;
    }
};
```
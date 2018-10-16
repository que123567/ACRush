---

Given a singly linked list *L*: *L*0→*L*1→…→*L**n*-1→*L*n,
reorder it to: *L*0→*L**n*→*L*1→*L**n*-1→*L*2→*L**n*-2→…

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example 1:

    Given 1->2->3->4, reorder it to 1->4->2->3.

Example 2:

    Given 1->2->3->4->5, reorder it to 1->5->2->4->3.

### My Sample
#### List
|L0|L1|L2|L3|L4|
|-:|-:|-:|-:|-:|
| 1| 2| 3| 4| 5|

#### Left part
|L0|L1|L2|  
|-:|-:|-:|        
|1|2|3|

#### Right part(reverse)
|L4|L3| 
|-:|-:|
| 5| 4|

#### merge
|L0|L4|L1|L3|L2|
|-:|-:|-:|-:|-:|
| 1|  | 2|  | 3|
|  | 5|  | 4|  |

####Answer: 1->5->2->4->3
------------------

### Approach 1: 
- 将其看作左右两个链表，右链表倒转，再按顺序插入左链表

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
    void reorderList(ListNode* head) {
        
    }
};
```






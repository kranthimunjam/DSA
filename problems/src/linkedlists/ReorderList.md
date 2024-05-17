# [Reorder List](https://leetcode.com/problems/reorder-list/description/)

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln
Reorder the list to be on the following form:

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

#### Example 1:
![img.png](img.png)
**Input:** head = [1,2,3,4]\
**Output:** [1,4,2,3]

#### Example 2:
![img_1.png](img_1.png)
**Input:** head = [1,2,3,4,5]\
**Output:** [1,5,2,4,3]

#### Constraints:

* The number of nodes in the list is in the range [1, 5 * 10<sup>4</sup>].
* 1 <= Node.val <= 1000

# Solution

```java

class Solution {
    public void reorderList(ListNode head) {
        //1. push the second half into stack 
        //2. take a node from first half and pop a node from stack and merge.
        //3. keep doing it until stack is empty. 

        if(head.next == null) return;

        ListNode slow=head, fast=head;
        int size=0;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast=fast.next.next;
            ++size;
        }

        ArrayDeque<ListNode> stack = new ArrayDeque<>();
        while(slow!=null){
            stack.push(slow);
            slow=slow.next;
        }

        ListNode curr = head, ahead=head.next;

        while(size>0){
            curr.next = stack.pop();
            curr.next.next = ahead;
            curr =ahead;
            if(ahead!=null) ahead = ahead.next;
            size--;
        }
        if(curr!=null) curr.next = null;
    }
}
```

#### Better Approach:

```java
class Solution {
    public void reorderList(ListNode head) {
        reorderList(head, head);
    }

    public ListNode reorderList(ListNode slow, ListNode fast) {
        if (fast.next == null) {
            ListNode tail = slow.next;
            slow.next = null;
            return tail;
        }
        if (fast.next.next == null) {
            ListNode tail = slow.next.next;
            slow.next.next = null;
            return tail;
        }
        ListNode reorder = reorderList(slow.next, fast.next.next);

        ListNode tmp = slow.next;
        slow.next = reorder;
        ListNode tmp1 = reorder.next;
        reorder.next = tmp;
        return tmp1;
    }
}
```
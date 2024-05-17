# [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

Given the `head` of a singly linked list, reverse the list, and return the _reversed_ list.

# Solution
Using head recursion:
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) return head;

        ListNode prev = null, curr = head, next = curr.next;

        while(curr!= null){
            curr.next = prev;
            prev = curr;
            curr = next;
            if(curr!=null) next = curr.next;
        }
        return prev;
    }
}
```
Iterative approach:

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode node = head;

    while (node != null) {
        ListNode aux = node.next;
        node.next = prev;
        
        prev = node;
        node = aux;
    }
    
    return prev;
}
```
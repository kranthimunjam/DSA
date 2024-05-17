# [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

Given the `head` of a `linked list`, remove the n<sup>th</sup> node from the end of the list and return its head.

# Solution

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode ptr1 = head, ptr2=null;
        int counter = 1;
        // move first pointer "n" nodes;
        while(counter<=n){
            if(ptr1 == null) return head;
            ptr1 = ptr1.next;
            ++counter;
        }  
        
        //move first pointer till the end.
        while(ptr1 != null ){
            if(ptr2 == null) ptr2 = head;
            else ptr2 = ptr2.next;
            ptr1 = ptr1.next;    
        }
        if(ptr2 == null) {          
            ptr1 = head.next;
            head.next = null;
            return ptr1;
        }
        if(ptr2.next == null) return null; //single node condition.
        ptr2.next = ptr2.next.next;        
        return head;
        
    }
}
```
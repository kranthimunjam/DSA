


# Solution

### Slow and Fast pointers: 

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next ==null) return false;
        ListNode slow = head, fast = head;
        while(slow!=null && fast!=null && fast.next!=null){
            slow=slow.next;
            fast=fast.next.next;
            if(slow==fast) return true;
        }
        return false;
    }
}
```
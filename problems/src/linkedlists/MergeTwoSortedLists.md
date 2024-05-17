# [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode header=null, prev =null;
        if(l1 == null && l2 == null) return null;
        else if (l1 == null ) return l2;
        else if(l2 == null) return l1;
        while(l1 !=null && l2 !=null ){
            if(l1.val<l2.val){
                if(header==null) header = l1;
                if(prev == null) prev=header;
                else {
                    prev.next = l1;
                    prev = l1;
                }
                l1=l1.next;
            }
            else{
                if(header==null) header = l2;
                if(prev == null) prev=header;
                else {
                    prev.next = l2;
                    prev = l2;
                }
                l2=l2.next;
            }
        }
        if(l1!=null){
            prev.next = l1;
        }
        if(l2!=null){
            prev.next = l2;
        }
        return header;
        
    }
}
```
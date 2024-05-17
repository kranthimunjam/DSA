# [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)

You are given an array of `k` linked-lists `lists`, each linked-list is **sorted** in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

#### Example 1:

**Input:** lists = [[1,4,5],[1,3,4],[2,6]]\
**Output:** [1,1,2,3,4,4,5,6]\
**Explanation:** 
The linked-lists are:\
[\
1->4->5,\
1->3->4,\
2->6\
]\
merging them into one sorted list:\
1->1->2->3->4->4->5->6

#### Example 2:

**Input:** lists = []\
**Output:** []

#### Example 3:

**Input:** lists = [[]]\
**Output:** []

#### Constraints:

* k == lists.length
* 0 <= k <= 10<sup>4</sup>
* 0 <= lists[i].length <= 500
* -10<sup>4</sup> <= lists[i]\[j] <= 10<sup>4</sup>
* lists[i] is sorted in ascending order.
* The sum of lists[i].length will not exceed 10<sup>4</sup>.

# Solution

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0) return null;
        
        return MergeSort(lists, 0, lists.length-1);
    }
    
    // Applying merge sort considering each list as a single element.
    private ListNode MergeSort(ListNode[] lists, int low, int high){
        if(low==high) return lists[low];
        else{
            int mid = low+ (high-low)/2;
             return Merge(MergeSort(lists, low, mid), MergeSort(lists, mid+1, high));
        }  
    }
    
    // merge two lists;
    private ListNode Merge(ListNode list1, ListNode list2){
        if(Objects.isNull(list1)) return list2;
        else if(Objects.isNull(list2)) return list1;
        else {
            if(list1.val < list2.val){
                list1.next = Merge(list1.next, list2);
                return list1;
            } else {
                list2.next = Merge(list1, list2.next);
                return list2;
            }
        }
    }   
}
```

Using priority queue:

```java
public class Solution {
    public ListNode mergeKLists(List<ListNode> lists) {
        if (lists==null||lists.size()==0) return null;
        
        PriorityQueue<ListNode> queue= new PriorityQueue<ListNode>(lists.size(),new Comparator<ListNode>(){
            @Override
            public int compare(ListNode o1,ListNode o2){
                if (o1.val<o2.val)
                    return -1;
                else if (o1.val==o2.val)
                    return 0;
                else 
                    return 1;
            }
        });
        
        ListNode dummy = new ListNode(0);
        ListNode tail=dummy;
        
        for (ListNode node:lists)
            if (node!=null)
                queue.add(node);
            
        while (!queue.isEmpty()){
            tail.next=queue.poll();
            tail=tail.next;
            
            if (tail.next!=null)
                queue.add(tail.next);
        }
        return dummy.next;
    }
}
```
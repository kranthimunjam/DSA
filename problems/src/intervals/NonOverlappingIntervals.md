# [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)

Given an array of intervals `intervals` where **intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]**, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

#### Example 1:

**Input:** intervals = [[1,2],[2,3],[3,4],[1,3]]\
**Output:** 1\
**Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.

#### Example 2:

**Input:** intervals = [[1,2],[1,2],[1,2]]\
**Output:** 2\
**Explanation:** You need to remove two [1,2] to make the rest of the intervals non-overlapping.

#### Example 3:

**Input:** intervals = [[1,2],[2,3]]\
**Output:** 0\
**Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.

#### Constraints:

* 1 <= intervals.length <= 10<sup>5</sup>
* intervals[i].length == 2
* -5 * 10<sup>4</sup> <= start<sub>i</sub> < end<sub>i</sub> <= 5 * 10<sup>4</sup>

# Solution

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        int start =0, end=1;
        int size = intervals.length;
        // sort the intervals based on start time
        // if start times are same then sort based on the end time.
        Arrays.sort(intervals,(int[] a, int[] b) -> Integer.compare(a[1],b[1]));

        int count =1; 
        int[] prev = intervals[0];// first one is always selected.

        for(int i=1;i<size;i++){
            int[] curr = intervals[i];
            // if curr start time 
            if(curr[start]>= prev[end]){
                ++count;
                prev = curr;
            }
        }
        return size-count;
        
    }
}
```

#### Another approach:

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a,b)-> a[1]-b[1]);
        int end = intervals[0][1]; int count=0;
        for(int i=1;i<intervals.length;i++) {
           if(intervals[i][0]<end){
            count++;
           } else {
            end = intervals[i][1];
           }
        }
        return count;
    }
    
}
```
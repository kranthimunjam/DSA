# [Insert Interval](https://leetcode.com/problems/insert-interval/description/)

You are given an array of `non-overlapping` intervals `intervals` where intervals[i] = [start<sub>i</sub>, end<sub>i</sub>] represent the start and the end of the i<sup>th</sup> intervals and `intervals` is sorted in ascending order by start<sub>i</sub>. 
You are also given an intervals `newInterval = [start, end]` that represents the start and end of another intervals.

Insert `newInterval` into intervals such that `intervals` is still sorted in ascending order by start<sub>i</sub> and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

**Note** that you don't need to modify `intervals` in-place. 
You can make a new array and return it.

#### Example 1:

**Input:** intervals = [[1,3],[6,9]], newInterval = [2,5]\
**Output:** [[1,5],[6,9]]

#### Example 2:

**Input:** intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]\
**Output:** [[1,2],[3,10],[12,16]]\
**Explanation:** Because the new intervals [4,8] overlaps with [3,5],[6,7],[8,10].

### Constraints:

* 0 <= intervals.length <= 10<sup>4</sup>
* intervals[i].length == 2
* 0 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>5</sup>
* intervals is sorted by start<sub>i</sub> in ascending order.
* newInterval.length == 2
* 0 <= start <= end <= 10<sup>5</sup>

# Solutions

```java
class Solution {
    class Interval {
        public int start;
        public int end;
        public Interval(int s, int e){
            start = s;
            end = e;
        }
        
        @Override
        public String toString(){
            return start+":"+end;
        }
    }
    
    
    /*
    * 1. Create a list and add new intervals in appropriate position in that list.
    * 2. Traverse the list and merge wherever needed.
    */
    
    public int[][] insert(int[][] intervals, int[] newInterval) {
        boolean addedNewInterval =false;
        int length = intervals.length;
        ArrayList<Interval> list = new ArrayList<>();
        for(int i=0;i<length;i++){
            // new intervals gets inserted in the list;
            if(!addedNewInterval && intervals[i][0]> newInterval[0]){
                list.add(new Interval(newInterval[0],newInterval[1]));
                addedNewInterval = true;
            }
            list.add(new Interval(intervals[i][0],intervals[i][1]));
        }
        
        // this is the case where new intervals gets added at the end of original list;
        if(!addedNewInterval) list.add(new Interval(newInterval[0],newInterval[1]));
        
        ArrayList<Interval> result = new ArrayList<>();
        Interval prevInterval=null;
        // Compare and merge;
        for(int i=0;i<=list.size();i++){
            if(i==0 )   prevInterval = list.get(i);
            else if(i==list.size()) result.add(prevInterval);
            else{
                if(prevInterval.end>=list.get(i).start){
                    int end = (prevInterval.end>list.get(i).end)?prevInterval.end : list.get(i).end;
                    prevInterval = new Interval(prevInterval.start,end);
                    
                } else{
                    result.add(prevInterval);
                    prevInterval= list.get(i);
                }
            }
        }
        
        int[][] finalResult = new int[result.size()][];
        
        // finally form the result and return it;
        for(int i=0;i<result.size(); i++){
            int[] temp = new int[2];
            temp[0]= result.get(i).start;
            temp[1]= result.get(i).end;
            finalResult[i] = temp;
        }
        
        return finalResult;
    }
}
```
### Better Approach:

There are only three possibilities:
1. If new intervals is less than start of first old intervals, then append new intervals in the front.
2. If new intervals is greater than end of last old intervals, then append new intervals in the end.
3. If above two are not satisfied then new intervals will be somewhere in the middle of old intervals.

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> list = new ArrayList<>();
        int n = intervals.length, i = 0, newIntervalStart = newInterval[0], newIntervalEnd = newInterval[1];
        boolean added = false;
        
        while (i < n) {
            int currStart = intervals[i][0];
            int currEnd = intervals[i][1];
            // new intervals smaller than first
            if (!added && newIntervalEnd < currStart) {
                list.add(newInterval);
                added = true;
            } else if (!added && newIntervalStart <= currEnd) { // merge intervals
                int newStart = Math.min(currStart, newIntervalStart);
                int newEnd = Math.max(currEnd, newIntervalEnd);                
                while (i+1 < n && newEnd >= intervals[i+1][0]) {
                    i++;
                }
                newEnd = Math.max(intervals[i][1], newEnd);

                list.add(new int[] {newStart, newEnd});
                added = true;
                i++;
            } else { // new intervals greater than last
                list.add(intervals[i]);
                i++;
            }
        }
        
        if (!added) list.add(newInterval);
        
        int[][] res = new int[list.size()][2];
        for (int j = 0; j < res.length; j++) {
            res[j] = list.get(j);
        }
        return res;
    }
}
```
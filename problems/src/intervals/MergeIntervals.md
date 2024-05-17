# [Merge Intervals](https://leetcode.com/problems/merge-intervals/description/)

Given an array of `intervals` where **intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]**, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

#### Example 1:

**Input:** intervals = [[1,3],[2,6],[8,10],[15,18]]\
**Output:** [[1,6],[8,10],[15,18]]\
**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

#### Example 2:

Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.


#### Constraints:

* 1 <= intervals.length <= 10<sup>4</sup>
* intervals[i].length == 2
* 0 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>4</sup>

# Solution

Optimized for memory: Sort and merge.

```java
class Solution {
    class Interval {
        public int start;
        public int end;
        public Interval(int s, int e){
            start = s;
            end = e;
        }
    }
    
    
    public int[][] merge(int[][] intervals) {
        int length = intervals.length;
        Interval[] intervalArr = new Interval[length];
        for(int i=0;i<length;i++){
            intervalArr[i] = new Interval(intervals[i][0],intervals[i][1]); 
        }
        
        Arrays.sort(intervalArr, (Interval a, Interval b) -> {
                                            if(a.start != b.start) return a.start-b.start;
                                            else return a.end-b.end;
                                        } );
        ArrayList<Interval> resultantList = new ArrayList<>();
        Interval prevInterval = null;
        
        // Compare and merge;
        for(int i=0;i<=length;i++){
            if(i==0 )   prevInterval = intervalArr[i];
            else if(i==length) resultantList.add(prevInterval);
            else{
                if(prevInterval.end>=intervalArr[i].start){
                    int end = (prevInterval.end>intervalArr[i].end)?prevInterval.end : intervalArr[i].end;
                    prevInterval = new Interval(prevInterval.start,end);
                    
                } else{
                    resultantList.add(prevInterval);
                    prevInterval= intervalArr[i];
                }
            }
        }
        
        int[][] finalResult = new int[resultantList.size()][];
        
        for(int i=0;i<resultantList.size(); i++){
            int[] temp = new int[2];
            temp[0]= resultantList.get(i).start;
            temp[1]= resultantList.get(i).end;
            finalResult[i] = temp;
        }
        
        return finalResult;
    }
}
```

Optimized for Time:

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals == null || intervals.length == 0) {
            return new int[0][];
        }

        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();
        int[] mergedInt = intervals[0];

        for(int i = 1; i < intervals.length; i++) {
            int[] intervals = intervals[i];

            if(intervals[0] <= mergedInt[1]) {
                mergedInt[1] = Math.max(mergedInt[1], intervals[1]);
            }
            else {
                merged.add(mergedInt);
                mergedInt = intervals;
            }
        }
        merged.add(mergedInt);
        return merged.toArray(new int[merged.size()][]);

    }

}
```
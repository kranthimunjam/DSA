
Given an array of meeting time intervals consisting of start and end times[[s<sub>1</sub>,e<sub>1</sub>],[s<sub>2</sub>,e<sub>2</sub>],...].
Find the minimum number of conference rooms required.

#### Example 1:

**Input:** [[0, 30],[5, 10],[15, 20]]\
**Output:** 2

#### Example 2:
**Input:** [[7,10],[2,4]]\
**Output:** 1

# Solution

```java
class Solution {
    public int minMeetingRooms(Interval[] intervals) {
        int[] starts = new int[intervals.length];
        int[] ends = new int[intervals.length];
        for (int i = 0; i < intervals.length; i++) {
            starts[i] = intervals[i].start;
            ends[i] = intervals[i].end;
        }
        Arrays.sort(starts);
        Arrays.sort(ends);
        int rooms = 0, endsItr = 0;
        for (int i = 0; i < starts.length; i++) {
            if (starts[i] < ends[endsItr]) {
                rooms++;
            } else {
                endsItr++;
            }
        }
        return rooms;
    }
}
```
Given an array of meeting time intervals consisting of start and end times[[s<sub>1</sub>,e<sub>1</sub>],[s<sub>2</sub>,e<sub>2</sub>],...], determine if a person could attend all meetings.

#### Example 1:

**Input:** [[0,30],[5,10],[15,20]]\
**Output:** false

#### Example 2:

**Input:** [[7,10],[2,4]]\
**Output:** true

# Solution

The idea here is to sort the meetings by starting time. Then, go through the meetings one by one and make sure that each meeting ends before the next one starts.

```java
public boolean canAttendMeetings(Interval[] intervals) {
    // Sort the intervals by start time
    Arrays.sort(intervals, (x, y) -> x.start - y.start);
    for (int i = 1; i <intervals.length; i++){
        if(intervals[i-1].end>intervals[i].start)return false;
    }
    return true;
}
```
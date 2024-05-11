# []()

You are given an integer array `nums`. 
You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

**Example 1:**

- Input: nums = [2,3,1,1,4]
- Output: true

Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

- Input: nums = [3,2,1,0,4]
- Output: false

Explanation: You will always arrive at index 3 no matter what. 
Its maximum jump length is 0, which makes it impossible to reach the last index.

**Constraints:**

- 1 <= nums.length <= 104
- 0 <= nums[i] <= 105

# Solutions

The basic idea is this: at each step, we keep track of the furthest reachable index. The nature of the problem (eg. maximal jumps where you can hit a range of targets instead of singular jumps where you can only hit one target) is that for an index to be reachable, each of the previous indices have to be reachable.

Hence, it suffices that we iterate over each index, and If we ever encounter an index that is not reachable, we abort and return false. By the end, we will have iterated to the last index. 
If the loop finishes, then the last index is reachable.

```java
class Solution {
    public boolean canJump(int[] nums) {
        if(nums.length==1) return true;
        int reach = nums[0];
        for(int i=1;i<nums.length;i++){
            if(reach<1) return false;
            reach = Math.max(reach-1,nums[i]);
        }
        return true;
    }
}
```

This can be made even faster by returning early. If we check if (reachable >= len - 1)(i.e., last index is reachable), we can return early without looping over the rest of the array.

```java
public boolean canJump(int[] nums) {
    int reachable = 0, len = nums.length;
    for (int i = 0; i < len && i <= reachable; i++) {
        reachable = Math.max(reachable, i + nums[i]);
        if (reachable >= len - 1) return true;   // terminate loop early to speed up
    }
    return false;

}
```
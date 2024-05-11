# [House Robber](https://leetcode.com/problems/house-robber/)

You are a professional robber planning to rob houses along a street. 
Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and 
it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, 
return the maximum amount of money you can rob tonight without alerting the police.

**Example 1:**

- Input: nums = [1,2,3,1]
- Output: 4

Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

**Example 2:**

- Input: nums = [2,7,9,3,1]
- Output: 12

Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.


**Constraints:**

- 1 <= nums.length <= 100
- 0 <= nums[i] <= 400

# Solutions

**Time Complexity:** `O(n)`\
**Space Complexity:** `O(n)`

```java
// logic is Math.max(nums[currIndex]+dp[currIndex-2], dp[currIndex-1])
public int rob(int[] nums) {
    int[] dp = new int[nums.length];
    dp[0]=nums[0];

    for(int i =1;i<nums.length;i++){
        int withoutCurrent = dp[i-1];
        int withoutLastOne = (i-2)>=0?dp[i-2]:0; // this is max we can ron till n-2
        int withCurrent = nums[i] + withoutLastOne;

        dp[i]= Math.max(withCurrent, withoutCurrent );
    }
    return dp[nums.length-1];
}
```
Here we can optimise further in terms of memory. 
We don’t need array to store, we just need `dp[i-1]`, `dp[i-2]` when at `i`. 
So we can use two variables.

```java
// logic is Math.max(nums[currIndex]+dp[currIndex-2], dp[currIndex-1])
public int rob(int[] nums) {
    int maxTillPrevPrev = 0, maxTillPrev = nums[0], maxTillHere=nums[0];

    for(int i =1;i<nums.length;i++){
        maxTillHere = Math.max(nums[i] + maxTillPrevPrev, maxTillPrev);
        maxTillPrevPrev = maxTillPrev;
        maxTillPrev = maxTillHere;
    }
    return maxTillHere;
}
```
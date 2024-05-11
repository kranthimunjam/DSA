# [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

Given an integer array `nums`, return the length of the longest strictly increasing
subsequence.



**Example 1:**

- Input: nums = [10,9,2,5,3,7,101,18]
- Output: 4

- Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

**Example 2:**

- Input: nums = [0,1,0,3,2,3]
- Output: 4

**Example 3:**

- Input: nums = [7,7,7,7,7,7,7]
- Output: 1

**Constraints:**

- 1 <= nums.length <= 2500
- -104 <= nums[i] <= 104


**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

# Solutions

Bottom-up DP solution:
```java
public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);

        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j] && dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                }
            }
        }
        return Arrays.stream(dp).max().orElse(0);
 }
```
Using binary search to reduce the complexity:

**Time Complexity:** `O(NlogN)`

The idea is that as you iterate the sequence, 
you keep track of the minimum value a subsequence of given length might end with, 
for all so far possible subsequence lengths. So `dp[i]` is the minimum value a subsequence of length `i+1` might end with. 
Having this info, for each new number we iterate to, we can determine the longest subsequence where it can be appended using binary search. 
The final answer is the length of the longest subsequence we found so far.

```java
public int lengthOfLIS(int[] nums) {
    int[] dp=new int[nums.length];
    ArrayList<Integer> arr=new ArrayList<>();
    for(int i=0;i<nums.length;i++)
    {
        if(arr.size()==0 || arr.get(arr.size()-1)<nums[i]) arr.add(nums[i]);
        else{
            int start=0;
            int end=arr.size()-1;
            
            while(start<=end)
            {
                int mid=start+(end-start)/2;
                if(arr.get(mid)>=nums[i]) end=mid-1;
                else start=mid+1;
            }
            arr.set(start,nums[i]);
        }
    }
    return arr.size();
 }
```
Using `binarySearch()` from Arrays:

```java
public class Solution {
    public int lengthOfLIS(int[] nums) {            
        int[] dp = new int[nums.length];
        int len = 0;

        for(int x : nums) {
            int i = Arrays.binarySearch(dp, 0, len, x);
            if(i < 0) i = -(i + 1);
            dp[i] = x;
            if(i == len) len++;
        }

        return len;
    }
}
```


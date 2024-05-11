# [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Given an integer array `nums`, find the
subarray
with the largest sum, and return its sum.

**Example 1:**

- Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
- Output: 6

Explanation: The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

- Input: nums = [1]
- Output: 1

Explanation: The subarray [1] has the largest sum 1.


**Example 3:**

- Input: nums = [5,4,-1,7,8]
- Output: 23

Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.

# Solutions:

**Kadane’s algorithm:**

```agsl
public int maxSubArray(int[] nums) {
    int max_ending_here=0, sum_so_far=Integer.MIN_VALUE;
    for(int current:nums){
        max_ending_here +=current;
        sum_so_far = Math.max(sum_so_far,max_ending_here);
        if(max_ending_here<0) max_ending_here=0;
    }
    return sum_so_far;
}
```

**Bottom-up DP:**

```agsl
public int maxSubArray(int[] nums) {
    int length = nums.length;
    int[] dp = new int[length];
    dp[0] = nums[0];
    int maxOverall = dp[0];
    for (int i=1;i<length;i++){
        dp[i] = Math.max(nums[i], dp[i-1] + nums[i]);
        maxOverall = Math.max(maxOverall, dp[i]);
    }
    return maxOverall;
}
```

**Divide and conquer:**

```agsl
protected int helper(int[] nums, int start, int mid , int end){
    int lSum = Integer.MIN_VALUE;
    int rSum = Integer.MIN_VALUE;
    
    int sum = 0;
    for (int i=mid;i>=start;i--){
        sum += nums[i];
        lSum = Math.max(sum , lSum);
    }
        
    sum = 0;
    for (int i=mid+1;i<=end;i++){
        sum += nums[i];
        rSum = Math.max(sum, rSum);
    }
        
    return lSum + rSum;
    
}
protected int helper(int[] nums, int start, int end){
    if (start == end)
        return nums[start];
    int mid = start + (end - start ) / 2 ;
    int left = helper(nums, start, mid);
    int right = helper(nums, mid+1, end);
    int both = helper(nums, start, mid , end);
    return Math.max(left, Math.max(right, both));
    
}
public int maxSubArray(int[] nums) {
    return helper(nums, 0, nums.length - 1);    
}
```
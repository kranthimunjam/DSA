# [Missing Number](https://leetcode.com/problems/missing-number/)

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, 
return the only number in the range that is missing from the array.

**Example 1:**
- Input: nums = [3,0,1]
- Output: 2

Explanation: n = 3 since there are 3 numbers, so all numbers are in the range `[0,3]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 2:**
- Input: nums = [0,1]
- Output: 2

- Explanation: n = 2 since there are 2 numbers, so all numbers are in the range `[0,2]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 3:**

- Input: nums = [9,6,4,2,3,5,7,0,1]
- Output: 8

Explanation: n = 9 since there are 9 numbers, so all numbers are in the range `[0,9]`. 8 is the missing number in the range since it does not appear in `nums`.


**Constraints:**

- n == nums.length
- 1 <= n <= 104
- 0 <= nums[i] <= n
- All the numbers of `nums` are unique.


**Follow up:** Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

# Solutions

**1.XOR**

```agsl
class Solution {
    public int missingNumber(int[] nums) {
        int number = 0;
        for(int i=0;i<=nums.length;i++){
            number = number ^ i;
            if( i != nums.length ) number = number ^ nums[i];
        }
        return number;
    }
}
```

**2.SUM**

```agsl
public int missingNumber(int[] nums) { //sum
    int len = nums.length;
    int sum = (0+len)*(len+1)/2;
    for(int i=0; i<len; i++)
        sum-=nums[i];
    return sum;
}
```

**3.Binary Search**

```agsl
public int missingNumber(int[] nums) { //binary search
    Arrays.sort(nums);
    int left = 0, right = nums.length, mid= (left + right)/2;
    while(left<right){
        mid = (left + right)/2;
        if(nums[mid]>mid) right = mid;
        else left = mid+1;
    }
    return left;
}
```

If the array is already sorted, I prefer Binary Search method. Otherwise, the XOR method is better.

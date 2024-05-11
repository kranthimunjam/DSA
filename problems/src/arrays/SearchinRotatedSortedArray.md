# [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` (1 <= k < nums.length) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed)`. 

For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index 3 and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer target, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

- Input: nums = [4,5,6,7,0,1,2], target = 0
- Output: 4

**Example 2:**

- Input: nums = [4,5,6,7,0,1,2], target = 3
- Output: -1

**Example 3:**

- Input: nums = [1], target = 0
- Output: -1


**Constraints:**

- 1 <= nums.length <= 5000
- -104 <= nums[i] <= 104
- All values of `nums` are unique.
- `nums` is an ascending array that is possibly rotated.
- -104 <= target <= 104

# Solutions

**Approach 1:**

```agsl
class Solution {
    public int search(int[] nums, int target) {
        int size = nums.length;
        if(nums[0]<nums[nums.length-1]){
            return binarySearch(nums, 0, size-1, target);
        } 
        int pivotIndex = getPivotIndex(nums,0, nums.length-1);
        if(nums[0]<=target) return binarySearch(nums, 0, pivotIndex, target);
        else return binarySearch(nums, pivotIndex+1, size-1, target);
    }

    int getPivotIndex(int[] nums, int low, int high) {
        if (low == high)  return low;
            
        int mid = low + (high - low) / 2;

        if (nums[low] < nums[mid]) {
            return getPivotIndex(nums, mid, high);
        } else {
            return getPivotIndex(nums, low, mid);
        }
    }

    int binarySearch(int[] nums, int low, int high, int target){
        // terminating condition
        if(low>high) return -1;
        
        if(low==high) return (nums[low]==target)?low:-1;
        
        int mid = low + (high - low) / 2;
        if(nums[mid]==target) return mid;
        else if(nums[mid]>target) return binarySearch(nums, low, mid-1, target);
        else return binarySearch(nums, mid+1, high, target); 
    }
}
```

**Approach 2:**

Another approach is to use the `binarySearch()` in Arrays. 
If `target` is not found then the return value is some negative(expected index) value but we need to send -1 if not found. 
Also, we have to create subarrays for binary search to work so that copying is O(n) in worst case.

```agsl
public int search(int[] nums, int target) {
  int index=-1;
  // if array is not rotated then directly use binary search for target
  if(nums[0]<nums[nums.length-1]){
      index  = Arrays.binarySearch(nums,target);
      return (index<0)?-1:index;
  } 
  int pivotIndex = getPivotIndex(nums,0, nums.length-1);
  if(target>=nums[0]){ // target will be in first subarray
      index = Arrays.binarySearch(Arrays.copyOfRange(nums,0, pivotIndex+1),target);
      index = (index<0)?-1:index;

  } else { // target will be in second subarray
  // here index returned by binary search is within the subarray
  // finalIndex = index + pivotIndex
      index = Arrays.binarySearch(Arrays.copyOfRange(nums,pivotIndex+1, nums.length),target);
      index = (index<0)?-1: (pivotIndex+1+index);
  }
  return index;
}

int getPivotIndex(int[] nums, int low, int high) {
  int mid = low + (high - low) / 2;
  if (low == high)
      return low;

  if (nums[low] < nums[mid]) {
      return getPivotIndex(nums, mid, high);
  } else {
      return getPivotIndex(nums, low, mid);
  }
}

```
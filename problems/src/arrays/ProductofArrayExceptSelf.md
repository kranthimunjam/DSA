# [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Given an integer array `nums`, return an array answer such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any `prefix` or `suffix` of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.



**Example 1:**

- Input: nums = [1,2,3,4]
- Output: [24,12,8,6]

**Example 2:**

- Input: nums = [-1,1,0,-3,3]
- Output: [0,0,9,0,0]


**Constraints:**

- 2 <= nums.length <= 105
- -30 <= nums[i] <= 30

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.


**Follow up:** Can you solve the problem in `O(1)` extra space complexity? 
(The output array does not count as extra space for space complexity analysis.)

# Solutions

**Solution 1** 

The idea is to save product till left index and product till right index calculated and saved so that later it can be used to calculate the final product. 
For example for given array `1, 2, 3, 4` left product is `1, 2, 6, 24` and
right product is `24, 24, 12, 4` Now for `index 0` there is no left product so only right product till index 1 which is 24 so final product is 24. 
for index 1, final product is multiplication of product till left index from left side and product till right index from right side which is `left_product[0]*right_product[2]`. 
Same for other indexes as well.

```agsl
public int[] productExceptSelf(int[] nums) {
    int size = nums.length;
    int[] right_product = new int[size];

    right_product[size-1] = nums[size-1];
    
    //build product till right index 
    for(int i=size-2;i>=0;i--){
        right_product[i] = nums[i]*right_product[i+1]; 
    }
    
    // build product till left index(using nums[] to save space)
    for(int i=1; i<size;i++){
        nums[i] = nums[i-1]*nums[i]; 
    }

    // now at index i, we just need to multiply i-1 in left product and i+1 in right index product
    for(int i=0;i<size;i++){

        if(i==0){ // no left product so right_product[i+1]
            right_product[i] = nums[i];
            nums[i] = right_product[i+1];
        } else if(i == size-1){ // no right product so left_product[i-1] which is stored at right_product[i-1];
            nums[i] = right_product[i-1];

        } else{
            right_product[i] = nums[i]; // storing left_product[i] in right_product[i] to save space.
            nums[i] = right_product[i-1]*right_product[i+1];
        }
    }
    return nums;
}
```

**Solution 2**

The logic is, _we don't actually need seperate array to store prefix product and suffix products_, 
we can do all the approach discussed in method 3 directly onto our final answer array.

The Time Complexity would be `O(n)` but now, the Auxilary Space is `O(1)` (excluding the final answer array).

```agsl
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int ans[] = new int[n];
        Arrays.fill(ans, 1);
        int curr = 1;
        for(int i = 0; i < n; i++) {
            ans[i] *= curr;
            curr *= nums[i];
        }
        curr = 1;
        for(int i = n - 1; i >= 0; i--) {
            ans[i] *= curr;
            curr *= nums[i];
        }
        return ans;
    }
}
```


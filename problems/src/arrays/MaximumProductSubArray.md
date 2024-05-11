# [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

A better maximum product subarray can be either of below:

Previous subarrays were better (in which case, we leave the solution as is)
Including the current value by adding the current value into an ongoing subarray and increase the running product

Case 1 is easy — we just keep a running max. However, case 2 is a bit tricky because unlike maximum subarray, we need to consider negative numbers.

Consider the case of [2, -2, 2, -2]

If our subarray includes an even number of negative numbers (e.g. 2 or 4 negative numbers), the product will still end up positive. This means we can’t only add positive numbers to our subarray, as the optimal solution above would be 2×−2×2×−2=16

Another problem is when you hit a zero. Any time your subarray hits a zero, the running product gets cancelled out to a zero.

With that said, let’s revisit the DP solution to Maximum Subarray: Kandane’s Algorithm. In Kandane’s, we keep track of two variables, the maximum subarray sum and the current subarray sum. We update the current subarray sum, and if it ever becomes 0 or less, we start another subarray. Every time we update the subarray, we also update the max subarray sum.

Approach

In this question, we need to keep track of two subarrays: the subarray with the maximum product so far and the subarray with the minimum product so far. Why do we need to keep track of the minimum product? To handle the negative case. Let’s revisit that negative subarray again: [2, -2, 2, -2]

When there are 3 indices in [2, -2, 2], the running product of all 3 elements is negative. The maximum product so far is 2 (just a singular element in the subarray), but the minimum subarray is [2 -2 2] with a product of -8. Now, when we calculate the solution for all 4 indices, we consider both the max product and the min product to get 16.

There are 3 DP cases to consider

Restart new subarray: Set the running product to be just the current element. This handles the case with 0s
Add current element to the current subarray with the minimum product. Set running product to be minimum_product * value
Add current element to the current subarray with the maximum product. Set running product to be maximum_product * value
Thus, both our min and max subarrays we keep track of need to consider all three of these cases. Our DP equations can be formulated as follows

- min_prod = min(value, value * old_min_prod, value * old_max_prod)
- max_prod = max(value, value * old_min_prod, value * old_max_prod)
- answer = max(max_prod, answer)

```agsl
public int maxProduct(int[] nums) {
        int maxProd = nums[0], minProd=nums[0], result = nums[0];

        for(int i=1;i<nums.length; i++){
            int curr = nums[i];
            int temp_max_prod = curr*maxProd;
            int temp_min_prod = curr*minProd;

            maxProd = Math.max(curr, Math.max(temp_max_prod, temp_min_prod));
            minProd = Math.min(curr, Math.min(temp_max_prod, temp_min_prod));
            result = Math.max(result, maxProd);
        }
        return result;  
    }
```
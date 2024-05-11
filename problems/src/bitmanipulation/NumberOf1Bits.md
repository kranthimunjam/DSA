# [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

Write a function that takes the binary representation of a positive integer and returns the number of
set bits it has (also known as the `Hamming weight`).

**Example 1:**

- Input: n = 11
- Output: 3

Explanation: The input binary string 1011 has a total of three set bits.

**Example 2:**

- Input: n = 128
- Output: 1

Explanation:
The input binary string 10000000 has a total of one set bit.

**Example 3:**

- Input: n = 2147483645
- Output: 30

Explanation: The input binary string 1111111111111111111111111111101 has a total of thirty set bits.


**Constraints:**

- 1 <= n <= 231 - 1


Follow up: If this function is called many times, how would you optimize it?

# Solution 
Actually, using `Integer.bitCount(n)` also gives the count of set bits but the interviewer may not be okay with this. 

**Approach 1:**

- Initialize a count variable to 0.
- Iterate through each bit of the number using the following steps:
  - If the current bit is 1, increment the count. 
  - Set the current bit to 0 using the expression n = n & (n - 1). 
- The count variable now holds the number of 1 bits.

**Time Complexity:** `O(n)`\
**Space Complexity:** `O(1)`

```agsl
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int ans=0;
        //brain kernighan algorithm
        while(n!=0){
            n&=n-1;
            ans++;
        }
        return ans;
    }
}
```
**Approach 2:**

An Integer in Java has 32 bits.

e.g. `00101000011110010100001000011010`.


To count the 1s in the Integer representation we put the input int
n in bit AND with 1 (that is represented as
`00000000000000000000000000000001`, and if this operation result is 1,
that means that the last bit of the input integer is 1. Thus we add it to the 1s count.
`count = count + (n & 1);`

Then we shift the input Integer by one on the right, to check for the
next bit.
`n = n>>>1;`

We need to use bit shifting unsigned operation `>>>` (while `>>` depends on sign extension)

We keep doing this until the input Integer is 0.

In Java we need to put attention on the fact that the maximum integer is `2147483647`. 
Integer type in Java is signed and there is no unsigned int. So the input `2147483648` is represented in Java as `-2147483648` (in java int type has a cyclic representation, that means `Integer.MAX_VALUE+1==Integer.MIN_VALUE`).
This force us to use

`n!=0`

in the while condition and we cannot use

`n>0` because the input `2147483648` would correspond to `-2147483648` in java and the code would not enter the while if the condition is `n>0` for `n=2147483648`.
```agsl
public static int hammingWeight(int n) {
	int ones = 0;
    	while(n!=0) {
    		ones = ones + (n & 1);
    		n = n>>>1;
    	}
    	return ones;
}
```
**Time Complexity:** `O(n)`\
**Space Complexity:** `O(1)`


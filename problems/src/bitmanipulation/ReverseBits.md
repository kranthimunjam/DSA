# [Reverse Bits](https://leetcode.com/problems/reverse-bits/)

Reverse bits of a given 32 bits unsigned integer.

**Note:**

- Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above, the input represents the signed integer -3 and the output represents the signed integer -1073741825.

**Example 1:**

- Input: n = 00000010100101000001111010011100
- Output:    964176192 (00111001011110000010100101000000)

Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.

**Example 2:**

- Input: n = 11111111111111111111111111111101
- Output:   3221225471 (10111111111111111111111111111111)

Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.

**Constraints:**
- The input must be a binary string of length 32

**Follow up:** If this function is called many times, how would you optimize it?

# Solutions

**COMPLEXITY**

**Time:** `O(1)`, constant time
**Space:** `O(1)`, in-place

**BASIC IDEA**

In this implementation we have followed `Divide and Conquer` strategy where Original problem is divided into sub problems

Reverse bits of a given 32 bits unsigned integer.

Suppose we have a decimal number `12345678` and we have to reverse it to get `87654321` as desired output
The process will be as follows:
Let's assume original number 12345678, 

- 5678 1234 swapped 4 digit-wise
- 78 56 34 12 swapped 2 digit-wise
- 8 7 6 5 4 3 2 1 --> swapped pairwise(reversed number)

Same logic applied to binary representation as well. 
- first we swap 16 bit wise then 8, then 4 then 2 and finally pair wise swapping which effectively means reversing the binary representation.



```agsl
public class Solution {
    // you need treat n as an unsigned value
    // 0xffff0000 means 11111111111111110000000000000000 (16 1's then 16 0's).
    // 0xff00ff00 means 11111111000000001111111100000000 (8 1's, 8 0's)
    // 0xf0f0f0f0 means 11110000111100001111000011110000 (4 1's then 4 0's)
    // 0x0f0f0f0f means 00001111000011110000111100001111 (4 0's then 4 1's)
    // 0xcccccc means   11001100110011001100110011001100
    // 0x33333333 means 00110011001100110011001100110011
    // 0xaaaaaaaa means 10101010101010101010101010101010
    // 0x55555555 means 01010101010101010101010101010101
    
    public int reverseBits(int n) {
        n = ((n & 0xffff0000)>>>16| (n & 0x0000ffff) << 16); 
        n = ((n & 0xff00ff00)>>>8 | (n & 0x00ff00ff)<< 8);
        n = ((n & 0xf0f0f0f0)>>>4 | (n & 0x0f0f0f0f)<< 4);
        n = ((n & 0xcccccccc)>>>2 | (n & 0x33333333)<< 2);
        n = ((n & 0xaaaaaaaa)>>>1 | (n & 0x55555555)<<1);
        return n;
    }
}
```
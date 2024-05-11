# [Counting Bits](https://leetcode.com/problems/counting-bits/)

Given an integer `n`, return an array ans of length `n + 1` such that for each `i (0 <= i <= n)`, `ans[i]` is the `number of 1's` in the binary representation of `i`.

**Example 1:**

- Input: n = 2
- Output: [0,1,1]

Explanation:\
0 --> 0\
1 --> 1\
2 --> 10\

**Example 2:**

- Input: n = 5
- Output: [0,1,1,2,1,2]

Explanation:\
0 --> 0\
1 --> 1\
2 --> 10\
3 --> 11\
4 --> 100\
5 --> 101

**Constraints:**
- 0 <= n <= 105

**Follow up:**

It is very easy to come up with a solution with a runtime of `O(n log n)`. Can you do it in linear time `O(n)` and possibly in a single pass?
Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?
# Solutions
```agsl
class Solution {
    public int[] countBits(int num) {
        int dp[] = new int[num+1];
        dp[0]=0;
        int powerOfTwo = 2,counter = 2;
        // count of set bit for all power's of 2(2,4,8,16...) is 1;
        for(int i=1;i<=num;i=i*2)   dp[i] = 1;
        // for other numbers, for example 15, setbits[15] = setbits[8]+setbits[7]; 
        for(int i=3;i<=num;i++){
            counter--;
            if(dp[i] != 1)  dp[i]=dp[powerOfTwo]+dp[i-powerOfTwo];
            if(counter==0){
                powerOfTwo*=2;
                counter = powerOfTwo;
            }
        }
        return dp;
    }
}
```

**Approach 2:**

An easy recurrence for this problem is `f[i] = f[i / 2] + i % 2`

For example, number of bits in 3 = `no of bits(3/2)`+ `(3%2)` that'll be 1+1 = 2;


```agsl
// this is more elegant representation of above logic.
int ans[]=new int[n+1];
ans[0]=0;
for (int i = 1; i <= n; i++) {
//ans[i >> 1] must be already calculated. 
//(i & 1) checks if the right most bit is set
 ans[i] = ans[i >> 1] + (i & 1); 
}
return ans;
```
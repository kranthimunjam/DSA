# [Decode Ways](https://leetcode.com/problems/decode-ways/)

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

'A' -> "1"\
'B' -> "2"\
...\
'Z' -> "26"\
To decode an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). 
For example, `11106` can be mapped into:

- "AAJF" with the grouping `(1 1 10 6)`
- "KJF" with the grouping `(11 10 6)`
Note that the grouping `(1 11 06)` is invalid because `06` cannot be mapped into `F` since `6` is different from `06`.

Given a string `s` containing only digits, return the number of ways to decode it.

The test cases are generated so that the answer fits in a 32-bit integer.

**Example 1:**
- Input: s = "12"
- Output: 2

Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).

**Example 2:**

- Input: s = "226"
- Output: 3

Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

**Example 3:**

- Input: s = "06"
- Output: 0

Explanation: `06` cannot be mapped to `F` because of the leading zero (`6` is different from `06`).

**Constraints:**

- 1 <= s.length <= 100
- s contains only digits and may contain leading zero(s).

# Solutions

Crude DP solution using substring
```java
public int numDecodings(String s) {
    int size = s.length();
    int[] dp = new int[size];
    dp[size-1] = (s.charAt(size-1) == '0') ? 0 : 1;

    for(int i=size-2;i>=0;i--){
        int count = 0;
        String str1 = s.substring(i,i+1);
        if(isValidCombo(str1) && dp[i+1]!=0) count+=dp[i+1];

        String str2 = s.substring(i,i+2);
        if(isValidCombo(str2)){
            if(i+2 >= size) ++count;
            else if(dp[i+2]!=0) count+=dp[i+2];

        }  
        dp[i] = count;
    }
    return dp[0];
}

boolean isValidCombo(String s){
    int val = Integer.parseInt(s);
    // below is to detect leading zero and values more than 26;
    if(s.length()==2 && (val<10|| val>26)) return false;
    else if(val==0) return false;
    return true;
}
```

Optimised time by using `charArray` instead of `substring`(which is inside loop):
```java
public int numDecodings(String s) {
        
    if(s.charAt(0)=='0') return 0;
    char[] input = s.toCharArray();

    int size = input.length;
    int[] dp = new int[size];
    dp[size-1] = (s.charAt(size-1) == '0') ? 0 : 1;

    for(int i=size-2;i>=0;i--){
        int count = 0;
        char currChar = input[i];
        char nextChar = input[i+1];
        if(currChar != '0'){
            if(dp[i+1]!=0) count+=dp[i+1];

            if(isValidCombo(input[i], input[i+1])){
                if(i+2 >= size) ++count;
                else if(dp[i+2]!=0) count+=dp[i+2];
            }  
        }
        dp[i] = count;
    }
    return dp[0];
}

boolean isValidCombo(char ch1, char ch2){
    int digit1 = ch1-'0', digit2= ch2-'0';
    return ((digit1*10)+digit2)<=26;
}
```
Another approach:
```java
class Solution {
    public int numDecodings(String s) {
        int n=s.length();
        int[] dp=new int[n+1];
        dp[n]=1;
        for(int i=n-1;i>=0;i--){
            char ch=s.charAt(i);
            if(ch=='0'){
                dp[i]=0;
            }
            else{
                dp[i]=dp[i+1];
            }
            if(i+1<n){
                if(ch=='1' || (ch=='2' && s.charAt(i+1)<='6')){
                    dp[i]=dp[i+1]+dp[i+2];
                }
            }
        }
        return dp[0];
    }
}
```

**Further optimising the memory :**

**DP with constant memory.**
we don’t need `dp[]` array. we need last two positions values.

```java
public int numDecodings(String s) {
    int dp1=1, dp2=0, n=s.length();
    for(int i=n-1;i>=0;i--) {
        int dp=s.charAt(i)=='0' ? 0 : dp1;
        if(i<n-1&&(s.charAt(i)=='1'||s.charAt(i)=='2'&& s.charAt(i+1)<'7'))
            dp+=dp2;
        dp2=dp1;
        dp1=dp;
    }
    return dp1;
}
```
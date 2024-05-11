# [Word Break](https://leetcode.com/problems/word-break/)

Given a string s and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

- Input: s = "leetcode", wordDict = ["leet","code"]
- Output: true

Explanation: Return true because "leetcode" can be segmented as "leet code".

**Example 2:**

- Input: s = "applepenapple", wordDict = ["apple","pen"]
- Output: true

Explanation: Return true because `applepenapple` can be segmented as `apple pen apple`.
Note that you are allowed to reuse a dictionary word.

**Example 3:**

- Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
- Output: false

**Constraints:**

- 1 <= s.length <= 300
- 1 <= wordDict.length <= 1000
- 1 <= wordDict[i].length <= 20
`s` and `wordDict[i]` consist of only lowercase English letters.
All the strings of `wordDict` are **unique**.

# Solutions

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        int size = s.length();
        boolean dp[] = new boolean [size];

        for(int i=0;i<size;i++){
            boolean val= false;
            for(int j=i;j>=0;j--){
                String sub = s.substring(j,i+1);
                if(wordDict.contains(sub)){
                    val = (j-1>=0)?dp[j-1]:true;
                    if(val) break;
                }
            }
            dp[i] = val;
        }
        return dp[size-1];
    }
}
```
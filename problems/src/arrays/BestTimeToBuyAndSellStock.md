# [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

You are given an array prices where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

**Example 1:**

- Input: prices = [7,1,5,3,6,4]
- Output: 5 

Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

- Input: prices = [7,6,4,3,1]
- Output: 0

Explanation: In this case, no transactions are done and the max profit = 0.

**Constraints:**

* 1 <= prices.length <= 105
* 0 <= prices[i] <= 104

# Solutions:

**Kadane's Algorithm**

Kadane's Algorithm is a dynamic programming technique used to find the maximum subarray sum in an array of numbers. The algorithm maintains two variables: `max_current` represents the maximum sum ending at the current position, and `max_global` represents the maximum subarray sum encountered so far. At each iteration, it updates `max_current` to include the current element or start a new subarray if the current element is larger than the accumulated sum. The `max_global` is updated if `max_current` surpasses its value.



```agsl
class Solution {
    public int maxProfit(int[] prices) {
        int today= prices.length-1, profit = 0, max_price_so_far=-1;
        while(today>=0){
            if(prices[today]>=max_price_so_far){
                max_price_so_far = prices[today];
            }
            else{
                int profit_today = max_price_so_far-prices[today];
                if(profit< profit_today) profit= profit_today;
            }
            --today;
        }
        return profit;
    }
}
```

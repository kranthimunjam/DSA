# Koko Eating Bananas

Koko loves to eat bananas. There are `n` piles of bananas, the i<sup>th</sup> pile has `piles[i]` bananas. 
The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. 
Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. 
If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the _minimum integer_ `k` such that she can eat all the bananas within `h` hours.

#### Example 1:

**Input:** piles = [3,6,7,11], h = 8\
**Output:** 4

#### Example 2:

**Input:** piles = [30,11,23,4,20], h = 5\
**Output:** 30

#### Example 3:

**Input:** piles = [30,11,23,4,20], h = 6\
**Output:** 23


#### Constraints:

* 1 <= piles.length <= 104
* piles.length <= h <= 109
* 1 <= piles[i] <= 109

# Solutions

1. get the maximum element from array and set max var.
2. If we eat max every hour then total hours required = piles.length(because every pile gets eaten in 1 time unit)
3. now use binarySearch() with low=1, high = max.
4. Each time calculate the hours required for a k (mid).
5. At the end of binarySearch(), we'll get the lowest K that'll still take <= h hours.

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1, right =Integer.MIN_VALUE;

        // calculate the max values of the array.
        for(int i=0;i<piles.length;i++){
            right = Math.max(right, piles[i]);
        }
        int prev = 0;
        
        while(left<=right){
            int mid = left+(right-left)/2;
            
            if(canEatInHours(piles, mid, h)) {
                prev= mid;
                right = mid-1;
            }
            else left = mid+1;
        }

        return prev;
    }

    boolean canEatInHours(int[] piles, int k, long h){
        long hours = 0;

        for(int pile:piles){
            int div = pile/k;
            hours+=div;
            if(pile%k != 0) hours++;
            if(hours>h) return false;
        }
        return hours<=h;
    }
}
```
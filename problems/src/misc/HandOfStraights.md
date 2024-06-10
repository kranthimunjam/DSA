# Hand of Straights
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, 
and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the i<sup>th</sup> card and an integer `groupSize`, 
return `true` if she can rearrange the cards, or `false` otherwise.

#### Example 1:

**Input:** hand = [1,2,3,6,2,3,4,7,8], groupSize = 3\
**Output:** true\
**Explanation:** Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]

#### Example 2:

**Input:** hand = [1,2,3,4,5], groupSize = 4\
**Output:** false\
**Explanation:** Alice's hand can not be rearranged into groups of 4.

**Constraints:**

* 1 <= hand.length <= 104
* 0 <= hand[i] <= 109
* 1 <= groupSize <= hand.length

# Solutions

**Approach 1:** 

1. Using the Map to keep frequencies. 
2. Iteratively reduce the frequency by 1. 

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int totalGroups = hand.length/groupSize;
        if(hand.length%groupSize != 0) return false;

        Map<Integer, Integer> frequencyMap  = new TreeMap<>();
        for(int i =0;i<hand.length;i++){
            int currFrequency = frequencyMap.getOrDefault(hand[i], 0);
            frequencyMap.put(hand[i], currFrequency+1);
        }

        for(int i=0;i<totalGroups;i++){
            int count = groupSize;
            
            // if map doesn't have enough entries then no need to calculate.
            if(frequencyMap.size()<groupSize) return false;

            int prev = -1;
            Iterator<Map.Entry<Integer, Integer>> itr = frequencyMap.entrySet().iterator();
            while(count-->0 && itr.hasNext()){
                Map.Entry<Integer, Integer> entry = itr.next();
                int card = entry.getKey();
                int val = entry.getValue();
                if(prev!=-1 && prev+1 != card) return false;
                if(val==1) itr.remove();
                else frequencyMap.put(card, val-1);
                prev = card;
            }
        }

        return true;
    }
}
```

**Approach 2:** 

We can use the sorted array. 

```java
class Solution {
    public boolean findsucessors(int[] hand, int groupSize, int i, int n) {
        int f = hand[i] + 1;
        hand[i] = -1;
        int count = 1;
        i += 1;
        while (i < n && count < groupSize) {
            if (hand[i] == f) {
                f = hand[i] + 1;
                hand[i] = -1;
                count++;
            }
            i++;
        }
        if (count != groupSize)
            return false;
        else
            return true;
    }

    public boolean isNStraightHand(int[] hand, int groupSize) {
        int n = hand.length;
        if (n % groupSize != 0)
            return false;
        Arrays.sort(hand);
        int i = 0;
        for (; i < n; i++) {
            if (hand[i] >= 0) {
                if (!findsucessors(hand, groupSize, i, n))
                    return false;
            }
        }
        return true;
    }
}
```

**Approach 3:**

By using min heap.

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int W) {
        if(hand.length % W != 0) return false;

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        for(int elem: hand) minHeap.add(elem);

        while(!minHeap.isEmpty()){
            int head = minHeap.poll();
            for(int i=1; i<W; i++)
                if(!minHeap.remove(head+i)) return false;
        }
        return true;
    }
}
```
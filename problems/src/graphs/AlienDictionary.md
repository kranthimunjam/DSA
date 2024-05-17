# [Alien Dictionary](https://www.lintcode.com/problem/892/)

There is a new alien language which uses the latin alphabet. 
However, the order among letters are unknown to you. 
You receive a list of **non-empty** words from the dictionary, where words are **sorted lexicographically by the rules of this new language.** 
Derive the order of letters in this language.

#### Example 1:

**Input：**["wrt","wrf","er","ett","rftt"]\
**Output:**"wertf"\
**Explanation:**\
from "wrt"and"wrf" ,we can get t<f\
from "wrt"and"er" ,we can get w<e\
from "er"and"ett" ,we can get r<t\
from "ett"and"rftt" ,we can get e<r\
So return "wertf"

#### Example 2:

**Input：**["z","x"]\
**Output**："zx"\
**Explanation**：\
from "z" and "x"，we can get z < x
So return "zx"

# Solution

* Create a directed **graphs g** with number of vertices equal to the number of unique characters in alien language, where each character represents a node and edges between nodes indicate the relative order of characters.
* Do following for every pair of adjacent words in given sorted array.
* Let the current pair of words be `word1` and `word2`. One by one compare characters of both words and find the first mismatching characters.
* Create an edge in g from mismatching character of word1 to that of word2.
* If the graphs created is **DAG (Directed Acyclic Grapgh)** then print topological sorting of the above created graphs else if the graphs contains a cycle then input data is inconsistent and valid order of characters does not exist.

```java
class Solution {
    private List<Integer> topologicalSort(int V, List<List<Integer>> adj) {
        int indegree[] = new int[V];
        for (int i = 0; i < V; i++) {
            for (int it : adj.get(i)) {
                indegree[it]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.add(i);
            }
        }
        List<Integer> topo = new ArrayList<>();
        while (!q.isEmpty()) {
            int node = q.peek();
            q.remove();
            topo.add(node);
            // node is in your topo sort
            // so please remove it from the indegree

            for (int it : adj.get(node)) {
                indegree[it]--;
                if (indegree[it] == 0) q.add(it);
            }
        }

        return topo;
    }
    
    public String findOrder(String [] dict, int N, int K) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < K; i++) {
            adj.add(new ArrayList<>());
        }


        for (int i = 0; i < N - 1; i++) {
            String s1 = dict[i];
            String s2 = dict[i + 1];
            int len = Math.min(s1.length(), s2.length());
            for (int ptr = 0; ptr < len; ptr++) {
                if (s1.charAt(ptr) != s2.charAt(ptr)) {
                    adj.get(s1.charAt(ptr) - 'a').add(s2.charAt(ptr) - 'a');
                    break;
                }
            }
        }

        List<Integer> topo = topologicalSort(K, adj);
        String ans = "";
        for (int it : topo) {
            ans = ans + (char)(it + (int)('a'));
        }

        return ans;

    }
}
```
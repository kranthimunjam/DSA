An undirected graph is a `tree`, if it has the following properties:

* There is no cycle.
* The graph is connected.
* For an undirected graph, we can either use **BFS** or **DFS** to detect the above two properties.

**Time Complexity:** `O(V + E)` For performing the **DFS** traversal\
**Auxiliary Space:** `O(V)` For storing the visited array
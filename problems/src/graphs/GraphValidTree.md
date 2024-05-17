An undirected graphs is a `tree`, if it has the following properties:

* There is no cycle.
* The graphs is connected.
* For an undirected graphs, we can either use **BFS** or **DFS** to detect the above two properties.

**Time Complexity:** `O(V + E)` For performing the **DFS** traversal\
**Auxiliary Space:** `O(V)` For storing the visited array

### DFS: 
1. From the start node explore all child nodes recursively
2. in order to avoid infinite loop caused by exploring same node multiple times, use visited array
3. or alternatively, we can modify the node/cell to some special character like '#' to indicate it is visited
4. and once it's exploration is complete, put the original value back.

### Topological Sort:
This will work only on DAG, if graph has cycle then we can't have a valid order and algorithm will fail.
If graph is not mentioned as DAG then use Kahn's algorithm instead.
1. start from all nodes with in-degree=0; 
2. Like in DFS, explore all non visited children of curr node. 
3. After exploring all children, push the current node into stack and mark visited. 
4. so, child node enters the stack before the parent. 
5. to get the order, simply pop elements from stack and print.


### Kahn's algorithm:
1. Compute in-degree (number of incoming edges) for each node.
2. Initialize a queue with all nodes having in-degree of 0.
3. While the queue is not empty:
    - Remove a node from the queue.
    - Append it to the topological order list.
    - Decrease the in-degree of all its neighbors by 1.
    - If any neighbor's in-degree becomes 0, add it to the queue.
4. If all nodes are processed, return the topological order; otherwise, there is a cycle.

### BFS:
1. start by pushing the root node to queue.
2. while queue is not empty: 
   - pop node from queue
   - if not visited already then process the curr node, mark as visited
   - and push all the adj nodes of curr node to queue.

### Dijikstra’s Algorithm(greedy): Single source shortest path algorithm
This algorithm is used to find the shortest path between two nodes in a weighted graph.
It only works if no negative cycles present.
For unweighted graph, we can use BFS or bidirectional BFS to find the shortest path.

Roughly, the idea is pick the lowest weight edge from source, update its distance. 
now from these two nodes, again pick the lowest weight edge, update the distance. 
repeat until all vertices are explored.

**Time complexity:** O(E + VlogV)\
**Space complexity:** O(V<sup>2</sup>)

1. Set distance to startNode to zero. 
2. Set all other distances to an infinite value. 
3. We add the startNode to the min heap (based on distance from source). 
4. While heap is not empty we:
   - poll from heap(since we are using min heap, it has the lowest distance from the source.) 
   - update new distances to direct neighbors by keeping the lowest distance at each evaluation. 
   - Add neighbors that are not yet visited to the heap.

**Problems:**
   - [Network delay time](https://leetcode.com/problems/network-delay-time/description/)
   - 

### Prim’s Algorithm: minimal spanning tree
This is a variation of dijikstra. we use minHeap for Edges. 
1. start with a node, pick the edge with minimum weight and add nodes it is connecting to `connectedSet`.
2. Add all the edges from these two nodes to minHeap. 
3. Again pick minimum weight edge, and Repeat until all nodes are connected(`connectedSet` size == total nodes in graph)
4. Discard redundant edges, i.e., if edge is connecting two nodes that are already connected, then it's introducing a cycle.
5. pick next best edge.

**Problems:**
 - [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/description/)
 - [leetcode list](https://leetcode.com/tag/minimum-spanning-tree/)

### Union-Find Algorithm(Disjoint sets)
used to identify groups. there will be two arrays, parent and rank.
1. initially, everyone is their own parent and rank is 1.
2. if two elements belong to same group then `union()` them.
3. `find()` is used to get the parent.

How to find total groups? it is the total no of parents(parent[i]==i) in parent array.
```java
class UnionFind {
        int[] parent;
        int[] rank;

        UnionFind(int size){
            this.parent = new int[size];
            this.rank = new int[size];
            for(int i=0;i<size;i++){
                parent[i]=i;
                rank[i]=1;
            }
        }

        void union(int i, int j){
            int iParent = find(i);
            int jParent = find(j);

            if(iParent == jParent) return;
            else{
                if(rank[iParent]>rank[jParent]){
                    parent[jParent]= iParent;
                } else if(rank[jParent]>rank[iParent]){
                    parent[iParent] = jParent;
                } else{
                    parent[jParent]= iParent;
                    rank[iParent]+=1;
                }
            }
        } 

        int find(int i){
            if(i== parent[i]) return i;
            return find(parent[i]);
        }  
    }
```

### Kruskal’s Algorithm:
1. Keep vertices in a set.
2. Sort edges by weight(add them to min heap). 
3. While set is not empty, pick the smallest edge(poll)
4. Check if this new edge is forming a cycle using `union-find`(check if both nodes don't belong to same group). 
5. If no cycle then add edge to graph, remove the both vertices from the set(means they are connected). 
6. else move on to the next edge. 
7. continue from step 3 till set is empty.

### Bellman-ford Algorithm:
Works even if graph has negative cycles.
The Bellman-Ford algorithm’s primary principle is that it starts with a single source and calculates the distance to each node. 
The distance is initially unknown and assumed to be infinite, but as time goes on, the algorithm relaxes those paths by identifying a few shorter paths. 
Hence it is said that Bellman-Ford is based on **"Principle of Relaxation"**.

Here, we relax the edges from source. Each iteration i, we update the distance till i<sup>th</sup> node. 
After n-1(maximum path possbile for `n` vertex graph) iterations, we know that all nodes are relaxed.

**Negative cycle detection:** 
In order to detect whether a negative cycle exists or not, relax all the edge one more time and if the shortest distance for any node reduces then we can say that a negative cycle exists. 
In short if we relax the edges N times, and there is any change in the shortest distance of any node between the N-1th and Nth relaxation than a negative cycle exists.

1. Initialize distance array `dist[]` for each vertex `v` as `dist[v] = INFINITY`.
2. Assume any vertex (let’s say `0`) as source and assign dist = `0`. 
3. Relax all the edges(u,v,weight) N-1 times as per the below condition:\
       `dist[v] = minimum(dist[v], distance[u] + weight)`

Now, Relax all the edges one more time i.e. the N<sup>th</sup> time and based on the below two cases we can detect the negative cycle:
- Case 1 (Negative cycle exists): For any edge(u, v, weight), if dist[u] + weight < dist[v]
- Case 2 (No Negative cycle) : case 1 fails for all the edges.

```java
void BellmanFord(Graph graph, int src) {
     int V = graph.V, E = graph.E;
     int dist[] = new int[V];

     // Step 1: Initialize distances from src to all
     // other vertices as INFINITE
     for (int i = 0; i < V; ++i)
         dist[i] = Integer.MAX_VALUE;
     dist[src] = 0;

     // Step 2: Relax all edges |V| - 1 times. A simple
     // shortest path from src to any other vertex can
     // have at-most |V| - 1 edges
     for (int i = 1; i < V; ++i) {
         for (int j = 0; j < E; ++j) {
             int u = graph.edge[j].src;
             int v = graph.edge[j].dest;
             int weight = graph.edge[j].weight;
             if (dist[u] != Integer.MAX_VALUE
                 && dist[u] + weight < dist[v])
                 dist[v] = dist[u] + weight;
         }
     }

     // Step 3: check for negative-weight cycles. The
     // above step guarantees shortest distances if graph
     // doesn't contain negative weight cycle. If we get
     // a shorter path, then there is a cycle.
     for (int j = 0; j < E; ++j) {
         int u = graph.edge[j].src;
         int v = graph.edge[j].dest;
         int weight = graph.edge[j].weight;
         if (dist[u] != Integer.MAX_VALUE
             && dist[u] + weight < dist[v]) {
             System.out.println(
                 "Graph contains negative weight cycle");
             return;
         }
     }
}
```
Time Complexity: O(VE)
Space Complexity: O(V<sup>2</sup>)

### Floyd-Warshall Algorithm(dynamic programming):
This is an all-pairs shortest path algorithm.
The idea behind floyd warshall algorithm is to treat each and every vertex from `1` to `N` as an intermediate node one by one.

1. Initialize the solution matrix same as the input graph matrix as a first step.
2. Then update the solution matrix by considering all vertices as an intermediate vertex.
3. The idea is to pick all vertices one by one and updates all shortest paths which include the picked vertex as an intermediate vertex in the shortest path.
4. When we pick vertex number k as an intermediate vertex, we already have considered vertices {0, 1, 2, .. k-1} as intermediate vertices.
5. For every pair (i, j) of the source and destination vertices respectively, there are two possible cases.
   - `k` is not an intermediate vertex in shortest path from `i` to `j`. We keep the value of `dist[i][j]` as it is. 
   - `k` is an intermediate vertex in shortest path from `i` to `j`. We update the value of `dist[i][j]` as `dist[i][k]` + `dist[k][j]`, if `dist[i][j]` > `dist[i][k]` + `dist[k][j]`
```java
void floydWarshall(int dist[][]){

     int i, j, k;

     // consider each vertex as intermediate node.
     for (k = 0; k < V; k++) {
         // Pick all vertices as source one by one
         for (i = 0; i < V; i++) {
             // Pick all vertices as destination for the
             // above picked source
             for (j = 0; j < V; j++) {
                 // If vertex k is on the shortest path
                 // from i to j, then update the value of
                 // dist[i][j]
                 if (dist[i][k] + dist[k][j]
                     < dist[i][j])
                     dist[i][j]
                         = dist[i][k] + dist[k][j];
             }
         }
     }
}
```
No matter how many edges are there in the graph the Floyd Warshall Algorithm runs for **O(V<sup>3</sup>)** times 
therefore it is best suited for Dense graphs.

### Kosaraju’s strongly connected components algorithm:

### KMP algorithm:

### Rabin-Karp algorithm:

### Boyer-Moore algorithm:

## Algorithmic Paradigms:

### Sliding window :

### Two pointer:

### Two Heaps:

### Monotonic Stack:

### Dynamic Programming:

### Greedy:

### Binary Search:



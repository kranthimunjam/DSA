# Data Structures:
### Stack:
Due to LIFO, stack is fundamentally used to `reverse`, be it a string or array. `Topological sort` uses stack to store the order of the execution from the sink node. 
Solving problems like `matching parenthesis`, `polish notation evaluation` etc requires use of stack.

To find next largest/smallest number on left/right in an array, we use `Monotonic stack`(increasing or decreasing).

### Queue:
Used in `BFS` and `Kahn’s` algorithm.

### Deque: 
If we want to find out max/min in a window we need to use `monotonic deque`, because we can delete from the front as the numbers go outside the window.


### Priority Queue: 
To sort the objects based on an attribute using a custom comparator. Used to sort edges(dijikstra’s), intervals(booking meeting rooms) etc.

### TreeSet: 
There are methods like `floor(E e)` returns the greatest element in this set less than or equal to the given element, or null if there is no such element., 
`ceiling(E e)` returns the least element in this set greater than or equal to the given element, or null if there is no such element in TreeSet that help avoid the binary search.

### TreeMap: 
There are methods like `ceilingKey(K key):` Returns the least key greater than or equal to the given key, or null if there is no such key. 
`floorKey(K key)`: Returns the greatest key less than or equal to the given key, or null if there is no such key in TreeMap. This helps us to avoid the binary search.

### Trie: 
Also called as `prefix tree` is used in auto complete, search suggestion scenarios.
```java
public class TrieNode {
    
    // Array for child nodes of each node
    TrieNode[] childNode; 
    // Used for indicating the end of a string
    boolean endOfWord=false;

    // Constructor
    public TrieNode() {
        childNode = new TrieNode[26]; //lazy initialization
    }
}
```

### Union Find: 
Used to segrigate given entities into distinct groups based on some criteria.
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
### B+ Tree:


### Segment Tree:

### Splay Tree:

### Balanced Binary Search Trees(AVL, Red Black Trees):

### Fenwick Tree:

### Sparse Table:

### Suffix Array:



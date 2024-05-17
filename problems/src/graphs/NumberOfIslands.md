# [Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

Given an `m x n` 2D binary `grid` which represents a map of `1`s (land) and `0`s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. 
You may assume all four edges of the grid are all surrounded by water.

### Example 1:

**Input:** grid = \
[ ["1","1","1","1","0"],\
["1","1","0","1","0"],\
["1","1","0","0","0"],\
["0","0","0","0","0"]]\

**Output:** 1

### Example 2:

**Input:** grid = 
[["1","1","0","0","0"],\
["1","1","0","0","0"],\
["0","0","1","0","0"],\
["0","0","0","1","1"]]\

**Output:** 3

### Constraints:

* m == grid.length
* n == grid[i].length
* 1 <= m, n <= 300
* grid[i]\[j] is `0` or `1`

# Solutions

```java
class Solution {
    public int numIslands(char[][] grid) {
        int rows = grid.length, cols = grid[0].length, count=0;
        
        for(int i=0;i<rows;i++){
            for(int j=0;j<cols;j++){
                if(grid[i][j] =='1'){
                    ++count;
                    exploreAndMark(i,j, rows, cols, grid);
                }
            }
        }
        return count;
    }
    
    private void exploreAndMark(int i,int j, int rows, int cols, char[][] grid){
        
        //BFS: add all neighbours of current cell to queue;
        if(!isValidCell(i, j, rows, cols) || grid[i][j] != '1' ) return;
        
        grid[i][j] = '0';
        
        //left cell
        exploreAndMark(i,j-1, rows, cols, grid);
        
        //right cell
        exploreAndMark(i,j+1, rows, cols, grid);
        
        // upper cell
        exploreAndMark(i-1,j, rows, cols, grid);
        
        //lower cell
        exploreAndMark(i+1,j, rows, cols, grid);
    }
    
    private boolean isValidCell(int x, int y, int rows, int cols){
       if(x<0 || x >=rows || y<0 || y >= cols) return false;
       else return true;
    }
}
```


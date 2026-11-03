# 1. Find the Number of Islands

- **Platform:** GeeksforGeeks
- **Problem Link:** [Find the Number of Islands](https://www.geeksforgeeks.org/problems/find-the-number-of-islands/1)
- **Topic:** Graphs, DFS, Connected Components, Matrix Traversal
- **Programming Language:** Java
- **Submission Status:** Solved

---

## My Intuition
The grid contains land (`'L'`) and water (`'W'`). Connected land cells form an island.
Since diagonal connections also count, I need to consider all eight neighboring cells of each land cell.
If I find an unvisited land cell, it represents a new island. I increment the island count and explore all connected land cells using DFS.

---

## My Approach
1. Create a `visited` matrix to track visited cells.
2. Traverse every cell in the grid.
3. If the current cell contains land (`'L'`) and has not been visited:
   - Increment the island count.
   - Start DFS from that cell.
4. During DFS:
   - Check whether the coordinates are within the grid bounds.
   - Stop if the cell contains water (`'W'`) or has already been visited.
   - Mark the current cell as visited.
   - Explore all eight neighboring cells.
5. Return the total number of islands.

---

## Where I Got Stuck / My Mistake
- Initially, I tried checking the eight neighboring cells manually.
- The important realization was that checking the neighbors alone is not enough. I must recursively or iteratively explore the entire connected region.

---

## What Clicked
- One DFS traversal explores one entire island.
- Once a cell is visited, it will not be counted again. Therefore, every new unvisited land cell encountered during the outer traversal represents a new island.
- The key difference from the standard Number of Islands problem is that this problem considers diagonal connectivity too.

---

## Final Submitted Code

```java
class Solution {
    public int countIslands(char[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        boolean[][] visited = new boolean[n][m];
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 'L' && !visited[i][j]) {
                    ans++;
                    dfs(grid, visited, i, j);
                }
            }
        }

        return ans;
    }

    public void dfs(char[][] grid, boolean[][] visited, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length) {
            return;
        }

        if (grid[i][j] == 'W' || visited[i][j]) {
            return;
        }

        visited[i][j] = true;

        dfs(grid, visited, i - 1, j - 1);
        dfs(grid, visited, i - 1, j);
        dfs(grid, visited, i - 1, j + 1);
        dfs(grid, visited, i, j - 1);
        dfs(grid, visited, i, j + 1);
        dfs(grid, visited, i + 1, j - 1);
        dfs(grid, visited, i + 1, j);
        dfs(grid, visited, i + 1, j + 1);
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N \times M)$ — each cell is visited at most once.
- **Space Complexity:** $O(N \times M)$ — the visited matrix and worst-case DFS recursion stack.

---

## Revision Notes
- DFS explores an entire connected component.
- Count a new component whenever an unvisited land cell is encountered.
- Remember the difference between four-directional and eight-directional connectivity.
- For large grids, iterative DFS can avoid recursion stack overflow.


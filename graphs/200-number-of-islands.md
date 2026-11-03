# 200. Number of Islands

- **Platform:** LeetCode
- **Problem Link:** [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- **Topic:** Graphs, DFS, Connected Components, Matrix Traversal
- **Programming Language:** Java
- **Submission Status:** Solved

---

## My Intuition
The grid contains land (`'1'`) and water (`'0'`).
Connected land cells form an island, but only horizontal and vertical connections count.
Whenever I find a land cell, I have found a new island. I increment the count and use DFS to visit all land cells belonging to that island.

---

## My Approach
1. Traverse the entire grid.
2. Whenever a cell contains `'1'`:
   - Increment the island count.
   - Start DFS from that cell.
3. During DFS:
   - Stop if the coordinates are outside the grid or the cell contains water.
   - Change the current cell from `'1'` to `'0'` to mark it as visited.
   - Explore its four neighbors: up, down, left, and right.
4. Return the island count.

Instead of maintaining a separate visited matrix, I modify the grid directly.

---

## Where I Got Stuck / My Mistake
- The important part is ensuring that the same land cell is not processed repeatedly.
- Marking a cell as water before exploring its neighbors prevents revisiting it and avoids infinite recursion.

---

## What Clicked
- The main insight is that the number of DFS traversals started by the outer loop equals the number of islands.
- Each DFS marks one complete island as visited.
- Compared with the GeeksforGeeks version, this problem considers only four directions rather than eight.

---

## Final Submitted Code

```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }
        return count;
    }

    public void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == '0') {
            return;
        }

        grid[i][j] = '0';

        dfs(grid, i - 1, j);
        dfs(grid, i + 1, j);
        dfs(grid, i, j - 1);
        dfs(grid, i, j + 1);
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(M \times N)$ — every cell is processed at most once.
- **Space Complexity:** $O(M \times N)$ in the worst case because of the recursive call stack.

---

## Revision Notes
- One DFS traversal corresponds to one island.
- Mark cells as visited before exploring their neighbors.
- In-place marking can eliminate the need for a separate visited matrix.
- Four directions: up, down, left, and right.
- This approach modifies the input grid.


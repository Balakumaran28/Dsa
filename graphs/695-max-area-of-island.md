# 695. Max Area of Island

- **Platform:** LeetCode
- **Problem Link:** [Max Area of Island](https://leetcode.com/problems/max-area-of-island/description/)
- **Topic:** Graphs, DFS, Connected Components, Matrix Traversal
- **Programming Language:** Java
- **Submission Status:** Solved

---

## My Intuition
Each connected group of land cells represents an island.
Instead of counting the islands, I need to calculate the area of every island and return the maximum.
Every land cell contributes 1 to the island's area. DFS explores all connected land cells and returns their combined area.

---

## My Approach
1. Traverse every cell in the grid.
2. If the current cell contains land (`1`):
   - Start DFS from that cell.
   - Store the returned area.
   - Update the maximum area if necessary.
3. During DFS:
   - Return 0 if the coordinates are outside the grid or the cell contains water (`0`).
   - Mark the current land cell as water (`0`).
   - Recursively calculate the area contributed by its four neighbors.
   - Return the sum of the four results plus 1 for the current cell.
4. Return the maximum area found.

---

## Where I Got Stuck / My Mistake
- The key challenge is returning a value from DFS rather than only visiting cells.
- For Number of Islands, DFS can be a void method because I only need to mark cells.
- Here, DFS must return the area of the connected component.

---

## What Clicked
- The current cell contributes one unit of area:
  $$\text{area} = 1 + \text{up} + \text{down} + \text{left} + \text{right}$$
- Each recursive call returns the area contributed by its direction.
- Marking a land cell as water prevents it from being counted multiple times.

---

## Final Submitted Code

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int count = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    int temp = dfs(grid, i, j);
                    if (temp > count) {
                        count = temp;
                    }
                }
            }
        }
        return count;
    }

    public int dfs(int[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == 0) {
            return 0;
        }

        grid[i][j] = 0;

        int l = dfs(grid, i - 1, j);
        int r = dfs(grid, i + 1, j);
        int b = dfs(grid, i, j - 1);
        int t = dfs(grid, i, j + 1);

        return l + r + b + t + 1;
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(M \times N)$ — each cell is processed at most once.
- **Space Complexity:** $O(M \times N)$ in the worst case because of the recursive call stack.

---

## Revision Notes
- DFS can return information about a connected component.
- For island count, each DFS traversal increments the count.
- For maximum area, each DFS traversal returns the component's size.
- Mark cells as visited to avoid double-counting.
- This solution modifies the input grid.


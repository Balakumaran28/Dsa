# Graph Problems Revision

A quick-revision checklist and summary of graph problems solved in this repository.

---

## Problem List

| # | Problem Name | Problem Link | Core Pattern / Algorithm | Revision Notes |
| :-: | :--- | :--- | :--- | :--- |
| **1** | [Find the Number of Islands](https://www.geeksforgeeks.org/problems/find-the-number-of-islands/1) | [GeeksforGeeks](https://www.geeksforgeeks.org/problems/find-the-number-of-islands/1) | Matrix DFS / 8-Directional Connected Components | [Detailed Notes](../graphs/1-find-the-number-of-islands.md) |
| **200** | [Number of Islands](https://leetcode.com/problems/number-of-islands/) | [LeetCode #200](https://leetcode.com/problems/number-of-islands/) | Matrix DFS / 4-Directional Connected Components | [Detailed Notes](../graphs/200-number-of-islands.md) |
| **695** | [Max Area of Island](https://leetcode.com/problems/max-area-of-island/description/) | [LeetCode #695](https://leetcode.com/problems/max-area-of-island/description/) | Matrix DFS / Component Size Accumulation | [Detailed Notes](../graphs/695-max-area-of-island.md) |
| **1319** | [Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected/) | [LeetCode #1319](https://leetcode.com/problems/number-of-operations-to-make-network-connected/) | Disjoint Set Union (DSU) / Connected Components | [Detailed Notes](../graphs/1319-number-of-operations-to-make-network-connected.md) |
| **2492** | [Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/) | [LeetCode #2492](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/) | Disjoint Set Union (DSU) / Graph Traversal | [Detailed Notes](../graphs/2492-minimum-score-of-a-path-between-two-cities.md) |

---

## Quick Revision Highlights

### 1. 1. Find the Number of Islands (GeeksforGeeks)
- **Core Idea**: Grid contains land (`'L'`) and water (`'W'`). Connected land cells form an island.
- **Key Detail**: Considers **all 8 directions** (including diagonals).
- **Approach**: Keep a `visited` matrix. Start DFS from unvisited land to explore the complete component and increment island count.

### 2. 200. Number of Islands (LeetCode)
- **Core Idea**: Grid contains `'1'` (land) and `'0'` (water). Only **4 directions** count.
- **Approach**: In-place sink (`grid[i][j] = '0'`) during DFS to avoid a separate `visited` matrix. Number of DFS calls from the outer loop equals the island count.

### 3. 695. Max Area of Island (LeetCode)
- **Core Idea**: Instead of counting islands, return the maximum component area.
- **Key Formula**: $\text{area} = 1 + \text{up} + \text{down} + \text{left} + \text{right}$.
- **Approach**: DFS returns the sum of sizes of connected land cells; sink visited cells in-place to avoid duplicate processing.

### 4. 1319. Number of Operations to Make Network Connected
- **Core Idea**: We need at least $n - 1$ edges to connect $n$ vertices. If total connections $< n - 1$, it is impossible (return `-1`).
- **Key Formula**: Connecting $k$ disconnected components requires exactly $k - 1$ operations.
- **Approach**: DSU with path compression and union by size. Count initial components, decrement on successful union of distinct components.

### 5. 2492. Minimum Score of a Path Between Two Cities
- **Core Idea**: Any edge in the connected component that connects city 1 and city $n$ can be visited (since paths can repeat edges/vertices).
- **Key Formula**: Find the minimum edge weight among all edges whose vertices belong to the same component as city 1.
- **Approach**: DSU to group connected cities, followed by finding the minimum weight road among edges in component 1.

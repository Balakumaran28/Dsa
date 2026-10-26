# Graph Problems Revision

A quick-revision checklist and summary of graph problems solved in this repository.

---

## Problem List

| # | Problem Name | LeetCode Link | Core Pattern / Algorithm | Revision Notes |
| :-: | :--- | :--- | :--- | :--- |
| **1319** | [Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected/) | [LeetCode #1319](https://leetcode.com/problems/number-of-operations-to-make-network-connected/) | Disjoint Set Union (DSU) / Connected Components | [Detailed Notes](../graphs/1319-number-of-operations-to-make-network-connected.md) |
| **2492** | [Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/) | [LeetCode #2492](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/) | Disjoint Set Union (DSU) / Graph Traversal | [Detailed Notes](../graphs/2492-minimum-score-of-a-path-between-two-cities.md) |

---

## Quick Revision Highlights

### 1. 1319. Number of Operations to Make Network Connected
- **Core Idea**: We need at least $n - 1$ edges to connect $n$ vertices. If total connections $< n - 1$, it is impossible (return `-1`).
- **Key Formula**: Connecting $k$ disconnected components requires exactly $k - 1$ operations.
- **Approach**: DSU with path compression and union by size. Count initial components, decrement on successful union of distinct components.

### 2. 2492. Minimum Score of a Path Between Two Cities
- **Core Idea**: Any edge in the connected component that connects city 1 and city $n$ can be visited (since paths can repeat edges/vertices).
- **Key Formula**: Find the minimum edge weight among all edges whose vertices belong to the same component as city 1.
- **Approach**: DSU to group connected cities, followed by finding the minimum weight road among edges in component 1.

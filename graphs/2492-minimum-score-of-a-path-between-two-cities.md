# 2492. Minimum Score of a Path Between Two Cities

- **LeetCode Link:** [Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/)
- **Topic:** Graphs, Disjoint Set Union (Union-Find)
- **Programming Language:** Java
- **Submission Status:** Accepted

---

## My Intuition
I thought of using Union-Find to identify which cities belong to the same connected component as city 1.

I also realized that I needed to maintain a variable `min` to track the smallest road distance. My initial idea was to consider only roads connected to the component containing city 1 and ignore roads belonging to other components.

---

## My Approach
I used Union-Find with path compression and union by size:
1. Initialize the Union-Find structure for $n + 1$ cities because the cities are numbered from $1$ to $n$.
2. Process every road and merge its two endpoint cities using `union()`.
3. Initialize `min` to `Integer.MAX_VALUE`.
4. Iterate through all roads again.
5. For each road, check whether its endpoint belongs to the same connected component as city 1 (`find(road[0]) == find(1)`).
6. If it does, update `min` using the road's distance (`min = Math.min(min, road[2])`).
7. Return `min`.

This allows me to identify the connected component containing city 1 and find the minimum road distance within that component.

---

## Where I Got Stuck / My Mistake
- I did not encounter a major implementation issue with this approach.
- The main challenge was understanding which roads should be considered when calculating the minimum score.
- Checking connectivity using Union-Find helped me distinguish roads in the component containing city 1 from roads in other components.

---

## What Clicked
The key observation was:
- The answer is simply the minimum road distance among all roads in the connected component containing city 1.
- Since the path is allowed to revisit cities and roads freely, we can travel from city 1 to the road with the smallest distance anywhere in its component, traverse that road, and then continue toward city $n$.
- Therefore, we do not need Dijkstra's algorithm or shortest-path search. We only need to identify the relevant connected component and find its minimum edge weight.

---

## Final Submitted Code

```java
class Solution {
    public int minScore(int n, int[][] roads) {
        UnionFind uf = new UnionFind(n + 1);
        for (int[] road : roads) {
            uf.union(road[0], road[1]);
        }

        int min = Integer.MAX_VALUE;
        for (int[] road : roads) {
            if (uf.find(road[0]) == uf.find(1)) {
                min = Math.min(min, road[2]);
            }
        }

        return min;
    }
}

class UnionFind {
    int[] parent;
    int[] size;

    public UnionFind(int n) {
        parent = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX == rootY) {
            return;
        }

        if (size[rootX] < size[rootY]) {
            parent[rootX] = rootY;
            size[rootY] += size[rootX];
        } else {
            parent[rootY] = rootX;
            size[rootX] += size[rootY];
        }
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(n + m \cdot \alpha(n))$, where $n$ is the number of cities and $m$ is the number of roads. $\alpha(n)$ is the inverse Ackermann function. This runtime is nearly linear because of path compression and union by size.
- **Space Complexity:** $O(n)$ for the `parent` and `size` arrays in the Union-Find structure.

---

## Revision Notes
- Use $n + 1$ when city labels range from $1$ to $n$.
- Union all road endpoints to identify connected components.
- `find(x) == find(1)` checks whether city `x` belongs to the same component as city 1.
- The minimum score is the minimum edge weight in the component containing city 1.
- Because paths can revisit cities and roads, we do not need to find a simple path or use Dijkstra's algorithm.
- Union-Find is useful for connectivity questions, while the edge weights can be processed separately.
- **Pattern to Remember:** When a graph problem asks for the minimum edge weight in a connected component, first identify the relevant component, then scan its edges to find the minimum weight.

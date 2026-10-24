# 1319. Number of Operations to Make Network Connected

- **LeetCode Link:** [Number of Operations to Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected/)
- **Topic:** Graphs, Disjoint Set Union (Union-Find)
- **Programming Language:** Java
- **Submission Status:** Accepted

---

## My Intuition
Initially, I understood that I could use Union-Find to identify the connected components in the network. I also realized that we need to check whether there are enough cables to connect all the computers.

However, I was unsure how to determine the minimum number of operations required and considered checking the number of cables in each component.

---

## My Approach
I decided to use Union-Find with path compression and union by size:
1. Initialize each computer as a separate component.
2. Process every connection using the `union()` operation.
3. Whenever two computers belong to different components, merge their components and decrement the component count.
4. If both computers already belong to the same component, the connection is redundant and does not reduce the component count.
5. If the number of connections is less than $n - 1$, return `-1` because there are not enough cables to connect all computers.
6. Otherwise, return `components - 1`, since connecting $k$ components requires $k - 1$ cable moves.

---

## Where I Got Stuck / My Mistake
- I initially thought I needed to count the number of connected components and the number of cables within each component separately.
- I also considered using $\lceil n / 2 \rceil$ as the minimum number of cables required.
- **The Mistake:** Connecting $n$ computers requires at least $n - 1$ cables, not $\lceil n / 2 \rceil$. Also, I do not need to count cables in every component separately. Union-Find can identify redundant connections and track the number of components directly.

---

## What Clicked
The key observation was:
- If there are $k$ connected components, we need exactly $k - 1$ operations to connect them all, provided there are enough cables in total.
- Each successful union merges two separate components and reduces the component count by one.
- For example, if there are 4 components, we need 3 operations to connect them all.

**Final Logic:**
- If `connections.length < n - 1`, return `-1`.
- Otherwise, return `components - 1`.

---

## Final Submitted Code

```java
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if (connections.length < n - 1) {
            return -1;
        }

        UnionFind uf = new UnionFind(n);
        for (int[] edge : connections) {
            uf.union(edge[0], edge[1]);
        }

        return uf.getComponents() - 1;
    }
}

class UnionFind {
    int[] parent;
    int[] size;
    int components;

    public UnionFind(int n) {
        parent = new int[n];
        size = new int[n];
        components = n;
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
        components--;
    }

    public int getComponents() {
        return components;
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(n + m \cdot \alpha(n))$, where $n$ is the number of computers and $m$ is the number of connections. $\alpha(n)$ is the inverse Ackermann function, which grows extremely slowly. The runtime is nearly linear because of path compression and union by size.
- **Space Complexity:** $O(n)$ to store the `parent` and `size` arrays in the Union-Find data structure.

---

## Revision Notes
- Initially, every node is its own component.
- `find(x)` returns the root representing the component containing `x`.
- `union(x, y)` merges two components only when their roots differ.
- Path compression makes future `find()` operations faster by flattening the tree structure.
- Union by size attaches the smaller component to the larger one to maintain a balanced tree depth.
- Decrement the component count only when a successful merge occurs.
- At least $n - 1$ cables are necessary to connect $n$ computers.
- If enough cables exist, the minimum operations equal `numberOfComponents - 1`.
- **Pattern to Remember:** When a graph problem asks for the minimum number of cable/edge rearrangements needed to connect all nodes, consider counting connected components with Union-Find and checking whether enough total edges exist ($m \ge n - 1$).

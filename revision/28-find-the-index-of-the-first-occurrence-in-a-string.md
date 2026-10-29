# 28. Find the Index of the First Occurrence in a String

- **LeetCode Link:** [Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/?envType=problem-list-v2&envId=string)
- **Topic:** Two Pointers, String, String Matching
- **Programming Language:** Java
- **Submission Status:** Accepted

---

## My Intuition
The problem asks us to find the index of the first occurrence of `needle` in `haystack`, or return `-1` if `needle` is not part of `haystack`.

The most direct and intuitive way to solve this is to test each possible starting position in `haystack`:
- For every index `i` in `haystack`, check if the substring starting at `i` matches `needle` character by character.
- Use two pointers: one pointer `j` to traverse `haystack` starting from `i`, and another pointer `k` to traverse `needle` from index `0`.
- If all characters match (i.e., `k == needle.length()`), we have found the very first complete match and can immediately return the starting index `i`.
- If the loop finishes without finding any match, return `-1`.

---

## My Approach
1. **Outer Loop (Anchor Index)**:
   - Iterate through `haystack` with index `i` from `0` to `haystack.length() - 1`. Each `i` represents a candidate starting position for `needle`.
2. **Inner Pointer Matching**:
   - For each starting index `i`, initialize `j = i` (to traverse `haystack`) and `k = 0` (to traverse `needle`).
   - While both pointers are within bounds (`j < haystack.length()` and `k < needle.length()`) and characters match (`needle.charAt(k) == haystack.charAt(j)`), advance both pointers: `k++` and `j++`.
3. **Check for Complete Match**:
   - After the `while` loop, check if `k == needle.length()`.
   - If true, every character of `needle` matched consecutively starting at `i`. Return `i` immediately.
4. **Fallback**:
   - If the outer loop finishes without finding any match, return `-1`.

---

## Where I Got Stuck / My Mistake
- **Pointer Synchronization**: It's crucial not to advance `i` directly during character matching. By anchoring `i` as the starting position and using a separate pointer `j = i` for the inner comparison, we preserve the starting point `i` and safely advance to `i + 1` if a mismatch occurs.
- **Bound Checking**: Checking `j < haystack.length()` and `k < needle.length()` inside the `while` condition prevents `StringIndexOutOfBoundsException` when checking substrings near the end of `haystack`.
- **Loop Boundary Optimization (Takeaway)**: In this solution, `i` runs up to `haystack.length()`. While correct, any index `i > haystack.length() - needle.length()` cannot possibly contain `needle` because there aren't enough remaining characters. Pruning the loop to `i <= haystack.length() - needle.length()` avoids unnecessary iterations.

---

## What Clicked
- **Two-Pointer Substring Matching**: Using `k` both as the index into `needle` and as the count of matched characters makes the completion check straightforward (`k == needle.length()`).
- **Clean In-Place Traversal**: No temporary substrings or string builder objects are allocated, keeping auxiliary memory strictly $O(1)$.
- **Preserved Anchor**: Separating `i` (window start) and `j` (sliding pointer within window) cleanly isolates each candidate match attempt.

---

## Final Submitted Code

```java
class Solution {
    public int strStr(String haystack, String needle) {
        for(int i=0;i<haystack.length();i++){
            int j=i,k=0;
            while(j<haystack.length() && k<needle.length()&&needle.charAt(k)==haystack.charAt(j)){
                k++;
                j++;
            }
            if(k==needle.length()) return i;
        }
        return -1;
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N \cdot M)$ in the worst case, where $N$ is `haystack.length()` and $M$ is `needle.length()`. For example, matching `"aaaaab"` against `"aaa"`. However, in practice on typical text, mismatches occur within the first few characters, leading to average-case performance close to $O(N)$.
- **Space Complexity:** $O(1)$ auxiliary space. Only a few integer pointers (`i`, `j`, `k`) are maintained without extra string allocations.

---

## Revision Notes
- **Problem Pattern**: String Pattern Matching / Substring Search (Sliding Window / Two Pointers).
- **Match Condition**: When `k == needle.length()`, return the anchor index `i`.
- **Optimization Note**: The loop can terminate early if `i > haystack.length() - needle.length()`, as remaining characters are fewer than `needle`.
- **Advanced Alternatives for Interviews**:
  - `haystack.indexOf(needle)`: Standard library implementation.
  - **KMP Algorithm (Knuth-Morris-Pratt)**: Precomputes a Longest Prefix Suffix (LPS) array in $O(M)$ time and searches in $O(N)$ time, achieving an optimal $O(N + M)$ worst-case time complexity without backtracking in `haystack`.
  - **Rabin-Karp Algorithm**: Uses rolling hash values to achieve $O(N + M)$ average time complexity.

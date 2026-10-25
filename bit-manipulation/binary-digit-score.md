# Q1. Binary Digit Score

- **LeetCode Link:** Today's Contest Problem
- **Topic:** Bit Manipulation, Counting Set Bits
- **Programming Language:** Java
- **Submission Status:** Accepted

---

## My Intuition
I understood that the score depends on the number of set bits (`1`s) in the binary representations of the two given integers, `a` and `b`.

Instead of manually converting the decimal numbers into binary and counting the `1`s, I identified that Java provides a built-in method to count set bits.

---

## My Approach
I used Java's `Integer.bitCount()` method to count the set bits in both integers:
1. Calculate `s1`, the number of set bits in `a`.
2. Calculate `s2`, the number of set bits in `b`.
3. If `s1 >= s2`, return `s1 * s2`.
4. Otherwise, return `s2 / s1` using integer division.

This approach directly implements the conditions given in the problem.

---

## Where I Got Stuck / My Mistake
- I did not encounter any major difficulty with this problem.
- The main concept was recognizing that I could use `Integer.bitCount()` instead of writing a separate algorithm to count the set bits manually.

---

## What Clicked
The key observation was that **Java's `Integer.bitCount(n)` directly returns the number of `1` bits in the binary representation of an integer**.

For example:
- `14` in binary is `1110`, containing 3 set bits.
- `6` in binary is `110`, containing 2 set bits.

Since $3 \ge 2$, the score is:
$$3 \times 2 = 6$$

This allowed me to implement the solution with only two bit-counting operations and a conditional statement.

---

## Final Submitted Code

```java
class Solution {
    public int calculateScore(int a, int b) {
        int s1 = Integer.bitCount(a);
        int s2 = Integer.bitCount(b);

        if (s1 >= s2) {
            return s1 * s2;
        }

        return s2 / s1;
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(1)$ for fixed-width Java integers (32-bit integers run in constant time).
- **Space Complexity:** $O(1)$ because only two counters are stored.

---

## Revision Notes
- `Integer.bitCount(n)` counts the set bits in an integer.
- Use `Long.bitCount(n)` when working with a `long`.
- Read conditional definitions carefully and implement the specified comparison exactly.
- Integer division in Java discards the fractional part.
- Built-in bit-manipulation methods can simplify solutions when the problem does not require implementing the operation manually.
- **Pattern to Remember:** When a problem depends on the number of set bits in an integer, consider `Integer.bitCount()` before implementing manual bit counting.

# 17. Letter Combinations of a Phone Number

- **LeetCode Link:** [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)
- **Topic:** Backtracking, Recursion, String, Hash Table
- **Programming Language:** Java
- **Submission Status:** Accepted

---

## My Intuition
The problem asks for all possible letter combinations that a given string of digits (from 2 to 9) can represent based on telephone keypad mappings.

Each digit maps to a set of letters (e.g., `'2'` maps to `"abc"`, `'3'` maps to `"def"`). To form all combinations, we can explore every possible letter for each digit sequentially. This naturally fits a **backtracking / recursive depth-first search (DFS)** approach, where at each step (current digit index), we try each mapped character, recurse to the next digit, and backtrack.

---

## My Approach
1. **Mapping**: Pre-populate a hash map (or array) mapping digit characters `'2'` through `'9'` to their corresponding string letters.
2. **Edge Case**: If the input `digits` string is empty, immediately return an empty list `[]`.
3. **Backtracking Function** (`letterCombinationsHelper`):
   - **Base Case**: When the length of the accumulated combination (`tillNow`) equals `digits.length()`, add `tillNow` to `result` and return.
   - **Recursive Step**:
     - Retrieve the letter string for `digits.charAt(curr)`.
     - Iterate through each letter in the string.
     - **Choose**: Append the letter to `tillNow`.
     - **Explore**: Recurse with `curr + 1`.
     - **Unchoose (Backtrack)**: Remove the appended character by restoring `tillNow = tillNow.substring(0, tillNow.length() - 1)`.

---

## Where I Got Stuck / My Mistake
- **Empty String Edge Case**: When `digits` is empty `""`, an unhandled base case would evaluate `tillNow.length() == digits.length()` as `0 == 0`, adding `""` to the result and returning `[""]`. However, LeetCode requires returning `[]` for empty input. An explicit guard clause `if (digits == null || digits.isEmpty()) return result;` resolves this.
- **Backtracking State Management**: When appending and backtracking using `String`, ensure the string is sliced back to its previous length (`tillNow.substring(0, tillNow.length() - 1)`), or alternatively pass `tillNow + ch` directly into the recursive call or use `StringBuilder`.

---

## What Clicked
- The total number of combinations is the product of the number of choices for each digit. For $n$ digits with up to 4 letters each, there are at most $4^n$ leaf nodes in the recursion tree.
- Backtracking cleanly handles generating all combinations because each depth level corresponds to one digit in the input string.
- The standard backtracking template:
  1. Base case: reached desired depth / length.
  2. Loop through candidate choices for the current position.
  3. Make choice $\rightarrow$ Recurse $\rightarrow$ Undo choice.

---

## Final Submitted Code

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    Map<Character, String> map = new HashMap<>();
    List<String> result = new ArrayList<>();

    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.isEmpty()) {
            return result;
        }

        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");

        letterCombinationsHelper(digits, "", 0);
        return result;
    }

    public void letterCombinationsHelper(String digits, String tillNow, int curr) {
        if (tillNow.length() == digits.length()) {
            result.add(tillNow);
            return;
        }

        String abets = map.get(digits.charAt(curr));
        for (int j = 0; j < abets.length(); j++) {
            tillNow = tillNow + "" + abets.charAt(j);
            letterCombinationsHelper(digits, tillNow, curr + 1);
            tillNow = tillNow.substring(0, tillNow.length() - 1);
        }
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(4^n \cdot n)$, where $n$ is the length of `digits`. In the worst case (digits `'7'` and `'9'`, each having 4 letters), there are $4^n$ combinations, and constructing each combination string takes $O(n)$ time.
- **Space Complexity:** $O(n)$ recursion stack depth (excluding the output list), bounded by the length of `digits` (at most 4 as per problem constraints).

---

## Revision Notes
- Always check for empty string inputs `digits.isEmpty()` to avoid returning `[""]` instead of `[]`.
- Digits `'7'` and `'9'` have 4 letters (`"pqrs"`, `"wxyz"`), while others have 3.
- Backtracking structure:
  - Base case: `curr == digits.length()` or `tillNow.length() == digits.length()`.
  - Loop through available options at current digit.
  - Recurse to `curr + 1`.
  - Undo modification (backtrack).
- **Alternative Optimization**: Using `StringBuilder` instead of repeated string concatenations reduces object allocation overhead.
- **Pattern to Remember:** When generating all Cartesian products / combinatorial branches of variable-length input sequences, use backtracking with a recursive helper maintaining the current index and running prefix.

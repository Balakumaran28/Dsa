# 5. Hashing - Four Sum

- **Platform:** CodeChef
- **Problem Link:** [Hashing - Four Sum](https://www.codechef.com/practice/course/amazon-interview-questions/AMAZONPREP/problems/PREP27)
- **Topic:** Arrays, Sorting, HashMap, Four Sum
- **Programming Language:** Java
- **Submission Status:** Further debugging required

---

## My Intuition
I need to find every unique quadruple `(a, b, c, d)` such that:
$$a + b + c + d = X$$
The elements must satisfy:
$$a \le b \le c \le d$$
Each array occurrence can be used at most once.
The condition allows equal values, provided the array contains enough occurrences.

---

## My Approach
1. Sort the array so that values appear in nondecreasing order.
2. Fix the first two elements, `a` and `b`.
3. For each possible `c`, calculate the required value:
   $$d = X - a - b - c$$
4. Use a `HashMap` to track the frequencies of values available after the current `c`.
5. Remove the current occurrence of `c` from the map before searching for `d`. This prevents reusing the same array occurrence.
6. If the required `d` exists in the remaining map, record the quadruple.
7. Eliminate duplicate quadruples and print the results in numerical lexicographical order.

---

## Where I Got Stuck / My Mistake
- I was confused about what the `HashMap` should store and how to enforce the ordering condition.
- The important distinction is:
  - The `HashMap` stores values and their remaining frequencies.
  - Sorting and choosing indices in order enforce $a \le b \le c \le d$.
  - The frequency count ensures that a value is available at a later index.
  - Duplicate elimination ensures that each unique quadruple appears once.
- The previous implementation also needed correction to match the required output format: print the number of quadruples first, followed by the quadruples.

---

## What Clicked
- For fixed `a`, `b`, and `c`, the fourth value is determined:
  $$d = X - a - b - c$$
- I don't need to guess every possible `d`. I can calculate it and check whether it is available.
- For example, if $X = 6$, $a = 0$, $b = 1$, and $c = 2$, then:
  $$d = 6 - 0 - 1 - 2 = 3$$
- If an unused occurrence of `3` exists after the chosen `c`, the quadruple `(0, 1, 2, 3)` is valid.

---

## Reference Implementation
This version uses sorting and two pointers. It is a simpler, standard $O(N^3)$ solution for the same problem, although it does not use hashing.

```java
import java.util.*;

class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            int n = sc.nextInt();
            long x = sc.nextLong();
            int[] arr = new int[n];
            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextInt();
            }

            Arrays.sort(arr);
            List<List<Integer>> ans = new ArrayList<>();

            for (int i = 0; i < n - 3; i++) {
                if (i > 0 && arr[i] == arr[i - 1]) {
                    continue;
                }
                for (int j = i + 1; j < n - 2; j++) {
                    if (j > i + 1 && arr[j] == arr[j - 1]) {
                        continue;
                    }
                    int left = j + 1;
                    int right = n - 1;
                    while (left < right) {
                        long sum = (long) arr[i] + arr[j] + arr[left] + arr[right];
                        if (sum == x) {
                            ans.add(Arrays.asList(arr[i], arr[j], arr[left], arr[right]));
                            int lv = arr[left];
                            int rv = arr[right];
                            while (left < right && arr[left] == lv) {
                                left++;
                            }
                            while (left < right && arr[right] == rv) {
                                right--;
                            }
                        } else if (sum < x) {
                            left++;
                        } else {
                            right--;
                        }
                    }
                }
            }

            System.out.println(ans.size());
            for (List<Integer> q : ans) {
                System.out.println(q.get(0) + " " + q.get(1) + " " + q.get(2) + " " + q.get(3));
            }
        }
        sc.close();
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N^3)$ after sorting.
- **Space Complexity:** $O(\log N)$ for sorting's stack in typical Java implementations, excluding the output list.

---

## Revision Notes
- Sort first to simplify ordering and duplicate handling.
- For fixed `a`, `b`, and `c`, calculate $d = X - a - b - c$.
- A `HashMap` can track available values and their frequencies.
- Never reuse the same array occurrence twice.
- Skip duplicate values when generating quadruples.
- The required output begins with the number of unique quadruples.
- The reference implementation above uses two pointers, not hashing. Revisit the `HashMap` version separately if the goal is to practise hashing specifically.


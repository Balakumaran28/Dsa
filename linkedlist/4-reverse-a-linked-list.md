# 4. Reverse a Linked List

- **Platform:** CodeChef
- **Problem Link:** [Reverse a Linked List](https://www.codechef.com/practice/course/amazon-interview-questions/AMAZONPREP/problems/RTLL)
- **Topic:** Linked List, Iteration, Pointer Manipulation
- **Programming Language:** Java
- **Submission Status:** Approach understood; corrected implementation

---

## My Intuition
A singly linked list normally points from each node to the next node.
To reverse it, I need to change each node's next pointer so that it points to the previous node.
I can do this iteratively using three pointers: `prev`, `curr`, and `next`.

---

## My Approach
1. Initialize `prev = null` and `curr = head`.
2. While `curr != null`:
   - Save the next node in `next`.
   - Reverse the current pointer by setting `curr.next = prev`.
   - Move `prev` to `curr`.
   - Move `curr` to `next`.
3. Return `prev`, which points to the new head.

Saving the next node before reversing the pointer is essential because otherwise I would lose access to the remaining list.

---

## Where I Got Stuck / My Mistake
- My original code used:
  ```java
  while (curr.next != null)
  ```
- This condition does not handle an empty linked list because `curr` could be null.
- I also returned `head`, but after reversal, the original head becomes the tail.

---

## What Clicked
The essential sequence is:
1. Save the next node.
2. Reverse the current link.
3. Move `prev` forward.
4. Move `curr` forward.

The loop should continue while `curr != null`.

---

## Final Submitted Code

```java
public static Node listReverse(Node head) {
    Node prev = null;
    Node curr = head;

    while (curr != null) {
        Node next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }

    return prev;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ — every node is processed once.
- **Space Complexity:** $O(1)$ — only a constant number of pointers are used.

---

## Revision Notes
- Always save `curr.next` before changing the current link.
- Use `while (curr != null)` to handle empty lists.
- Return `prev`, not the original `head`.
- This is an in-place iterative reversal.


# 24. Swap Nodes in Pairs

- **LeetCode Link:** [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
- **Topic:** Linked List, Two Pointers, Recursion
- **Programming Language:** Java
- **Submission Status:** Accepted

---

## My Intuition
The goal is to swap every two adjacent nodes in a singly linked list without modifying the values stored inside the nodes (i.e., by altering the pointers/links themselves).

When swapping the first two nodes (`head` and `head.next`), the head of the list itself changes. To avoid handling the head node as a special edge case, using a **dummy node** that points to `head` simplifies pointer adjustments significantly.

---

## My Approach
I used an iterative pointer manipulation approach with a dummy node:
1. **Dummy Node**: Create a dummy node with `dummy.next = head`.
2. **Pointer Initialization**: Maintain a pointer `prev` initialized to `dummy`.
3. **Traversal & Pair Swapping**:
   - Loop while both `prev.next` and `prev.next.next` exist (at least two nodes left to swap).
   - Identify the two nodes to swap:
     - `first = prev.next`
     - `second = first.next`
   - Re-wire the pointers:
     - Point `first.next` to `second.next` (links `first` to the remainder of the list).
     - Point `second.next` to `first` (places `second` before `first`).
     - Point `prev.next` to `second` (links the previous segment to `second`).
   - Advance `prev` to `first` (since `first` is now the second node of the swapped pair).
4. **Return Result**: Return `dummy.next` as the new head of the modified list.

---

## Where I Got Stuck / My Mistake
- **Pointer Overwriting**: When rewiring pointers without temporary variables, it is easy to accidentally lose the reference to the next pair (e.g., setting `second.next = first` before saving or pointing `first.next = second.next`). Defining `first` and `second` explicitly before making any link modifications prevents pointer severance.
- **Odd vs. Even Length Lists**: The loop condition `prev.next != null && prev.next.next != null` cleanly terminates whether the list has an even number of nodes (both become `null`) or an odd number (only one node remains, so `prev.next.next == null`).

---

## What Clicked
- Using a **dummy node** eliminates the need for separate edge-case logic for the list head and returns `dummy.next` seamlessly.
- In each step, swapping nodes $A$ and $B$ preceded by $P$:
  ```
  P -> A -> B -> C
  ```
  Becomes:
  ```
  P -> B -> A -> C
  ```
  Then $P$ moves forward to $A$.
- Three pointer reassignments per pair:
  1. `first.next = second.next`
  2. `second.next = first`
  3. `prev.next = second`

---

## Final Submitted Code

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode prev = dummy;

        while (prev.next != null && prev.next.next != null) {
            ListNode first = prev.next;
            ListNode second = first.next;

            first.next = second.next;
            second.next = first;
            prev.next = second;

            prev = first;
        }

        return dummy.next;
    }
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(n)$, where $n$ is the number of nodes in the linked list. Each node is visited and its pointer modified once in a single pass.
- **Space Complexity:** $O(1)$, because pointer manipulation is performed in-place with a constant number of auxiliary pointers (`dummy`, `prev`, `first`, `second`).

---

## Revision Notes
- Use a **dummy node** whenever the head of a linked list may change or when operations require access to the node preceding `head`.
- For pair-wise or $k$-group manipulations, check both `curr.next != null` and `curr.next.next != null` to safely access adjacent pairs.
- After swapping, advance `prev` to `first` because `first` has moved to the second position in the pair.
- Can also be solved recursively:
  ```java
  if (head == null || head.next == null) return head;
  ListNode second = head.next;
  head.next = swapPairs(second.next);
  second.next = head;
  return second;
  ```
  However, the iterative solution has $O(1)$ space vs $O(n)$ call-stack space for recursion.
- **Pattern to Remember:** For in-place linked list node reordering, introduce a dummy predecessor (`dummy.next = head`), cache the target nodes (`first`, `second`), re-wire the 3 connecting pointers, and advance the predecessor pointer to the end of the swapped group.

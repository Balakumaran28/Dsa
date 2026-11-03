# 6. Cycle in a Linked List

- **Platform:** CodeChef
- **Problem Link:** [Cycle in a Linked List](https://www.codechef.com/practice/course/amazon-interview-questions/AMAZONPREP/problems/PREP58)
- **Topic:** Linked List, Fast and Slow Pointers, Floyd's Cycle Detection Algorithm
- **Programming Language:** Java
- **Submission Status:** Approach understood; implementation reviewed

---

## My Intuition
If a linked list contains a cycle, following its next pointers eventually brings us back to a node we have already visited.
I can detect a cycle using two pointers:
- `slow` moves one step at a time.
- `fast` moves two steps at a time.

If the list has a cycle, the pointers will eventually meet.
However, the problem asks for the node where the cycle begins, not just whether a cycle exists.

---

## My Approach
**Phase 1: Detect the cycle**
1. Initialize `slow = head` and `fast = head`.
2. Move `slow` one step and `fast` two steps.
3. If `fast` or `fast.next` becomes `null`, the list has no cycle.
4. If the pointers meet, a cycle exists.

**Phase 2: Find the cycle's starting node**
1. Reset `slow = head`.
2. Leave `fast` at the meeting point.
3. Move both pointers one step at a time.
4. Return the node where they meet again.

---

## Where I Got Stuck / My Mistake
- I understood how to detect a cycle but was unsure how to find its starting node.
- The important insight is that detecting a cycle and finding its entrance are two separate phases.
- After the first meeting, I must reset one pointer to the head and move both pointers at the same speed.

---

## What Clicked
- Floyd's algorithm uses the relationship between the distance from the head to the cycle entrance and the distance travelled within the cycle.
- After the first meeting, resetting `slow` to the head allows both pointers to reach the cycle entrance together when they move one step at a time.
- The returned value must be the actual node reference, not its value.

---

## Final Code

```java
Node detectCycle(Node head) {
    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }
    }

    return null;
}
```

---

## Complexity Analysis
- **Time Complexity:** $O(N)$ — both phases take linear time overall.
- **Space Complexity:** $O(1)$ — only two pointers are used.

---

## Revision Notes
- First meeting: confirms a cycle exists.
- Reset one pointer to `head`.
- Move both pointers one step at a time.
- Second meeting: identifies the cycle entrance.
- Compare node references, not node values.
- If `fast == null` or `fast.next == null`, return `null`.


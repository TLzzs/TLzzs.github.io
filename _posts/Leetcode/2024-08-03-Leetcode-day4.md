---
title: "Leetcode Day 4 | 24. Swap Nodes in Pairs、19. Remove Nth Node From End of List、 160. Intersection of Two Linked Lists、 142. Linked List Cycle II"
date: 2024-08-03 00:00:00
categories: [Leetcode, LinkedList]
tags: [LinkedList]
---
## 24. Swap Nodes in Pairs
**LeetCode Problem:** [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>
- **Check Base Cases**: If `head` is `null` or `head.next` is `null`, return `head`.

- **Initialize New Head**: Set `newHead` to `head.next`.

- **Initialize Pointers**:
  - `prev` to `null`.
  - `next` and `nextNext` for swapping.

- **Iterate Through the List**: Use `while` loop to process pairs of nodes until `head` or `head.next` is `null`.

- **Swap Pairs**:
  - Set `next = head.next`.
  - Set `nextNext = next.next`.
  - Swap nodes:
    - `next.next = head`.
    - `head.next= nextNext`.

- **Link Previous Pair**: If `prev` is not `null`, set `prev.next` to `next`.

- **Update Pointers**:
  - Set `prev` to `head`.
  - Set `head` to `nextNext`.

- **Return New Head**: Return `newHead`.


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public ListNode swapPairs(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode newHead= head.next;
    ListNode prev = null;
    ListNode next;
    ListNode nextNext;
    while (head != null && head.next != null) {
      // define second and third node;
      next = head.next;
      nextNext = next.next;

      // second node -> next  = first node
      next.next = head;
      // first node -> next = third node
      head.next = nextNext;

      // the last node before head , its next should be current head
      if (prev != null) {
        prev.next = next;
      }
      // update the variable
      prev = head;
      head = nextNext;
    }
    return newHead;
  }
}
```
### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **Best Case == Average Case == Worst Case**: O(n), since we have to go through each element in Linked List
- Swap Operation: O(1)

#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space.


## 19. Remove Nth Node From End of List
**LeetCode Problem:** [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>

### Thinking Process for Removing N-th Node from End of List
- **Initialize Two Pointers**: Start by using two pointers, `slow` and `fast`, both initially pointing to the `head` of the list.

- **Move Fast Pointer Ahead**: Move the `fast` pointer `n` steps ahead. This creates a gap of `n` nodes between the `slow` and `fast` pointers.

- **Check for Edge Case**: If the `fast` pointer becomes `null` after moving `n` steps, it indicates that the node to be removed is the head node. Return `head.next` as the new head of the list.

- **Move Both Pointers Together**: Move both `slow` and `fast` pointers one step at a time until `fast.next` is `null`. At this point, the `slow` pointer is just before the node to be deleted.

- **Delete the Target Node**: Update the `next` pointer of the `slow` pointer to skip the node that needs to be removed (`slow.next = slow.next.next`).

- **Return the Modified List**: Return the `head` of the modified list.

### Summary

The algorithm leverages two pointers to create a gap of `n` nodes between them. By the time the `fast` pointer reaches the end of the list, the `slow` pointer is positioned just before the node that needs to be deleted. This method allows for efficient removal of the node with a single pass through the list.



### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode slow = head;
    ListNode fast = head;
    for (int i = 0; i < n; i++) {
      fast = fast.next;
    }

    // if fast is null, then means we are deleting the first node
    if (fast == null) return head.next;

    // moving the fast and slow together until fast.next is null
    // then slow. next is the node we are going to delete
    while (fast.next != null) {
      fast = fast.next;
      slow = slow.next;
    }

    // skip that node
    slow.next = slow.next.next;

    return head;
  }
}
```

#### _Time Complexity_
  - The algorithm makes a single traversal of the linked list.
  - Initially, it moves the `fast` pointer `n` steps ahead.
  - Then, it moves both `slow` and `fast` pointers together until the end of the list.
  - Thus, the overall time complexity is <u>**O(n)**</u>, where `n` is the number of nodes in the linked list.


#### _Space Complexity_
  - The algorithm uses a constant amount of extra space for the pointers (`slow`, `fast`, etc.).
  - No additional data structures are used that depend on the input size.
  - Therefore, the space complexity is <u>**O(1)**</u>.


## 160. Intersection of Two Linked Lists
**LeetCode Problem:** [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>
-  **Calculate Lengths**: Traverse both linked lists to get their lengths (`countA` and `countB`).

- **Align Lists**:
  - Identify the longer and shorter lists.
  - Advance the pointer of the longer list by the difference in lengths (`diff`) to align both lists.

- **Find Intersection**: Traverse both lists simultaneously until the pointers meet at the intersection node or reach the end of the lists.

- **Return Result**:  Return the intersection node, if found, or `null` if no intersection exists.


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
public class Solution {
  public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int countA=0,countB=0;
    ListNode curA=headA, curB=headB;
    // get the length of LinkedList A
    while (curA != null) {
      countA++;
      curA = curA.next;
    }

    // get the length of LinkedList B
    while (curB != null) {
      countB++;
      curB = curB.next;
    }

    // determine the longer linked list and shorter linked list
    ListNode longerNode = headA, shorterNode = headB;
    int longerCount = countA, shorterCount = countB;
    if (countA < countB) {
      longerNode = headB; longerCount= countB;
      shorterNode = headA; shorterCount= countA;
    }

    // first traverse the lonnger linked list to make sure they are having same length
    int diff = longerCount - shorterCount;
    int i = 0;
    while (i < diff) {
      longerNode = longerNode.next;
      i++;
    }

    // moving together until found same node or reach the end of list
    while (longerNode != null &&  shorterNode !=null && longerNode != shorterNode) {
      longerNode = longerNode.next;
      shorterNode = shorterNode.next;
    }

    return longerNode;
  }
}
```

#### _Time Complexity_
- Calculating the lengths of the two linked lists takes O(m) for list A and O(n) for list B, where `m` and `n` are the lengths of the two lists.
- Aligning the longer list takes O(abs(m-n)), which is still within the bounds of O(m + n).
- Traversing both lists together to find the intersection takes O(min(m, n)).
- Thus, the overall time complexity is **O(m + n)**.

#### _Space Complexity_
- The algorithm uses a constant amount of extra space for pointers (`curA`, `curB`, `longerNode`, `shorterNode`) and counters (`countA`, `countB`, `diff`).
- No additional data structures are used that depend on the input size.
- Therefore, the space complexity is **O(1)**.


## 142. Linked List Cycle II
**LeetCode Problem:** [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>
#### Determine if the Linked List has a Cycle

1. **Using Fast and Slow Pointers**:
  - Define two pointers, `fast` and `slow`, both starting from the head node.
  - `fast` pointer moves two nodes at a time, while `slow` pointer moves one node at a time.
  - If `fast` and `slow` meet along the way, it indicates that the linked list has a cycle.

#### Why Fast and Slow Pointers Will Meet in a Cycle

- **Principle of Meeting in the Cycle**:
  - The `fast` pointer will enter the cycle first. If the `fast` and `slow` pointers meet, they will meet inside the cycle.
  - This means that starting from the head node with one pointer and from the meeting point with another pointer, both moving one node at a time, they will meet at the cycle's entrance.

#### Finding the Cycle's Entrance

1. **Reset One Pointer to the Head Node**:
  - After the `fast` and `slow` pointers meet, reset the `slow` pointer to the head node.
  - Keep the `fast` pointer at the meeting point.

2. **Move Both Pointers One Step at a Time**:
  - Move both `slow` and `fast` pointers one node at a time.
  - When they meet again, the meeting point is the entrance of the cycle.

![](https://code-thinking.cdn.bcebos.com/gifs/142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II%EF%BC%88%E6%B1%82%E5%85%A5%E5%8F%A3%EF%BC%89.gif)

### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next==null  )
            return null;
        ListNode fast = head, slow = head;
        // every time fast go 2 steps , and slow go 1 step;
        // if circle exists, they will meet in the circle;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow){
                break;
            }
        }
        // if fast reaches null node, then means no circle exists
        if(fast == null || fast.next == null) return null; 

        // make one node back to start, and now slow and fast both move 1 step each time
        slow = head;
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        // the node they meet again is the node of start of the circle
        return slow;
    }
}
```

#### _Time Complexity_ 
  - The algorithm consists of two main phases:
    1. Determining if a cycle exists by moving `fast` and `slow` pointers through the list. This phase takes <u>**O(n)**</u> time, where `n` is the number of nodes in the list.
    2. Finding the cycle entrance by moving both pointers one step at a time until they meet. This phase also takes <u>**O(n)**</u> time in the worst case.
  - Therefore, the overall time complexity is <u>**O(n)**</u>.

#### **Space Complexity**:
  - The algorithm uses a constant amount of extra space for the pointers (`fast`, `slow`), and no additional data structures are used.
  - Therefore, the space complexity is **O(1)**.

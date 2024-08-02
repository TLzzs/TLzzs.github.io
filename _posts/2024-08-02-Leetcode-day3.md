---
title: "Leetcode Day 3 | 203. Remove Linked List Elements、707. Design Linked List、206. Reverse Linked List"
date: 2024-08-01 00:00:00
categories: [Leetcode, LinkedList]
tags: [LinkedList]
---
## 203. Remove Linked List Elements
**LeetCode Problem:** [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/description/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>
Handle of deleting **Head Node(s)** and **normal nodes**
- _**Head Nodes**_ : Use a `while` loop to skip over all nodes at the beginning of the list that match the `target` value. This adjusts the `head` pointer to the first node that doesn't have the value val or to `null` if all nodes have the value val.
- **_Normal Nodes_**: 
  - Initialize Traversal: Start traversing from the new `head` using a pointer `cur` to manage the traversal and potential node removals.
  - Traverse and Remove: 
    - Iterate through the list, checking each node's next value.
    - If `cur.next.val == val`, skip `cur.next` by linking `cur.next` to `cur.next.next` to remove the node.
    - If the next `node` does not match `val`, simply advance the `cur` pointer.
- Return Modified List


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public ListNode removeElements(ListNode head, int val) {

    while (head!= null && head.val == val) {
      head = head.next;
    }
    ListNode cur = head;

    while (cur != null && cur.next != null) {
      ListNode next = cur.next;
      if (next.val == val) {
        cur.next = cur.next.next;
        continue;
      }
      cur = cur.next;
    }

    return head;
  }
}
```
### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **Best Case == Average Case == Worst Case**: O(n), since we have to go through each element in Linked List
- Delete Operation: O(1)

#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space.


## 707. Design Linked List
**LeetCode Problem:** [707. Design Linked List](https://leetcode.com/problems/design-linked-list/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>

#### Node Class

- **Purpose**: Represents an individual element in the list with a value and pointers to both previous and next nodes.
- **Properties**:
  - `val`: The value stored in the node.
  - `prev`: Pointer to the previous node in the list.
  - `next`: Pointer to the next node in the list.
- **Constructor**:
  - Initializes `val`, `prev`, and `next`.

#### Linked List Class

- **Attributes**:
  - `head`: Points to the first node of the list.
  - `tail`: Points to the last node of the list.
  - `size`: Tracks the number of nodes in the list.



### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Node {
  int val; // Value stored in the node
  Node prev; // Pointer to the previous node in the list
  Node next; // Pointer to the next node in the list

  // Constructor to initialize the node with a value
  public Node(int val) {
    this.val = val;
  }
}

class MyLinkedList {
  Node head; // Head of the list
  Node tail; // Tail of the list
  int size; // Number of elements in the list

  // Constructor to create an empty MyLinkedList
  public MyLinkedList() {
    this.head = null;
    this.tail = null;
    size = 0;
  }

  // Retrieves the value at the specified index
  public int get(int index) {
    if (index >= size) return -1; // Return -1 if index is out of bounds
    Node cur = head;
    int curIdx = 0;
    while (curIdx < index) {
      cur = cur.next;
      curIdx++;
    }
    return cur.val;
  }

  // Adds a node at the head of the list
  public void addAtHead(int val) {
    Node newNode = new Node(val);
    if (size == 0) {
      head = newNode;
      tail = newNode;
    } else {
      Node next = head;
      next.prev = newNode;
      newNode.next = next;
      head = newNode;
    }
    size++;
  }

  // Adds a node at the tail of the list
  public void addAtTail(int val) {
    Node newNode = new Node(val);
    if (size == 0) {
      head = newNode;
      tail = newNode;
    } else {
      Node prev = tail;
      newNode.prev = prev;
      prev.next = newNode;
      tail = newNode;
    }
    size++;
  }

  // Adds a node at the specified index
  public void addAtIndex(int index, int val) {
    if (index > size) return; // Exit if the index is greater than size
    if (index == 0) {
      addAtHead(val); // Add at head if index is 0
      return;
    } else if (index == size) {
      addAtTail(val); // Add at tail if index is at the size (end of list)
      return;
    }
    Node cur = head;
    for (int curIdx = 0; curIdx < index; curIdx++) {
      cur = cur.next;
    }
    Node newNode = new Node(val);
    Node prevNode = cur.prev;
    prevNode.next = newNode;
    newNode.prev = prevNode;
    newNode.next = cur;
    cur.prev = newNode;
    size++;
  }

  // Deletes a node at the specified index
  public void deleteAtIndex(int index) {
    if (index >= size) return; // Exit if index is out of bounds
    if (index == 0) {
      deleteAtHead(); // Special case to delete at head
      return;
    } else if (index == size - 1) {
      deleteAtTail(); // Special case to delete at tail
      return;
    }
    Node cur = head;
    for (int count = 0; count < index; count++) {
      cur = cur.next;
    }
    Node prev = cur.prev;
    Node next = cur.next;
    prev.next = next;
    next.prev = prev;
    size--;
  }

  // Deletes the head node of the list
  public void deleteAtHead() {
    if (size == 1) {
      head = null;
      tail = null;
    } else {
      head = head.next;
      head.prev = null;
    }
    size--;
  }

  // Deletes the tail node of the list
  public void deleteAtTail() {
    if (size == 1) {
      head = null;
      tail = null;
    } else {
      tail = tail.prev;
      tail.next = null;
    }
    size--;
  }
}

```

#### _Time Complexity_
- Operations that traverse the list (`get`, `addAtIndex`, `deleteAtIndex`) have a linear time complexity
  **O(n)**, where n is the distance to the index from the list's start. Direct operations on the head or tail (`addAtHead`, `addAtTail`, `deleteAtHead`, `deleteAtTail`) have a constant time complexity **O(1)**.

#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space


## 206. Reverse Linked List
**LeetCode Problem:** [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/?envType=problem-list-v2&envId=mgkzjl13)


### <u>Thinking Process</u>
- **Initialize Pointers**:
  - Define a pointer `cur` to point to the head node.
  - Define another pointer `pre`, initialized to `null`.

- **Start Reversing**:
  - Save the `cur->next` node using a temporary pointer `tmp`. This step is crucial to avoid losing the rest of the list.
  - Change `cur->next` to point to `pre`. At this point, the first node is reversed.

- **Iterate Through the List**:
  - Continue the reversal process by moving the `pre` and `cur` pointers forward.
  - Use the `tmp` pointer to advance `cur` to the next node.

- **Complete the Reversal**:
  - When `cur` points to `null`, the loop ends, and the list reversal is complete.
  - Return `pre`, which now points to the new head of the reversed list.

![reverse linked list](https://code-thinking.cdn.bcebos.com/gifs/206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.gif)


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode temp = cur.next; // 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
}
```

#### _Time Complexity_
- O(n): Each node in the linked list is visited exactly once. The operations performed on each node (like reassigning pointers) are constant time operations, so the overall time complexity for reversing a linked list is linear relative to the number of nodes n in the list.
#### _Space Complexity_
- O(1): The reversal is done in place, meaning no additional data structures are used that grow with the size of the input

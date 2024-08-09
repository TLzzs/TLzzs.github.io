---
title: "Leetcode Day 9 | 232. Implement Queue using Stacks、225. Implement Stack using Queues、20. Valid Parentheses"
date: 2024-08-09 00:00:00
categories: [Leetcode, Stack & Queue]
tags: [Stack & Queue]
---
## 232. Implement Queue using Stacks
**LeetCode Problem:** [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

## Problem Statement
We need to implement a queue using two stacks. The queue should support the following operations:
- `push(int x)`: Add an element to the end of the queue.
- `pop()`: Remove the element from the front of the queue and return it.
- `peek()`: Get the front element of the queue without removing it.
- `empty()`: Check whether the queue is empty.

## Approach and Thought Process

### 1. Understanding the Problem
A queue is a First-In-First-Out (FIFO) data structure, whereas a stack is a Last-In-First-Out (LIFO) data structure. We need to use two stacks to simulate the behavior of a queue.

### 2. Identifying Key Operations
- **Push operation (`push`)**: Since we want to add an element to the end of the queue, we can simply push it onto `stackIn`.
- **Pop operation (`pop`)**: To remove the element at the front of the queue, we need to reverse the order of elements. We can do this by transferring all elements from `stackIn` to `stackOut`, and then popping the top element from `stackOut`.
- **Peek operation (`peek`)**: This operation should return the front element of the queue without removing it. It can be implemented by calling `pop()` to get the front element and then pushing it back to `stackOut`.
- **Empty check (`empty`)**: The queue is empty if both `stackIn` and `stackOut` are empty.

### 3. Detailed Design
- **Push Operation (`push`)**:
  - Simply push the element onto `stackIn`.

- **Pop Operation (`pop`)**:
  - If `stackOut` is empty, transfer all elements from `stackIn` to `stackOut`. This reverses the order of elements, so the oldest element (the front of the queue) will be on top of `stackOut`.
  - Pop the top element from `stackOut` and return it.

- **Peek Operation (`peek`)**:
  - Call `pop()` to get the front element and store it in a temporary variable.
  - Push the temporary variable back onto `stackOut` to restore the state.
  - Return the temporary variable.

- **Empty Check (`empty`)**:
  - The queue is empty if both `stackIn` and `stackOut` are empty.

### 4. Code Implementation

```java
class MyQueue {
    Stack<Integer> stackIn = new Stack<>();
    Stack<Integer> stackOut = new Stack<>();
    
    // Pushes element x to the back of the queue.
    public void push(int x) {
        stackIn.add(x);
    }
    
    // Removes the element from in front of the queue and returns it.
    public int pop() {
        // Transfer elements from stackIn to stackOut if stackOut is empty
        if (stackOut.isEmpty()) {
            while (!stackIn.isEmpty()) {
                stackOut.add(stackIn.pop());
            }
        }
        return stackOut.pop();
    }
    
    // Gets the front element of the queue without removing it.
    public int peek() {
        int val = pop(); // Use pop to get the front element
        stackOut.add(val); // Push it back to stackOut to restore the state
        return val;
    }
    
    // Returns true if the queue is empty, false otherwise.
    public boolean empty() {
        return stackOut.isEmpty() && stackIn.isEmpty();
    }
}
```

### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **push:** O(1), as we are just pushing an element onto stackIn.
- **pop:** Amortized O(1). The worst case occurs when we need to transfer all elements from stackIn to stackOut, but each element is moved only once per operation, leading to an amortized O(1) time complexity.
- **peek:** Amortized O(1), as it internally calls pop and restores the state.
- **empty:** O(1), as we are simply checking if both stacks are empty.

#### _Space Complexity_
O(n), where n is the number of elements in the queue. This is because we are using two stacks to store the elements.



## 225. Implement Stack using Queues
**LeetCode Problem:** [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)


## Problem Statement
We need to implement a stack using two queues. The stack should support the following operations:
- `push(int x)`: Push an element onto the top of the stack.
- `pop()`: Remove the element from the top of the stack and return it.
- `top()`: Get the top element of the stack without removing it.
- `empty()`: Check whether the stack is empty.

## Approach and Thought Process

### 1. Understanding the Problem
A stack is a Last-In-First-Out (LIFO) data structure, whereas a queue is a First-In-First-Out (FIFO) data structure. We need to use two queues to simulate the behavior of a stack.

### 2. Identifying Key Operations
- **Push operation (`push`)**: Simply add the element to the back of the `queueIn`.
- **Pop operation (`pop`)**: To remove the element at the top of the stack, we need to remove all elements from `queueIn` except the last one, which is the top of the stack. We then move all other elements to `queueBackUp` and eventually back to `queueIn`.
- **Top operation (`top`)**: This operation should return the top element of the stack without removing it. It can be implemented by calling `pop()` to get the top element and then pushing it back to `queueIn`.
- **Empty check (`empty`)**: The stack is empty if both `queueIn` and `queueBackUp` are empty.

### 3. Detailed Design
- **Push Operation (`push`)**:
  - Simply add the element to `queueIn`.

- **Pop Operation (`pop`)**:
  - Transfer all elements from `queueIn` to `queueBackUp` except the last one. This last element is the top of the stack.
  - Store the last element in a variable (`ans`) and return it.
  - Transfer all elements back from `queueBackUp` to `queueIn`.

- **Top Operation (`top`)**:
  - Call `pop()` to get the top element and store it in a temporary variable (`ans`).
  - Push the temporary variable back to `queueIn` to restore the state.
  - Return the temporary variable.

- **Empty Check (`empty`)**:
  - The stack is empty if both `queueIn` and `queueBackUp` are empty.

### 4. Code Implementation

```java
class MyStack {
  Deque<Integer> queueIn = new ArrayDeque<>();
  Deque<Integer> queueBackUp = new ArrayDeque<>();

  // Pushes element x onto the stack.
  public void push(int x) {
    queueIn.add(x);
  }

  // Removes the element on top of the stack and returns it.
  public int pop() {
    // Move all elements except the last one from queueIn to queueBackUp
    while (queueIn.size() > 1) {
      queueBackUp.add(queueIn.poll());
    }
    // The last element in queueIn is the top of the stack
    int ans = queueIn.poll();
    // Move everything back to queueIn
    while (!queueBackUp.isEmpty()) {
      queueIn.add(queueBackUp.poll());
    }
    return ans;
  }

  // Gets the top element of the stack without removing it.
  public int top() {
    int ans = pop(); // Use pop to get the top element
    queueIn.add(ans); // Push it back to queueIn to restore the state
    return ans;
  }

  // Returns true if the stack is empty, false otherwise.
  public boolean empty() {
    return queueBackUp.isEmpty() && queueIn.isEmpty();
  }
}
```

#### _Time Complexity_
- push: O(1), as we are just adding an element to the end of queueIn.
- pop: O(n), where n is the number of elements in the stack. This is because we need to transfer elements between the two queues.
- top: O(n), since it calls pop() and then restores the state.
- empty: O(1), as it simply checks if both queues are empty.

#### _Space Complexity_
O(n), where n is the number of elements in the stack. This is because we are using two queues to store the elements.


## 20. Valid Parentheses
**LeetCode Problem:** [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/?envType=problem-list-v2&envId=mzikka8g)

## Problem Statement
Given a string `s` containing just the characters `'('`, `')'`, `'{', '}'`, `'['`, and `']'`, determine if the input string is valid. The string is considered valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

## Approach and Thought Process

### 1. Understanding the Problem
The problem requires checking whether the parentheses in a given string are balanced. A string of parentheses is balanced if:
- Every opening bracket has a corresponding closing bracket.
- The brackets are closed in the correct order.

### 2. Identifying Key Operations
- **Mapping Opening to Closing Brackets**: We can use a `HashMap` to map each closing bracket to its corresponding opening bracket.
- **Using a Stack**: A stack is ideal for this problem because it allows us to efficiently track and validate the most recent unmatched opening bracket. As we encounter each closing bracket, we can check if it matches the top of the stack (which should be the most recent unmatched opening bracket).

### 3. Detailed Design
- **HashMap for Bracket Mapping**:
  - We'll use a `HashMap` to store the mappings of closing brackets to their corresponding opening brackets. For example, `')'` maps to `'('`, `'}'` maps to `'{'`, and `']'` maps to `'['`.

- **Iterating Through the String**:
  - We'll iterate through each character in the string.
  - If the character is a closing bracket (i.e., it's a key in our `HashMap`), we check if it matches the top element of the stack.
    - If the stack is empty or the top element doesn't match, the string is invalid, and we return `false`.
    - If it matches, we pop the top element from the stack.
  - If the character is an opening bracket, we push it onto the stack.

- **Final Validation**:
  - After iterating through the string, if the stack is empty, all brackets were matched correctly, and the string is valid. Otherwise, the string is invalid.

### 4. Code Implementation

```java
class Solution {
    // HashMap that takes care of the mappings between closing and opening brackets.
    private HashMap<Character, Character> mappings;

    // Constructor to initialize the mappings.
    public Solution() {
        this.mappings = new HashMap<Character, Character>();
        this.mappings.put(')', '(');
        this.mappings.put('}', '{');
        this.mappings.put(']', '[');
    }

    // Method to check if the given string has valid parentheses.
    public boolean isValid(String s) {
        // Initialize a stack to keep track of opening brackets.
        Stack<Character> stack = new Stack<Character>();

        // Iterate over each character in the string.
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            // If the character is a closing bracket
            if (this.mappings.containsKey(c)) {
                // Pop the top element from the stack if it's not empty, otherwise use a dummy character.
                char topElement = stack.empty() ? '#' : stack.pop();

                // Check if the popped element matches the corresponding opening bracket.
                if (topElement != this.mappings.get(c)) {
                    return false; // Mismatch found
                }
            } else {
                // If it's an opening bracket, push it onto the stack.
                stack.push(c);
            }
        }

        // If the stack is empty, the string is valid. Otherwise, it's invalid.
        return stack.isEmpty();
    }
}
```


#### _Time Complexity_
- O(n), where n is the length of the string s. We iterate over each character in the string once.
#### _Space Complexity_
O(n) in the worst case, where n is the length of the string. This happens when all characters are opening brackets, and they are all pushed onto the stack.


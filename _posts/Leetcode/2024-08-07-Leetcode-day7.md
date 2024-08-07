---
title: "Leetcode Day 7 | 344. Reverse String、 541. Reverse String II"
date: 2024-08-07 00:00:00
categories: [Leetcode, String]
tags: [String]
---
## 344. Reverse String
**LeetCode Problem:** [344. Reverse String](https://leetcode.com/problems/reverse-string/description/)

### Thinking Process

1. **Understand the Problem**:
  - Reverse an array of characters `s` in place.

2. **Initialize Pointers**:
  - Use two pointers: `left` starting at the beginning (index 0) and `right` starting at the end (index `s.length - 1`).

3. **Swapping Characters**:
  - Swap characters at `left` and `right` positions.
  - Continue this until the pointers meet or cross each other.

4. **Swapping Logic**:
  - Use a temporary variable `tmp` to hold the value of `s[left]`.
  - Swap `s[left]` and `s[right]`.
  - Increment `left` and decrement `right` to move towards the center.

5. **End Condition**:
  - Loop terminates when `left` is no longer less than `right`.



### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public void reverseString(char[] s) {
    int left = 0; int right = s.length-1;

    while (left < right) {
      char tmp = s[left];
      s[left] = s[right];
      s[right] = tmp;
      left++; right--;
    }
  }
}
```

### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
1. **Initialization**:
  - Initializing the `left` and `right` pointers takes constant time, O(1).

2. **Loop Execution**:
  - The `while` loop runs until the `left` pointer is no longer less than the `right` pointer.
  - In each iteration of the loop, the `left` pointer is incremented and the `right` pointer is decremented. This means the number of iterations is proportional to half the size of the array, which is O(n/2) where `n` is the length of the array.
  - However, in Big-O notation, constant factors are ignored, so O(n/2) simplifies to O(n).

3. **Swapping**:
  - Each swap operation within the loop takes constant time, O(1).

Combining these observations, the overall time complexity is O(n), where `n` is the number of characters in the array.

#### _Space Complexity_
1. **Input Space**:
  - The input is an array of characters, and the space required to store the input is O(n). However, this is not considered additional space since it is given as part of the problem.

2. **Auxiliary Space**:
  - The algorithm uses a few variables (`left`, `right`, and `tmp`) which each take constant space, O(1).
  - No additional data structures are used that depend on the input size.

Since the algorithm only uses a fixed amount of extra space regardless of the input size, the auxiliary space complexity is O(1).



## 541. Reverse String II
**LeetCode Problem:** [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/)


### <u>Thinking Process</u>
Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

- If there are fewer than `k` characters left, reverse all of them.
- If there are at least `k` but fewer than `2k` characters left, reverse the first `k` characters and leave the rest as they are.


1. **Initial Check**:
  - If `k` is 1, reversing single characters has no effect, so return the string as is.

2. **Convert String to Character Array**:
  - Convert the string `s` into a character array `str` to allow in-place modifications.

3. **Loop to Process Every 2k Characters**:
  - Use a loop to iterate through the string in chunks of `2k`.
  - For each chunk, determine the start and end indices for the first `k` characters to reverse.

4. **Reverse Logic**:
  - For each `2k` segment, reverse the first `k` characters.
  - Ensure to handle the cases where fewer than `k` characters remain, reversing all remaining characters if necessary.


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public String reverseStr(String s, int k) {
    if ( k == 1) return s;
    char[] str = s.toCharArray();
    for (int i = 0 ; i * k * 2 < s.length(); i++) {
      reverse(str, i * k * 2, i * k * 2 + k - 1);
    }

    return String.valueOf(str);
  }

  public void reverse(char[] s, int start, int end) {
    if (end > s.length - 1) end = s.length - 1;
    while (start < end) {
      char tmp = s[start];
      s[start] = s[end];
      s[end] = tmp;
      start++; end--;
    }
  }
}
```

#### Time Complexity

- The main loop iterates over the string in steps of 2k.
- Each reverse call processes up to k characters, taking O(k) time.
- Therefore, the overall time complexity is O(n), where n is the length of the string.
#### Space Complexity

- The input string is converted to a character array, taking O(n) space.
- The algorithm uses a fixed amount of extra space, O(1), for variables.

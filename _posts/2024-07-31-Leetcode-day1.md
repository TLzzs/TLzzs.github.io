---
title: "Leetcode Day1 | 704. Binary Search、27. Remove Element、 977. Squares of a Sorted Array "
date: 2024-07-31 00:00:00
categories: [Leetcode, Array]
tags: [Array]
---
# 704. Binary Search

**LeetCode Problem:** [View Problem](https://leetcode.com/problems/binary-search)

## Introduction
Binary search is a highly efficient method for finding an element in a sorted array. This algorithm reduces the problem size by half with each step, rapidly narrowing down the possible locations of the target value.

## Ways of Thinking
To solve the binary search problem, understanding the following key concepts is crucial:
- **Divide and Conquer**: This strategy involves dividing the data set into smaller chunks and solving them independently.
- **Midpoint Calculation**: Careful calculation of the midpoint is essential to avoid integer overflow and to accurately split the search range.
- **Conditional Logic**: Decisions to adjust the search range based on comparison results between the target and the current middle value.

## Solution
Here's the Java implementation of the binary search algorithm:

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

## Time and Space Complexity Analysis

### Time Complexity
- **Best Case**: O(1), where the target is at the midpoint of the array.
- **Average Case**: O(log n), because the search space is halved with each step.
- **Worst Case**: O(log n), applicable when the target is at one of the ends of the array or not present at all.

### Space Complexity
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space regardless of the input array size.




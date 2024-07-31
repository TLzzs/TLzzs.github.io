---
title: "Leetcode Day1 | 704. Binary Search、27. Remove Element、 977. Squares of a Sorted Array "
date: 2024-07-31 00:00:00
categories: [Leetcode, Array]
tags: [Array]
---
# Binary Search Algorithm Explanation

The `search` function in the `Solution` class implements the binary search algorithm to find a target value within a sorted array. The function returns the index of the target if it is present in the array; otherwise, it returns -1. Below is a detailed step-by-step explanation of how the function works.


```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0; int right = nums.length -1;

        while (left <= right) {
            int mid = (right + left) + 1 / 2 ;
            if (nums[mid] == target) {
                return mid;
            }

            if (nums[mid] > target) {
                right = mid - 1;
            }

            if (nums[mid] < target) {
                left = mid + 1;
            }
        }

        return -1;
    }
}
```



---
title: "Leetcode Day1 | 704. Binary Search、27. Remove Element、 977. Squares of a Sorted Array "
date: 2024-07-31 00:00:00
categories: [Leetcode]
tags: [Array]
---

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



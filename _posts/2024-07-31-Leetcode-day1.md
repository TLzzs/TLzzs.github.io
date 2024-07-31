---
title: "Leetcode Day1 | 704. Binary Search、27. Remove Element、 977. Squares of a Sorted Array "
date: 2024-07-31 00:00:00
categories: [Leetcode, Array]
tags: [Array]
---
## 704. Binary Search

**LeetCode Problem:** [Binary Search](https://leetcode.com/problems/binary-search)

### <u>Ways of Thinking</u>

In this approach, we use the two-pointer method with pointers `[left, right]`, where `left` represents the starting index and `right` represents the ending index of the current search range.

#### _Loop Condition_: 
- We continue the search with the condition `while (left <= right)`, which ensures that the search space is not exhausted.

#### _Conditions Within the Loop:_ 
- **If the target is greater than `nums[mid]`**: Adjust the search range to the right half by setting `left` to `mid + 1`.
- **If the target is less than `nums[mid]`**: Adjust the search range to the left half by setting `right` to `mid - 1`.

#### _Termination:_
- The loop terminates early if the target number equals `nums[mid]`, meaning the target has been found.

### <u> Solution </u>

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

### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **Best Case**: O(1), where the target is at the midpoint of the array.
- **Average Case**: O(log n), because the search space is halved with each step.
- **Worst Case**: O(log n), applicable when the target is at one of the ends of the array or not present at all.

#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space regardless of the input array size.

### <u>Related Question List</u>
- [Leetcode 35. Search Insert Position](https://leetcode.com/problems/search-insert-position/description/)
- [Leetcode 34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)
- [Leetcode 69. Sqrt(x)](https://leetcode.com/problems/sqrtx/description/)
- [Leetcode 367. Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/description/)

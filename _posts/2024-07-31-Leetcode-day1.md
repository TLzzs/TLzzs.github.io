---
title: "Leetcode Day 1 | 704. Binary Search、27. Remove Element"
date: 2024-07-31 00:00:00
categories: [Leetcode, Array]
tags: [Array, Double Pointers, Binary Search]
---
## 704. Binary Search

**LeetCode Problem:** [704. Binary Search](https://leetcode.com/problems/binary-search)

### <u>Thinking Process</u>

In this approach, we use the two-pointer method with pointers `[left, right]`, where `left` represents the starting index and `right` represents the ending index of the current search range.
#### _Loop Condition:_ 
- We continue the search with the condition `while (left <= right)`, which ensures that the search space is not exhausted.

#### _Conditions Within the Loop:_ 
- **If the target is greater than `nums[mid]`**: Adjust search range to the right half by setting `left` to `mid + 1`.
- **If the target is less than `nums[mid]`**: Adjust search range to the left half by setting `right` to `mid - 1`.

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

---------------------------------------------------------------------------------------------------------------

## 27. Remove Element

**LeetCode Problem:** [27. Remove Element](https://leetcode.com/problems/remove-element/description/)

### <u>Thinking Process</u>

In this approach, we use the two-pointer method with pointers `[left, right]`, where `left` represents the starting index and `right` represents the ending index of the current search range.
- left pointer is the current first element that equals to target
- right pointer is the current last element that not equals to target
- everytime we need to swap left and right element , and then update left and right pointers

#### _Loop Condition:_
- We continue the search with the condition `while (left <= right)`, which ensures that the search space is not exhausted.

#### _Inside the loop, update the pointers to expect position:_
- **If the left pointer is not equal to `target`**: Adjust `left = left + 1` until left points to first target.
- **If the right pointer is equal to `target`**: Adjust `right = right - 1` until right points to first non target.

#### _Termination:_
- Do another check ensure `left < right`
- Swap `nums[left]` and `nums[right]`
- move both pointers, `left++`, `right--`

### <u> Solution </u>

Here's the Java implementation of the algorithm:

```java
class Solution {
  public int removeElement(int[] nums, int val) {
    int left = 0;
    int right = nums.length - 1;

    while (left <= right) {
      while (left <= right && nums[left] != val) left++;
      while (left <= right && nums[right] == val) right--;
      if (left < right) {
        int tmp = nums[left];
        nums[left] = nums[right];
        nums[right] = tmp;
        left++;
        right--;
      }
    }
    return left;
  }
}
```

### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **Best Case** == **Average Case** == **Worst Case**: O(n), where the array already sorted, but we still need to go through each element 


#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space regardless of the input array size.

### <u>Related Question List</u>
- [Leetcode 26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)
- [Leetcode 283. Move Zeroes](https://leetcode.com/problems/move-zeroes/description/)
- [Leetcode 844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/description/)
- [Leetcode 977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

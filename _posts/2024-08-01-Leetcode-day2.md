---
title: "Leetcode Day 2 | 209. Minimum Size Subarray Sum、59. Spiral Matrix II"
date: 2024-08-01 00:00:00
categories: [Leetcode, Array]
tags: [Array, Slide Windows]
---
## 209. Minimum Size Subarray Sum
**LeetCode Problem:** [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/?envType=problem-list-v2&envId=mgmdbgjt)


### <u>Thinking Process</u>
In this approach, we use the slide window method with pointers `[left, right]`, where `left` represents the left bound index and `right` represents the right bounds index of the windows.

#### _Initialise:_ 
- A variable used to count the current sum within the slide window `sum=0`
- A variable used to record the minimum size of slide window where `sum >= target`, `minSize = Integer.MAX_VALUE`
- left right pointers `left=0, right=0`

#### _Loop Condition:_ 
-  We continue the search with the condition `while (right < nums.length)`, ensure slide window is valid

#### _Conditions Within the Loop:_ 
- if `sum >= target`, `minSize = min (minSize, currentWindowSize)` where currentSize is `left - right + 1`
  - we need further explore the optimal solution , move my `left` point and `right` point one step forward, hang on ! before that we need  `sum-=nums[i]`
- if `sum < target`, we move **right point** one step forward, to get extra number in slide window

#### _Return_
- if `minSize` still `=Integer.MAX_VALUE`, no result found `return 0`, else we return `minSize`


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int i = 0, j = 0;
        int sum = 0;
        int minSize = Integer.MAX_VALUE;
        int N = nums.length;

        while ( j < N) {
            sum += nums[j];
            while (sum >= target) {
                minSize = Math.min(minSize, j - i + 1);
                sum -= nums[i];
                i++;
            }
            j++;
        }

        return minSize== Integer.MAX_VALUE ? 0 :minSize;
    }
}
```
### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **Best Case == Average Case == Worst Case**: O(n), since we have to go through each element using right pointer

#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space regardless of the input array size.


### <u>Related Question List</u>
- [904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/description/)

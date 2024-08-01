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


## 59. Spiral Matrix II
**LeetCode Problem:** [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/?envType=problem-list-v2&envId=mgmdbgjt)


### <u>Thinking Process</u>
simulating the clockwise drawing of a matrix:

- Fill the top row from left to right
- Fill the right column from top to bottom
- Fill the bottom row from right to left
- Fill the left column from bottom to top
- 
**Continue this pattern from the outside in, circling inward.**


#### _Initialise:_ 
- `int[][] ans = new int[n][n];`: This initializes an n x n matrix filled with zeros.
- `int circle = 0;`: represents the current layer or level of the spiral being filled. Starting from the outermost layer and moving inward.
- `int cur = 0;`: This variable tracks the current number to be placed in the matrix. It starts from 0 and increments as we fill the matrix.


#### _Loop Condition:_ 
The loop continues `while cur < n*n`, meaning until all elements of the matrix are filled.

#### _Conditions Within the Loop:_ 
- **Top Row Iterate Fill:**
- **Right Row Iterate Fill:**
- **Bottom Row Iterate Fill:**
- **Left Row Iterate Fill:**

### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int circle = 0, cur = 1;
        while (cur < n*n) {
            //top
            for (int i = circle; i<= n-1-circle && cur < n*n; i++){
                ans[circle][i] = ++cur;
            }
            //right
            for (int j = circle + 1; j <= n-1-circle && cur < n*n ; j++) {
                ans[j][n-1-circle]= ++cur;
            }
            
            //bottom
            for (int k = n-2-circle; k >= circle && cur < n*n; k--) {
                ans[n-1-circle][k] = ++cur;
            }
            
            //left
            for (int l = n-2-circle; l >= circle+1 && cur < n*n; l--) {
                ans[l][circle] = ++cur;
            }
            
            circle++;
        }
        return ans;
    }
}
```

#### _Time Complexity_
- **Best Case == Average Case == Worst Case**: O(n^2), since we have to simulate the matrix

#### _Space Complexity_
- **O(1)**: The space complexity is constant as the algorithm uses a fixed amount of space regardless of the input array size.

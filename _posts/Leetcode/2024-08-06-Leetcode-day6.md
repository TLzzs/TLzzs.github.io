---
title: "Leetcode Day 6 |15. 3Sum、 18. 4Sum"
date: 2024-08-06 00:00:00
categories: [Leetcode, Hashset, HashMap]
tags: [HashSet, HashMap]
---
## 15. 3Sum
**LeetCode Problem:** [15. 3Sum](https://leetcode.com/problems/3sum/description/?envType=problem-list-v2&envId=m1hcfiks)


### Thinking Process

**Explain**: Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, 
`i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

1. **Sort the Array**:
  - Start by sorting the input array `nums`. Sorting helps in easily skipping duplicate elements and simplifies finding triplets that sum up to zero.

2. **Initialize Result List**:
  - Create an empty list `ans` to store the resulting triplets.

3. **Iterate through the Array**:
  - Use a for loop to iterate through each element in `nums`:
    - **Break Early**: If the current element `nums[i]` is greater than zero, break the loop since no further triplet will sum to zero (array is sorted).
    - **Skip Duplicates**: If the current element is the same as the previous one, skip it to avoid duplicates in the result.

4. **Two-Pointer Approach**:
  - For each element `nums[i]`, use a two-pointer approach to find pairs that sum to the negative of `nums[i]`:
    - Set `left` to `i + 1` and `right` to the last index of the array.
    - Use a while loop to move the `left` and `right` pointers towards each other.
      - **Check Sum**: If the sum of `nums[i]`, `nums[left]`, and `nums[right]` equals zero, add the triplet to the result list `ans`.
      - **Skip Duplicates**: Move `left` and `right` pointers past any duplicate values to ensure unique triplets.
      - **Adjust Pointers**: If the sum is less than zero, increment `left`. If the sum is greater than zero, decrement `right`.

5. **Return Result**:
  - After processing all elements, return the list `ans` containing all unique triplets that sum up to zero.




### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();

    Arrays.sort(nums);

    for (int i = 0; i < nums.length; i ++) {
      if (nums[i] > 0) break;
      if (i > 0 && nums[i] == nums[i-1]) continue;
      int left = i + 1, right = nums.length -1;

      while (left < right) {
        if (nums[left] + nums[right] + nums[i] == 0) {
          ans.add(List.of(nums[i], nums[left], nums[right]));
          while (left < right && nums[left] == nums[left+1]) left ++;
          while (left < right && nums[right] == nums[right-1]) right --;
          left++;
          right--;
        } else if (nums[left] + nums[right] + nums[i] < 0) {
          left++;
        } else {
          right --;
        }
      }
    }
    return ans;
  }
}
```
### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **Sorting**: The initial sorting of the array takes `O(n log n)`.
- **Iteration**: The main iteration through the array takes `O(n)`.
  - **Two-Pointer Approach**: For each element, the two-pointer approach takes `O(n)` in the worst case. This is because both pointers `left` and `right` traverse the array.
  - Since the two-pointer approach runs within the main iteration, the total time complexity for the two-pointer approach is `O(n^2)`.

- **Overall Time Complexity**: Combining the sorting and the two-pointer approach, the overall time complexity is `O(n log n) + O(n^2)`, which simplifies to `O(n^2)`.

#### _Space Complexity_
- **Sorting**: The space complexity for sorting is `O(n)` due to the space required to store the sorted array.
- **Result List**: The result list `ans` could store up to `O(n^2)` triplets in the worst case, where each triplet consists of 3 integers. Hence, the space required for `ans` is `O(n^2)`.
- **Auxiliary Space**: Apart from the result list, the algorithm uses a constant amount of extra space for pointers and variables, which is `O(1)`.

- **Overall Space Complexity**: The overall space complexity is dominated by the space required for the result list, which is `O(n^2)`.



## 18. 4Sum
**LeetCode Problem:** [18. 4Sum](https://leetcode.com/problems/4sum/description/?envType=problem-list-v2&envId=m1hcfiks)


### <u>Thinking Process</u>
1. **Sort the Array**:
  - Start by sorting the input array `nums`. Sorting helps in easily skipping duplicate elements and simplifies finding quadruplets that sum up to the target.

2. **Initialize Result List**:
  - Create an empty list `ans` to store the resulting quadruplets.

3. **Iterate through the Array**:
  - Use two nested for loops to fix the first two elements of the quadruplet:
    - **First Fixed Number**: Iterate through the array with index `i`:
      - **Skip Duplicates**: If the current element `nums[i]` is the same as the previous one, skip it to avoid duplicates in the result.
    - **Second Fixed Number**: For each `i`, iterate through the array with index `j` starting from `i + 1`:
      - **Skip Duplicates**: If the current element `nums[j]` is the same as the previous one, skip it to avoid duplicates in the result.

4. **Two-Pointer Approach**:
  - For each pair of fixed elements `nums[i]` and `nums[j]`, use a two-pointer approach to find pairs that sum to `target - (nums[i] + nums[j])`:
    - Set `left` to `j + 1` and `right` to the last index of the array.
    - Use a while loop to move the `left` and `right` pointers towards each other.
      - **Check Sum**: If the sum of `nums[i]`, `nums[j]`, `nums[left]`, and `nums[right]` equals the target, add the quadruplet to the result list `ans`.
      - **Skip Duplicates**: Move `left` and `right` pointers past any duplicate values to ensure unique quadruplets.
      - **Adjust Pointers**: If the sum is less than the target, increment `left`. If the sum is greater than the target, decrement `right`.

5. **Return Result**:
  - After processing all elements, return the list `ans` containing all unique quadruplets that sum up to the target.



### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  List<List<Integer>> ans = new ArrayList<>();
  public List<List<Integer>> fourSum(int[] nums, int target) {
    Arrays.sort(nums);

    // fixed number 1
    for (int i = 0; i < nums.length; i++) {
      //pruning
      if (i > 0 && nums[i] == nums[i-1]) continue;
      // fixed number 2
      for (int j = i + 1; j < nums.length; j++) {
        //pruning
        if (j > i + 1 && nums[j] == nums[j-1]) continue;
        int left = j + 1;
        int right = nums.length - 1;

        while (left < right) {
          long num = (long) nums[i] + nums[j] + nums[left] + nums[right];
          if (num == target) {
            ans.add(List.of(nums[i], nums[j], nums[left], nums[right]));
            while (left < right && nums[left] == nums[left + 1]) left++;
            while (left < right && nums[right] == nums[right - 1]) right--;
            left++;
            right--;
          } else if (num < target) {
            left++;
          } else {
            right--;
          }
        }
      }
    }
    return ans;
  }
}
```

#### Time Complexity

- **Sorting**: The initial sorting of the array takes `O(n log n)`.
- **Iteration**: The main iteration through the array using two nested loops takes `O(n^2)`.
  - **Two-Pointer Approach**: For each pair of fixed elements, the two-pointer approach takes `O(n)` in the worst case. This is because both pointers `left` and `right` traverse the array.
  - Since the two-pointer approach runs within the nested loops, the total time complexity for the two-pointer approach is `O(n^3)`.

- **Overall Time Complexity**: Combining the sorting and the two-pointer approach, the overall time complexity is `O(n log n) + O(n^3)`, which simplifies to `O(n^3)`.

#### Space Complexity

- **Sorting**: The space complexity for sorting is `O(n)` due to the space required to store the sorted array.
- **Result List**: The result list `ans` could store up to `O(n^3)` quadruplets in the worst case, where each quadruplet consists of 4 integers. Hence, the space required for `ans` is `O(n^3)`.
- **Auxiliary Space**: Apart from the result list, the algorithm uses a constant amount of extra space for pointers and variables, which is `O(1)`.

- **Overall Space Complexity**: The overall space complexity is dominated by the space required for the result list, which is `O(n^3)`.


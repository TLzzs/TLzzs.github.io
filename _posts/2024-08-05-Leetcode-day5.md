---
title: "Leetcode Day 5 | 242. Valid Anagram、 349. Intersection of Two Arrays、 202. Happy Number、 1. Two Sum"
date: 2024-08-05 00:00:00
categories: [Leetcode, LinkedList]
tags: [LinkedList]
---
## 242. Valid Anagram
**LeetCode Problem:** [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)


### Thinking Process

**Explain**: An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

Two Ways of Doing this Question: Either using `map` or `array`

#### Method 1: Using a Map
1. **Check Length**: First, check if the two strings have the same length. If not, return false.
2. **Count Characters in First String**: Iterate through the first string and put each character into a map `<Character, Integer>`, where the key is the character and the value is the number of occurrences.
3. **Verify Characters in Second String**: Iterate through the second string. For each character:
  - If `map.contains(char)` and `map.get(char) > 0`, continue.
  - Else, return false.

#### Method 2: Using an Array
1. **Check Length**: First, check if the two strings have the same length. If not, return false.
2. **Count Characters in First String**: Iterate through the first string and put each character into an array of size 26. The index is `ASCII value of char - 'a'`, and the value is the number of occurrences.
3. **Verify Characters in Second String**: Iterate through the second string. For each character:
  - If `array[char - 'a'] > 0`, continue.
  - Else, return false.

These methods ensure that all characters in the strings are used exactly once and match between the two strings.



### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    Map<Character, Integer> map = new HashMap<>();
    char[] s1 = s.toCharArray();
    char[] s2 = t.toCharArray();

    for (int i = 0; i < s1.length; i++) {
      map.put(s1[i], map.getOrDefault(s1[i], 0) + 1);
    }

    for (int i = 0; i < s2.length; i++) {
      if (map.containsKey(s2[i]) && map.get(s2[i]) > 0) {
        map.put(s2[i], map.get(s2[i]) - 1);
      }  else {
        return false;
      }
    }

    return true;
  }
}
```
### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **O(n)** - Each string is traversed once, where n is the length of the strings.

#### _Space Complexity_
- **O(1)**: The map can have at most 26 entries (for lowercase English letters), or array has constant 26 indexes, so the space is constant.



## 349. Intersection of Two Arrays
**LeetCode Problem:** [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/description/?envType=problem-list-v2&envId=m1hcfiks)


### <u>Thinking Process</u>
- We can return the elements in any order, and we can use a HashSet to solve this problem.
- Define two HashSets: `set1` and `set2`.
- Iterate through the first array and add each element to `set1`.
- Iterate through the second array and for each element, check if the element exists in `set1`. If it does, add it to `set2`.
- Convert `set2` to an `int[]` array and return.


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public int[] intersection(int[] nums1, int[] nums2) {
    if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) {
      return new int[0];
    }
    Set<Integer> set1 = new HashSet<>();
    Set<Integer> resSet = new HashSet<>();
    //iterate through first array
    for (int i : nums1) {
      set1.add(i);
    }
    // Iterate through second array, see if the element is in the set1 already
    for (int i : nums2) {
      if (set1.contains(i)) {
        resSet.add(i);
      }
    }
    return resSet.stream().mapToInt(x -> x).toArray();
  }
}
```

#### _Time Complexity_
- **HashSet Operations**: Adding an element to a HashSet and checking if an element exists in a HashSet both have an average time complexity of O(1).
- **First Array Iteration**: O(n) - where n is the length of `nums1`.
- **Second Array Iteration**: O(m) - where m is the length of `nums2`.
- **Total Time Complexity**: O(n + m) - because we iterate through both arrays once.



#### _Space Complexity_
- **HashSet Storage**: We use two HashSets. In the worst case, each set could contain all elements from `nums1` and `nums2`.
- **Total Space Complexity**: O(n + m) - where n is the number of elements in `nums1` and m is the number of elements in `nums2`.


## 202. Happy Number
**LeetCode Problem:** [202. Happy Number](https://leetcode.com/problems/happy-number/description/?envType=problem-list-v2&envId=m1hcfiks)




###  <u>Explain</u>
Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1  or it loops endlessly in a cycle which does not include 1.
- Those numbers for which this process ends in 1 are happy.
- Return true if n is a happy number, and false if not.

### <u>Thinking Process</u>
- Use a HashSet to record any occurrence of the number we get to detect cycles.
- Initialize an empty HashSet to store the numbers encountered during the process.
- While the number `n` is not 1:
  - Calculate the sum of the squares of its digits.
  - If the new number has already been seen (exists in the HashSet), a cycle is detected, and return false.
  - Add the new number to the HashSet.
  - Update `n` to the new number.
- If the number `n` becomes 1, return true as it is a happy number.


### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public boolean isHappy(int n) {
    Set<Integer> record = new HashSet<>();
    while (n != 1 && !record.contains(n)) {
      record.add(n);
      n = getNextNumber(n);
    }
    return n == 1;
  }

  private int getNextNumber(int n) {
    int res = 0;
    while (n > 0) {
      int temp = n % 10;
      res += temp * temp;
      n = n / 10;
    }
    return res;
  }
}
```

#### _Time Complexity_
- **Sum of Squares Calculation**: Each calculation involves extracting digits, squaring them, and summing them up, which is O(log n) where n is the number. This is because the number of digits in a number n is proportional to log n (base 10).
- **Overall Complexity**: In the worst case, the process might take O(n log n) where n is the number of digits in the input number. Typically, it converges much faster, as the sums tend to form cycles quickly or reach 1.


#### _Space Complexity_
- **HashSet Storage**: The space used by the HashSet depends on the number of unique numbers encountered before detecting a cycle or reaching 1. In the worst case, all intermediate numbers are distinct.
- **Total Space Complexity**: O(k) where k is the number of unique numbers encountered during the process. This is generally much smaller than n.


## 1. Two Sum
**LeetCode Problem:** [1. Two Sum](https://leetcode.com/problems/two-sum/description/?envType=problem-list-v2&envId=m1hcfiks)

### <u>Thinking Process</u>

**Explain**: Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

#### Steps to Solve the Problem
1. **Define a Map**: Define a `Map<Integer, Integer>`, where the key is an element of `nums` and the value is the index of that key in `nums`.
2. **Iterate through nums**: For each element `nums[i]` in the array:
  - Compute `diff = target - nums[i]`. We want `diff` to be in `nums` such that `diff + nums[i] = target`.
  - Check if `diff` exists in the map:
    - If it does, return the indices `{i, map.get(diff)}` because we have found the solution.
    - If it does not, add the current element and its index to the map: `map.put(nums[i], i)`.

### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0 ; i < nums.length; i++) {
      int diff = target - nums[i];

      if (map.containsKey(diff)) {
        int index = map.get(diff);
        return new int[] {index, i};
      }

      map.put(nums[i], i);
    }

    return new int[]{};
  }
}
```

#### Time Complexity
- **Iteration through nums**: O(n), where `n` is the length of the array `nums`. Each element is processed exactly once.
- **Map Operations**: Both `containsKey` and `put` operations on the map take O(1) time on average.
- **Total Time Complexity**: O(n) - as we iterate through the array once and perform constant-time operations for each element.

#### Space Complexity
- **Map Storage**: The space used by the map is proportional to the number of elements stored in it. In the worst case, all `n` elements are stored in the map.
- **Total Space Complexity**: O(n) - for storing the elements and their indices in the map.






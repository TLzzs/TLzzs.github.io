---
title: "Leetcode Day 10 | 239. Sliding Window Maximum、347. Top K Frequent Elements"
date: 2024-08-11 00:00:00
categories: [Leetcode, Stack & Queue]
tags: [Stack & Queue]
---
## 239. Sliding Window Maximum
**LeetCode Problem:** [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/?envType=problem-list-v2&envId=mzikka8g)

### Problem Statement
Given an array of integers nums and an integer k, representing the size of a sliding window, return an array of the 
maximum values from each sliding window as it moves from left to right across the array.

### Approach and Thought Process
keep the queue.peek() to be the largest element in the queue
#### 3 step of thinking
1. pop(value): where `value = num[i-k]` while the sliding window moves we need to pop out the first value, actually we check if the peek == the value we are popping out
   if not equal means we already filtered previously
2. push(value): whenever we try to push(nums[i]), from the queue tail, remove all elements if that value is < the value we are pushing
3. getMaxVal(): return the peek;

#### Code Implementation

```java
class Solution {
  public int[] maxSlidingWindow(int[] nums, int k) {
    ArrayDeque<Integer> deque = new ArrayDeque<>();
    int n = nums.length;
    int[] res = new int[n - k + 1];
    int idx = 0;
    for(int i = 0; i < n; i++) {
      // 根据题意，i为nums下标，是要在[i - k + 1, i] 中选到最大值，只需要保证两点
      // 1.队列头结点需要在[i - k + 1, i]范围内，不符合则要弹出
      while(!deque.isEmpty() && deque.peek() < i - k + 1){
        deque.poll();
      }
      // 2.既然是单调，就要保证每次放进去的数字要比末尾的都大，否则也弹出
      while(!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
        deque.pollLast();
      }

      deque.offer(i);

      // 因为单调，当i增长到符合第一个k范围的时候，每滑动一步都将队列头节点放入结果就行了
      if(i >= k - 1){
        res[idx++] = nums[deque.peek()];
      }
    }
    return res;
  }
}
```

### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
The algorithm iterates over the array nums exactly once, resulting in O(n) time for the loop.
Each element is added and removed from the deque at most once, so all deque operations across the entire array also take O(n) time.
Therefore, the overall time complexity is **O(n).**

#### _Space Complexity_
The space complexity is primarily determined by the space required for the deque, which stores at most k indices at any time.
Therefore, the auxiliary space complexity is **O(k).**


## 347. Top K Frequent Elements
**LeetCode Problem:** [347. Top K Frequent Elements](https://leetcode.com/problems/implement-stack-using-queues/)


## Problem Statement
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

## Approach and Thought Process

### 4. Code Implementation
1. Count the frequency of each number using a HashMap.
2. Use a min-heap (priority queue) to keep track of the top k most frequent elements.
3. Extract the elements from the heap to get the final result.

```java
class Solution {
  public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
      map.put(num, map.getOrDefault(num, 0) + 1);
    }

    PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2) -> pair1[1]-pair2[1]);

    for (Map.Entry<Integer, Integer> entry : map.entrySet()) { //小顶堆只需要维持k个元素有序
      if (pq.size() < k) { //小顶堆元素个数小于k个时直接加
        pq.add(new int[]{entry.getKey(), entry.getValue()});
      } else {
        if (entry.getValue() > pq.peek()[1]) { //当前元素出现次数大于小顶堆的根结点(这k个元素中出现次数最少的那个)
          pq.poll(); //弹出队头(小顶堆的根结点),即把堆里出现次数最少的那个删除,留下的就是出现次数多的了
          pq.add(new int[]{entry.getKey(), entry.getValue()});
        }
      }
    }

    int[] ans = new int[k];
    for (int i = k - 1; i >= 0; i--) { //依次弹出小顶堆,先弹出的是堆的根,出现次数少,后面弹出的出现次数多
      ans[i] = pq.poll()[0];
    }

    return ans;
  }
}
```

#### _Time Complexity_
1. **Counting Frequencies:** Building the frequency map using a single pass through the array takes O(N), where N is the number of elements in the array nums.
2. **Building the Min-Heap:**
   - Inserting each unique element into the min-heap (priority queue) takes O(log k) time.
   - Since you might insert all M unique elements into the heap, this step can take up to O(M log k) time, where M is the number of unique elements in nums.
   - However, since the heap is maintained at size k, in most cases, this is more efficient than processing all elements.
The dominant part is building the min-heap, so the overall time complexity is` O(N + M log k).`

#### _Space Complexity_
1. Frequency Map: The HashMap storing the frequencies requires O(M) space, where M is the number of unique elements.
2. Min-Heap: The priority queue (min-heap) stores up to k elements, requiring O(k) space.

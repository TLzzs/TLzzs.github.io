---
title: "Leetcode Day 8 | 151. Reverse Words in a String、28. Find the Index of the First Occurrence in a String(KMP)"
date: 2024-08-08 00:00:00
categories: [Leetcode, String]
tags: [String]
---
## 151. Reverse Words in a String
**LeetCode Problem:** [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/description/?envType=problem-list-v2&envId=mc8firmv)

### Thinking Process

This problem effectively examines various string operations.

Some people might use the `split` library function to separate words, then define a new string, and finally concatenate the words in reverse order. This approach would make the problem very easy and somewhat miss its original intent.

Therefore, I propose increasing the difficulty of this problem: do not use auxiliary space, aiming for a space complexity of O(1).

Without the use of auxiliary space, the only option left is to manipulate the original string.

Consider this: if we reverse the entire string, the order of the words will indeed be reversed, but the words themselves will also be reversed. If we then reverse each word individually, wouldn't the words be correctly oriented?

Thus, the solution involves the following steps:
1. Remove any extra spaces.
2. Reverse the entire string.
3. Reverse each word individually.

For example, consider the original string: `"the sky is blue "`

- **Remove extra spaces**: `"the sky is blue"`
- **String reversal**: `"eulb si yks eht"`
- **Word reversal**: `"blue is sky the"`

By following these steps, we successfully reverse the words in the string.

### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public String reverseWords(String s) {
    // 1.Remove all useless space
    StringBuilder sb = removeSpace(s);
    // 2.reverse the whole string
    reverseString(sb, 0, sb.length() - 1);
    // 3.reverse each word
    reverseEachWord(sb);
    return sb.toString();
  }

  private StringBuilder removeSpace(String s) {
    // System.out.println("ReverseWords.removeSpace() called with: s = [" + s + "]");
    int start = 0;
    int end = s.length() - 1;
    while (s.charAt(start) == ' ') start++;
    while (s.charAt(end) == ' ') end--;
    StringBuilder sb = new StringBuilder();
    while (start <= end) {
      char c = s.charAt(start);
      if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
        sb.append(c);
      }
      start++;
    }
    // System.out.println("ReverseWords.removeSpace returned: sb = [" + sb + "]");
    return sb;
  }

  public void reverseString(StringBuilder sb, int start, int end) {
    // System.out.println("ReverseWords.reverseString() called with: sb = [" + sb + "], start = [" + start + "], end = [" + end + "]");
    while (start < end) {
      char temp = sb.charAt(start);
      sb.setCharAt(start, sb.charAt(end));
      sb.setCharAt(end, temp);
      start++;
      end--;
    }
    // System.out.println("ReverseWords.reverseString returned: sb = [" + sb + "]");
  }

  private void reverseEachWord(StringBuilder sb) {
    int start = 0;
    int end = 1;
    int n = sb.length();
    while (start < n) {
      while (end < n && sb.charAt(end) != ' ') {
        end++;
      }
      reverseString(sb, start, end - 1);
      start = end + 1;
      end = start + 1;
    }
  }
}
```

### <u>Time and Space Complexity Analysis</u>

#### _Time Complexity_
- **O(N):** This means that the time it takes to complete the task increases linearly with the size of the input string. So if your 
string is twice as long, the operations to reverse the words will take roughly twice as long. This is because we have to
look at each character in the string a few times (to remove spaces, to reverse the whole string, and then to reverse each word).

#### _Space Complexity_
O(1): This tells us that the amount of additional memory (or space) needed doesn't depend on the size of the input string. We use only a tiny bit of extra space no matter how big the string is, just enough to hold a few temporary values while we rearrange the characters directly in the original string.
Since the algorithm only uses a fixed amount of extra space regardless of the input size, the auxiliary space complexity is O(1).



## 28. Find the Index of the First Occurrence in a String
**LeetCode Problem:** [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/?envType=problem-list-v2&envId=mc8firmv)


### <u>Thinking Process</u>
#### The KMP algorithm
![](https://code-thinking.cdn.bcebos.com/gifs/KMP%E7%B2%BE%E8%AE%B22.gif)

#### construct next array
- Define two pointers, `i` and `j`. The pointer `j` points to the end of the prefix, and `i` points to the end of the suffix.
- **Purpose of` next[i]`** : The `next[i]` represents the length of the longest proper prefix which is also a suffix for the substring ending at i-1. It directly influences how the search skips characters.

  1. **Handling Mismatches**: When a mismatch occurs between `s[i]` and `s[j]`, the algorithm must efficiently find where to restart the comparison
     ```
        while (j > 0 && s[i] != s[j]) { j = next[j-1]; }  // Use the previously computed value to skip ahead
     ```
  2. **Handling Matches**: If a match is found (`s[i] `== `s[j]`), both i and j are incremented, acknowledging the extension of a known matching prefix:
     ```
     if (s[i] == s[j]) {
        j++;  // Extend the matching prefix length
     }
     next[i] = j;
     ```
     
### <u> Solution </u>

Here's the Java implementation of the binary search algorithm:

```java
class Solution {
  public int strStr(String haystack, String needle) {
    int [] next = getNext(needle);

    int j = 0;
    for (int i = 0; i < haystack.length(); i++) {
      while (j > 0 && needle.charAt(j) != haystack.charAt(i))
        j = next[j - 1];
      if (needle.charAt(j) == haystack.charAt(i))
        j++;
      if (j == needle.length())
        return i - needle.length() + 1;
    }
    return -1;
  }

  int [] getNext(String str) {
    int [] ans = new int [str.length()];
    int j = 0; // j means the end char of the prefix

    for (int i = 1; i < str.length(); i++) {
      while (j > 0 && str.charAt(j) != str.charAt(i)) {
        j = ans[j-1];
      }

      if (str.charAt(j) == str.charAt(i)) {
        j++;
      }
      ans[i] = j;
    }

    return ans;
  }
}
```

#### _Time Complexity_
- O(n + m), which allows the search process to be significantly faster than the naive approach, especially for large texts and when the pattern contains repeated subpatterns.

#### _Space Complexity_
O(m), constrained primarily by the need to store the prefix table.

# 🕵️ Case: Valid Anagram

**Platform:** LeetCode  
**Problem ID / Code:** 242  
**Difficulty:** Easy  
**Topics:** Array, String, Hashing  
**Problem Link:** [https://leetcode.com/problems/valid-anagram/](https://leetcode.com/problems/valid-anagram/)

---

## Summary

Given two strings `s` and `t`, determine whether `t` is an anagram of `s`. Return `true` if both strings contain the same characters with the same frequencies; otherwise, return `false`.

```
Example 1:
Input: s = "anagram", t = "nagaram"
Output: true

Example 2:
Input: s = "rat", t = "car"
Output: false
```

Constraints:
```
1 <= s.length, t.length <= 5 * 104
s and t consist of lowercase English letters.
```

---

## 🔎 First Instinct — Brute Force

What was the first straightforward solution I thought of?

### Investigation

1. Use a loop to iterate through each character in string `s`.
2. For every character, search for a matching character in string `t` using another loop.
3. Check whether each character appears the same number of times in both strings. If the frequencies do not match, return `false`.
4. Mark matched characters to avoid counting the same occurrence more than once.

### Complexity

**Time:** `O(n²)`  
**Why?** Each character may require scanning the other string to find a matching occurrence.

**Space:** `O(n)`  
**Why?** An additional structure may be needed to track matched characters and prevent counting them more than once.

---

## 🧠 Better Solution

### Steps

1. Sort both strings in alphabetical order.
2. Compare the sorted strings.
3. If both strings are equal, return `true`; otherwise, return `false`.

### Complexity

**Time:** `O(n log n)`  
**Why?** Sorting both strings takes `O(n log n)` time.

**Space:** `O(n)`  
**Why?** Additional space may be required to convert the strings into character arrays for sorting, depending on the implementation.

---

## 🧠 Actual Solution

What is the key idea that solves the bottleneck?

-> Instead of repeatedly searching for matching characters or sorting both strings, count the frequency of each character. Since the strings contain only lowercase English letters, an array of size 26 is sufficient.

### Steps

1. Create an integer array of size 26 to store the frequency of each character in string `s`.
2. Traverse string `s` and increment the count for each character.
3. Traverse string `t` and decrement the count for each character.
4. If a character's count is already zero or negative before decrementing, return `false` because `t` contains an extra occurrence of that character.
5. If all characters are processed successfully, return `true`.

### Code

```java
// Optimized solution
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        int n = s.length();
        int[] arr = new int[26];

        for (int i = 0; i < n; i++) {
            int c = s.charAt(i) - 'a';
            arr[c]++;
        }

        for (int i = 0; i < n; i++) {
            int c = t.charAt(i) - 'a';

            if (arr[c] <= 0) {
                return false;
            }

            arr[c]--;
        }

        return true;
    }
}
```

### Complexity

**Time:** `O(n)`  
**Why?** Both strings are traversed once. Each character requires a constant-time array operation.

**Space:** `O(1)`  
**Why?** The algorithm uses a fixed-size array of 26 integers, regardless of the input length.

---

## ⚖️ Final Report

| Approach | Time | Space |
|----------|------|-------|
| Brute Force | O(n²) | O(n) |
| Better | O(n log n) | O(n) |
| Optimal | O(n) | O(1) |
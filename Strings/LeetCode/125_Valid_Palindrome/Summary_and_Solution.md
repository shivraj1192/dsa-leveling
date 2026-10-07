# Valid Palindrome

**Platform:** LeetCode  
**Problem ID:** 125  
**Difficulty:** Easy  
**Problem Link:** https://leetcode.com/problems/valid-palindrome/  
**Topics:** Strings, Two Pointers

## Problem Summary

We are given a string. We need to check whether it is a valid palindrome.

For this problem:

- Consider only alphanumeric characters.
- Ignore spaces and special characters.
- Ignore the difference between uppercase and lowercase letters.

### Example

```text
Input: "A man, a plan, a canal: Panama"
Output: true
```

---

## Approach 1 — Brute Force

### Idea

First create a cleaned version of the string containing only alphanumeric characters in lowercase.

Then reverse the cleaned string and compare it with the original cleaned string.

### Steps

1. Traverse the input string.
2. Keep only alphanumeric characters.
3. Convert characters to lowercase.
4. Store the cleaned string.
5. Reverse the cleaned string.
6. Compare the original and reversed strings.

### Java

```java
public boolean isPalindrome(String s) {
    StringBuilder cleaned = new StringBuilder();

    for (char c : s.toCharArray()) {
        if (Character.isLetterOrDigit(c)) {
            cleaned.append(Character.toLowerCase(c));
        }
    }

    String original = cleaned.toString();
    String reversed = cleaned.reverse().toString();

    return original.equals(reversed);
}
```

### Complexity

**Time:** `O(n)`

We scan the string and then reverse the cleaned string.

**Space:** `O(n)`

We create a cleaned string and its reversed representation.

### Limitation

The solution works, but it uses extra space to store the cleaned string.

---

## Approach 2 — Optimal

### Idea

Instead of creating another cleaned string, use two pointers:

- `left` starts from the beginning.
- `right` starts from the end.

Move the pointers toward each other.

If a character is not alphanumeric, skip it.

When both characters are valid, compare them after converting them to lowercase.

If they are different, the string is not a palindrome.

### Steps

1. Set `left = 0`.
2. Set `right = n - 1`.
3. Move `left` forward while its character is not alphanumeric.
4. Move `right` backward while its character is not alphanumeric.
5. Compare the two valid characters.
6. If they are different, return `false`.
7. Move both pointers toward the center.
8. If all characters match, return `true`.

### Java

```java
public boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;

    while (left < right) {

        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }

        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }

        if (Character.toLowerCase(s.charAt(left)) !=
            Character.toLowerCase(s.charAt(right))) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

### Complexity

**Time:** `O(n)`

Each character is processed at most a constant number of times.

**Space:** `O(1)`

We only use two pointers and do not create another string.

### Why This Is Better

Both approaches take `O(n)` time.

The important improvement is space:

```text
Brute Force → O(n) extra space
Optimal     → O(1) extra space
```

The optimal solution checks the palindrome directly in the original string instead of creating a cleaned copy.

---

## Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| Brute Force | O(n) | O(n) |
| Optimal | O(n) | O(1) |

---

## Key Learning

When checking something from both ends of a string or array, consider using **two pointers** instead of creating another data structure.

The main optimization here is removing the need to create a cleaned string.
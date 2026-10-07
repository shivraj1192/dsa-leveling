# 🕵️ Case: Valid Palindrome

**Platform:** LeetCode  
**Problem ID:** 125  
**Difficulty:** Easy  
**Topics:** Strings, Two Pointers  
**Problem Link:** https://leetcode.com/problems/valid-palindrome/

---

## 🧩 What puzzled me

We are given a string and need to determine whether it is a palindrome.

But there is a catch:

- Ignore spaces.
- Ignore special characters.
- Consider only alphanumeric characters.
- Ignore uppercase/lowercase differences.

The main thing that made me pause was:

> **How can I compare the characters while ignoring unwanted characters without creating unnecessary extra data?**

### Example

```text
"A man, a plan, a canal: Panama"
→ true
```

```text
"race a car"
→ false
```

---

## 🔎 First instinct — Brute Force

My first thought was:

> **Clean the string first, then check whether the cleaned string is a palindrome.**

### Investigation

1. Traverse the string.
2. Keep only alphanumeric characters.
3. Convert them to lowercase.
4. Store the cleaned string.
5. Reverse the cleaned string.
6. Compare the original cleaned string with the reversed string.

### Code

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

We traverse the string and reverse the cleaned string.

**Space:** `O(n)`

We create an additional cleaned string.

### What bothered me

The solution works, but I am creating another representation of the input just to check something that can potentially be checked directly.

That is the bottleneck.

---

## 🔍 Mini Investigation — Find the Bottleneck

The actual palindrome property is about comparing:

```text
first  ↔ last
second ↔ second-last
third  ↔ third-last
```

So why should I create another string?

I can directly look at both ends of the original string.

But there is another problem:

```text
"ab, cba"
   ↑
special characters
```

I cannot compare every character directly.

So I need to **skip characters that are not alphanumeric**.

This leads to the two-pointer approach.

---

## 🧠 Actual Solution — Two Pointers

Use two pointers:

```text
left  → beginning
right → end
```

Then:

1. Move `left` forward until it reaches an alphanumeric character.
2. Move `right` backward until it reaches an alphanumeric character.
3. Compare both characters after converting them to lowercase.
4. If they are different → not a palindrome.
5. If they match → move both pointers.
6. Continue until the pointers meet.

### Code

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

---

## 🧪 Mini Investigation — Dry Run

Input:

```text
"A man, a plan, a canal: Panama"
```

Ignoring non-alphanumeric characters:

```text
A m a n a p l a n a c a n a l P a n a m a
↑                                   ↑
L                                   R
```

Compare:

```text
A ↔ a  → match
m ↔ m  → match
a ↔ a  → match
n ↔ n  → match
...
```

The pointers keep moving toward the center.

No mismatch is found.

Therefore:

```text
true
```

---

## ⚖️ Final Report

| Approach | Time | Space |
|---|---:|---:|
| Brute Force | O(n) | O(n) |
| Two Pointers | O(n) | O(1) |

The time complexity did **not** improve.

The important improvement was:

```text
O(n) extra space
      ↓
O(1) extra space
```

I removed the need to create a cleaned string and its reversed representation.

---

## 💡 What I Learned

The important lesson is not just:

> "Use two pointers for palindrome."

The deeper lesson is:

> **Before creating another data structure, check whether the problem can be solved directly on the original input.**

When I need to compare elements from opposite ends, two pointers should come to mind.

---

## 🕵️ Recognition Clue

When I see:

- palindrome
- compare beginning and end
- symmetric structure
- process from both directions

I should immediately investigate:

```text
Two Pointers
```

If there are unwanted characters, don't immediately create a cleaned string.

Ask:

> **Can I skip them while moving the pointers?**

---

## ⚠️ Mistakes to Remember

- Don't forget to ignore non-alphanumeric characters.
- Comparison should be case-insensitive.
- The `right` pointer moves **backward**, not forward.
- Don't create an additional string if it isn't necessary.
- Make sure both pointers are still within the valid range before accessing characters.

---

## 🔁 Revisit

**No**

Reason:

The final approach is straightforward once the two-pointer pattern is understood.

---

## 🧠 Case Summary

```text
Problem
   ↓
Need to check palindrome
   ↓
First instinct
Clean → Reverse → Compare
   ↓
Works, but uses O(n) extra space
   ↓
Find the bottleneck
Unnecessary string creation
   ↓
Investigation
Compare directly from both ends
   ↓
Two Pointers
   ↓
O(n) Time + O(1) Space
```
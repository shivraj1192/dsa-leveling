# 🕵️ Case: Contains Duplicate

**\*\*Platform:\*\*** LeetCode  
**\*\*Problem ID / Code:\*\*** 217  
**\*\*Difficulty:\*\*** Easy  
**\*\*Topics:\*\*** Array, HashSet, Sorting  
**\*\*Problem Link:\*\*** [https://leetcode.com/problems/contains-duplicate/description/](https://leetcode.com/problems/contains-duplicate/description/)

---

## Summary

Given an integer array `nums`, return `true` if any element appears at least twice. Return `false` if all elements are distinct.

Constraints:
```
1 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9
```

---

## 🧩 What Puzzled Me

I considered using extra space to track elements that had already been processed.

1. HashMap
2. HashSet
3. Array

I also wanted to investigate whether this problem could be solved using **O(1) space complexity**.

---

## 🔎 First Instinct — Brute Force

What was the first straightforward solution I thought of?

### Investigation

1. Use a nested loop to compare elements.
2. For each element, check every subsequent element for a match.
3. If a duplicate is found, return `true`.
4. If no duplicates are found after all comparisons, return `false`.

### Complexity

**Time:** `O(n^2)`  
Why?: In the worst case, nested loops compare elements repeatedly.

**Space:** `O(1)`  
Why?: No additional data structure is used.

---

## 🔍 Mini Investigation — Find the Bottleneck

What exactly makes the first approach inefficient?

### Observation

The solution performs repeated comparisons between elements, resulting in quadratic time complexity for large arrays.

### Direction

I identified two alternative approaches:

1. Sort the array and compare consecutive elements.
2. Use extra space to track previously encountered elements.

---

## 🧠 Enhanced Solution

### Steps

1. Sort the array in ascending order.
2. Iterate through the array starting from index `1`.
3. Compare each element with the previous element.
4. If both elements are equal, return `true`.
5. If no consecutive duplicates are found, return `false`.

### Complexity

**Time:** `O(n log n)`  
Why?: Sorting takes `O(n log n)` time, followed by a linear scan.

**Space:** `O(1)` auxiliary space, if the sorting implementation supports it.  
Why?: No additional data structure is required, although the actual space complexity depends on the sorting implementation. In Java, `Arrays.sort()` for primitive arrays may use additional stack space.

**Note:** Sorting modifies the original array.

---

## 🧠 Actual Solution

### Steps

1. Create a `HashSet`.
2. Iterate through the array.
3. If the current element already exists in the set, return `true`.
4. Otherwise, add the element to the set.
5. If the loop finishes without finding a duplicate, return `false`.

### Code

```java
import java.util.HashSet;

class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> set = new HashSet<>();

        for (int num : nums) {
            if (set.contains(num)) {
                return true;
            }
            set.add(num);
        }

        return false;
    }
}
```

### Complexity

**Time:** `O(n)` average case  
Why?: Each element is processed once, and `HashSet` operations take `O(1)` average time.

**Space:** `O(n)`  
Why?: In the worst case, all elements are distinct, so the set stores all `n` elements.

---

## ⚖️ Final Report

| Approach | Time | Space |
|----------|------|-------|
| Brute Force | O(n²) | O(1) |
| Sorting | O(n log n) | O(1) auxiliary space, depending on implementation |
| HashSet | O(n) average | O(n) |

---

## ⚠️ Mistakes to Remember

- Initially, I considered using an array to track previously encountered elements. However, the values can range from `-10^9` to `10^9`, so creating an array indexed by the values would be impractical and could cause memory issues.
- A `HashSet` is suitable because it stores only the elements encountered, rather than allocating space for the entire possible value range.
- If I need to preserve the original array, sorting may not be suitable because it modifies the array.
- The HashSet approach is optimal in average-case time complexity, but it requires `O(n)` extra space.


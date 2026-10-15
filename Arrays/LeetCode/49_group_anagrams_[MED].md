# 🕵️ Case: 49. Group Anagrams

**Platform:** LeetCode 
**Problem ID / Code:** 49  
**Difficulty:** Medium  
**Topics:** Arrays  
**Problem Link:** https://leetcode.com/problems/group-anagrams/description/

---

## Summary

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

Example 1:
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
Explanation:
There is no string in strs that can be rearranged to form "bat".
The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.
```

---

## 🔎 First Instinct — Brute Force

**Initial Idea:** Iterate over array, let suppose picked up the "eat" mark it as `grouped` (for this marking as grouped we can use boolean array of same length and when ever that string got added in any group will mark it as grouped) grouped[i]=true; then in inner loop again iterate over the array but start from `j = i+1; j<n; j++` index then check (isNotGrouped && isAnagram(strs[i], strs[j])) if this condition satisfies then mark that `j` indexed element as "grouped" by simply making grouped[j] = true.
When the outer loop goes to next element then it will first check is this grouped if yes then  `continue` loop go for next element and if not then contnue checking the isAnagram. 


### Code

```java
// Brute-force solution
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> ans = new ArrayList<>();
        int n = strs.length;
        boolean[] visited = new boolean[n]; // Tracks strings that are already grouped

        for (int i = 0; i < n; i++) {
            if (visited[i]) continue; // Skip if already grouped

            List<String> group = new ArrayList<>();
            group.add(strs[i]);
            visited[i] = true;

            // Compare with all remaining strings
            for (int j = i + 1; j < n; j++) {
                if (!visited[j] && isAnagram(strs[i], strs[j])) {
                    group.add(strs[j]);
                    visited[j] = true; // Mark as grouped
                }
            }
            ans.add(group);
        }
        return ans;
    }

    // Helper method to check if two strings are anagrams
    private boolean isAnagram(String s1, String s2) {
        if (s1.length() != s2.length()) return false;
        
        char[] c1 = s1.toCharArray();
        char[] c2 = s2.toCharArray();
        Arrays.sort(c1);
        Arrays.sort(c2);
        
        return Arrays.equals(c1, c2);
    }
}
```

### Complexity

**Time:** `O(n^2.klogk)` — Nested loop and inner loop checking isAnagram by sorting the string.

**Space:** `O(n)` — to track the grouped or not.

---

## 🧠 Better Solution

**Improved Idea:** Instead of comparing each element with other elements using isAnagram, we simply create one map. This map will store the anagram string list.

### Steps

1. Create a Map<String, List<String>> and iterate over the input array. For each string (e.g., "eat"), sort its characters to generate a uniform key (e.g., "aet"). Check if the map contains this key: if yes, append the original string to that key's list; if not, initialize a new list for the key first.
2. Convert the map values directly into a new list (new ArrayList<>(map.values())). This bypasses the need to run an explicit manual loop to transfer the groups into the final List<List<String>> structure.
3. Return the final list which now contains all the grouped anagram arrays.

### Code

```java
// Better solution
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        for(String s: strs){
            char[] charArray = s.toCharArray();
            Arrays.sort(charArray);
            String sorted = new String(charArray);
            map.computeIfAbsent(sorted, k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```

### Complexity

**Time:** `O(n.klogk)` — Two seperate loop, one of them doing sorting.

**Space:** `O(n.k)` — HashMap (To store keys and value)

---

## 🧠 Final Solution — Character frequecy mapping

**Key Idea:** Group anagrams without sorting by counting how many times each letter appears (from 'a' to 'z') to create a lightweight, 26-character unique key.

### Steps

1. Count letter frequencies for each word using a char array (new char[26]) instead of an integer array to keep data structures lightweight.
2. Generate an invisible key string by converting that raw character array directly into a String (new String(count)), completely skipping heavy text formatting like commas or brackets.
3. Group and return instantly by inserting the original string into a HashMap using the raw 26-character layout as the lookup key.

### Code

```java
// Optimized solution
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<>();
        for (String s : strs) {
            char[] count = new char[26]; 
            for (int i = 0; i < s.length(); i++) {
                count[s.charAt(i) - 'a']++;
            }
            String key = new String(count);
            map.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```

### Complexity

**Time:** `O(n.k)` — Nested loop

**Space:** `O(n.k)` — HashMap 

---

## ⚖️ Final Report

| Approach | Time | Space |
|---|---|---|
| Brute Force | O(n^2.klogk) | O(n) |
| Better | O(n.klogk) | O(n.k) |
| Optimal | O(n.k) | O(n.k) |

### Why the Final Approach Wins

Instead of sorting each element we are calculating its frequecy and mapping it.



# 🕵️ Case: <Problem Name>

**Platform:** CodeChef  
**Problem ID / Code:** TYPWRL
**Difficulty:** Easy  
**Topics:** String  
**Problem Link:** https://www.codechef.com/problems/TYPWRL

---

## Summary

Given a srting(s) which is Alice is typing and give one another string(l) which shows the letters which are only typed by left hand. Another remaining characters which are not in string(l) will be typed by right hand.

Calculate the number of letters are getting types by same hand consecutively of string(s).

Ex:
```
s = "abcde"
l = "dac"
==> 2     ---- Explanation{(a=l, b=r, c=l, d=l, e=r), so the answer is two}
```

**CONSTRAINTS:**
- all chars are lower case

---

## 🧩 What Puzzled Me

I can solve it using O(n) space complexity and time complexity, but can i solve it using O(1) space complexity and O(n) time complexity.

---

## 🔎 First Instinct — Brute Force

1. Create three variables: lc->left hand count, rc->right hand count, ans->final answer (All are ZERO initially).
2. Apply nested loop external loop will iterate on string(s) and inner will iterate on string(l), let suppose got 'a' from string(s) then iterate on string(l)
3. if found in string(l) then increase count of left hand by one, keep ans modified as max(ans, lc) and make rc =0 
4. Resturn the ans at last.

### Complexity

**Time:** `O(n^2)`  
Why?: Nested loop.

**Space:** `O(1)`  
Why?: No extra space

---

## 🔍 Mini Investigation — Find the Bottleneck

What exactly makes the first approach inefficient?

### Bottleneck

Iterating over string(l) again and again, so think about
- Can I use hashMap, hashSet or Array to keep track of lehd hand typed keys?
- Can I solve it using O(n) time complexity.

---

## 🧠 Actual Solution — (Optimal solution)

### Steps

1. Create three variables: lc->left hand count, rc->right hand count, ans->final answer (All are ZERO initially).
2. Iterate over string(l) and save all left hand types keys in hashSet, hashMap or Array.
3. Iterate over string(s) and check if this char is typed by which hand.
4. if found in string(l) then increase count of left hand by one, keep ans modified as max(ans, lc) and make rc =0 
5. Resturn the ans at last.

### Code

```java
// Optimized solution
import java.util.*;
import java.lang.*;
import java.io.*;

class Codechef
{
	public static void main (String[] args) throws java.lang.Exception
	{
		// your code goes here
		Scanner sc = new Scanner(System.in);
		int t = sc.nextInt();
		while(t-->0){
		    int n = sc.nextInt();
		    int m = sc.nextInt();
		    sc.nextLine();
		    String s = sc.nextLine();
		    String l = sc.nextLine();
		    int ans = 0;
		    int lc = 0;
		    int rc = 0;
		    int[] isPresent = new int[256];
		    for(int i = 0; i<m;i++){
		        isPresent[l.charAt(i)] = 1;
		    }
		    for(int i = 0; i<n;i++){
		        if(isPresent[s.charAt(i)]==1){
		            lc++;
		            rc = 0;
		            ans = Math.max(lc, ans);
		        }else{
		            rc++;
		            lc = 0;
		            ans = Math.max(rc, ans);
		        }
		        
		    }
		    System.out.println(ans);
		}

	}
}

```

### Complexity

**Time:** `O(n)`  
Why?: Only traversing on string (No nested loop)

**Space:** `O(n)`  
Why?: Used extra space of array 256 length (Technically O(n) but only 256 size so we can consider O(1))

---

## ⚖️ Final Report

| Approach | Time | Space |
|----------|------|-------|
| Brute Force | O(n^2) | O(1) |
| Optimal | O(n) | O(n) OR O(1) |

---

## 🕵️ Recognition Clue

When I see a future problem with:

- I need to repeatedly check whether a character/value belongs to a known group.
- Searching that group directly would create a nested loop.
- The group of possible values is small and fixed.
- I only need to know whether something is present or not.

I should think about:

**Presence Array / HashSet for O(1) lookup**
And when the value range is small and known, I should prefer a fixed-size array over a HashSet when appropriate.

---

## 🧠 Case Summary

```text
Problem
   ↓
Need to find the longest consecutive sequence
of characters typed by the same hand
   ↓
First instinct
For every character in s,
search the complete string l
   ↓
Bottleneck
Repeatedly scanning l → O(n²)
   ↓
Observation
I only need to know whether a character
belongs to the left-hand character set
   ↓
Better idea
Store left-hand characters in a presence array
   ↓
Check each character of s in O(1)
   ↓
Track consecutive left/right counts
and maintain the maximum
   ↓
Final complexity
O(n) Time + O(1) Space
   ↓
Reusable recognition clue
Repeated membership checking
→ Presence Array / HashSet
```
---
title: "LeetCode Daily 07/09/2025 - 1304. Find N Unique Integers Sum up to Zero"
date: 2025-09-07
tags: [Daily Question, LeetCode, Array, Math, Easy, Algorithm]
categories: [Algorithm, Daily LeetCode]
---

## 📋 Problem: Construct an Array With n Unique Integers Summing to Zero

**Difficulty**: Easy  
**Topic**: Array, Math  
**Daily Question**: 7th September 2025

---

## 🎥 Video Explanation

{% youtube 8v1bnrWWjtw %}

_Watch the complete solution walkthrough on my YouTube channel!_

---

## 💡 Intuition

When we want `n` unique integers that sum to zero, the first idea is **symmetry**.  
Pairs like `(x, -x)` cancel each other out perfectly. So the solution is to build the array from opposite pairs, and if needed, include `0` as a neutral element.

## 🔍 Approach

- **If `n` is even**: take `n // 2` pairs of opposite numbers, e.g. `(1, -1), (2, -2), ...`.
- **If `n` is odd**: do the same for `n // 2` pairs and add a single `0` at the end.
- **The order does not matter**, since the problem does not require sorting.

This guarantees:

- ✅ **Uniqueness**: Each number appears exactly once
- ✅ **Correct count**: Exactly `n` elements
- ✅ **Sum equals zero**: Opposite pairs cancel out

## 📊 Complexity Analysis

- **Time complexity**: `O(n)` - We construct the result with one loop up to `n // 2`.
- **Space complexity**: `O(n)` - For storing the resulting array of size `n`.

## 💻 Solution

```python
class Solution:
    def sumZero(self, n: int) -> List[int]:
        res = []
        for i in range(1, n // 2 + 1):
            res.append(i)
            res.append(-i)
        if n % 2 == 1:
            res.append(0)
        return res
```

## 🧪 Example Walkthrough

**Example 1**: `n = 5`

- Pairs: `(1, -1), (2, -2)` → `[1, -1, 2, -2]`
- `n` is odd, add `0` → `[1, -1, 2, -2, 0]`
- Sum: `1 + (-1) + 2 + (-2) + 0 = 0` ✅

**Example 2**: `n = 4`

- Pairs: `(1, -1), (2, -2)` → `[1, -1, 2, -2]`
- Sum: `1 + (-1) + 2 + (-2) = 0` ✅

## 🎯 Key Takeaways

1. **Symmetry approach** is perfect for sum-to-zero problems
2. **Handle odd/even cases** separately for completeness
3. **Simple mathematics** often leads to elegant solutions
4. **Order independence** simplifies the implementation

---

## 🔗 Problem Link

[LeetCode 1304 - Find N Unique Integers Sum up to Zero](https://leetcode.com/problems/find-n-unique-integers-sum-up-to-zero/)

---

_Part of my daily LeetCode journey! Follow along for more algorithm solutions and explanations._ 🚀

---
title: "LeetCode Daily 08/09/2025 - 1317. Convert Integer to the Sum of Two No-Zero Integers"
date: 2025-09-08
tags: [Daily Question, LeetCode, Array, Math, Easy, Algorithm]
categories: [Algorithm, Daily LeetCode]
---

## Problem Link

[LeetCode 1317 - Convert Integer to the Sum of Two No-Zero Integers](https://leetcode.com/problems/convert-integer-to-the-sum-of-two-no-zero-integers/)

**Difficulty**: Easy  
**Topic**: Array, Math  
**Daily Question**: 8th September 2025

---

## Video Explanation

{% youtube 8v1bnrWWjtw %}

_Watch the complete solution walkthrough on my YouTube channel!_

---

## Intuition

We need two **positive** integers `a` and `b` such that `a + b = n` **and** neither `a` nor `b` contains the digit **`0`** anywhere in its **decimal representation**.

> Important clarification: "No-Zero integer" does **not** mean "the number is not equal to 0"; it means **no digit `'0'` is allowed in the number**.

Examples: `7`, `19`, `345` ✅; `10`, `101`, `203` ❌ (because they contain the digit `0`).

Given `2 ≤ n ≤ 10^4`, a simple linear scan over `a` (and thus `b = n - a`) is sufficient. As soon as we find a pair where **both** contain no `'0'`, we can return it (the problem guarantees at least one valid answer).

## Approach

- Loop `a` from `1` to `n - 1` (ensures both parts are positive).
- Let `b = n - a`.
- Check the "No-Zero" property by verifying `'0' not in str(a)` **and** `'0' not in str(b)`.
- Return the first valid pair `[a, b]` immediately (early exit).

**Why this works:**

- The constraints are small; the worst-case scan is at most `n-1` iterations.
- String containment check is straightforward and unambiguous for the "no digit `0`" requirement.
- A valid pair is guaranteed to exist, so the loop will terminate with a result.

## Complexity Analysis

- **Time complexity**: `O(n)` — we may try each `a` once until we find a valid pair.
- **Space complexity**: `O(1)` — constant extra space.

## Solution

```python
from typing import List

class Solution:
    def getNoZeroIntegers(self, n: int) -> List[int]:
        for a in range(1, n):
            b = n - a
            if '0' not in str(a) and '0' not in str(b):
                return [a, b]
```

## Example Walkthrough

**Example 1**: `n = 11`

- Try `a = 1`, `b = 10` → `'0' in str(10)` ❌
- Try `a = 2`, `b = 9` → Both contain no `'0'` ✅
- Return `[2, 9]`

**Example 2**: `n = 10000`

- Try `a = 1`, `b = 9999` → Both contain no `'0'` ✅
- Return `[1, 9999]`

## Key Takeaways

1. **String-based digit checking** is the most straightforward approach for "no digit 0" requirement
2. **Linear search** is sufficient given the small constraint bounds
3. **Early termination** optimizes performance when valid pair is found
4. **Problem guarantees** ensure a valid solution always exists

---

_Part of my daily LeetCode journey! Follow along for more algorithm solutions and explanations._

---
title: "LeetCode Daily 12/09/2025 - 3227. Vowels Game in a String"
date: 2025-09-12
tags: [Daily Question, LeetCode, Game Theory, String, Medium, Algorithm]
categories: [Algorithm, Daily LeetCode]
---

## Problem Link

[LeetCode 3227 - Vowels Game in a String](https://leetcode.com/problems/vowels-game-in-a-string/)

**Difficulty**: Medium  
**Topic**: Game Theory, String  
**Daily Question**: 12th September 2025

---

## Problem Recap

We are given a string `s`. Two players, Alice and Bob, take turns deleting substrings under these rules:

- **Alice's move**: she must delete a non-empty substring that contains an **odd** number of vowels.
  - Odd numbers are: 1, 3, 5, 7, ...
  - For example, substrings like `"a"` (1 vowel), `"iou"` (3 vowels), `"banana"` (3 vowels).
- **Bob's move**: he must delete a non-empty substring that contains an **even** number of vowels.
  - Even numbers are: 0, 2, 4, 6, ...
  - Important: **0 is even**, so Bob is allowed to remove consonant-only substrings like `"bbb"`.

The first player who cannot make a valid move **loses**.

## Intuition

The key insight is understanding what each player can and cannot do:

- Alice needs a substring with an **odd ≥ 1** number of vowels.
- If the string has **no vowels at all**, Alice cannot move. She loses instantly.

## Approach

### Step 1: Think about Alice

- Alice needs a substring with an **odd ≥ 1** number of vowels.
- If the string has **no vowels at all**, Alice cannot move. She loses instantly.

### Step 2: If there is at least one vowel

- Suppose the string has **at least one vowel**.
- Alice can always make her first move.
- What happens after?

**Example**: `s = "leetcoder"`

1. Alice can remove `"leetcod"` (this part has **3 vowels**, which is odd).  
   Remaining string: `"er"`.
2. Bob now plays. `"er"` has **1 vowel**, which is odd. Bob cannot take `"er"` (odd).  
   But he can take `"r"` (0 vowels → even ✅).  
   So Bob deletes `"r"`. Remaining string: `"e"`.
3. Alice's turn. `"e"` has **1 vowel**, which is odd. She deletes it.  
   String becomes empty.
4. Bob's turn. Empty string, no moves. Bob loses. ✅ Alice wins.

### Step 3: Why Bob Cannot Stop Alice

- If there is at least **1 vowel**, Alice can always force the game toward a situation where exactly **1 vowel remains**.
- Bob can remove consonant-only pieces (0 vowels, even).
- But Bob **cannot remove that last vowel**, because:
  - Any substring containing it has **1 vowel (odd)**.
  - Bob's rule requires **even vowels**.
- Eventually Bob runs out of consonants to remove, leaving Alice the last vowel. She deletes it and wins.

## Complexity Analysis

- **Time complexity**: `O(n)` - Single pass through the string to check for vowels.
- **Space complexity**: `O(1)` - Only using a constant set for vowel lookup.

## Solution

```python
class Solution:
    def doesAliceWin(self, s: str) -> bool:
        vowels = set("aeiou")
        for ch in s:
            if ch in vowels:
                return True   # Found at least one vowel → Alice wins
        return False          # No vowels at all → Alice loses
```

## Example Walkthrough

**Example 1**: `s = "leetcoder"`

- Contains vowels: 'e', 'e', 'o', 'e' ✅
- Alice can always make moves and force a win ✅
- Return `True`

**Example 2**: `s = "bbcd"`

- Contains no vowels ❌
- Alice cannot make any move on her first turn ❌
- Return `False`

## Key Takeaways

1. **Game theory simplification**: Complex game reduces to simple vowel existence check
2. **Strategic analysis**: Alice can always force victory if vowels exist
3. **Edge case handling**: Zero vowels means immediate Alice loss
4. **Optimal play assumption**: Both players play perfectly, leading to deterministic outcome

---

_Part of my daily LeetCode journey! Follow along for more algorithm solutions and explanations._

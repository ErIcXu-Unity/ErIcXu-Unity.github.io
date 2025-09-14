---
title: "LeetCode Daily 14/09/2025 - 966. Vowel Spellchecker"
date: 2025-09-14
tags: [Daily Question, LeetCode, String, Medium, Algorithm, Hash Table]
categories: [Algorithm, Daily LeetCode]
---

## Problem Link

[LeetCode 966 - Vowel Spellchecker](https://leetcode.com/problems/vowel-spellchecker/)

**Difficulty**: Medium  
**Topic**: String, Hash Table  
**Daily Question**: 14th September 2025

---

## Problem Recap

We need to implement a spellchecker that corrects query words based on a given wordlist. The spellchecker handles two types of spelling mistakes with specific precedence rules:

1. **Capitalization errors**: Case-insensitive matching
2. **Vowel errors**: Vowels ('a', 'e', 'i', 'o', 'u') can be interchanged

**Precedence Rules** (in order):

1. **Exact match** (case-sensitive) → return the same word
2. **Capitalization match** → return first match in wordlist
3. **Vowel error match** → return first match in wordlist
4. **No match** → return empty string

## Intuition

This problem is essentially about **efficient string matching** with different levels of tolerance. The key insight is to preprocess the wordlist into different data structures to enable fast lookups:

- **Exact matching**: Use a set for O(1) lookups
- **Case-insensitive matching**: Map lowercase versions to first occurrence
- **Vowel-insensitive matching**: Map "devoweled" versions to first occurrence

The tricky part is handling the **precedence correctly** and ensuring we return the **first match** from the wordlist when multiple candidates exist.

## Approach

### Step 1: Preprocess the Wordlist

We create three data structures:

1. **`exact`**: Set containing all words as-is for exact matching
2. **`case_map`**: Maps lowercase words to their first occurrence in wordlist
3. **`vowel_map`**: Maps devoweled words to their first occurrence in wordlist

### Step 2: Devowel Function

Create a helper function that:

- Converts word to lowercase
- Replaces all vowels with a placeholder (e.g., '\*')
- This normalizes words for vowel-error matching

### Step 3: Query Processing

For each query, check in **precedence order**:

1. Is it in `exact` set? → return as-is
2. Is its lowercase version in `case_map`? → return mapped value
3. Is its devoweled version in `vowel_map`? → return mapped value
4. Otherwise → return empty string

### Step 4: Handle First Occurrence

When building `case_map` and `vowel_map`, use `setdefault()` to ensure only the **first occurrence** from wordlist is stored.

## Complexity Analysis

- **Time complexity**:
  - Preprocessing: `O(W * L)` where W = wordlist length, L = average word length
  - Query processing: `O(Q * L)` where Q = queries length
  - Total: `O((W + Q) * L)`
- **Space complexity**: `O(W * L)` - for storing the three data structures

## Solution

```python
class Solution:
    def spellchecker(self, wordlist: List[str], queries: List[str]) -> List[str]:
        vowels = set("aeiou")

        def devowel(word: str) -> str:
            return "".join('*' if c in vowels else c for c in word.lower())

        # Build data structures
        exact = set(wordlist)
        case_map = {}
        vowel_map = {}

        for w in wordlist:
            wl = w.lower()
            case_map.setdefault(wl, w)          # First occurrence only
            vowel_map.setdefault(devowel(w), w) # First occurrence only

        ans = []
        for q in queries:
            if q in exact:
                ans.append(q)
            elif q.lower() in case_map:
                ans.append(case_map[q.lower()])
            elif devowel(q) in vowel_map:
                ans.append(vowel_map[devowel(q)])
            else:
                ans.append("")

        return ans
```

## Example Walkthrough

**Input**: `wordlist = ["KiTe","kite","hare","Hare"]`, `queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]`

### Preprocessing:

**`exact`**: `{"KiTe", "kite", "hare", "Hare"}`

**`case_map`**:

- `"kite"` → `"KiTe"` (first occurrence)
- `"hare"` → `"hare"` (first occurrence)

**`vowel_map`**:

- `"k*t*"` → `"KiTe"` (first occurrence)
- `"h*r*"` → `"hare"` (first occurrence)

### Query Processing:

1. `"kite"` → **exact match** ✅ → `"kite"`
2. `"Kite"` → case match (`"kite"` in case_map) → `"KiTe"`
3. `"KiTe"` → **exact match** ✅ → `"KiTe"`
4. `"Hare"` → **exact match** ✅ → `"Hare"`
5. `"HARE"` → case match (`"hare"` in case_map) → `"hare"`
6. `"Hear"` → no exact, no case, devowel(`"Hear"`) = `"h**r"` ≠ `"h*r*"` → `""`
7. `"hear"` → no exact, no case, devowel(`"hear"`) = `"h**r"` ≠ `"h*r*"` → `""`
8. `"keti"` → vowel match (devowel(`"keti"`) = `"k*t*"`) → `"KiTe"`
9. `"keet"` → no match (devowel(`"keet"`) = `"k**t"` ≠ `"k*t*"`) → `""`
10. `"keto"` → vowel match (devowel(`"keto"`) = `"k*t*"`) → `"KiTe"`

**Output**: `["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]`

## Key Takeaways

1. **Precedence matters**: Always check exact → case → vowel → none in that order
2. **First occurrence rule**: Use `setdefault()` to capture only the first match from wordlist
3. **Efficient preprocessing**: Build lookup structures once, query many times
4. **Vowel normalization**: Replace vowels with consistent placeholder for matching
5. **String manipulation**: Careful handling of case conversion and character replacement

This problem demonstrates the importance of **strategic preprocessing** and **clear precedence handling** in string matching algorithms.

---

_Part of my daily LeetCode journey! Follow along for more algorithm solutions and explanations._

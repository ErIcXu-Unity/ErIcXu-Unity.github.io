---
title: "LeetCode Daily 24/09/2025 - 166. Fraction to Recurring Decimal"
date: 2025-09-24
tags: [Daily Question, LeetCode, Hash Table, Math, String, Medium, Algorithm]
categories: [Algorithm, Daily LeetCode]
---

## Problem Link

[LeetCode 166 - Fraction to Recurring Decimal](https://leetcode.com/problems/fraction-to-recurring-decimal/)

**Difficulty**: Medium  
**Topic**: Hash Table, Math, String  
**Daily Question**: 24th September 2025

---

## Problem Recap

Given two integers representing the numerator and denominator of a fraction, we need to return the fraction in string format. The key challenge is handling **recurring decimal patterns** properly.

**Requirements**:

1. If the fractional part repeats, enclose the repeating part in **parentheses**
2. Handle **negative signs** correctly (only when numerator and denominator have different signs)
3. Return **integer part only** if there's no fractional remainder
4. The answer string length is guaranteed to be less than 10⁴

**Examples**:

- `1/2 = "0.5"` (terminating decimal)
- `4/333 = "0.(012)"` (recurring decimal)
- `2/1 = "2"` (integer result)

## Intuition

This problem is fundamentally about **long division simulation**. The key insight is that **recurring decimals occur when we encounter the same remainder twice** during division.

Think about manual long division:

- We divide, get a quotient and remainder
- Multiply remainder by 10, divide again
- **If we see the same remainder again, we've found our cycle!**

The challenge lies in:

1. **Detecting cycles**: Use hash table to track remainder positions
2. **Handling signs**: XOR to determine if result should be negative
3. **Building result**: Carefully construct string with parentheses at correct positions

## Approach

### Step 1: Handle Edge Cases and Signs

- If numerator is 0, return "0"
- Calculate sign using XOR: negative if exactly one of numerator/denominator is negative
- Work with absolute values to simplify logic

### Step 2: Integer Division

- Calculate integer part: `n // d`
- Get initial remainder: `n % d`
- If remainder is 0, return integer result (no fractional part)

### Step 3: Fractional Part with Cycle Detection

**Key Data Structures**:

- `res`: List to build result string incrementally
- `seen`: Hash map tracking `{remainder: position_in_result}`

**Long Division Process**:

1. For each remainder, check if we've seen it before
2. If yes → **cycle detected!** Insert opening parenthesis at stored position, append closing parenthesis
3. If no → record current position, continue division
4. Multiply remainder by 10, divide by denominator
5. Append quotient digit, update remainder

### Step 4: Result Construction

- Use list for efficient string building
- Insert parentheses at correct positions when cycle is detected
- Join all parts into final string

## Complexity Analysis

- **Time complexity**: `O(d)` where d is the denominator
  - In worst case, we might see d-1 different remainders before finding a cycle
  - Each iteration involves constant time hash operations
- **Space complexity**: `O(d)` for the hash map storing remainder positions

## Solution

```python
class Solution:
    def fractionToDecimal(self, numerator: int, denominator: int) -> str:
        if numerator == 0:
            return "0"

        sign = "-" if (numerator < 0) ^ (denominator < 0) else ""
        n, d = abs(numerator), abs(denominator)

        integer = n // d
        rem = n % d
        if rem == 0:
            return sign + str(integer)

        res = [sign + str(integer), "."]
        seen = {}

        while rem != 0:
            if rem in seen:
                idx = seen[rem]
                res.insert(idx, "(")
                res.append(")")
                break
            seen[rem] = len(res)
            rem *= 10
            res.append(str(rem // d))
            rem %= d

        return "".join(res)
```

## Example Walkthrough

**Input**: `numerator = 4, denominator = 333`

### Step 1: Handle Signs and Integer Part

- Both positive → `sign = ""`
- `n = 4, d = 333`
- `integer = 4 // 333 = 0`
- `rem = 4 % 333 = 4`
- Since `rem ≠ 0`, we have fractional part
- `res = ["0", "."]`

### Step 2: Long Division Process

**Iteration 1**: `rem = 4`

- `4` not in `seen` → `seen[4] = 2` (position after decimal point)
- `rem = 4 * 10 = 40`
- `40 // 333 = 0` → append `"0"`
- `rem = 40 % 333 = 40`
- `res = ["0", ".", "0"]`

**Iteration 2**: `rem = 40`

- `40` not in `seen` → `seen[40] = 3`
- `rem = 40 * 10 = 400`
- `400 // 333 = 1` → append `"1"`
- `rem = 400 % 333 = 67`
- `res = ["0", ".", "0", "1"]`

**Iteration 3**: `rem = 67`

- `67` not in `seen` → `seen[67] = 4`
- `rem = 67 * 10 = 670`
- `670 // 333 = 2` → append `"2"`
- `rem = 670 % 333 = 4`
- `res = ["0", ".", "0", "1", "2"]`

**Iteration 4**: `rem = 4`

- `4` **already in `seen`** at position `2`! 🎯
- **Cycle detected!** Insert `"("` at position `2`, append `")"`
- `res = ["0", ".", "(", "0", "1", "2", ")"]`

**Result**: `"0.(012)"`

## Key Takeaways

1. **Long division simulation**: The core algorithm mimics manual long division process
2. **Cycle detection with hash map**: Track remainder positions to identify when repetition begins
3. **Sign handling**: Use XOR for elegant negative sign determination
4. **String building strategy**: Use list for efficiency, insert parentheses when cycle found
5. **Edge case handling**: Zero numerator and integer-only results need special treatment
6. **Mathematical insight**: Recurring decimals are inevitable when denominators have prime factors other than 2 and 5

This problem beautifully combines **mathematical concepts** with **algorithmic techniques**, demonstrating how hash tables can solve seemingly complex mathematical problems efficiently.

---

_Part of my daily LeetCode journey! Follow along for more algorithm solutions and explanations._

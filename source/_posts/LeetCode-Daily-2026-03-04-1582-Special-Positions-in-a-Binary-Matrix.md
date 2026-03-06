---
title: "LeetCode Daily 04/03/2026 - 1582. Special Positions in a Binary Matrix"
date: 2026-03-04
tags: [Daily Question, LeetCode, Array, Hash Table, Matrix, Easy, Algorithm]
categories: [Algorithm, Daily LeetCode]
---

## Problem Link

[LeetCode 1582 - Special Positions in a Binary Matrix](https://leetcode.com/problems/special-positions-in-a-binary-matrix/)

**Difficulty**: Easy  
**Topic**: Array, Hash Table, Matrix  
**Daily Question**: 4th March 2026

---

## Problem Recap

Given an `m x n` binary matrix `mat` where each element is either 0 or 1, return the number of **special** positions in the matrix.

A position `(i, j)` is called **special** if:

1. `mat[i][j] == 1`
2. All other elements in row `i` are 0
3. All other elements in column `j` are 0

**Constraints**:

- `1 <= m, n <= 100`
- `mat[i][j]` is either 0 or 1

**Examples**:

- `mat = [[1,0,0],[0,0,1],[1,0,0]]` -> Output: `1` (only position (1,2) is special)
- `mat = [[1,0,0],[0,1,0],[0,0,1]]` -> Output: `3` (all diagonal positions are special)

## Intuition

A position is "special" if it is the **only 1 in its entire row AND its entire column**. This means if we count the number of 1s in each row and column, a position `(i, j)` is special when `mat[i][j] == 1`, `row_count[i] == 1`, and `col_count[j] == 1`.

Instead of checking every element in the row and column for each candidate (which would be O(m * n * (m + n))), we can **precompute** row and column sums in a single pass, then verify candidates in a second pass.

## Approach

### Step 1: Count Row and Column Sums

- Create two arrays: `row[i]` stores the total number of 1s in row `i`, `col[j]` stores the total number of 1s in column `j`
- Fill both arrays in a single O(m * n) traversal of the matrix

### Step 2: Identify Special Positions

- Iterate through the matrix again
- For each cell where `mat[i][j] == 1`, check if `row[i] == 1` and `col[j] == 1`
- If both conditions hold, this position is special -- increment the count

This guarantees:

- ✅ **Correctness**: `row[i] == 1` means (i, j) is the only 1 in its row
- ✅ **Correctness**: `col[j] == 1` means (i, j) is the only 1 in its column
- ✅ **Efficiency**: Two simple passes over the matrix

## Complexity Analysis

- **Time complexity**: `O(m * n)` - Two passes through the matrix, each taking O(m * n).
- **Space complexity**: `O(m + n)` - For storing the row and column count arrays.

## Solution

```python
class Solution:
    def numSpecial(self, mat: List[List[int]]) -> int:
        m, n = len(mat), len(mat[0])
        row = [0] * m
        col = [0] * n

        for i in range(m):
            for j in range(n):
                if mat[i][j] == 1:
                    row[i] += 1
                    col[j] += 1
        count = 0

        for i in range(m):
            for j in range(n):
                if mat[i][j] == 1 and row[i] == 1 and col[j] == 1:
                    count += 1
        return count
```

## Example Walkthrough

**Example 1**: `mat = [[1,0,0],[0,0,1],[1,0,0]]`

### Step 1: Count Row and Column Sums

- `row = [1, 1, 1]` (each row has exactly one 1)
- `col = [2, 0, 1]` (column 0 has two 1s, column 2 has one 1)

### Step 2: Check Each 1

- Position (0, 0): `mat[0][0] = 1`, `row[0] = 1` ✅, `col[0] = 2` (not 1, two 1s in column 0)
- Position (1, 2): `mat[1][2] = 1`, `row[1] = 1` ✅, `col[2] = 1` ✅ -> **Special!**
- Position (2, 0): `mat[2][0] = 1`, `row[2] = 1` ✅, `col[0] = 2` (not 1)

**Result**: `1`

**Example 2**: `mat = [[1,0,0],[0,1,0],[0,0,1]]`

### Step 1: Count Row and Column Sums

- `row = [1, 1, 1]`
- `col = [1, 1, 1]`

### Step 2: Check Each 1

- Position (0, 0): `row[0] = 1` ✅, `col[0] = 1` ✅ -> **Special!**
- Position (1, 1): `row[1] = 1` ✅, `col[1] = 1` ✅ -> **Special!**
- Position (2, 2): `row[2] = 1` ✅, `col[2] = 1` ✅ -> **Special!**

**Result**: `3`

## Key Takeaways

1. **Precomputation strategy**: Counting row/column sums first avoids redundant checks
2. **Two-pass approach**: First pass collects data, second pass uses it for decisions
3. **Simple condition check**: A special position is simply a 1 with unique row and column counts
4. **Space-time trade-off**: Using O(m + n) extra space reduces time from O(m * n * (m + n)) to O(m * n)

---

_Part of my daily LeetCode journey! Follow along for more algorithm solutions and explanations._

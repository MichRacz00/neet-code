# NeetCode 150

Simple overview of use/purpose.

## Two Pointers

### Two Integer Sum II

**Problem:** given a sorted integer array `numbers` return a 1-indexed array `[index1, index2]` such that the values of the two indices sum up to `target`.

```python
def twoSum(numbers: List[int], target: int) -> List[int]:
    l, r = 0, len(numbers) - 1

    while l < r:
        curSum = numbers[l] + numbers[r]

        if curSum == target:
            return [l + 1, r + 1]

        if curSum > target:
            r -= 1

        if curSum < target:
            l -= 1
```

## Backtracking

### Subsets

**Problem:** given a list of unique integers `nums`, generate all possible subsets of `nums`.

#### Implementation

```python
def subsets(nums: List[int]) -> List[List[int]]:
    res = []
    subset = []

    def backtrack(i):
        if i >= len(nums):
            res.append(subset.copy())

        subset.append(nums[i])
        backtrack(i + 1)

        subset.pop()
        backtrack(i + 1)

    backtrack(0)
    return res
```

#### Call Graph

```text
subsets([1, 2]) → backtrack(0)
│
├── include 1 → [1] → backtrack(1)
│   │
│   ├── include 2 → [1, 2] → backtrack(2) ✅ add [1,2]
│   │
│   └── exclude 2 → [1] → backtrack(2) ✅ add [1]
│
└── exclude 1 → [] → backtrack(1)
    │
    ├── include 2 → [2] → backtrack(2) ✅ add [2]
    │
    └── exclude 2 → [] → backtrack(2) ✅ add []
```

### Combination Sum
Given an array of distinct integers `nums` and a target integer `target`, your task is to return a list of all unique combinations of `nums` where the chosen numbers sum to `target`.

```python
def combinationSum(nums: List[int], target: int) -> List[List[int]]:
    res = []

    def backtrack(i, cur, total):
        if total == target:
            res.append(cur.copy())
            return

        if i >= len(nums) or total > target:
            return

        cur.append(nums[i])
        backtrack(i, cur, total + nums[i])

        cur.pop()
        backtrack(i + 1, cur, total)

    backtrack(0, [], 0)
    return res
```

### Generate Parentheses

**Problem:** given `n` generate all unique, well-formed strings of parentheses.

#### Implementation

```python
def generateParentheses(n: int) -> str:
    res = []
    stack = []

    def backtrack(opened, closed):
        # Parentheses are well formed and of size n
        if opened == closed == n:
            res.append("".join(stack))
            return

        # New parentheses can still be opened
        if opened < n:
            stack.append("(")
            backtrack(opened + 1, closed)
            stack.pop()

        # There are parentheses to close
        if closed < opened:
            stack.append(")")
            backtrack(opened, closed + 1)
            stack.pop()

    backtrack(0, 0)
    return res
```

#### Call Graph

The algorithm performs DFS. At call, there is possibility to add both '(' and ')', depending on the value of `opened` and `closed`. The algorithm uses stack as the common mutable state, which is prerequisite for backtracking.

```text
backtrack(0, 0)
│
├── add '(' → backtrack(1, 0)
│   │
│   ├── add '(' → backtrack(2, 0)
│   │   │
│   │   └── add ')' → backtrack(2, 1)
│   │       │
│   │       └── add ')' → backtrack(2, 2) ✅ "()()"
│   │
│   └── add ')' → backtrack(1, 1)
│       │
│       └── add '(' → backtrack(2, 1)
│           │
│           └── add ')' → backtrack(2, 2) ✅ "(())"
│
└── no add ')' because closed == opened == 0
```

### Lorem Ipsum
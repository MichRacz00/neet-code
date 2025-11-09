# NeetCode 150

Simple overview of use/purpose.

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
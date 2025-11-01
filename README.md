# NeetCode 150

Simple overview of use/purpose.

## Backtracking

### Generate Parentheses

**Problem:** given `n` generate all unique, well-formed strings of parentheses.

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
└── (no add ')' because closed == opened == 0)
```

### Lorem Ipsum
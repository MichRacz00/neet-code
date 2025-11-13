# NeetCode 150

## Binary Search

TODO: add visual example on how pointers move

### Example Problems

#### Binary Search

**Problem:** you are given an array of distinct integers `nums`, sorted in ascending order, and an integer `target`. Implement a function to search for `target` within `nums`. If it exists, then return its index, otherwise, return `-1`.

```python
def search(self, nums: List[int], target: int) -> int:
    nums.sort()
    l, r = 0, len(nums) - 1

    while l <= r:
        m = (l + r) // 2

        if target < nums[m]:
            r = m - 1
        elif target > nums[m]:
            l = m + 1
        else:
            return m

    return -1
```

#### Search a 2D Matrix

**Problem:** you are given an `m x n` 2-D integer array `matrix` and an integer `target`. Each row in matrix is sorted in non-decreasing order. The first integer of every row is greater than the last integer of the previous row. Return `true` if target exists within matrix or `false` otherwise.

```python
def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
    ROW, COL = len(matrix) - 1, len(matrix[0]) - 1

    l, r = 0, ROW
    while l <= r:
        m = (l + r) // 2
        if matrix[m][0] <= target <= matrix[m][COL]:
            l, r = 0, COL
            while l <= r:
                k = (l + r) // 2

                if matrix[m][k] == target:
                    return True
                elif matrix[m][k] > target:
                    r = k - 1
                elif matrix[m][k] < target:
                    l = k + 1
            return False

        elif matrix[m][COL] < target:
            l = m + 1
        elif matrix[m][0] > target:
            r = m - 1
    return False
```

#### Koko Eating Bananas

**Problem:** You are given an integer array `piles` where `piles[i]` is the number of bananas in the `ith` pile. You are also given an integer `h`, which represents the number of hours you have to eat all the bananas. You may decide your bananas-per-hour eating rate of `k`. Each hour, you may choose a pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, you may finish eating the pile but you can not eat from another pile in the same hour. Return the minimum integer `k` such that you can eat all the bananas within `h` hours.

💡 **Insight:** binary search is not limited to searching for an index in a sorted array; it can also be used to find a minimum (or maximum) value that satisfies a certain **monotonic condition**.

```python
def minEatingSpeed(self, piles: List[int], h: int) -> int:
    l, r = 1, max(piles)
    res = r

    while l <= r:
        k = (l + r) // 2
        total = 0

        for p in piles:
            total += math.ceil(p / k)

        if total <= h:
            r = k - 1
            res = min(res, k)
        else:
            l = k + 1

    return res
```

## Two Pointers

### 🧭 Recognizing Two-Pointer Problems

Two-pointer problems are common in LeetCode and other coding interviews. They involve using two indices (or “pointers”) that move through a sequence (like an array or string) to efficiently find a result without nested loops.

#### 🔍 When to Suspect a Two-Pointer Problem

You can usually recognize two-pointer problems when **one or more** of these clues appear:

| Pattern | Typical Input | Typical Goal | Common Clues |
|----------|----------------|---------------|---------------|
| **Opposite Ends (Converging Pointers)** | Sorted arrays, strings | Compare or find pairs from both ends | “sorted array”, “pair sums to target”, “palindrome”, “container” |
| **Sliding Window (Expanding / Contracting)** | Arrays, strings | Find/maximize/minimize a subarray or substring | “subarray”, “substring”, “at most k”, “longest/shortest” |
| **Same Direction (Moving Window)** | Strings, arrays | Maintain valid range or count | “longest substring without repeating characters”, “subarray sum” |
| **Partitioning / Merging** | Sorted arrays or linked lists | Split or merge data efficiently | “merge”, “partition”, “combine sorted arrays” |
| **Meet in the Middle** | Arrays, sorted inputs | Two sides moving toward a target condition | “sum closest to target”, “difference equals k”, “3Sum”, “4Sum” |

#### 🧠 How to Identify Them in the Wild

Look for problems that mention or imply:

- **Sorted input**  
  Sorting makes pointer movement predictable.  
  _Example: `Two Sum II`, `3Sum`, `Container With Most Water`._

- **Comparing elements at different positions**  
  Usually means moving inward from both ends or maintaining two markers.  
  _Example: `Valid Palindrome`._

- **Finding pairs, triplets, or windows**  
  Often requires adjusting two (or more) pointers dynamically.  
  _Example: `3Sum`, `Subarray Product < K`._

- **Returning indices or subarrays**  
  Commonly solved by pointer-based traversal rather than brute force.

### Example Problems

#### Valid Palindrome

**Problem:** given a string `s`, return `true` if it is a palindrome, otherwise return `false`.

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:
            if not self.isAlphanum(s[l]):
                l += 1
                continue
            if not self.isAlphanum(s[r]):
                r -= 1
                continue
            
            if s[l].lower() != s[r].lower():
                return False
            
            l, r = l + 1, r - 1
        
        return True

    def isAlphanum(self, c):
        return (ord('A') <= ord(c) <= ord('Z') or
            ord('a') <= ord(c) <= ord('z') or
            ord('0') <= ord(c) <= ord('9'))
```

#### Two Integer Sum II

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

#### 3Sum

**Problem:** given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.

```python
def threeSum(nums: List[int]) -> List[List[int]]:
    res = []
    nums.sort() # fits within n^2

    for i, n in enumerate(nums):
        total = n
        l, r = i + 1, len(nums) - 1

        while l < r:
            curSum = total + nums[l] + nums[r]

            # Skip duplicate numbers for index i
            if i > 0 and n == nums[i - 1]:
                continue

            if curSum > 0:
                r -= 1
            elif curSum < 0:
                l += 1
            else:
                res.append([n, nums[l], nums[r]])
                l += 1

                # Skip duplicate numbers under pointers.
                # Only need to move one pointer, as the 
                # sum will evaluate accrodingly.
                while l < r and nums[l] == nums[l - 1]:
                    l += 1

    return res
```

#### Container With Most Water

**Problem:** you are given an integer array `heights` where `heights[i]` represents the height of the ith bar. You may choose any two bars to form a container. Return the maximum amount of water a container can store.

```python
def maxArea(heights: List[int]) -> int:
    l, r = 0, len(heights) - 1
    volume = 0

    for i in range(len(heights)):
        curVol = min(heights[l], heights[r]) * (r - l)
        volume = max(volume, curVol)

        if heights[l] > heights[r]:
            r -= 1
        else:
            l += 1

    return volume
```

#### Trapping Rain Water

**Problem:** you are given an array of non-negative integers `height` which represent an elevation map. Each value `height[i]` represents the height of a bar, which has a width of `1`. Return the maximum area of water that can be trapped between the bars.

```python
def trap(height: List[int]) -> int:
    n = len(height)
    maxHeightLeft = []
    maxHeightRight = [0] * n

    maxVolLeft = 0
    for i in range(n):
        maxVolLeft = max(maxVolLeft, height[i])
        maxHeightLeft.append(maxVolLeft)

    maxVolRight = 0
    for i in range(n - 1, -1, -1):
        maxVolRight = max(maxVolRight, height[i])
        maxHeightRight[i] = maxVolRight

    totalVol = 0
    for i in range(n):
        totalVol += min(maxHeightRight[i], maxHeightLeft[i]) - height[i]

    return totalVol
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
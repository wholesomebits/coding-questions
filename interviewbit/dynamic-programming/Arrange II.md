# Arrange II
[https://www.interviewbit.com/problems/arrange-ii/](https://www.interviewbit.com/problems/arrange-ii/)

## Explanation
_TBD_

## Recursive Relation
_TBD_

## Top-down
### Complexity
- Time: _TBD_
- Space: <img src="https://render.githubusercontent.com/render/math?math=\mathcal{O}(n)">
```python
class Solution:
    def arrange(self, horses: str, k: int) -> int:
        n = len(horses)
        if n < k:
            return -1

        return self._helper(horses, k, n)

    def _helper(self, horses: str, k: int, n: int) -> int:
        if n == 0 or k == 0:
            return 0

        ans = float('inf')
        min_size = 1 if k > 1 else n
        max_size = n - (k - 1)
        for size in range(min_size, max_size + 1):
            black, white = self._count(horses, n - size, n)
            ans = min(ans, black * white + self._helper(horses, k - 1, n - size))

        return ans

    @staticmethod
    def _count(nums, lo, hi):
        black = 0
        for i in range(lo, hi):
            black += 1 if nums[i] == 'B' else 0

        white = hi - lo - black
        return black, white
```

## Top-down with memoization
### Complexity
- Time:  _TBD_
- Space: _TBD_

```python
class Solution:
    def arrange(self, horses: str, k: int) -> int:
        n = len(horses)
        if n < k:
            return -1

        memo = [[-1] * (k + 1) for _ in range(n + 1)]
        return self._helper(horses, k, n, memo)

    def _helper(self, horses: str, k: int, n: int, memo) -> int:
        if n == 0 or k == 0:
            return 0

        if memo[n][k] != -1:
            return memo[n][k]

        ans = float('inf')
        min_size = 1 if k > 1 else n
        max_size = n - (k - 1)
        for size in range(min_size, max_size + 1):
            black, white = self._count(horses, n - size, n)
            ans = min(ans, black * white + self._helper(horses, k - 1, n - size, memo))

        memo[n][k] = ans
        return ans

    @staticmethod
    def _count(nums, lo, hi):
        black = 0
        for i in range(lo, hi):
            black += 1 if nums[i] == 'B' else 0

        white = hi - lo - black
        return black, white
```

## Bottom-up
### Complexity
- Time:  <img src="https://render.githubusercontent.com/render/math?math=\mathcal{O}(k \times n^2)">
- Space: <img src="https://render.githubusercontent.com/render/math?math=\mathcal{O}(k \times n)">

```python
class Solution:
    def arrange(self, horses: str, k: int) -> int:
        n = len(horses)
        if n < k:
            return -1

        dp = [[0] * (n + 1) for _ in range(k + 1)]
        for i in range(1, k + 1):
            for j in range(1, n + 1):
                ans = float('inf')
                min_size = 1 if i > 1 else j
                max_size = j - (i - 1)
                for size in range(min_size, max_size + 1):
                    black, white = self._count(horses, j - size, j)
                    ans = min(ans, black * white + dp[i - 1][j - size])

                dp[i][j] = ans

        return dp[k][n]

    @staticmethod
    def _count(nums, lo, hi):
        black = 0
        for i in range(lo, hi):
            black += 1 if nums[i] == 'B' else 0

        white = hi - lo - black
        return black, white
```

## Bottom-up (optimized space)
### Complexity
- Time:  <img src="https://render.githubusercontent.com/render/math?math=\mathcal{O}(k \times n^2)">
- Space: <img src="https://render.githubusercontent.com/render/math?math=\mathcal{O}(n)">

```python
class Solution:
    def arrange(self, horses: str, k: int) -> int:
        n = len(horses)
        if n < k:
            return -1

        dp = [0] * (n + 1)
        for i in range(1, k + 1):
            temp = [0] * (n + 1)
            for j in range(1, n + 1):
                ans = float('inf')
                min_size = 1 if i > 1 else j
                max_size = j - (i - 1)
                for size in range(min_size, max_size + 1):
                    black, white = self._count(horses, j - size, j)
                    ans = min(ans, black * white + dp[j - size])

                temp[j] = ans
            dp = temp

        return dp[n]

    @staticmethod
    def _count(nums, lo, hi):
        black = 0
        for i in range(lo, hi):
            black += 1 if nums[i] == 'B' else 0

        white = hi - lo - black
        return black, white
```

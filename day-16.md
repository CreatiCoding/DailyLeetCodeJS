# [Medium] 64. Minimum Path Sum

https://leetcode.com/problems/minimum-path-sum/

<details>
<summary>문제내용보기</summary>

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

#### 예시 1:

![](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

#### 예시 2:

```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

#### Constraints:

- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 200
- 0 <= grid[i][j] <= 100

</details>

### review

왼쪽 최상단에서 오른쪽 최하단 까지 이동하면서 가장 최소 합을 구하면 됩니다. 이도 결국 누적해서 계산해주면 됩니다.
전날에 했던 내용과 거의 유사합니다.

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minPathSum = function (grid) {
  let dp = [];
  for (let i = 0; i < grid[0].length; i++) {
    if (!i) dp.push(grid[0][i]);
    else dp.push(dp[i - 1] + grid[0][i]);
  }
  for (let i = 1; i < grid.length; i++) {
    const row = grid[i];
    dp[0] = dp[0] + row[0];
    for (let j = 1; j < row.length; j++) {
      if (dp[j - 1] + row[j] < dp[j] + row[j]) {
        dp[j] = dp[j - 1] + row[j];
      } else {
        dp[j] = dp[j] + row[j];
      }
    }
  }
  return dp.pop();
};
```

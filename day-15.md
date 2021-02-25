# [Medium] 62. Unique Paths

https://leetcode.com/problems/unique-paths/

<details>
<summary>문제내용보기</summary>

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

#### 예시 1:

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
Input: m = 3, n = 7
Output: 28
```

#### 예시 2:

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

#### 예시 3:

```
Input: m = 7, n = 3
Output: 28
```

#### 예시 4:

```
Input: m = 3, n = 3
Output: 6
```

#### Constraints:

- 1 <= m, n <= 100
- It's guaranteed that the answer will be less than or equal to 2 \* 109.

</details>

### review

어렸을 적, 최단경로 구하는 문제 푸는 방법을 적용시키면 됩니다. 이를 DP와 활용한건데, 하나의 행을 list라고 보고
step을 진행할 때마다, 그 밑 행으로 이동하면서 최종 목적지까지 가면 됩니다.

```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
var uniquePaths = function (m, n) {
  if (m === 1 && n === 1) return 1;
  const big = m > n ? m : n;
  const small = m > n ? n : m;

  let dp = new Array(small - 1).fill(1);
  for (let i = 1; i < big; i++) {
    dp[0] = 1 + dp[0];
    for (let j = 1; j < small - 1; j++) {
      dp[j] = dp[j] + dp[j - 1];
    }
  }
  return dp.pop();
};
```

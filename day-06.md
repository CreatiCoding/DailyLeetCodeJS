# [Medium] 113. Path Sum II

https://leetcode.com/problems/path-sum-ii/

<details>
<summary>문제내용보기</summary>

### 이진 트리의 `root`와 정수  `targetSum`이 주어지면, 각 경로의 합이 `targetSum`과 같은 모든 루트 대 리프 경로를 반환합니다.

`leaf`은 아이가 없는 노드다.

#### 예시 1

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
```

#### 예시 2

![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: []
```

#### 예시 3

```
Input: root = [1,2], targetSum = 0
Output: []
```

Constraints:

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

- The number of nodes in `head` is in the range `[0, 2 * 104]`.
- `-10^5 <= Node.val <= 10^5`

</details>

### review

우선 leaf 노드를 찾아보자는 관점에서 시작했습니다. 일단, leaf를 찾고 나서부터는 부모노드를 누적해서 아래로 보내면 되기 때문에 문제는 어렵지 않게 풀 수 있었습니다.
결과를 반환하는 방법을 함수의 반환값을 재귀에 적용하지는 않았고, result라는 reference 변수 파라미터를 넘겨 그 곳에 추가하는 방법으로 했습니다.

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} targetSum
 * @return {number[][]}
 */
const sum = (arr) => arr.reduce((a, c) => a + c, 0);
const calc = (root = {}, targetSum, acc = [], result) => {
  if (root.left === null && root.right === null) {
    if (sum(acc) + root.val === targetSum) {
      result.push([...acc, root.val]);
    }
  }
  if (root.left) calc(root.left, targetSum, [...acc, root.val], result);
  if (root.right) calc(root.right, targetSum, [...acc, root.val], result);
  return result;
};
var pathSum = function (root, targetSum) {
  if (!root) return [];
  return calc(root, targetSum, [], []);
};
```

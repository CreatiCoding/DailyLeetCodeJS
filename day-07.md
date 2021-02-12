# [Medium] 114. Flatten Binary Tree to Linked List

https://leetcode.com/problems/flatten-binary-tree-to-linked-list/

<details>
<summary>문제내용보기</summary>

### 바이너리 트리의 루트가 주어지면 트리를 "연결된 리스트"으로 평평하게 만듭니다.

### "연결된 리스트"은 오른쪽 자식 포인터가 목록의 다음 노드를 가리키고 왼쪽 자식 포인터가 항상 null 인 동일한 TreeNode 클래스를 사용해야합니다.

### "연결된 리스트"은 바이너리 트리의 사전 주문 순회와 동일한 순서 여야합니다.

#### 예시 1

![](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

#### 예시 2

```
Input: root = []
Output: []
```

#### 예시 3

```
Input: root = [0]
Output: [0]
```

Constraints:

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

</details>

### review

해당 문제에는 함정이 있었습니다. 함수의 반환값이 void로, 주어진 root를 내부적으로 바꿔야 했습니다.
해당 함정을 이해하지 못하여 게속 return 했으나 의도했던 output이 나오지 않아 좀 헤맸습니다.
LEFT로 넘어오는 CASE를 모조리 올바른 노드의 형태로 변환했습니다.

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
const isLeaf = (node) => node && !node.left && !node.right;
const isLR = (node) =>
  node &&
  node.left &&
  node.right &&
  !node.left.left &&
  !node.left.right &&
  !node.right.left &&
  !node.right.right;
const isLL = (node) =>
  node && node.left && !node.right && node.left.left && !node.left.right;
const isL = (node) => node && node.left && !node.right;
/**
 * @param {TreeNode} root
 * @return {void} Do not return anything, modify root in-place instead.
 */
const check = (root) => {
  if (isLeaf(root)) return;
  if (isLR(root)) {
    root.left.right = root.right;
    root.right = root.left;
    root.left = null;
  } else if (isLL(root)) {
    root.right = root.left;
    root.left = null;
    root.right.right = root.right.left;
    root.right.left = null;
  } else if (root && root.left && root.right) {
    const temp = root.right;
    root.right = root.left;
    root.left = null;
    let cur = root;
    while (cur.right) cur = cur.right;
    cur.right = temp;
  } else if (isL(root)) {
    root.right = root.left;
    root.left = null;
  }
  if (root && root.left) check(root.left);
  if (root && root.right) check(root.right);
};
var flatten = function (root) {
  check(root);
};
```

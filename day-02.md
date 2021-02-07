# [Medium] 98. Validate Binary Search Tree

https://leetcode.com/problems/validate-binary-search-tree/

<details>
<summary>문제내용보기</summary>

#### 이진 트리의 루트가 주어지면 유효한 이진 검색 트리 (BST)인지 확인합니다.

#### 유효한 BST는 다음과 같이 정의됩니다.

#### 노드의 왼쪽 하위 트리에는 노드의 키보다 작은 키가있는 노드 만 포함됩니다.

#### 노드의 오른쪽 하위 트리에는 노드의 키보다 큰 키가있는 노드 만 포함됩니다.

#### 왼쪽 및 오른쪽 하위 트리도 모두 이진 검색 트리 여야합니다.

### Example 1:

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

### Example 2:

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

### Constraints:

- The number of nodes in the tree is in the range `[1, 104]`.
- `-2^31 <= Node.val <= 2^31 - 1`
</details>

### review

경계값 체크에 집중을 해야할 것 같다. 0도 가능한 입력값이고, 같은 숫자가 들어왔다면 올바른 BST가 아닌 결과를 내야하기 때문에 디테일한 조건이 중요하다.
또 min, max여야할 부모 node의 값들을 리스트로 넣었지만, 실은 가장 작은 max, 가장 큰 min만 넘기면 가능했던걸 이후에 알게 되었다.

## 풀이1

```javascript
const DFS = ({ tree, callback, minList = [], maxList = [] }) => {
  let result = [];
  if (tree) {
    result = callback(tree, minList, maxList);
    if (!result) return false;
  }
  if (tree.left) {
    result = DFS({
      tree: tree.left,
      callback,
      minList,
      maxList: [...maxList, tree.val],
    });
    if (!result) return false;
  }
  if (tree.right) {
    result = DFS({
      tree: tree.right,
      callback,
      minList: [...minList, tree.val],
      maxList,
    });
    if (!result) return false;
  }
  return true;
};
var isValidBST = function (root) {
  let result = DFS({
    tree: root,
    callback: (tree, minList, maxList) => {
      if (minList.filter((min) => min >= tree.val).length > 0) {
        return false;
      }
      if (maxList.filter((max) => max <= tree.val).length > 0) {
        return false;
      }
      return true;
    },
  });
  return result;
};
```

## 풀이2 [Best Practice]

```javascript
const check = (root, min = null, max = null) => {
  if (!root || root.val === null) return true;
  if (min !== null && root.val <= min) return false;
  if (max !== null && root.val >= max) return false;
  const { left: l, right: r, val: v } = root;
  return check(l, min, v) && check(r, v, max);
};

var isValidBST = function (root) {
  return check(root);
};
```

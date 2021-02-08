# [Medium] 105. Construct Binary Tree from Preorder and Inorder Traversal

https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

<details>
<summary>문제내용보기</summary>

#### 트리의 사전 순서 및 순서 순회가 주어지면 이진 트리를 구성합니다.

#### 참고 :

#### 트리에 중복이 존재하지 않는다고 가정 할 수 있습니다.

#### 예를 들어, 주어진

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

#### 다음 이진 트리를 반환합니다.

```
    3
   / \
  9  20
    /  \
   15   7
```

### review

`preorder`, `inorder`가 주어지면 Tree를 재구축하는 내용인데, 역으로 구현하려니 굉장히 까다로웠다.
`preorder`에서 주어진 데이터 역할은 가장 첫번째 요소는 계속 트리에 추가되는 역할
`inorder`에서 주어진 데이터의 역할은 추가된 데이터를 기점으로 left와 right로 나누는 역할이다.
이런 내용을 바탕으로 코드로 구현하면 아래와 같은 내용이 된다.

```javascript
/**
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
var add = (preorder, inorder) => {
  if (!preorder.length) return null;
  const root = new TreeNode(preorder[0]);
  const index = inorder.findIndex((e) => e === root.val);
  root.left = add(preorder.slice(1, index + 1), inorder.slice(0, index));
  root.right = add(preorder.slice(index + 1), inorder.slice(index + 1));
  return root;
};
var buildTree = function (preorder, inorder) {
  return add(preorder, inorder);
};
```

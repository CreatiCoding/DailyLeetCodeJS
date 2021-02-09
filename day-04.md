# [Medium] 106. Construct Binary Tree from Inorder and Postorder Traversal

https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/

<details>
<summary>문제내용보기</summary>

#### 트리의 순서 및 후 순서 순회가 주어지면 이진 트리를 구성합니다.

#### 참고 : 트리에 중복이 존재하지 않는다고 가정 할 수 있습니다.

#### 예를 들어, 주어진

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

#### 다음 이진 트리를 반환합니다.

```
    3
   / \
  9  20
    /  \
   15   7
```

</details>

### review

어제 풀었던 문제와 비슷한 내용이다. `postorder`, `inorder`가 주어지면 Tree를 재구축하는 내용인데,
`postorder`에서 주어진 데이터 역할은 가장 마지막 요소를 계속 트리에 추가하는 역할
`inorder`에서 주어진 데이터의 역할은 추가된 데이터를 기점으로 left와 right로 나누는 역할이다.
이런 내용을 바탕으로 코드로 구현하면 아래와 같은 내용이 된다.

어제와 오늘 풀게되면서 알게된 내용이 있는데, `inorder`에서 찾은 후,
`postorder`에서 `left`, `right`로 나뉜 만큼 `inorder`도 마찬가지로 나뉘어야 하는데
`left`, `right`로 넘어갈 때, 동일한 갯수가 넘어가야하기 때문에 결국 같은 `index`를 본다는 점이다.

```javascript
/**
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
var add = (inorder, postorder) => {
  if (!postorder.length) return null;
  const last = postorder.length - 1;
  const index = inorder.findIndex((e) => e === postorder[last]);
  return new TreeNode(
    postorder[last],
    add(inorder.slice(0, index), postorder.slice(0, index)),
    add(inorder.slice(index + 1), postorder.slice(index, last))
  );
};
var buildTree = function (inorder, postorder) {
  return add(inorder, postorder);
};
```

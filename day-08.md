# [Medium] 116. Populating Next Right Pointers in Each Node

https://leetcode.com/problems/flatten-binary-tree-to-linked-list/

<details>
<summary>문제내용보기</summary>

### 모든 잎이 같은 수준에 있고 모든 부모에게 두 자녀가있는 완벽한 이진 트리가 제공됩니다. 이진 트리에는 다음과 같은 정의가 있습니다.

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

다음 오른쪽 노드를 가리 키도록 각 다음 포인터를 채 웁니다. 다음 오른쪽 노드가 없으면 다음 포인터를 `NULL`로 설정해야합니다.

처음에는 모든 다음 포인터가 `NULL`로 설정됩니다.

후속 조치 :

```
일정한 추가 공간만 사용할 수 있습니다.
재귀적 접근 방식은 괜찮습니다. 암시적 스택 공간이 문제에 대한 추가 공간으로 간주되지 않는다고 가정 할 수 있습니다.
```

#### 예시 1

![](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

Constraints:

- The number of nodes in the given tree is less than 4096.
- `-100 <= Node.val <= 100`

</details>

### review

이번 문제에서는 반환값이 Node였지만, 일정한 추가 공간만 쓸 수 있다고 하여 새로운 변수에 넣는 방식으로는 하지 않았습니다.
주어진 Node 트리 변수에는 어차피 next 값이 null이기 때문에 그 부분만 값을 넣어주면 됩니다.
이번에는 각 재귀 함수별로 부모 노드와 지금 부모의 left인지, right인지, root인지만 알면 경우의 수에 따라 처리할 수 있는 알고리즘입니다.
따라서, 파라미터로 left, right 타입을 넣어주고, 부모 노드도 같이 넣어준 후, 각 경우의 수에 따라 처리해주었습니다.

```javascript
/**
 * // Definition for a Node.
 * function Node(val, left, right, next) {
 *    this.val = val === undefined ? null : val;
 *    this.left = left === undefined ? null : left;
 *    this.right = right === undefined ? null : right;
 *    this.next = next === undefined ? null : next;
 * };
 */
const calc = (root, type = "root", parent = null) => {
  if (root && root.val !== null) {
    if (type === "root") {
      root.next = null;
    } else if (type === "left") {
      root.next = parent.right;
    } else if (type === "right") {
      if (parent.next === null) {
        root.next = null;
      } else {
        root.next = parent.next.left;
      }
    }
  }
  if (root && root.left) calc(root.left, "left", root);
  if (root && root.right) calc(root.right, "right", root);
  return root;
};
/**
 * @param {Node} root
 * @return {Node}
 */
var connect = function (root) {
  return calc(root);
};
```

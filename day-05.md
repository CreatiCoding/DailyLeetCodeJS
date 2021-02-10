# [Medium] 109. Convert Sorted List to Binary Search Tree

https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/

<details>
<summary>문제내용보기</summary>

### 요소가 오름차순으로 정렬되는 단일 연결 목록의 헤드가 주어지면 높이 균형 BST로 변환합니다.

### 이 문제에서 높이 균형 이진 트리는 모든 노드의 두 하위 트리의 깊이가 1보다 크지 않은 이진 트리로 정의됩니다.

#### 예시 1

![](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.
```

#### 예시 2

```
Input: head = []
Output: []
```

#### 예시 3

```
Input: head = [0]
Output: [0]
```

#### 예시 4

```
Input: head = [1,3]
Output: [3,1]
```

Constraints:

- The number of nodes in `head` is in the range `[0, 2 * 104]`.
- `-10^5 <= Node.val <= 10^5`

</details>

### review

이번 문제는 좀 쉽게 풀고 싶어서 깊게 생각해보았습니다.
ListNode의 구조로는 리스트의 길이를 알 수 없어, 일반 배열로 변환했습니다.
일반 배열로 변환한 뒤에는, 배열의 크기를 참조하여 가운데 값 혹은 그 옆 값을 기준으로
왼쪽과 오른쪽으로 나누었습니다. 이미 정렬이 되어 있기 때문에, 크기 값으로 비교하면서 처리할 필요는 없습니다.
5일차까지 진행하면서 가장 깔끔하고 내힘으로 풀어낸 문제같아 보람을 느끼게 되었습니다.

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {ListNode} head
 * @return {TreeNode}
 */
var add = (l) => {
  const size = l.length;
  if (size === 0) return null;
  const center = Math.floor(size / 2);
  const r = new TreeNode(l[center]);
  r.left = add(l.slice(0, center));
  r.right = add(l.slice(center + 1));
  return r;
};
var sortedListToBST = function (head) {
  let list = [];
  while (head) {
    list.push(head.val);
    head = head.next;
  }
  return add(list);
};
```

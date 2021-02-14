# [Medium] 129. Sum Root to Leaf Numbers

https://leetcode.com/problems/sum-root-to-leaf-numbers/

<details>
<summary>문제내용보기</summary>

### 0-9의 숫자 만 포함하는 이진 트리가 주어지면 각 루트에서 리프 경로는 숫자를 나타낼 수 있습니다.

### 예는 숫자 123을 나타내는 루트에서 리프 경로 1-> 2-> 3입니다.

### 모든 루트에서 리프 숫자의 총합을 찾으십시오.

### 참고 : 리프는 자식이없는 노드입니다.

### 예시 1:

```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

### 예시 2:

```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

</details>

### review

Leaf 구하기는 생각보다 쉽습니다. left와 right가 동시에 없으면 leaf로 볼 수 있습니다.
재귀로 호출할 때, 다음 노드와 현재 노드를 같이 넣어서 리스트로 넣어주면 나를 포함한 모든 부모의 노드를 구할 수 있습니다.
숫자 배열을 자릿수에 해당하는 숫자로 변환하는 것도 reduce를 쓰면 간단히 만들 수 있습니다.

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
 * @return {number}
 */
const num = (arr) => {
  return parseInt(arr.reduce((acc, cur) => acc + `${cur}`, ""));
};
const calc = (root, list = [], sum = 0) => {
  if (!root) return sum;
  if (!root.left && !root.right) {
    list.push(root.val);
    return num(list);
  }
  if (root.left) sum += calc(root.left, list.concat([root.val]));
  if (root.right) sum += calc(root.right, list.concat([root.val]));
  return sum;
};
var sumNumbers = function (root) {
  return calc(root);
};
```

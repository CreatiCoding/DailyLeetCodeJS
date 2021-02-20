# [Medium] 310. Minimum Height Trees

https://leetcode.com/problems/course-schedule/

<details>
<summary>문제내용보기</summary>

```
A tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.

Given a tree of n nodes labelled from 0 to n - 1, and an array of n - 1 edges where edges[i] = [ai, bi] indicates that there is an undirected edge between the two nodes ai and bi in the tree, you can choose any node of the tree as the root. When you select a node x as the root, the result tree has height h. Among all possible rooted trees, those with minimum height (i.e. min(h))  are called minimum height trees (MHTs).

Return a list of all MHTs' root labels. You can return the answer in any order.

The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.
```

(대략 방향없는 그래프를 트리로 볼때, 가장 작은 높이를 갖는 트리의 갯수를 구하라는 문제)

#### 예시 1:

![](https://assets.leetcode.com/uploads/2020/09/01/e1.jpg)

```
Input: n = 4, edges = [[1,0],[1,2],[1,3]]
Output: [1]
Explanation: As shown, the height of the tree is 1 when the root is the node with label 1 which is the only MHT.
```

#### 예시 2:

![](https://assets.leetcode.com/uploads/2020/09/01/e2.jpg)

```
Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
Output: [3,4]
```

#### 예시 3:

```
Input: n = 1, edges = []
Output: [0]
```

#### 예시 4:

```
Input: n = 2, edges = [[0,1]]
Output: [0,1]
```

#### Constraints:

- 1 <= n <= 2 \* 104
- edges.length == n - 1
- 0 <= ai, bi < n
- ai != bi
- All the pairs (ai, bi) are distinct.
- The given input is guaranteed to be a tree and there will be no repeated edges.

</details>

### review

그래프로 넘어오면서 어려운 문제가 정말 많다는 걸 느꼈습니다. 문제를 풀었지만 O(n^2)가 나와 Time Limit Error가 발생하여 결국 혼자서는 답을 찾지 못했습니다.
해당 문제의 해설을 확인한 결과 위상정렬(topology sort)를 이용하여 푼다고 나와있습니다. 헌데, 이 위상정렬도 문제에 적용하려면 한번 꼬아서 생각해야합니다.

~~(위상정렬도 모르는데, 어떻게 찾아..)~~

위상 정렬은 순서가 정해진 그래프의 노드들을 정렬하는 방법입니다. 문제에서 구하라고 한 내용은 가능한 모든 트리에 대해서, 가장 높이가 낮은(작은) 트리의 루트 배열입니다.
위상 정렬을 하게되면, 리프 노드부터 하나씩 순서대로 사라지면서 남아있게 되는데 공교롭게도 최소 높이의 루트 노드를 찾는 방법으로 가장 적절했습니다.
말단 노드부터 하나씩 거슬러 올라가면 만나는게 결국 가장 최소 높이를 가질 수 있는 트리의 루트였습니다.

이 내용을 깨닫고 얼마나 제가 우물안 개구리였는지 다시한번 깨달았습니다. 😢

더욱 열심히 정진해야겠습니다.

```javascript
const removeIndex = (arr, fn) => arr.splice(arr.findIndex(fn), 1);
const Node = function (v = null, e = []) {
  [this.val, this.edges] = [v, e];
};
const makeGraphList = (l, v = {}, n) => {
  while ((n = l.pop())) {
    let s = v[n[0]] || new Node(n[0]);
    if (!v[s.val]) v[s.val] = s;
    let d = v[n[1]] || new Node(n[1]);
    if (!v[d.val]) v[d.val] = d;
    s.edges.push(d);
    d.edges.push(s);
  }
  return Object.values(v);
};
var findMinHeightTrees = function (n, edges) {
  const list = makeGraphList(edges);
  let leaves = list.filter((e) => e.edges.length === 1);
  while (n > 2) {
    n -= leaves.length;
    const new_leaves = [];
    for (const leaf of leaves) {
      let neighbor = leaf.edges.pop();
      removeIndex(neighbor.edges, (e) => e.val === leaf.val);
      if (neighbor.edges.length == 1) {
        new_leaves.push(neighbor);
      }
    }
    leaves = new_leaves;
  }
  return leaves.length ? leaves.map((e) => e.val) : [0];
};
```

### ref

참고하면 좋을 해법코드

https://velog.io/@pyh8618/LeetCode-310.-Minimum-Height-Trees

# [Medium] 1133. Clone Graph

https://leetcode.com/problems/surrounded-regions/

<details>
<summary>문제내용보기</summary>

#### 연결된 무 방향 그래프의 노드 참조가 주어집니다.

#### 그래프의 전체 복사본 (복제)을 반환합니다.

#### 그래프의 각 노드에는 val (int) 및 이웃 목록 (List [Node])이 포함됩니다.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

#### 테스트 케이스 형식 :

#### 간단히하기 위해 각 노드의 값은 노드의 인덱스 (1 색인)와 동일합니다. 예를 들어, val = 1 인 첫 번째 노드, val = 2 인 두 번째 노드 등이 있습니다. 그래프는 인접 목록을 사용하여 테스트 케이스에 표시됩니다.

#### 인접 목록은 유한 그래프를 나타내는 데 사용되는 정렬되지 않은 목록 모음입니다. 각 목록은 그래프에서 노드의 이웃 집합을 설명합니다.

#### 주어진 노드는 항상 val = 1 인 첫 번째 노드가됩니다. 복제 된 그래프에 대한 참조로 주어진 노드의 복사본을 반환해야합니다.

#### 예시 1:

![](https://assets.leetcode.com/uploads/2019/11/04/133_clone_graph_question.png)

```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

#### 예시 2:

![](https://assets.leetcode.com/uploads/2020/01/07/graph.png)

```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```

#### 예시 3:

```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

#### 예시 4:

![](https://assets.leetcode.com/uploads/2020/01/07/graph-1.png)

```
Input: adjList = [[2],[1]]
Output: [[2],[1]]
```

Constraints:

- `1 <= Node.val <= 100`
- `Node.val` is unique for each node.
- Number of Nodes will not exceed 100.
- There is no repeated edges and no self-loops in the graph.
- The Graph is connected and all nodes can be visited starting from the given node.

</details>

### review

이번 문제는 그래프의 깊은 복사 문제입니다. 그래프 문제로 넘어와도 기존 DFS 풀이의 형태로 생각하면 어렵지 않게 풀 수 있습니다.
다만, 무한루프에 빠지지 않도록 주의해야 합니다. visits 이라는 Object를 만들어 Map으로 사용하였습니다.
unique val에 착안하여 노드의 값을 key로 맵을 구성하면 되겠다고 생각했습니다. 처음에는 true/false만 넣어서
방문 여부만 저장하려고 했습니다. 하지만 트리가 아닌 그래프이기 때문에 생성했던 노드를 또다시 생성하지 않고 재활용하기 위해서
visits에 val을 키로 node를 넣었습니다. 이렇게 하니 이미 visits에 있는 노드는 추가 생성하지 않고 링크만 연결할 수 있습니다.

```javascript
/**
 * // Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
const calc = (node, visits = {}) => {
  if (!node) return null;
  if (visits[node.val]) return null;
  const newNode = new Node(node.val);
  visits[node.val] = newNode;
  for (const n of node.neighbors) {
    const r = calc(n, visits);
    if (r) {
      newNode.neighbors.push(r);
    } else {
      const f = visits[n.val];
      newNode.neighbors.push(f);
    }
  }
  return newNode;
};
var cloneGraph = function (node) {
  return calc(node);
};
```

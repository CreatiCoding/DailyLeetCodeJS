# [Medium] 207. Course Schedule

https://leetcode.com/problems/course-schedule/

<details>
<summary>문제내용보기</summary>

#### 수강해야하는 총 `numCourses` 과정이 있습니다. `0`부터 `numCourses-1`까지 레이블이 지정되어 있습니다. `prerequisites[i] = [ai, bi]`는 ai를 수강하려는 경우 먼저 과정 `bi`를 수강해야 함을 나타내는 배열 `prerequisites`가 제공됩니다.

- 예를 들어 `[0, 1]` 쌍은 코스 `0`을 수강하려면 먼저 코스 `1`을 수강해야 함을 나타냅니다.

#### 모든 과정을 마칠 수 있으면 `true`를 반환합니다. 그렇지 않으면 `false`를 반환합니다.

#### 예시 1:

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0. So it is possible.
```

#### 예시 2:

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take.
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

#### Constraints:

- `1 <= numCourses <= 105`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.

</details>

### review

12일동안 시작하면서 역대급으로 여러웠던 문제였습니다. 문제를 계속 읽다보니 이건 그래프에서 순환되는 그래프를 찾는 문제라는 걸 깨달았습니다.
문제는 주어진 것도 방향을 나타낸 리스트지 그래프가 아니였습니다. 그래서 그래프를 주어진 데이터로 생성해야 했습니다.
그 후에, 각 그래프의 노드에서 edges로 계속 뻗어 나가는 방식으로 탐색했습니다. 각 노드를 트리의 루트로 본 후, 모든 노드를 검사했습니다.
하나라도 노드의 값이 edges로 퍼진 것들에서 발견되면 그건 결국 제자리로 돌아온 것으로 판단했습니다.

항상 뭔가 점진적으로 검사해야하는데, 이게 나 자신으로도 돌아올 수 있는 상황이었던 알고리즘 문제를 맞닥뜨리면 어떻게 해야할지 감이 잡히지 않았지만 graph와 dfs를 알게되니 풀 수 있을 것 같습니다.

```javascript
/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
const Node = function (val, edges) {
  this.val = val || null;
  this.edges = edges || [];
};
const makeGraph = (p) => {
  const visits = {};
  for (let i = 0; i < p.length; i++) {
    if (visits[p[i][0]]) {
      if (visits[p[i][1]]) {
        visits[p[i][0]].edges.push(visits[p[i][1]]);
      } else {
        let n1 = new Node(p[i][1]);
        visits[p[i][1]] = n1;
        visits[p[i][0]].edges.push(n1);
      }
    } else {
      let n1 = new Node(p[i][0]);
      visits[p[i][0]] = n1;
      if (visits[p[i][1]]) {
        n1.edges.push(visits[p[i][1]]);
      } else {
        let n2 = new Node(p[i][1]);
        visits[p[i][1]] = n2;
        n1.edges.push(n2);
      }
    }
  }
  return visits;
};
const calc = (node, val, visits = {}) => {
  if (visits[node.val]) return true;
  visits[node.val] = true;
  for (const n of node.edges) {
    if (n.val === val) return false;
    if (!calc(n, val, visits)) return false;
  }
  return true;
};
var canFinish = function (numCourses, p) {
  const g = makeGraph(p);
  for (const k in g) {
    const v = g[k];
    if (!calc(v, v.val)) return false;
  }
  return true;
};
```

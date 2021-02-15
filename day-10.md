# [Medium] 130. Surrounded Regions

https://leetcode.com/problems/surrounded-regions/

<details>
<summary>문제내용보기</summary>

#### 'X'와 'O'(문자 O)가 포함 된 2D 보드가 주어지면 'X'로 둘러싸인 모든 영역을 캡처합니다.

#### 모든 'O'를 둘러싼 영역에서 'X'로 뒤집어 영역을 캡처합니다.

#### 예시 1:

```
X X X X
X O O X
X X O X
X O X X
```

#### 함수를 실행 한 후 보드는 다음과 같아야합니다.

```
X X X X
X X X X
X X X X
X O X X
```

#### 설명:

#### 주변 영역은 테두리에 있어서는 안됩니다. 즉, 보드 테두리의 `'O'`가 `'X'`로 반전되지 않습니다. 테두리에 있지 않고 테두리의 `'O'`에 연결되지 않은 `'O'`는 `'X'`로 반전됩니다. 인접한 셀이 가로 또는 세로로 연결된 경우 두 셀이 연결됩니다.

</details>

### review

드디어 DFS의 열번째 문제입니다. 2차원 배열을 주고 퍼지듯 검증확인이 필요한 문제입니다. 이와 같은 문제는 매우 많이 출제되며 저도 과거 이런 형태의 문제를 만난 적이 많은 것으로 기억합니다. 그때는 어려워서 접근조차 할 수 없었던 문제를 이제는 쉽게 도전할 수 있습니다.
2차원으로 DFS를 구성하되 위아래왼쪽오른쪽으로 검사합니다. 테두리일때만, DFS를 실행하며 실행된 좌표에 한해서 O인 경우 특별한 기호를 그곳에 넣습니다.
DFS가 모두 종료된 후, 특별한 기호를 제외하고 모두 X로 치환하며 특별한 기호는 다시 O로 치환하면 됩니다.

```javascript
/**
 * @param {character[][]} board
 * @return {void} Do not return anything, modify board in-place instead.
 */
const isBorder = (board, y, x) => {
  const [h, w] = [board.length, board[0].length];
  return x === 0 || x === w - 1 || y === 0 || y === h - 1;
};
const dfs = (board, y, x) => {
  if (!board[y] || !board[y][x] || board[y][x] !== "O") return;
  if (board[y][x] === "O") board[y][x] = "T";
  dfs(board, y, x + 1);
  dfs(board, y, x - 1);
  dfs(board, y + 1, x);
  dfs(board, y - 1, x);
};
var solve = function (board) {
  for (let y = 0; y < board.length; y++) {
    for (let x = 0; x < board[y].length; x++) {
      if (isBorder(board, y, x) && board[y][x] === "O") {
        dfs(board, y, x);
      }
    }
  }
  for (let y = 0; y < board.length; y++) {
    for (let x = 0; x < board[y].length; x++) {
      board[y][x] = board[y][x] === "T" ? "O" : "X";
    }
  }
};
```

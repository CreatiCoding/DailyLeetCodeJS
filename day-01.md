# [Medium] 17. Letter Combinations of a Phone Number

https://leetcode.com/problems/letter-combinations-of-a-phone-number/

<details>
<summary>문제내용보기</summary>

#### 2-9까지의 숫자가 포함 된 문자열이 주어지면 숫자가 나타낼 수있는 가능한 모든 문자 조합을 반환합니다. 임의의 순서로 답변을 반환하십시오.

#### 숫자와 문자의 매핑 (전화 버튼과 동일)은 다음과 같습니다. 1은 어떤 문자에도 매핑되지 않습니다.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

### Example 1:

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

### Example 2:

```
Input: digits = ""
Output: []
```

### Example 3:

```
Input: digits = "2"
Output: ["a","b","c"]
```

### Constraints:

- 0 <= digits.length <= 4
- digits[i] is a digit in the range ['2', '9'].
</details>

### review

DFS를 학습할 목적으로 푼 문제였지만, 문제가 단순해서 중첩 for문으로도 쉽게 풀 수 있는 문제였다. DFS 관련 문제를 좀 더 풀어서 DFS에 익숙해 지고 싶다.

## 풀이1 [DFS 없이]

```javascript
const DigitMap = {
  2: ["a", "b", "c"],
  3: ["d", "e", "f"],
  4: ["g", "h", "i"],
  5: ["j", "k", "l"],
  6: ["m", "n", "o"],
  7: ["p", "q", "r", "s"],
  8: ["t", "u", "v"],
  9: ["w", "x", "y", "z"],
};
var letterCombinations = function (digits) {
  let result = [];
  for (let i = 0; i < digits.length; i++) {
    const list = DigitMap[digits[i]];
    let next = [];
    for (let j = 0; j < list.length; j++) {
      if (i === 0) {
        next.push(list[j]);
      } else {
        next.push(...result.map((e) => e + list[j]));
      }
    }
    result = next;
  }
  return result;
};
```

## 풀이2 [DFS]

```javascript
const DigitMap = {
  2: ["a", "b", "c"],
  3: ["d", "e", "f"],
  4: ["g", "h", "i"],
  5: ["j", "k", "l"],
  6: ["m", "n", "o"],
  7: ["p", "q", "r", "s"],
  8: ["t", "u", "v"],
  9: ["w", "x", "y", "z"],
};
const node = (value = null, children = []) => ({ value, children });
const DFS = (root, callback) => {
  if (root === null) return;
  const list = [root];
  while (list.length !== 0) {
    const cur = list.shift();
    list.unshift(...cur.children);
    callback(cur);
  }
};
var letterCombinations = (digits) => {
  const digit_list = digits.split("");
  const root = node("");
  let layer = [root];
  for (const digit of digit_list) {
    let temp = [];
    for (const item of layer) {
      let d_list = DigitMap[digit].map((e) => node(`${item.value}${e}`));
      item.children.push(...d_list);
      temp.push(...d_list);
    }
    layer = temp;
  }
  let result = [];
  DFS(root, (e) => {
    if (e.value && e.value.length === digits.length) {
      result.push(e.value);
    }
  });
  return result;
};
```

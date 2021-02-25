# [Medium] 5. Longest Palindromic Substring

https://leetcode.com/problems/longest-palindromic-substring/

<details>
<summary>문제내용보기</summary>

Given a string `s`, return the longest palindromic substring in `s`.

#### 예시 1:

```

Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.

```

#### 예시 2:

```

Input: s = "cbbd"
Output: "bb"

```

#### 예시 3:

```

Input: s = "a"
Output: "a"

```

#### 예시 4:

```

Input: s = "ac"
Output: "a"

```

#### Constraints:

- 1 <= s.length <= 1000
- s consist of only digits and English letters (lower-case and/or upper-case),

</details>

### review

Palindromic(좌우 대칭) 문자열 판별하는 함수를 만듭니다.
주어진 문자열을 하나씩 덧붙인 요소를 push하면서 계속해서 판별합니다.
판별하면서 최대길이의 값을 계속 최신화합니다.
주의할 점은 dp로 사용된 변수를 step 별로 계속 만드는게 아니라 덮어씌워 안쓰는 변수의 메모리를 해제해줍니다.

```javascript
/**
 * @param {string} s
 * @return {string}
 */
const isPalindromic = (s) => {
  for (let i = 0; i < s.length / 2; i++) {
    if (s[i] !== s[s.length - i - 1]) {
      return false;
    }
  }
  return true;
};
var longestPalindrome = function (s) {
  if (s.length === 0) return "";
  if (s.length === 1) return s;

  const list = s.split("");
  let dp = [list[0]];
  let max = { v: 0, s: "" };

  for (let i = 1; i < list.length; i++) {
    const r = dp.filter((e) => isPalindromic(e) && max.v < e.length).pop();
    if (r) {
      max = { v: r.length, s: r };
    }
    dp = dp.map((e) => e + list[i]);
    dp.push(list[i]);
  }

  const r = dp.filter((e) => isPalindromic(e) && max.v < e.length).pop();
  if (r) {
    max = { v: r.length, s: r };
  }
  return max.s;
};
```

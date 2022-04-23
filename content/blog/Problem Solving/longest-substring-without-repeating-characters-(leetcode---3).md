---
title: Longest Substring Without Repeating Characters (LeetCode - 3)
date: 2022-04-23 13:04:71
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] **Longest Substring Without Repeating Characters**

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

```

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

### 문제 접근

- 전체 문자열중 반복하는 문자열이 없으면서 가장 길이가 긴 substring (연속하는 부분 문자열)의 길이를 구하는 문제다. 얼핏보면 단순해 보이지만 여러가지 케이스를 고려해서 풀어야 하는 문제였다.
- 가령, 1번 케이스의 경우 처음 abc까지는 반복되는 문자열이 없지만 그 다음에 a가 오면 반복되는 문자열이 생기므로, 최대 길이 문자열은 이 abc가 되므로 3을 반환한다.

### Solution 1 (실패)

```jsx
const lengthOfLongestSubstring = s => {
  let maxLenStr = ''

  let curStr = ''

  for (let i = 0; i < s.length; i += 1) {
    if (curStr === '') curStr += s[i]
    else if ([...curStr].includes(s[i])) {
      if (curStr.length > maxLenStr.length) maxLenStr = curStr
      curStr = s[i]
    } else {
      curStr += s[i]
      if (curStr.length > maxLenStr.length) {
        maxLenStr = curStr
      }
    }
  }

  if (curStr.length > maxLenStr.length) {
    maxLenStr = curStr
  }

  return maxLenStr.length
}
```

- 가장 길이가 긴 부분 문자열을 저장하기 위한 변수(maxLenStr)와, 현재 누적된 문자열을 저장해둘 변수(curStr)을 선언한다.
- 그리고 반복하면서 3가지 케이스로 나눠 반복한다.
  - 현재 누적 문자열이 비어있을때는 현재 문자를 저장한다.
  - 그렇지 않을 경우 현재 누적 문자열을 배열로 풀어 현재 문자를 포함하고 있는지 체크한다.
    - 만약 있다면, 현재 누적 문자열의 길이가 최대 길이 부분 문자열보다 길이가 크면 갱신한다.
  - 그렇지 않은 경우 현재 누적 문자열에 현재 문자를 누적하고 길이를 다시 비교해서 최대로 갱신한다.
- 길이가 1인 문자열의 경우 반복문에서 갱신하지 않기 때문에 다시 한번 길이 비교 후 갱신한다.

### 실패 원인

- 반복되는 문자열을 체크하고 존재하면 아예 새로운 문자열로 초기화했는데 그렇게 하면 연결되는 문자도 모두 초기화 해버리기 때문에 최대길이를 구하는데 실패한다.

### Solution 2 substring 사용

```jsx
const lengthOfLongestSubstring = s => {
  const sLen = s.length
  let maxLen = 0
  let maxStr = ''
  let curChar = ''
  let postIdx = ''

  for (let i = 0; i < sLen; i += 1) {
    curChar = s[i]
    postIdx = maxStr.indexOf(curChar)

    if (postIdx > -1) {
      maxStr = maxStr.substring(postIdx + 1) // 문자열 제거
    }

    maxStr += curChar // 뒤에 추가
    maxLen = Math.max(maxLen, maxStr.length) // 최대 길이 갱신
  }

  return maxLen
}
```

- 선언부에 선언된 변수들은 다음과 같다.
  - sLen : 인자로 받는 문자열의 길이
  - maxLen : 최대 부분 문자열의 길이를 갱신하기 위해 담는 변수
  - maxStr : 최대 부분 문자열을 담기 위한 변수
  - curChar : 현재 문자를 담기 위한 변수
  - postIdx : 최대 부분 문자열에서 현재 문자의 인덱스
- 로직은 Solution1 과 비슷하지만 현재 문자를 포함하는 지를 index로 찾고 해당 문자열이 존재하면 제거후 마지막에 추가하는 방식을 반복한다.(substring)
  - 이게 가능한 이유는 우리는 최대길이의 문자열만 찾을 것이기 때문에 반복 문자가 나타나더라고 그 위치부터 최대 길이를 계속 갱신하며 찾으면 된다.

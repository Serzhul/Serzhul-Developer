---
title: Longest Repeating Character Replacement (LeetCode - 424)
date: 2022-04-23 15:04:07
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] **Longest Repeating Character Replacement**

You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return *the length of the longest substring containing the same letter you can get after performing the above operations*.

**Example 1:**

```
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.

```

**Example 2:**

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.

```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of only uppercase English letters.
- `0 <= k <= s.length`

### 문제 접근

- 문자열 s와 숫자 k가 주어졌을 때, k는 문자열중 임의의 문자를 바꿀 수 있는 횟수를 의미한다. 임의의 문자를 k번 바꿨을때 같은 문자가 연속하는 최대 길이를 구하는 문제이다.
- 예를 들어 문자열 ‘ABAB’가 있을 때 2개의 문자를 바꿔서 최대 연속하는 문자를 만들려면 B를 모두 A로 바꿔 ‘AAAA’ 로 만들거나 A를 모두 B로 바꿔 ‘BBBB’ 로 만들어서 최대 4글자로 연속하는 문자를 만들 수 있다.
- 문제는 이해를 했지만, 어떻게 풀어야 하는지 감이 오지 않아 찾아보니 Sliding window 기법을 통해 풀 수 있는 문제 였다.
  - **Cf. Sliding window**
    - 슬로잉 윈도우는 주어진 배열 혹은 문자열에서 일부 데이터를 추출한 후, 그 데이터를 특정 조건에 따라 확장 또는 축소하면서 마치 슬라이딩 하는 효과를 주는 기법을 의미한다.
    - 슬라이딩 윈도우는 연속하는 요소들을 추적하거나, 서브 배열의 합을 구할 때 등에 유용하다.
- 문제의 핵심 컨셉은 슬라이딩 윈도우 기법을 사용해 윈도우(조건에 부합하는 문자열)의 길이를 최대로 만들면된다.

### Solution 1

```jsx
const characterReplacement = (s, k) => {
  let left = 0
  let right = 0
  let maxCharCount = 0
  const visited = {}

  while (right < s.length) {
    const char = s[right]
    visited[char] = visited[char] ? visited[char] + 1 : 1

    if (visited[char] > maxCharCount) maxCharCount = visited[char]

    if (right - left + 1 - maxCharCount > k) {
      visited[s[left]]--
      left++
    }

    right++
  }

  return right - left
}
```

- 먼저 선언부에 선언된 변수들의 의미는 다음과 같다.
  - left(윈도우의 시작점)
  - right(윈도우의 끝 점)
  - maxCharCount(윈도우 내에 가장 많이 등장한 문자의 개수)
  - visited(문자를 해싱하기 위한 객체)
- 윈도우의 끝점이 배열의 마지막에 도착하면 종료하며, 다음과 같은 내용을 반복한다.
  - 먼저 윈도우의 현재 끝 문자(char)를 기준으로 사용되었는지를 맵에서 찾는다.
    - 처음이면 1, 처음이 아니면 기존 횟수에 + 1을 더한다.
  - 만약 맵에서 char의 값이 가 최대 사용된 횟수보다 크다면 최대 사용 횟수를 갱신한다.
  - 현재 문자열의 길이(right- left +1)에서 최대사용 횟수를 뺀 값이 k 보다 큰지 아닌지 체크한다.
    - k보다 더 크다면 남아있는 문자열의 종류를 k 횟수를 사용해서는 다 같은 문자로 바꿀 수 없기 때문에 현재 문자열은 답이 될 수 없다는 것을 의미한다. 따라서 시작점의 문자를 제거하기 위해 left을 옮기고, map에서 사용횟수를 감소시킨다.
    - 그렇지 않다면 조건에 부합하는 조건이므로 right 를 하나 옮기면서 다시 체크한다.
  - 마지막에 남아있는 윈도우의 크기가 문제 조건에 맞는 문자열의 최대 길이이므로 이를 반환한다.

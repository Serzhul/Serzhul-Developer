---
title: Longest Common Subsequence (LeetCode - 1143)
date: 2022-04-26 12:04:39
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] **Longest Common Subsequence (1143)**

Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

```
Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: The longest common subsequence is "ace" and its length is 3.

```

**Example 2:**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.

```

**Example 3:**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.

```

**Constraints:**

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

### 문제 접근

- 인자로 두 개의 텍스트가 주어질 때, 가장 길이가 긴 공통 서브 문자열의 길이를 반환하는 문제이다. 만약 존재하지 않으면 0을 반환하면 된다.
- 문제에서 말하는 서브 문자열이란, 원본 문자열에서 상대적 위치를 유지하면서 다른 문자열을 제거했을 때의 문자열을 말한다.
- 문제의 해결법은 총 3가지 방식으로 풀 수 있다.

### Solution 1 Brute Force 방식 (시간 초과)

```jsx
const longestCommonSubsequence = (text1, text2) => {
  const getLongestCommonSubsequence = (str1, str2, i, j) => {
    if (i === str1.length || j === str2.length) return 0 // 종료 조건

    if (str1[i] === str2[j])
      return 1 + getLongestCommonSubsequence(str1, str2, i + 1, j + 1) // subsequence면 값 증가

    return Math.max(
      getLongestCommonSubsequence(str1, str2, i + 1, j), // text1 의 idx 옮기기
      getLongestCommonSubsequence(str1, str2, i, j + 1) // text2의 idx 옮기기
    )
  }

  return getLongestCommonSubsequence(text1, text2, 0, 0)
}
```

- 재귀 함수를 사용해 두 개의 텍스트를 하나씩 비교하는 방식이다.
- 종료 조건은 i(text1의 인덱스)가 text1의 길이와 같아지거나 j(text2의 인덱스)가 text2의 길이와 같아지면 종료한다.
- 만약 같은 문자열을 찾았으면 반환되는 값에 1을 추가하며 인덱스를 각각 하나씩 옮겨준다.
- 못 찾았으면 i를 옮기거나, j를 옮겨가면서 추가되는 값을 찾는다 둘 중에 더 큰 값을 반환한다.

### Solution 2 Top-down DP

```jsx
const longestCommonSubsequence = (text1, text2) => {
  const dp = Array.from({ length: text1.length + 1 }, () =>
    Array(text2.length + 1)
  )

  const getLongestCommonSubsequence = (str1, str2, i, j) => {
    if (i === str1.length || j === str2.length) return 0

    if (dp[i][j] !== undefined) return dp[i][j]

    if (str1[i] === str2[j]) {
      dp[i][j] = 1 + getLongestCommonSubsequence(str1, str2, i + 1, j + 1)
      return dp[i][j]
    }

    dp[i][j] = Math.max(
      getLongestCommonSubsequence(str1, str2, i + 1, j),
      getLongestCommonSubsequence(str1, str2, i, j + 1)
    )
    return dp[i][j]
  }

  return getLongestCommonSubsequence(text1, text2, 0, 0)
}
```

- 전체 로직은 Brute Force 방식과 유사하지만 dp 배열을 생성하여 각 값을 Memoization 한 후, 한번 확인 한 값은 즉시 반환하는 식으로 실행 속도를 단축했다.

### Solution 3 Bottom-up DP

```jsx
const longestCommonSubsequence = (text1, text2) => {
  const m = text1.length
  const n = text2.length

  const dp = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0))

  // text1, text2의 길이 만큼 반복하며 문자열이 존재하는지 체크 후 저장
  for (let i = 1; i <= m; i += 1) {
    for (let j = 1; j <= n; j += 1) {
      if (text1[i - 1] !== text2[j - 1]) {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])
      } else {
        dp[i][j] = dp[i - 1][j - 1] + 1
      }
    }
  }

  return dp[m][n]
}
```

- 마찬 가지로 2차원 dp 배열을 선언하되, 반복문을 통해 하나씩 비교 대조하며 dp 배열을 채워 나간다. 최대값을 갱신하면서 마지막까지 반복문을 수행하기 때문에 결국 dp의 마지막 값이 최종적인 답이 된다.

---
title: Longest Increasing Subsequence (LeetCode - 300)
date: 2022-04-25 14:04:90
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] **Longest Increasing Subsequence (300)**

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4

```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1

```

**Constraints:**

- `1 <= nums.length <= 2500`
- `104 <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

### 문제 접근

- 유명한 알고리즘 중 하나인 LIS 알고리즘 문제이다. LIS 알고리즘은 DP를 사용해서 푸는 것이 정석으로 알려져 있으므로, DP로 푸는 솔루션을 잘 이해하는 것이 중요하다.

### Solution 1 : DP

- 솔루션은 크게 2가지 파트로 나뉜다.
  - 이진 탐색 로직 구현하기
  - DP 배열을 사용한 최대 길이값 갱신하기

```jsx
const lengthOfLIS = nums => {
  const binarySearchPosition = (dp, target, hi) => {
    let low = 0
    while (low <= hi) {
      const mid = Math.floor((low + hi) / 2)
      if (target === dp[mid]) return mid
      if (target < dp[mid]) hi = mid - 1
      else low = mid + 1
    }
    return low
  }

  if (nums === null || nums.length === 0) return 0

  if (nums.length === 1) return 1

  const dp = new Array(nums.length).fill(Number.MAX_SAFE_INTEGER)
  for (let i = 0; i < nums.length; i += 1) {
    const pos = binarySearchPosition(dp, nums[i], i)
    dp[pos] = nums[i]
  }

  for (let i = dp.length - 1; i >= 0; i -= 1) {
    if (dp[i] !== Number.MAX_SAFE_INTEGER) return i + 1
  }

  return 0
}
```

- 먼저 binarySearch 탐색이 가능한 이유는 dp 배열이 순차적으로 증가하는 정렬된 배열이라는 특징을 갖고 있기 때문이다. 따라서 이진 탐색을 통해 현재 정수 배열의 값중 dp 배열에 들어갈 수 있는 위치를 이진 탐색으로 찾는다고 할 수 있다.
- binarySearch 로직을 살펴보면, dp 배열과 target(현재 정수 배열의 값), 그리고 hi(현재 정수 배열의 값의 인덱스)를 받는다는 걸 알 수 있다. 그리고 low (초기값 0)와 hi사이의 값 중간 위치를 구한다. 그러면 target이 dp 배열에서 어느 쪽에 들어가야 하는 지 파악할 수 있다.
  - target과 dp[mid]가 같다면 같은 값이므로 아무것도 바뀌지 않는다.
  - 만약 target이 dp[mid] 보다 작다면, mid보다 왼쪽에 위치하게 되므로 high 값을 mid-1로 갱신한다.
  - 그렇지 않다면 mid 보다 오른쪽에 위치하게 되므로 low 를 mid+1로 갱신한다.
  - 이 과정을 반복하다보면 결국 target이 dp 배열에 들어갈 위치를 찾게 된다.
- 마지막에는 dp 배열을 뒤에서 부터 순회하면서 값이 들어있는지를 체크한다. MAX_SAFE_INTEGER가 아니라면 값이 들어가 있다는 의미이므로, 해당 인덱스 + 1이 LIS의 길이가 된다.

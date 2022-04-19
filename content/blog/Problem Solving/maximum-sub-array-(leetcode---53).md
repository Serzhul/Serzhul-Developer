---
title: Maximum sub Array (LeetCode - 53)
date: 2022-04-19 15:04:84
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return *its sum*.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

```

**Example 2:**

```
Input: nums = [1]
Output: 1

```

**Example 3:**

```
Input: nums = [5,4,-1,7,8]
Output: 23
```

**Constraints:**

- `1 <= nums.length <= 105`
- `104 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

### 문제 해설

- 정수 배열 nums가 주어졌을 때, 연속하는 **서브배열의 최대합**을 구하는 문제이다.
  - 서브배열이라 함은 배열의 일부 또는 전체가 될 수 있으며, 인덱스가 연속해야한다.
- 시간복잡도는 O(n)으로 한다.

### 문제 접근

- 시간복잡도가 O(n)이므로 한번의 순회 과정에서 최댓값을 구할 수 있는 방법을 생각해 보았다.
  - 이전 값을 저장해 두기 위해 배열을 만들고, 누적합 값에 최신 값을 더해준다. 이때, 기존 누적합 값이 -라면 최댓값은 0보다는 크다는 가정을 할 수 있기 때문에 0으로 초기화하고 누적값을 더해준다.
  - 배열을 순회하며 이 방식을 반복한다.

### Solution 1 - dp 방식

- 문제의 핵심은 현재 누적값이 0보다 크냐 작냐이다.
  - 0보다 작다면 최대값은 어차피 0보다는 클 것이므로 0으로 초기화 해준 다음 nums[i]값을 더해준다.
  - 최대값을 항상 비교하며 갱신한다.

```tsx
const maxSubArray = nums => {
  const dp = []

  dp[0] = nums[0]

  let max = dp[0]

  for (let i = 1; i < nums.length; i += 1) {
    dp[i] = nums[i] + (dp[i - 1] > 0 ? dp[i - 1] : 0)

    max = Math.max(max, dp[i])
  }

  return max
}
```

### Solution 2 - 누적합과 비교

- 누적합 비교 로직 역시 DP와 마찬가지로 2가지 방법으로 비교한다.
  - 만약 현재 누적값이 0보다 작다면 nums[i] 값으로 초기화한다.
  - 현재 누적값이 0보다 크다면 nums[i] 값을 더해준다.
- 최대 누적합과, 현재 누적합을 비교해 갱신한다.

```tsx
const maxSubArray = nums => {
  let currentSum = -Infinity
  let maxSum = -Infinity

  for (let i = 0; i < nums.length; i += 1) {
    currentSum = Math.max(currentSum, 0)
    currentSum += nums[i]
    maxSum = Math.max(maxSum, currentSum)
  }

  return maxSum
}
```

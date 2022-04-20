---
title: Maximum Product Subarray (LeetCode - 152)
date: 2022-04-20 15:04:73
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] Maximum Product Array

Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return *the product*.

The test cases are generated so that the answer will fit in a **32-bit** integer.

A **subarray** is a contiguous subsequence of the array.

**Example 1:**

```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

```

**Example 2:**

```
Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

### 문제 접근

- sub Array 요소들의 곱 중 최댓값을 구하는 문제로 풀이 자체는 Maximum sub Array 문제와 유사하다. 한가지 다른 점은, Maximum sub Array는 음수가 나올 때 나올때 0으로 초기화해야 하지만, Maximum Prodcut Array 문제에서는 가장 작은 최솟값을 저장해 두고 있어야 한다는 점이다.
- 배열의 요소들은 정수이기 때문에 음의 정수도 올 수 있다. 음의 정수가 연속해서 2번 이상 등장하면 그 곱이 양수가 되므로, 최댓값이 바뀔 수도 있다. 따라서 최댓값은 물론 저장해두어야 하지만, 최솟값도 저장해서 만약 음의 정수가 곱해졌을 때 최댓값이 더 클 수도 있다는 가정을 해야한다.

## Solution

```tsx
const maxProduct = nums => {
  let prevMax = nums[0] // 최솟값
  let prevMin = nums[0] // 최댓값
  let result = nums[0] // 최대 결과값

  for (let i = 1; i < nums.length; i += 1) {
    const curMax = Math.max(prevMax * nums[i], nums[i], prevMin * nums[i])
    const curMin = Math.min(prevMax * nums[i], nums[i], prevMin * nums[i])

    prevMax = curMax
    prevMin = curMin

    result = Math.max(curMax, result)
  }

  return result
}
```

- 먼저 비교할 최솟값과 최댓값을 갱신하기 위한 변수를 선언하며, 최종적으로 반환할 최댓값 변수도 선언한다. (초기값은 nums 배열의 첫번째 값을 넣는다.)
- 2번째 인덱스부터 순회하면서 최댓값을 비교를 하는데 이때 주의 할점은 3가지 인수를 비교해야한다.
  - 이전 최댓값에 현재 요소를 곱한 값
  - 현재 요소
  - 이전 최솟값에 현재 요소를 곱한 값
- 이렇게 3가지 경우의 수를 따지는 최댓값과 최솟값을 구하고, 저장해둔다. 그리고 result는 curMax값과 비교해 갱신한다.

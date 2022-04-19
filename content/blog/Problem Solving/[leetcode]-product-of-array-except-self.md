---
title: Product of Array Except Self (LeetCode-238)
date: 2022-04-19 13:04:42
category: Problem Solving
thumbnail: { thumbnailSrc }
draft: false
---

# [LeetCode] **Product of Array Except Self**

**Category : Array**

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]

```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]

```

**Constraints:**

- `2 <= nums.length <= 105`
- `30 <= nums[i] <= 30`
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

**Follow up:**
 Can you solve the problem in `O(1)` 
extra space complexity? (The output array **does not**
 count as extra space for space complexity analysis.)

## 문제 해설

- 정수 배열 array가 주어졌을 때, answer 배열을 반환해야 한다. 이때 **answer 배열의 i 번째 인덱스 값**은, **nums 배열의 i 번째 인덱스 값을 제외한 모든 배열 요소의 곱의 값**이 되어야 한다.
- 또한 시간 복잡도는 O(n)으로 풀어야 하며, 추가로 공간 복잡도 역시 O(1)으로 푸는 방법을 제시해야 한다. (이때 새롭게 선언하는 answer 배열은 추가 공간으로 치지 않는다.)

## 나의 문제 접근

- 처음엔 단순한 문제라고 생각했다. nums의 모든 요소의 곱의 값을 구하고, 다시 nums 배열을 순회하면서 answer 배열의 i 인덱스의 모든 요소 값 나누기 nums의 i 인덱스 값을 구하며 된다고 생각했다.
  - 그러나 이 경우에 요소 값에 0이 들어가 있을 경우가 문제가 되었다. 요소들의 곱 중 0이 포함되어있으면 최종값이 0이 되는데, answer에는 0이 아닌 나머지 값들의 곱을 구해야 됐기 때문
  - 또한 문제에서도 나누기 쓰지 말고 풀라고 되어있었으므로, 접근법이 완전히 잘못되었다는 사실을 깨달을 수 있었다.

## 다른 문제 접근

결국 문제 풀이에 실패하고 다른 솔루션의 풀이 방식을 살펴보니 접근법은 다음과 같았다.

### Solution 1

- 먼저 nums 배열을 (왼→오른) 순으로 곱하면서, 마지막 배열 전까지 곱셈값을 누적한다.

```jsx
// [1,2,3,4] 배열이라고 했을 때

const leftArr = []
let leftMultiPlication = 1

for (let i = 0; i < nums.length; i += 1) {
  leftArr[i] = leftMultiplication
  leftMultiplication *= nums[i]
}
```

- 그러면 마지막 요소 전까지 모든 값을 곱한 값이 누적된 배열이 만들어진다.
  - 위 테스트케이스를 예시로 들면 [1, (1*1), (1*1*2), (1*1*2*3)]이 된다.
- 그 다음에는 이제 반대로 오른쪽에서부터 곱한 값을 누적하는 배열을 만들되, 앞서 만든 왼쪽 배열과의 곱셈값을 곱하면서 계산한다.
  - 그럼 다음과 같은 배열이 만들어진다 [(1*4*3*2) ,(1*4*3), (1*4) ,1]
- 그러면 왼쪽 배열의 각 인덱스에는 해당 인덱스 이전의 값들의 누적곱 값이 들어있고, 오른쪽 배열에는 이후의 누적곱 값이 들어있다.
  - 두 배열을 곱함으로써 원하는 배열 값을 도출한다.

```jsx
const rightArr = []
let rightMultiplication = 1

for (let i = nums.length - 1; i >= 0; i -= 1) {
  rightArr[i] = rightMultiplication
  rightMultiplication *= nums[i]
  rightArr[i] *= leftArr[i]
}
```

### 최종 코드

```jsx
const productExceptSelf = nums => {
  const leftArr = []
  let leftMultiplication = 1

  for (let i = 0; i < nums.length; i += 1) {
    leftArr[i] = leftMultiplication
    leftMultiplication *= nums[i]
  }

  const rightArr = []
  let rightMultiplication = 1

  for (let i = nums.length - 1; i >= 0; i -= 1) {
    rightArr[i] = rightMultiplication
    rightMultiplication *= nums[i]
    rightArr[i] *= leftArr[i]
  }

  return rightArr
}
```

### Solution 2 (Solution1의 공간 복잡도 개선)

- Solution1은 정답으로 반환하는 rightArr 외에도 leftArr을 추가로 선언하기 때문에 추가 Space를 차지한다고 볼 수 있다. 그렇기 때문에 하나의 배열에서 위와 비슷한 작업을 처리하기 위해 코드를 개선할 수 있다.

```jsx
const productExceptSelf = nums => {
  const result = []
  let multiplication = 1

  for (let i = 0; i < nums.length; i += 1) {
    result[i] = multiplication
    multiplication *= nums[i]
  }

  multiplication = 1

  for (let i = nums.length - 1; i >= 0; i -= 1) {
    result[i] *= multiplication
    multiplication *= nums[i]
  }

  return result
}
```

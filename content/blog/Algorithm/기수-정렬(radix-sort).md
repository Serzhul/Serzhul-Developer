---
title: 기수 정렬(Radix Sort)
date: 2021-08-26 19:08:58
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

# 정렬 알고리즘 끼리의 복잡도 비교

- 버블 정렬(Bubble Sort) - $O(n^2)$
- 삽입 정렬(Insertion Sort) - $O(n^2)$
- 선택 정렬(Selection Sort) - $O(n^2)$
- 퀵 정렬(Quick Sort) - $O(nlog(n))$
- 병합 정렬(Merge Sort) - $O(nlog(n))$

## 기수 정렬 개관

기수 정렬은 숫자 목록에서 동작하는 특별한 정렬 알고리즘이다. ⇒ 2진법 숫자

- 기수 정렬에서는 요소들끼리의 비교를 하지 않는다.
- 기수 정렬은 숫자의 크기는 그 숫자의 자릿수를 통해 알아낼 수 있다는 정보를 도출한다.
- 즉, 자릿수가 많을 수록 더 큰 숫자라는 것이다.

기수 정렬은 숫자 목록에서 사용하는 특별한 정렬 알고리즘이다.

- 두개의 요소를 비교하지 않는다.
- 숫자의 아라비아를 통해 숫자의 크기에 관한 정보를 추출한다.
- 아라비아 숫자(자릿수)가 많을수록 더 큰 숫자이다.

## 기수 정렬 도움함수들

기수 정렬을 구현하기 위해, 몇몇 도움함수들을 먼저 구하는 것이 유용하다

- getDigit(num, place) - 주어진 값의 아라비아숫자를 반환한다.

```jsx
function getDigit(num, i) {
  return Math.floor(Math.abs(num) / Math.pow(10, i)) % 10
}
```

- digitCount(num) - 자릿수를 카운트한다.

```jsx
function digitCount(num) {
  if (num === 0) return 1
  return Math.floor(Math.log10(Math.abs(num))) + 1
}
```

- mostDigits(nums) - 숫자 배열이 주어지면, 목록에서 가장 큰 숫자의 자릿수를 반환한다.

```jsx
function mostDigits(arr) {
  var max = 0
  for (let i = 0; i < arr.length; i++) {
    max = Math.max(max, digitCount(arr[i]))
  }
  return max
}
```

## 기수 정렬 의사코드

- 숫자 목록을 받는 함수를 정의한다.
- 가장 큰 숫자의 자릿수를 알아낸다.
- k = 0부터 가장 큰 숫자의 자릿수만큼 루핑한다.
- 각각의 루프에선
  - 각 숫자(0 ~ 9)에 해당하는 버킷을 만든다.
  - k번째 숫자의 해당하는 버킷에 각 숫자를 배치한다.
- 0부터 9로 이루어진 바깥쪽 버킷으로 현재 존재하는 값들을 대체한다.
- 끝에서 리스트를 반환한다

```jsx
function getDigit(num, i) {
  return Math.floor(Math.abs(num) / Math.pow(10, i)) % 10
}
// Math.pow(10, i) => 10의 i승을 의미;

function digitCount(num) {
  if (num === 0) return 1
  return Math.floor(Math.log10(Math.abs(num))) + 1
}

function mostDigits(arr) {
  var max = 0
  for (let i = 0; i < arr.length; i++) {
    max = Math.max(max, digitCount(arr[i]))
  }
  return max
}

function radixSort(nums) {
  let maxDigitCount = mostDigits(nums)
  for (let k = 0; k < maxDigitCount; k++) {
    let digitBuckets = Array.from({ length: 10 }, () => [])
    for (let i = 0; i < nums.length; i++) {
      let digit = getDigit(nums[i], k)
      digitBuckets[digit].push(nums[i])
    }
    console.log(digitBuckets)
    nums = [].concat(...digitBuckets)
    console.log(nums)
  }
  return nums
}

radixSort([23, 345, 5467, 12, 2345, 9852])
```

# 기수 정렬 Big O

- 시간 복잡도(Best) ⇒ $O(nk)$
- 시간 복잡도(Average) ⇒ $O(nk)$
- 시간 복잡도(Worst) ⇒ $O(nk)$
- 공간 복잡도 ⇒ $O(n+k)$

  n - 배열의 길이

  k - 자릿수 (평균적인)

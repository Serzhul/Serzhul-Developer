---
title: 탐색 알고리즘(Searching Algorithms)
date: 2021-08-26 18:08:20
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## 목표

- 검색 알고리즘이란 무엇인가?
- 배열에서 선형 검색 적용
- 정렬된 배열에서 이진 검색 하기
- 순수 문자열 탐색 알고리즘 하기
- KMP 문자열 탐색 알고리즘

## 검색하는 방법?

배열이 주어지면 가장 간단한 방법은, 배열안의 모든 요소들의 값을 보고 그 값이 우리가 원하는 값인지를 보는 방법이다. ⇒ 순차 검색 알고리즘

## 선형 탐색(Linear Search)

자바스크립트에는 기본적으로 선형 검색을 위한 메소드들이 내장되어 있다. 대표적인 메소드들은 다음과 같다.

- indexOf ⇒ 해당 value의 인덱스가 무엇인지 찾아주는 메소드
- includes ⇒ 대상이 되는 배열에 해당 값이 존재하는이 여부를 반환하는 메소드
- find ⇒ 배열내에서 해당하는 값의 인덱스를 반환하는 메소드
- findIndex ⇒ 주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환한다.

## 선형 탐색 의사코드(Linear Search Pseudo code)

의사코드란 ? 알고리즘을 로직을 말로 풀어서 정리한 것을 말한다.

- 배열과, 값을 인자로 받는다.
- 배열을 루핑하면서 현재 배열의 요소가 값과 일치하는지를 확인한다.
- 만약 그렇다면, 찾은 요소의 인덱스를 반환한다.
- 만약 값을 찾을 수 없다면 -1을 반환한다.

## 선형 탐색 예시(Linear Search Example)

```jsx
function linearSearch(arr, val) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === val) return i
  }
  return -1
}
```

### ⇒ 시간 복잡도(Time complexity): O(N)

## 선형 탐색 BIG O(Linear Search BIG O)

- 최고 케이스의 시간 복잡도(Best case of Time Complexity) ⇒ O(1)
- 최악 케이스의 시간 복잡도(Worst case of Time Complexity) ⇒ O(n)
- 평균적인 시간 복잡도(Average of Time Complexity) ⇒ O(n)

## 이진 탐색

- 이진 탐색은 훨씬 빠른 탐색 알고리즘이다. (선형 탐색 보다)
- 한번에 하나씩 요소를 지워나가는 대신, 남은 요소들의 절반을 지워나간다.
- 이진 탐색은 정렬된 배열에서만 작동한다.

## 이진 탐색 의사코드

- 정렬된 배열과 값을 인자로 받는다.
- 배열의 첫 시작점을 왼쪽 포인터로 만들고, 배열의 끝을 오른쪽 포인터로 만든다.
- 왼쪽 포인터가 오른쪽 포인터로 올 때까지, 값을 찾을 때까지 아래 로직을 반복한다.
  - 가운데 점을 가리키는 포인터를 만든다.
  - 만약 원하는 값을 찾았다면 해당 인덱스를 반환한다.
  - 만약 값이 너무 작다면, 왼쪽 포인터의 값을 올린다. ⇒ 우측으로 상향
  - 만약 값이 너무 크다면, 오른쪽 포인터의 값을 내린다. ⇒ 좌측으로 하향
- 만약 값을 찾을 수 없다면 -1을 반환한다.

## 이진 탐색의 예시

```jsx
function binarySearch(arr, elem) {
  // add whatever parameters you deem necessary - good luck!

  let start = 0
  let end = arr.length - 1
  let middle = Math.floor((start + end) / 2)

  while (arr[middle] !== elem && start <= end) {
    if (elem < arr[middle]) {
      end = middle - 1
    } else {
      start = middle + 1
    }
    middle = Math.floor((start + end) / 2)
  }
  return arr[middle] === elem ? middle : -1
}
```

## 이진 탐색의 BIG O(Binary Search BIG O)

- 최악와 평균 케이스(Worst and Average Case) ⇒ O(log n)
- 최고의 케이스(Best Case) ⇒ O(1)

## 순수 문자열 탐색

- 긴 문자열 안에서 작은 문자열이 몇번 등장하는지 세고 싶다고 가정
- 직접적인 접근방법은 개별 문자의 쌍을 체크하는 방법

## 순수 문자열 탐색 의사코드

- 긴 문자열을 루프한다.
- 작은 문자열을 루프한다.
- 만약 문자가 일치하지 않으면, 내부 루프에서 break한다.
- 만약 문자가 일치하면 계속한다.
- 만약 내부 매칭되는 문자를 찾은채로 완료하면, 매치 카운트를 올린다.
- 카운트를 반환한다.

## 순수 문자열 탐색 예시(Example of Naive String Search)

```jsx
function naiveSearch(long, short) {
  let count = 0
  for (let i = 0; i < long.length; i++) {
    for (let j = 0; j < short.length; j++) {
      if (short[j] !== long[i + j]) break
      if (j === short.length - 1) count++
    }
  }
  return count
}
```

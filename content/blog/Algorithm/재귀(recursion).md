---
title: 재귀(Recursion)
date: 2021-08-26 18:08:06
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

# 재귀란 무엇인가?

재귀(Recursion)란 그 자신을 불러오는 과정(이 경우에는 함수)이라고 할 수 있다.

# 어디서 재귀를 찾아볼 수 있는가?

- JSON.parse / JSON.stringify
- document.getElementbyId and DOM traversal algorithms
- Object traversal
- Other complex data structures
- it's sometimes a cleaner alternative to iteration

# 콜 스택(Call stack)

- 콜 스택은 스택 자료구조이다. (LIFO)
- 함수가 호출될 때마다 콜 스택의 top에 위치하게 된다.(push)
- 자바스크립트가 키워드를 반환하거나 함수가 끝날 때, 컴파일러가 해당 함수를 제거하게 된다.(pop)

## 콜 스택을 알아야 하는 이유

- 콜 스택에 함수가 push되고 pop되는 과정을 이해해야 하기 때문
- 재귀 함수를 작성할 때 우리는 계속 콜 스택에 새로운 함수들을 push하게 된다. 따라서 콜 스택의 동작 방식을 알아야 제대로 된 재귀 함수를 작성할 수 있다.

# 재귀 함수는 어떻게 동작하는가?

Base Case에 도달할 때까지 같은 함수를 다른 인풋으로 계속해서 호출한다.

재귀함수의 두 가지 핵심 부분

- Base Case ⇒ 재귀가 끝나는 상황을 가정한 케이스
- Different Input ⇒ 재귀 함수는 매번 다른 input이 들어가야 한다.

## Example 1

```jsx
function sumRange(num) {
  if (num === 1) return 1 // base case
  return num + sumRange(num - 1)

  // base case : => num이 1이 될 때 재귀 종료
  // different input : num - 1 => 재귀가 반복될 때마다 input이 1씩 감소한다
  //
}

console.log(sumRange(3))
```

## Example 2

Factorial

```jsx
// Non recursive function

function factorial(num) {
  let total = 1
  for (let i = num; i > 1; i--) {
    total *= i
  }
  return total
}

console.log(factorial())

// Recursive function

function factorial(num) {
  if (num === 1) return 1 // base case
  return num * factorial(num - 1)
}
```

## 오류가 발생하는 경우

- Base Case가 없다 ⇒ 무한 루프에 빠지므로 return 문이 필요하다.
- return 문을 빼먹거나 다른 값을 return한다.
- Stack Overflow (콜 스택 사이즈가 초과되어 더 이상 콜 스택에 함수를 호출하지 못하는 경우 ⇒ 무한재귀)

## Helper Method Recursion

- 다른 자료구조나 데이터를 선언하여, 그곳에 알고리즘 조건에 해당하는 결괏값을 담아 return하는 방식

```jsx
function collectOddValues(arr) {
  let result = []

  function helper(helperInput) {
    if (helperInput.length === 0) {
      return
    }

    if (helperInput[0] % 2 !== 0) {
      result.push(helperInput[0])
    }

    helper(helperInput.slice(1))
  }

  helper(arr)

  return result
}

collectOddValues([1, 2, 3, 4, 5, 6, 7, 8, 9])
```

## Pure Recursion

- 리스트를 변형하지 않고, 새롭게 만든 데이터를 담아 합치는 방식으로 재귀 구현. 단 객체는 Object.assign이나 spread 연산자를 사용한다.
- 배열에서는 slice같은 메서드나, spread 연산자, concat 등으로 배열의 복사본을 만든므로, 원본을 변화시키지 않는다.
- 문자열은 변환할 수 없다는 것을 기억한다. 따라서 slice, substr 또는 substring을 통해 string의 복사본을 만든다.
- 객체의 복사본을 만들기 위해서 Object.assign 혹은 spread 연산자를 사용한다.

```jsx
function collectOddValues(arr) {
  let newArr = []

  if (arr.length === 0) {
    // base case
    return newArr
  }

  if (arr[0] % 2 !== 0) {
    newArr.push(arr[0])
  }

  newArr = newArr.concat(collectOddValues(arr.slice(1))) // index 0을 제외한 나머지 값들을 copy

  return newArr
}

collectOddValues([1, 2, 3, 4, 5, 6, 7, 8, 9])
```

### Test

1. Power function

```jsx
// power(2, 0) => 1
// power(2, 2) => 4
// power(2, 4) => 16

function power(base, exponent) {
  if (exponent === 0) return 1

  return base * power(base, exponent - 1)
}
```

2. Factorial

```jsx
function factorial(num) {
  if (num === 1) return 1 // base case
  return num * factorial(num - 1)
}
```

3. productOfArray

```jsx
function productOfArray(arr) {
  if (arr.length === 0) {
    return 1
  }
  return arr[0] * productOfArray(arr.slice(1))
}
```

- Array.slice(1) ⇒ 인덱스 0번을 제외한 나머지 값들 출력, length < 2이면 빈 배열 return

4. recursiveRange

```jsx
function recursiveRange(num) {
  if (num === 0) return num

  return num + recursiveRange(num - 1)
}
```

5. Fibonacci

```jsx
function fib(n) {
  if (n <= 2) return 1
  return fib(n - 1) + fib(n - 2)
}
```

6. reverse

```jsx
function reverse(word) {
  // add whatever parameters you deem necessary - good luck!

  let newWord = []

  if (word.split('').length === 0) return newWord.join('')

  newWord.push(word.split('').slice(-1)[0])

  newWord = newWord.concat(
    reverse(
      word
        .split('')
        .slice(0, word.length - 1)
        .join('')
    )
  )

  return newWord.join('')
}
```

7. isPalindrome

⇒ 앞으로 읽으나 뒤로 읽으나 같은 단어를 Palindrome이라고 한다.

```jsx
function isPalindrome(word) {
  // add whatever parameters you deem necessary - good luck!

  if (word.split('')[0] !== word.split('').splice(-1)[0]) return false

  let newWord = []

  newWord = word.split('')

  newWord.shift()
  newWord.pop()

  console.log(newWord)

  if (newWord.length <= 1) return true

  isPalindrome(newWord.join(''))

  return true
}
```

8. someRecursive

```jsx
function someRecursive(array, callback) {
  if (array.length === 0) return false
  if (callback(array[0])) return true
  return someRecursive(array.slice(1), callback)
}
```

9. flattern

```jsx
function flatten(arr) {
  // add whatever parameters you deem necessary - good luck!

  let newArr = []

  if (arr.length === 0) return arr

  if (Array.isArray(arr[0])) {
    newArr = newArr.concat(flatten(arr[0]))
  } else {
    newArr.push(arr[0])
  }

  newArr = newArr.concat(flatten(arr.slice(1)))

  return newArr
}
```

10. capitalizeFirst

```jsx
function capitalizeFirst(wordArr) {
  // add whatever parameters you deem necessary - good luck!
  if (wordArr.length === 0) return wordArr

  let newArr = []
  let newWord = wordArr[0].split('')

  newArr.push(
    newWord[0].toUpperCase() + newWord.splice(1, newWord.length).join('')
  )

  newArr = newArr.concat(capitalizeFirst(wordArr.slice(1)))

  return newArr
}

capitalizeFirst(['car', 'taco', 'banana']) // ['Car','Taco','Banana']
```

11. nestedEvenSum

```jsx
function nestedEvenSum(obj, sum = 0) {
  for (let key in obj) {
    if (typeof obj[key] === 'object') {
      sum += nestedEvenSum(obj[key])
    } else if (typeof obj[key] === 'number' && obj[key] % 2 === 0) {
      sum += obj[key]
    }
  }
  return sum
}

var obj1 = {
  outer: 2,
  obj: {
    inner: 2,
    otherObj: {
      superInner: 2,
      notANumber: true,
      alsoNotANumber: 'yup',
    },
  },
}

var obj2 = {
  a: 2,
  b: { b: 2, bb: { b: 3, bb: { b: 2 } } },
  c: { c: { c: 2 }, cc: 'ball', ccc: 5 },
  d: 1,
  e: { e: { e: 2 }, ee: 'car' },
}

nestedEvenSum(obj1) // 6
// nestedEvenSum(obj2); // 10
```

12. capitalizeWords

```jsx
function capitalizeWords(words) {
  // add whatever parameters you deem necessary - good luck!
  let newWords = []

  if (words.length === 0) return words

  let newWord = words[0].toUpperCase()

  newWords.push(newWord)

  newWords = newWords.concat(capitalizeWords(words.splice(1)))

  return newWords
}
```

13. stringifyNumbers

```jsx
function stringifyNumbers(obj) {
  var newObj = {}
  for (var key in obj) {
    if (typeof obj[key] === 'number') {
      newObj[key] = obj[key].toString()
    } else if (typeof obj[key] === 'object' && !Array.isArray(obj[key])) {
      newObj[key] = stringifyNumbers(obj[key])
    } else {
      newObj[key] = obj[key]
    }
  }
  return newObj
}

/*
let obj = {
    num: 1,
    test: [],
    data: {
        val: 4,
        info: {
            isRight: true,
            random: 66
        }
    }
}
/*

stringifyNumbers(obj)

/*
{
    num: "1",
    test: [],
    data: {
        val: "4",
        info: {
            isRight: true,
            random: "66"
        }
    }
}
*/
```

14. collectStrings

```jsx
function collectStrings(obj) {
  let strArr = []

  for (let key in obj) {
    if (typeof obj[key] === 'string') {
      strArr.push(obj[key])
    } else if (typeof obj[key] === 'object') {
      strArr = strArr.concat(collectStrings(obj[key]))
    }
  }
  return strArr
}
```

---
title: 해시 테이블(Hash Tables)
date: 2021-08-26 19:08:37
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

## 해시 테이블이란 무엇인가?

- 해시 테이블은 key-value 쌍을 저장하는 자료 구조 이다.
- 배열하고 닮았지만, key에는 순서가 없다.
- 배열과는 달리 해시 테이블은 다음과 같은 기능에서 빠른 속도를 보여준다.
  - 값을 찾는 것
  - 새로운 값을 추가하는 것
  - 값을 삭제하는 것

## 해시 테이블을 알아야 하는 이유

거의 모든 프로그래밍 언어에서 일종의 해시 테이블 자료구조를 갖고 있다. 속도면에서 해시 테이블은 일반적으로 많이 사용되기 때문이다.

Ex)

- Python - Dictionaries
- JS - Objects,Maps\*
- Jave , Go & Scala have Maps
- Ruby has Hashed

## 해시 부분

- 값들을 key를 통해 조회하기 위해서 유효한 배열의 index를 key로 치환하는 방식으로 구현한다.
- 이 방식을 구현하는 함수를 해시 함수라 한다.

## 좋은 해시를 만들기 위해선

1. 빨라야 한다. (일정한 시간)
2. 일정하게 분포되어 있어야한다. (특정 인덱스에 집중되어 생성되는 것이 아니라 골고루 분포되야 좋은 해시라고 할 수 있다.)
3. Deterministic ⇒ 어떤 해시값을 넣으면 그 값이 정확히 도출되야 한다.(무작위면 안됨)

## 기본 해시 함수

```jsx
function hash(key, arrayLen) {
  let total = 0
  for (let char of key) {
    let value = char.charCodeAt(0) - 96
    total = (totla + value) % arraLen
  }
  return total
}
```

## 해시 함수의 개선

1. 해시는 String 만을 사용해야 한다.
2. 일정하지 않은 시간 - key 길이에 따라 선형으로 시간이 증가한다. (이상적이지 않음)
3. 더 무작위적으로 구현해야함 - 특정값에 해시가 집중될 수 있음

### 수정버전 1

```jsx
function hash(key, arrayLen) {
  let total = 0
  let WEIRD_PRIME = 31
  for (let i = 0; i < Math.min(key.length, 100); i++) {
    let char = key[i]
    let value = char.charCodeAt(0) - 96
    total = (total * WEIRD_PRIME + value) % arrayLen
  }
  return total
}
```

- key의 길이에 따라 100이하는 유동적으로 변하지만, 100이상인 경우 100으로 고정한다. (속도를 일정하게 하기 위해서 - 2번 조건 개선)
- WEIRD_PRIME이라는 소수를 곱하게 함으로써 해시 충돌을 줄이게 된다. (3번 조건 개선)

## 충돌 해결하기

훌륭한 해시함수와 큰 길이의 배열을 갖고 있더라도, 해시 충돌은 피할수가 없다.

- 충돌을 해결하기 위한 다양한 전략이 있지만 대표적으로 2가지 방법이 있다.
  - 분리 체이닝
  - 선형 탐사

## 분리 체이닝(Separate Chaining)

분리 체이닝을 통해 배열의 각 인덱스에는 좀 더 정교한 자료구조 사용해 값을 저장한다.

⇒ 충돌이 발생하면 다차원 배열 혹은 링크드 리스트로 구현하여, 해당 인덱스에 저장되어 있는 자료구조를 루핑해서 값을 찾는 방식

## 선형 탐사(Linear Probing)

충돌을 발견하면 선형 탐사를 통해 다른 빈 공간을 찾아서 저장하는 방식이다.

분리 체이닝과는 달리 각 인덱스에는 하나의 키-값 쌍이 들어가게 된다.

⇒ 충돌이 발생하면, 다른 빈공간을 찾아서 그 키 값을 채워넣는 방

## 해시 테이블 클래스 예

```jsx
class HashTable {
  constructor(size = 53) {
    this.keyMap = new Array(size)
  }

  _hash(key) {
    let total = 0
    let WEIRD_PRIME = 31
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      let char = key[i]
      let value = char.charCodeAt(0) - 96
      total = (total * WEIRD_PRIME + value) % this.keyMap.length
    }
    return total
  }
}
```

## Set / Get 메소드 의사코드

### Set 메소드

- 키와 값을 인자로 받는다.
- 키를 해시 값으로 만든다.
- 키와 값 쌍을 분리 체이닝을 사용해 해시 테이블에 저장한다.

### Get 메소드

- 키를 인자로 받는다.
- 키를 해시 값으로 만든다.
- 해시 테이블의 키-값 쌍을 가져온다.
- 키를 찾을 수 없다면 undefined를 반환한다.

### Set / Get 메소드 구현 예시

```jsx
set(key, value) {
        let index = this._hash(key);
        if(!this.keyMap[index]) {
            this.keyMap[index] = [];
        }
        this.keyMap[index].push([key, value]);
        return index;
    }

    get(key) {
        let index = this._hash(key);
        if(this.keyMap[index]) {
            for(let i = 0; i<this.keyMap[index].length; i++) {
                if(this.keyMap[index][i][0] === key) {
                    return this.keyMap[index][i][1]
                }
            }
            return this.keyMap[index];
        }
        return undefined

    }
```

## Keys / Values 메소드 의사코드

### Keys

- 해시 테이블 배열을 루핑하여 테이블의 키 값 배열을 반환한다.

### Values

- 해시 테이블 배열을 루핑하여 테이블의 값 배열을 반환한다.

### Keys, Values 구현 예시

```jsx
keys() {
        let keysArr = [];

        for(let i = 0; i<this.keyMap.length; i++) {
            if(this.keyMap[i]) {
                for(let j = 0; j < this.keyMap[i].length; j++) {
                    if(!keysArr.includes(this.keyMap[i][j][0])) {
                        keysArr.push(this.keyMap[i][j][0])
                    }

                }
            }
        }

        return keysArr;
    }

    values() {
        let valuesArr = [];

        for(let i = 0; i<this.keyMap.length; i++) {
            if(this.keyMap[i]) {
                for(let j = 0; j < this.keyMap[i].length; j++) {
                    if(!valuesArr.includes(this.keyMap[i][j][1])) {
                        valuesArr.push(this.keyMap[i][j][1])
                    }

                }
            }
        }

        return valuesArr;
    }
```

## 해시 테이블의 Big O (일반적인 케이스)

- 삽입(Insert) : $O(1)$
- 삭제(Deletion) : $O(1)$
- 접근(Access) : $O(1)$
- 최악 케이스 ⇒ $O(N)$

## 요약

- 해시 테이블은 키-값 쌍의 콜렉션이다.
- 해시 테이블은 주어진 키로 값을 빠르게 찾을 수 있다.
- 해시 테이블은 새로운 키-값을 빠르게 추가할 수 있다.
- 해시 테이블은 키를 해싱함으로써, 큰 배열에 자료를 저장한다.
- 좋은 해시는 빨라야 하고, 키가 골고루 분포되어야 하며, 결정적이어야 한다.
- 같은 인덱스에 두 개의 키를 해시하는 방법으로 분리 체이닝과 선형 탐사라는 두 가지 전략이 있다.

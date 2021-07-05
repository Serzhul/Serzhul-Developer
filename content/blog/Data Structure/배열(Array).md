---
title: 배열(Array)
date: 2021-07-05 19:07:37
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

## **꼭 알아둬야 할 자료 구조: 배열 (Array)**

- 데이터를 나열하고, 각 데이터를 인덱스에 대응하도록 구성한 데이터 구조
- 파이썬에서는 리스트 타입이 배열 기능을 제공함

### 1. 배열은 왜 필요할까?

- 같은 종류의 데이터를 효율적으로 관리하기 위해 사용
- 같은 종류의 데이터를 순차적으로 저장
- 장점:
  - 빠른 접근 가능
    - 첫 데이터의 위치에서 상대적인 위치로 데이터 접근(인덱스 번호로 접근)
- 단점:
  - 데이터 추가/삭제의 어려움
    - 미리 최대 길이를 지정해야 함

> 엑셀로 이해해보기

### String 변수

```python
country = 'US'
print (country)
```

### **2. 파이썬과 배열**

- 파이썬에서는 리스트로 배열 구현 가능

```python
# 1차원 배열: 리스트로 구현시
data_list = [1, 2, 3, 4, 5]
print(data_list) # [1, 2, 3, 4, 5]

# 2차원 배열: 리스트로 구현시
data_list = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(data_list) # [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# 출력 예시

print (data_list[0])     # [1, 2, 3]

print (data_list[0][0])  # 1
print (data_list[0][1])  # 2
print (data_list[0][2])  # 3
print (data_list[1][0])  # 4
print (data_list[1][1])  # 5
```

### **참고**

- range(stop): range(10)은 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
- range(start, stop): range(1, 11)은 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
- range(start, stop, step): range(0, 20, 2)은 0, 2, 4, 6, 8, 10, 12, 14, 16, 18
  - start, stop, step은 음수로 지정 가능

### 정리

- 유사한 데이터를 유사한 공간에 넣는 것
- 맨 앞에 있는 주소의 인덱스에 따라 값을 찾을 수 있다.
- 최대 길이를 알지 못하면, 삽입 삭제가 어렵다.

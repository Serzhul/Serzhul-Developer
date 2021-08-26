---
title: 배열과 객체의 성능 분석 (Big O)
date: 2021-08-26 18:08:82
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

# 객체(Objects)

- 객체는 순서가 없지만 key와 value을 사용해 빠르다는 장점이 있다.
- Big O로 보는 성능은 다음과 같다.
  - 삽입(Insertion) ⇒ $O(1)$
  - 삭제(Removal) ⇒ $O(1)$
  - 검색(Searching) ⇒ $O(N)$
  - 접근(Access) ⇒ O(1)

# 객체 메소드들의 Big O

- Object.keys - $O(N)$
- Object.values - $O(N)$
- Object.entries - $O(N)$
- hasOwnProperty - $O(1)$ (Access개념이므로)

⇒ hasOwnProperty를 제외한 나머지 메소드들은 Obejct에 할당된 모든 데이터들을 1번은 순회해야하므로 Linear한 complexity를 갖는다.

# 배열

- 순서가 있는 목록으로, 순서가 필요할 때 사용하는 자료구조이다.
- 빠른 접근이 가능하다.
- index만 있으면 특정 data의 위치에 접근할 수 있기 때문에 접근은 일정하다.(constant) ⇒ $O(1)$
- 검색은 Object와 마찬가지로 Linear하다 $O(N)$

# 배열의 메소드들과 시간복잡도

- Array.push ⇒ $O(1)$ : 끝에 추가하는 것이므로 constant
- Array.pop ⇒ $O(1)$ : 끝에 빼는 것이므로 constant
- Array.shift ⇒ $O(N)$ :
- Array.unshift ⇒ $O(N)$
- Array.concat ⇒ $O(N)$
- Array.slice ⇒ $O(N)$
- Array.splice ⇒ $O(N)$
- Array.sort ⇒ $O(N*log N)$
- Array.forEach/map/filter/reduce/etc. ⇒ $O(N)$

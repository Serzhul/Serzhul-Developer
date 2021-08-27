---
title: Big O Notation (빅 오 표기법)
date: 2021-08-26 18:08:29
category: Algorithm
thumbnail: { thumbnailSrc }
draft: false
---

- 빅 오 표기법(Big O Notation) 은 알고리즘에서 가장 최선의 방법을 찾는 기준이 되는 방법이다.
- 코드에서 속도 저하나 크래시가 발생할 경우 비효율적인 부분을 찾아서 수정할 수 있는 기준이 된다.
- 알고리즘은 기본적으로 정답이 없지만, 빅 오 표기법을 통해 더 좋은 코드를 만들기 위한 기준점을 정할 수 있게 된 것이다.
- 빅 오 표기법에서 정확도는 중요하지 않으며 일반적인 경향에 대해 측정한다(linear, quadratic, constant)

  $ex) linear => O(N)$

  $quadratic => O(N^2)$

  $constant => O(1)$

### 더 좋은 코드란?

- 더 빠른 코드, 더 적은 메모리를 사용하는 코드, 더 읽기 쉬운 코드 등이 기준이 될 수 있다.
- 알고리즘에서 더 좋은 코드를 정하는 기준은 보통 시간복잡도(Time Complexity)와 공간 복잡도(Space Complexity)가 있다.

- [Time complexity(시간복잡도)](<https://serzhul.io/Algorithm/time-complexity(%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84)/>)

- [Space Complexity(공간 복잡도)](<https://serzhul.io/Algorithm/space-complexity(%EA%B3%B5%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84)/>)

$cf)$ 로가리듬 복잡도(Logarithms Complexity)

- 로그로 복잡도를 계산하는 방식으로, 로가리듬에 의한 연산의 반복으로 복잡도를 측정한다.
- 로가리듬으로 구성된 알고리즘은 매우 효과적인 알고리즘이다.
- 회귀는 때때로 로가리듬적인 공간복잡도를 갖는다.

---
title: 단방향 링크드리스트(Singly Linked Lists)
date: 2021-08-26 19:08:43
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

# 링크드 리스트란 무엇인가?

- 링크드 리스트는 head와 tail 그리고 length라는 property를 가지고 있는 자료구조이다.
- 링크드 리스트는 node로 구성되어있고, 각각의 노드는 각각의 값과 다른 노드 간의 포인터, 혹은 null값으로 구성된다.

# 배열과의 특징 비교

### 링크드리스트

- 인덱스를 갖고 있지 않다.
- next라는 포인터를 통해 노드끼리 연결되어 있다.
- 무작위 접근이 불가능하다.

### 배열

- 각각의 요소엔 순서대로 인덱스가 매겨져 있다.
- 삽입과 삭제가 복잡하다.
- 특정 인덱스에 빠르게 접근할 수 있다.

# 링크드 리스트의 Class 구조 예시

```jsx
class Node {
  constructor(val) {
    this.val = val
    this.next = null
  }
}

class SinglyLinkedList {
  constructor() {
    this.head = null
    this.tail = null
    this.length = 0
  }
}
```

## Start Code

```jsx
class Node {
  constructor(val) {
    this.val = val
    this.next = null
  }
}

class SinglyLinkedList {
  constructor() {
    this.length = 0
    this.head = null
    this.tail = null
  }
  push(val) {}
}
```

- Node 객체와, SinglyLinkedList 객체를 선언한다.
- 각각의 객체는 val, next 그리고 length,head,tail을 생성자로 초기화한다.
- push 메소드를 구현한다.

# 링크드리스트 메소드들

## push

- 푸쉬는 링크드 리스트의 tail에 값을 삽입하는 방식이다.

## push 메소드의 의사코드

- (삽입하고자 하는) 값을 인자로 받는다.
- 함수에 넘겨줄 값은 새로운 노드로 만들어준다.
- 만약 링크드리스트에 head 값이 없으면, head와 tail에 새롭게 만든 노드를 넣어준다.
- 그렇지 않다면, 원래 tail의 next 값에 새로운 노드를 할당하고, 새로운 노드를 tail로 할당한다.
- 이 작업을 할 때마다 length는 1씩 증가한다.
- linkelist 객체를 반환한다.

```jsx
class Node {
  constructor(val) {
    this.val = val
    this.next = null
  }
}

class SinglyLinkedList {
  constructor() {
    this.length = 0
    this.head = null
    this.tail = null
  }

  push(val) {
    var newNode = new Node(val)

    if (!this.head) {
      this.head = newNode
      this.tail = this.head
    } else {
      this.tail.next = newNode
      this.tail = newNode
    }
    this.length++
    return this
  }
}
```

# pop

- pop 메소드는 링크드리스트에서 끝(tail)의 값을 추출하며, 해당 값을 리스트로 부터 제거하는 메소드이다.

# Pop 메소드의 의사코드

- 만약 리스트에 노드가 없다면, undefined를 반환한다.
- tail에 도달할 때까지 리스트를 순회한다.
- 가장 마지막에서 두 번째 노드의 다음 속성을 null로 한다.
- tail을 가장 마지막에서 두 번째 노드로 한다.
- list의 길이를 1 감소시킨다.
- 제거된 노드의 값을 반환한다.

```jsx
pop() {
        var current = this.head;

        if(!current) return undefined;

        while(current.next) {
            var tail = current
            current = current.next;
        }

        this.tail = tail;
        this.tail.next = null;

        this.length--;

        return current;
    }
```

## Shift

Shift 메소드는 링크드 리스트의 시작점에서 새로운 노드를 제거한다.

## Shift 의사코드

- 만약 노드가 없다면, undefined를 반환한다.
- 현재 head 속성의 값을 변수로 저장한다.
- head 속성을 현재 head의 next 속성으로 저장한다.
- 리스트의 길이를 1 감소시킨다.
- 제거된 노드의 값을 반환한다.

```jsx
shift() {

        if(!this.head) return undefined;

        var currentHead = this.head;
        var newHead = current.next;
        this.head = newHead;

        this.length--;

        if(this.length === 0) {
            this.head = null;
            this.tail = null;
        }

        return currentHead
    }
```

## Unshift

링크드 리스트의 시작점에 새로운 노드를 추가한다.

## Unshift 의사코드

- Unshift 메소드는 값을 인자로 받는다.
- 함수에 값을 전달하여 새로운 노드를 만든다.
- 만약 리스트에 head 속성 값이 없다면, head와 tail 속성 값을 새로 만든 노드로 한다.
- 그렇지 않다면 새롭게 만든 노드의 next 속성은 현재 리스트의 head 속성으로 한다.
- 리스트의 head 속성은 새로 만든 노드로 한다.
- 리스트의 길이를 1 증가 시킨다.
- 링크드 리스트를 반환한다.

```jsx
unshift(val) {
        var newNode = new Node(val);

        if(!this.head) {
            this.head = newNode;
            this.tail = this.head;
            this.length++;
            return this;
        }

        var currentHead = this.head;
        this.head = newNode;
        this.head.next = currentHead;

        this.length++;

        return this;
    }
```

## Get

링크드 리스트의 특정 위치에 있는 노드를 반환한다.

## Get 의사코드

- Get 메소드는 index 값을 인자로 받는다.
- 만약 index가 0보다 작거나 리스트의 길이보다 크거나 같으면 null을 반환한다.
- index에 도달할 때까지 리스트를 순회하고, 특정 인덱스의 노드를 반환한다.

```jsx
get(index) {
        var curIndex = 0;
        var current = this.head

        if(index < 0 || index >= this.length) return null

        while(curIndex !== index) {
           current = current.next;
           curIndex++;
        }

        return current;
    }
```

## Set

Set 메소드는 링크드 리스트의 특정 위치의 노드 값을 바꾼다.

# Set 의사코드

- Set 메소드는 index와 값을 인자로 받는다.
- 특정 노드를 찾는 데에는 get 메소드를 사용한다.
- 노드를 찾을 수 없다면 false를 반환한다.
- 노드를 찾았다면, 함수를 통해 전달 받은 값을 노드에 set하고 true를 반환한다.

```jsx
set(index, val) {

        var current = this.get(index);

        if(!this.get(index)) return false;

        current.val= val;

        return true;
    }
```

## Insert

링크드 리스트의 특정 위치에 노드를 추가한다.

## Insert 의사코드

- index가 0보다 작거나 리스트의 길이보다 크다면, false를 반환한다.
- index가 list의 길이와 같다면, 리스트의 끝에 새로운 노드를 push한다.
- index가 0이라면, 리스트의 시작점에 새로운 노드를 unshift한다.
- 그렇지 않다면 get 메소드를 활용해 index-1 위치의 노드에 접근한다.
- 해당 노드의 next속성을 새로운 노드로 한다.
- 리스트 길이를 1 증가시킨다.
- true를 반환한다.

```jsx
insert(index, val) {
       if(index < 0 || index > this.length) {
           return false
       }
       if(index === this.length) return this.push(val)

       if(index === 0) return this.unshift(val);

           let newNode = new Node(val);
           let prev = this.get(index - 1);
           let cur = this.get(index);

           newNode.next = cur;
           prev.next = newNode;
           this.length++;

       return true;
    }
```

## Remove

링크드 리스트의 특정 위치에서 노드를 제거한다.

## Remove 의사코드

- index가 0보다 작거나 리스트의 length보다 크다면, undefined를 반환한다.
- index가 리스트의 길이 - 1와 같다면 pop한다.
- index가 0이라면 shift한다.
- 그렇지 않다면 get 메소드를 사용해 index - 1 위치의 노드에 접근한다.
- 해당 노드의 next property 속성을 next의 next 노드로 한다.
- 리스트의 길이를 1 감소 시킨다.
- 제거된 노드의 값을 반환한다.

```jsx
remove(index) {
       if(index < 0 || index > this.length) {
           return undefined
       }
       if(index === this.length - 1) return this.pop(val)

       if(index === 0) return this.shift(val);

           let prev = this.get(index - 1);
           let removed = prev.next;
           prev.next = removed.next;
           this.length--;

       return true;

    }
```

## Reverse

링크드 리스트를 거꾸로 뒤집는다.

## Reverse 의사코드

- head와 tail을 스왑한다.
- next라는 변수를 새로 만든다.
- prev라는 변수를 새로 만든다.
- 노드라는 새로운 변수를 만들고 head 속성을 초기화 한다.
- list를 순회한다.
- next 변수에 노드의 next속성 값을 할당한다.
- next 속성값에 노드를 할당한다.
- prev 변수에 node 변수의 값을 할당한다.
- node 변수에 next 변수의 값을 할당한다.

```jsx
reverse() {

        var node = this.head;
        this.head = this.tail;
        this.tail = node;

        var prev = null;
        var next;

        for(var i = 0; i < this.length; i++) {
            next = node.next;
            node.next = prev;
            prev = node;
            node = next;
        }
        return this;
    }
```

⇒ 스왑 원리를 사용해서 prev, next를 node(현재 노드) 기준으로 분류하여 둘을 스왑하는 방식으로 진행

## 단반향 링크드 리스트의 Big O

- 삽입 - $O(1)$
- 제거 - 값에 따라 $O(1) or O(n)$
- 탐색 - $O(n)$
- 접근 - $O(n)$

## 요약

- 단방향 링크드 리스트는 시작점에서 요소의 삽입(insertion)과 제거(deletion) 자주 요구되는 경우에 배열의 훌륭한 대체재가 될 수 있다.
- 배열은 index를 내장하고 있지만 링크드 리스트는아니다.
- 노드로 이루어져 있는 다른 자료구조로는 Stack과 Queue가 있다.

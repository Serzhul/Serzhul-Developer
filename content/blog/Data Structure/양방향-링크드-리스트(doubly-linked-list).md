---
title: 양방향 링크드 리스트(Doubly Linked List)
date: 2021-08-26 19:08:75
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

## 양방향이란?

단방향 링크드 리스트와 거의 유사하지만, 모든 노드가 previous라는 또 다른 pointer를 갖고 있는 것이 특징이다.

## 단방향 링크드 리스트와의 비교

- 단방향 링크드 리스트에 비해 더 많은 메모리를 차지하지만 대신 더 유연성을 갖고 있다.
- 즉, 메모리와 유연성은 거의 항상 상충관계에 있음을 알 수 있다.

# 양방향 링크드 리스트 메소드들

## Pushing

Pushing 메소드는 양방향 링크드 리스트의 끝에 노드를 추가하는 기능을 갖는다.

## Pushing 의사코드

- 함수로 부터 전달받은 값으로 새로운 노드를 만든다.
- head 속성이 null이라면, head와 tail에 새로운 노드를 할당한다.
- 아니라면, tail의 next 속성에 해당 노드를 할당한다.
- 새로 만든 노드의 previous 속성에는 tail을 할당한다.
- tail에 새로운 노드를 할당한다.
- 리스트의 길이를 1 증가시킨다.
- 양방향 링크드 리스트를 반환한다.

```jsx
push(val) {
        var newNode = new Node(val);
        if (this.length === 0) {
            this.head = newNode;
            this.tail = newNode;
        } else {
            this.tail.next = newNode;
            newNode.prev = this.tail;
            this.tail = newNode;
        }
        this.length++;
        return this

    }
```

## Popping

Popping 메소드는 양방향 링크드 리스트의 끝 값을 제거한다.

## Popping 메소드 의사코드

- head가 없다면, undefined를 반환한다.
- 현재 tail 값을 변수로 저장하고 나중에 반환한다.
- 리스트의 길이가 1 이라면, head와 tail을 null로 할당한다.
- tail을 previous Node로 업데이트한다.
- newTail의 next값을 null로 할당한다.
- 길이를 감소시킨다.
- 제거된 값을 반환한다.

```jsx
pop() {
        if(!this.head) return undefined;

        var popItem = this.tail;

        if(this.length === 1) {
            this.head = null
            this.tail = null
        }
        else {
            this.tail = popItem.prev;
            this.tail.next = null;
            popItem.prev = null;
        }

        this.length--;

        return popItem;

    }
```

## Shifting

Shifting 메소드는 양방향 링크드 리스트의 시작점의 노드를 제거한다.

## Shifting 메소드 의사코드

- 리스트의 길이가 0이라면 undefined를 반환한다.
- 현재 head 속성값을 변수에 저장한다.
- 리스트의 길이가 1이라면
  - head와 tail을 null로 한다.
- 현재 head을 이전 head의 next로 업데이트 한다.
- head의 prev(previous)을 null로 한다.
- 이전 head의 next를 null로 한다.
- 리스트의 길이를 1 감소시킨다.
- 이전 head를 반환한다.

```jsx
shift() {
        if(this.length === 0 ) return undefined;

        var oldHead = this.head

        if(this.length === 1) {
            this.head = null;
            this.tail = null;
        }
        else {
            this.head = oldHead.next
            this.head.prev = null;
            oldHead.next = null;

        }

        this.length--;
        return oldHead;

    }
```

## Unshifting

Unshifting 메소드는 양방향 링크드 리스트의 시작점에 노드를 추가한다.

## Unshifting 메소드 의사코드

- 함수를 통해 전달받은 값을 새로운 노드로 만든다.
- 리스트의 길이가 0이라면 head와 tail을 새로 만든 node로 한다.
- 그렇지 않다면
  - 리스트의 head의 prev 속성을 새로 만든 node로 한다.
  - 새로운 노드의 next 속성을 head로 한다.
  - head를 새로운 노드로 업데이트 한다.
- 리스트의 길이를 증가시킨다.
- 링크드 리스트를 반환한다.

```jsx
 unshift(val) {
        var newNodw = new Node(val);
        if(this.length === 0) {
            this.head = newNode;
            this.tail = newNode;
        }
        else {
            var oldHead = this.head;
            oldHead.prev = newNode;
            this.head = newNode;
            this.head.next = oldHead;
        }

        this.length++;

        return this;
    }
```

## Get

Get 메소드는 양방향 링크드 리스트의 특정 위치에 접근하도록 한다.

## Get 메소드 의사코드

- index가 0보다 작거나 리스트의 길이보다 크거나 같다면 null을 반환한다.
- index가 리스트의 길이의 절반보다 작거나 같다면
  - 리스트의 head부터 middle(리스트의 길이의 절반)까지 순회한다.
  - node를 찾으면 반환한다.
- index가 길이의 절반보다 크거나 같다면
  - 리스트의 tail부터 middle까지 순회한다.
  - node를 찾으면 반환한다.
- 노드를 반환한다.

```jsx
get(index) {

        if(index < 0 || index >= this.length) return null;

        if(index <= Math.floor(this.length / 2)) {

            var current = this.head;
            var curIndex = 0;
            while(index !== curIndex) {
                current = current.next
                curIndex++;
            }
            return current;
        }
        else {
            var current = this.tail;
            var curIndex = this.length - 1;
            while(index !== curIndex) {
                current = current.prev
                curIndex--;
            }
            return current;
        }

    }
```

## Set

Set 메소드는 양방향 링크드 리스트의 노드 값을 대체한다.

## Set 메소드 의사코드

- 함수를 통해 전달받은 index로 얻은 get 메소드의 결괏값을 저장할 변수를 생성한다.
  - get 메소드가 유효한 node를 반환하면, 함수를 통해 전달받은 값을 그 노드의 값으로 할당한다.
  - true를 반환한다.
- 아니라면 false를 반환한다.

```jsx
set(index, val) {

        if(this.get(index) !== null) {
            var target = this.get(index);

            target.val = val;
            return true
        }
        else {
            return false
        }

    }
```

## Insert

Insert 메소드는 양방향 링크드 리스트의 특정 위치에 노드를 추가한다.

## Insert 메소드 의사코드

- index가 0보다 작거나 리스트의 길이보다 크거나 같으면 false를 반환한다.
- index가 0이라면 unshift한다.
- index가 리스트의 길이와 같다면 push한다.
- get 메소드를 활용해 index-1의 노드에 접근한다.
- 모든 노드들을 제ㅐ로 연결하기 위해 next와 prev 값을 할당해줘야 한다.
- 리스트의 길이를 1 증가한다.
- true를 반환한다.

```jsx
insert(index, val) {

        if(index < 0 || index >= this.length) return false

        if(index === 0) return this.unshift(val);

        if(index === this.length - 1) return this.push(val);

        var newNode = new Node(val);

        var beforeNode = this.get(index-1);
        var afterNode = beforeNode.next;

        beforeNode.next = newNode;
        newNode.prev = beforeNode;
        newNode.next = afterNode;
        afterNode.prev = newNode;

        this.length++;

        return true;

    }
```

## Remove

Remove 메소드는 양방향 링크드 리스트의 특정 위치의 노드를 제거한다.

## Remove 메소드 의사코드

- index가 0보다 작거나 리스트의 길이보다 크거나 같다면 undefined를 반환한다.
- index가 0이라면 shift한다.
- index가 리스트 길이 -1이라면 pop한다.
- get 메소드를 사용해 지워야할 item을 가져온다.
- 리스트에서 찾은 지워야할 노드의 next와 prev 속성들을 업데이트 한다.
- 찾은 노드의 next와 prev 값을 null로 한다.
- 리스트의 길이를 1만큼 감소시킨다.
- 지운 node를 반환한다.

```jsx
remove(index) {
        if(index < 0 || index >= this.length) return false

        if(index === 0) return this.shift();

        if(index === this.length - 1) return this.pop();

        var beforeNode = this.get(index -1);
        var targetNode = beforeNode.next;
        var afterNode = targetNode.next;

        beforeNode.next = afterNode;
        afterNode.prev = beforeNode;

        targetNode.prev = null;
        targetNode.next = null;

        this.length--;

        return targetNode;

    }
```

# 양방향 링크드 리스트의 Big O

## Big O of Doubly Linked Lists

- 삽입(Insertion) - $O(1)$
- 삭제(Removal) - $O(1)$
- 탐색(Searching)- $O(N)$
- 접근(Access) - $O(N)$

## 요약

- 양방향 링크드 리스트는 previous node라는 추가적인 포인터를 제외하곤 거의 비슷하다.
- 단방향 링크드 리스트보다 노드를 찾는데 유용하다. (시간적으로는 절반밖에 안 걸린다.)
- 그러나 추가 포인터 때문에 더 많은 메모리를 차지하게 된다.

---
title: 링크드리스트(Linkedlist)
date: 2021-07-06 20:38:37
category: Data Structure
thumbnail: { thumbnailSrc }
draft: false
---

## **대표적인 데이터 구조: 링크드 리스트 (Linked List)**

### **1. 링크드 리스트 (Linked List) 구조**

- 연결 리스트라고도 함
- 배열은 순차적으로 연결된 공간에 데이터를 나열하는 데이터 구조
- 링크드 리스트는 떨어진 곳에 존재하는 데이터를 화살표로 연결해서 관리하는 데이터 구조
- 본래 C언어에서는 주요한 데이터 구조이지만, 파이썬은 리스트 타입이 링크드 리스트의 기능을 모두 지원
- 링크드 리스트 기본 구조와 용어
  - `노드(Node)`: 데이터 저장 단위 (데이터값, 포인터) 로 구성
  - `포인터(pointer)`: 각 노드 안에서, 다음이나 이전의 노드와의 연결 정보를 가지고 있는 공간
- 일반적인 링크드 리스트 형태

![https://www.fun-coding.org/00_Images/linkedlist.png](https://www.fun-coding.org/00_Images/linkedlist.png)

(출처: wikipedia, https://en.wikipedia.org/wiki/Linked_list)

### 링크드 리스트 내용 정리

- 배열에 새로운 공간을 추가하려고 할 때, 불가능하다. (크기가 정해져 있기 때문)
- 링크드리스트는 값을 넣을 공간과, 주소값을 같이 넣음
- 가령 A,B값을 가지는 링크드리스트를 구성한다고 할때, 먼저 A라는 값과 A의 다음 값을 가리키는 주소값을 같이 저장한다. 이후 새로운 B라는 값을 추가할 때, B는 A의 값과 같이 들어가 있던 주소를 바라보고 또 다음 데이터를 가리키는 주소값을 저장한다. 이런식으로 노드들이 연결되어있는 형태라서 링크드 리스트라고 한다.
- 만약 다음 데이터를 가리키는 주소값이 없다면 그 노드값이 끝이라는 것을 의미한다.

노드의 구조(데이터 , 다음 데이터의 주소값)

\***\*2. 간단한 링크드 리스트 예\*\***

```python
class Node:
    def __init__(self, data, next=None):
        self.data = data
        self.next = next

node1 = Node(1)
node2 = Node(2)
node1.next = node2
head = node1 # 가장 앞에 있는 값의 주소를 알기 위해 저장
```

링크드 리스트로 데이터 추가하기

```python
class Node:
    def __init__(self, data, next=None):
        self.data = data
        self.next = next

def add(data):
    node = head
    while node.next: #node의 next가 있으면, 다음 값이 있다는 말이므로...
        node = node.next
    node.next = Node(data) # while문을 빠져나오면 다음 node가 없다는 말이므로...

node1 = Node(1)
head = node1
for index in range(2, 10):
    add(index) # 반복해서 add하면서 링크드리스트를 구현가능함.

node = head
while node.next:
    print(node.data)
    node = node.next
print (node.data)
```

### **Node 구현**

- 보통 파이썬에서 링크드 리스트 구현시, 파이썬 클래스를 활용함
  - 파이썬 객체지향 문법 이해 필요
  - 참고: [https://www.fun-coding.org/PL&OOP1-3.html](https://www.fun-coding.org/PL&OOP1-3.html)

### **3. 링크드 리스트의 장단점 (전통적인 C언어에서의 배열과 링크드 리스트)**

- 장점
  - 미리 데이터 공간을 `미리 할당하지 않아도 됨`
    - 배열은 **미리 데이터 공간을 할당** 해야 함
- 단점
  - 연결을 위한 `별도 데이터 공간이 필요하므로, 저장공간 효율이 높지 않음`
  - 연결 정보를 찾는 시간이 필요하므로 `접근 속도가 느림`
  - `중간 데이터 삭제시, 앞뒤 데이터의 연결을 재구성해야 하는 부가적인 작업 필요`

### **4. 링크드 리스트의 복잡한 기능1 (링크드 리스트 데이터 사이에 데이터를 추가)**

- 링크드 리스트는 유지 관리에 부가적인 구현이 필요함

![https://www.fun-coding.org/00_Images/linkedlistadd.png](https://www.fun-coding.org/00_Images/linkedlistadd.png)

(출처: wikipedia, https://en.wikipedia.org/wiki/Linked_list)

```python
node3 = Node(1.5)

node = head
search = True
while search:
    if node.data == 1:
        search = False
    else:
        node = node.next

node_next = node.next # 임시로 저장
node.next = node3 # 본래의 주소값이 없어지므로
node3.next = node_next #

# 앞의노드의 주소를 새로운 노드로 바꿔주고,
# 새로운 노드의 주소를 다음 노드로 바꿔줘야 한다.
```

### 5. 파이썬 객체지향 프로그래밍으로 링크드 리스트 구현하기

```python
class Node(data):
	def __init__(self, data, next=None):
			self.data = data
			self.next = next

class NodeMgmt:
	def __init__(self, data):
		self.head = Node(data)

	def add(self, data):
		if self.head == '':
			self.head = Node(data)
		else:
			node = self.head
			while node.next:
				node = node.next
			node.next = Node(data)
	def desc(self):
			node = self.head
			while : node
				print(node.data)
				node = node.next
```

### **6. 링크드 리스트의 복잡한 기능2 (특정 노드를 삭제)**

- 다음 코드는 위의 코드에서 delete 메서드만 추가한 것이므로 해당 메서드만 확인하면 됨

- 링크드 리스트는 항상 맨 앞(head라고 정의한)의 값은 갖고 있어야 한다.

- 만약 head에 있던 값을 지운다고 한다면, head의 값 자체가 그 다음 노드로 변경이 되어야 한다..

- 만약 마지막에 있는 node를 삭제한다면 바로 삭제만 하면 되지만, 삭제하는 값을 바라보는 주소를 갖고 있는 그 값의 주소역시 null 혹은 None 으로 만들어 줘야 한다.

- 만약 중간 노드를 삭제한다면, 앞의 값의 주소 값은 삭제하는 값의 Next값을 바라보는 주소로 바꿔줘야한다.

```python
class Node:
    def __init__(self, data, next=None):
        self.data = data
        self.next = next

class NodeMgmt:
    def __init__(self, data):
        self.head = Node(data)

    def add(self, data):
        if self.head == '':
            self.head = Node(data)
        else:
            node = self.head
            while node.next:
                node = node.next
            node.next = Node(data)

    def desc(self):
        node = self.head
        while node:
            print (node.data)
            node = node.next

	  def delete(self, data): ## 특정 데이터 값의 노드를 삭제하라
				if self.head == '' :
					print('해당 값을 가진 노드가 없습니다.');
					return
				if self.head.data == data:
					temp = self.head
					self.head = self.head.next
					del temp
				else:
					node = self.head
					while node.next:
						if node.next.data:
							temp = node.next
							node.next = node.next.next
							del temp


```

**연습2: 위 코드에서 노드 데이터가 특정 숫자인 노드를 찾는 함수를 만들고, 테스트해보기**

테스트: 임의로 1 ~ 9까지 데이터를 링크드 리스트에 넣어보고, 데이터 값이 4인 노드의 데이터 값 출력해보기

```python
class Node:
		def __init__(self, data, next=None):
			self.data = data
			sefl.next = next

class NodeMgmt(data):
		def __init__(self, data):
			self.head = Node(data)

		def add(data):
			if self.head = '':
				self.head = Node(data)
			else:
				node = self.head
            while node.next:
                node = node.next # 반복할때마다 node.next를 node에다가 넣어줌(주소 따라갈 수 있도록)
            node.next = Node(data)

		def desc(data):
				node = self.head
        while node:
            print (node.data)
            node = node.next

		def delete(data):
		# 1 head 값이 들어오면 삭제하고, next를 head값으로 넣는다.
		if self.head == '':
			print('노드 값이 없습니다.')
			return

		if self.head.data == data:
			temp = self.head
			self.head = self.head.next
			del temp
		else:
			node = self.head
					while node.next:
						if node.next.data:
							temp = node.next
							node.next = node.next.next
							del temp
```

### 7. 다양한 링크드 리스트 구조

- 더블 링크드 리스트(Doubly linked list) 기본 구조

  - 이중 연결 리스트라고도 함
  - 장점: 양방향으로 연결되어 있어서 노드 탐색이 양쪽으로 모두 가능(출처: wikipedia, https://en.wikipedia.org/wiki/Linked_list)

    ![https://www.fun-coding.org/00_Images/doublelinkedlist.png](https://www.fun-coding.org/00_Images/doublelinkedlist.png)

링크드 리스트의 단점 :

- 특정 데이터의 값을 찾으려면 head의 값부터 반복하면서 찾아야됨. (즉 앞에서부터 찾아야됨)

- 내가 원하는 데이터가 전반부에 있는지 후반부에 있는지 판단할 수 있으면 반복횟수를 줄일 수 있음

### 더블링크드 리스트

- 기존 링크드 리스트의 노드에 추가로 데이터 앞쪽에 이전데이터 주소를 저장할 수 있는 공간을 만든다.

- 즉 데이터 주소를 공간을 저장할 공간이 데이터 앞뒤로 2개가 있어 더블(Double) 링크드 리스트이다.

```python
class Node:
	def __init__(self, data, prev=None, next=None):
		self.prev = prev
		self.data = data
		self.next = next

class NodeMgmt:
		def __init__(self, data):
		self.head = Node(data)
		self.tail = self.head # 뒤에서 탐색이 가능하므로 tail이 추가가 된다.

		def insert(self, data):
			if self.head == None:
					self.head = Node(data)
					self.tail = self.head
			else:
					node = self.head
					while node.next:
						node = node.next
					new = Node(data)
					node.next = new
					new.prev = node
					self.tail = new
		def desc(data):
			node = self.head
			while node:
				print(node.data)
				node = node.next

```

**연습4: 위 코드에서 노드 데이터가 특정 숫자인 노드 뒤에 데이터를 추가하는 함수를 만들고, 테스트해보기**

- 더블 링크드 리스트의 head 에서부터 다음으로 이동하며, 특정 숫자인 노드를 찾는 방식으로 함수를 구현하기
- 테스트: 임의로 0 ~ 9까지 데이터를 링크드 리스트에 넣어보고, 데이터 값이 1인 노드 다음에 1.7 데이터 값을 가진 노드를 추가해보기

```python
class Node:
	def __init__(self, data, prev:None, next:None):
		self.data = data
		self.prev = prev
		self.next = next

class NodeMgmt:
	def __init__(self, data):
		self.head = Node(data)
		self.tail = self.head # 뒤에서 탐색이 가능하기 위해 tail이 추가된다.

	def insert(data):
		if(self.head = ''):
			self.head = Node(data)
			self.tail. = self.head
		else:
			node = self.head
			while node.next: # node의 next가 있으면 계속해서 반복
				node = node.next # next가 없어질때까지 새로운 node를 찾음
			new = Node(data) # while문이 끝났다는 건 끝 노드에 도달했다는 것이므로 새로운 노드 값을 new로 생성
			node.next = new # new 값을 마지막 node의 next 값으로 설정해주고
			new.prev = node # 현재 노드의 값을 new의 prev 값으로 설정해줌
			self.tail = new # 그리고 tail 값도 new로 설정해줌

	def desc(data):
		node = self.head
		while node:
			print(node.data)
			node = node.next

```

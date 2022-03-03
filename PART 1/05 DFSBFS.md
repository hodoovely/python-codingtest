# chapter05 DFS/BFS   
## 꼭 필요한 자료구조 기초   
***탐색*** 이란 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정을 의미한다. 
프로그래밍에서는 그래프, 트리 등의 자료구조 안에서 탐색을 하는 문제를 자주 다룬다.
대표적인 탐색 알고리즘으로 DFS와 BFS를 꼽을 수 있는데 이 두 알고리즘의 원리를 제대로 이해해야 코딩 테스트의 탐색 문제 유형을 풀 수 있다.   
***자료구조*** 란 '데이터를 표현하고 관리하고 처리하기 위한 구조'를 의미한다. 
그 중 스택과 큐는 자료구조의 기초 개념으로 다음의 두 핵심적인 함수로 구성된다.
- 삽입(push) : 데이터를 삽입한다
- 삭제(pop) : 데이터를 삭제한다

<br/>
<br/>

***스택(stack)*** 은 박스 쌓기에 비유할 수 있다.
흔히 박스는 아래에서부터 위로 쌓는다.
이러한 구조를 선입후출구조 혹은 후입선출구조라고 한다.   

   
#### 스택 예제(5-1.py)
```python
stack = []

# 삽입(5) - 삽입(2) - 삽입(3) - 삽입(7) - 삭제() - 삽입(1) - 삽입(4) - 삭제()
stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop()         #7이 사라짐
stack.append(1)
stack.append(4)
stack.pop()         #4가 사라짐

print(stack)        #최하단 원소부터 출력
print(stack[::-1])  #최상단 원소부터 출력
```

<br/>
<br/>

***큐(queue)*** 는 대기 줄에 비유할 수 있다.
흔히 놀이공원에 입장하기 위해 줄을 설 때, 먼저 온 사람이 먼저 들어가게 된다.
이러한 구조를 선입선출구조라고 한다.

#### 큐 예제(5-2.py)
```python
from collections import deque

# 큐(queue) 구현을 위해 deque라이브라리 사용
queue = deque()

# 삽입(5) - 삽입(2) - 삽입(3) - 삽입(7) - 삭제() - 삽입(1) - 삽입(4) - 삭제()
queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popleft()  #5가 사라짐
queue.append(1)
queue.append(4)
queue.popleft()  #2가 사라짐

print(queue)     #먼저 들어온 순서대로 출력
queue.reverse()  #다음 출력을 위해 역순으로 바꾸기
print(queue)     #나중에 들어온 원소부터 출력
````

<br/>
<br/>

***재귀함수(recursive function)*** 란 자기자신을 다시 호출하는 함수를 의미한다.
가장 간단한 재귀함수는 다음과 같다.

#### 재귀 함수 예제(5-3.py)
```python
def recursive_function():
    print('재귀 함수를 호출합니다.')
    recursive_function()
    
recursive_function()
````

이 코드를 실행하면 '재귀 함수를 호출합니다.'라는 문자열을 무한히 출력한다. 
여기에서 정의한 recursive_function이 자기 자신을 계속해서 추가로 불러오기 때문이다.
따라서 재귀함수를 문제 풀이에서 사용할 때는 재귀함수가 언제 끝날지, 종료 조건을 꼭 명시해야한다.
예를 들어 다음은 재귀함수를 100번 호출하도록 작성한 코드이다.
재귀함수 초반에 등장하는 if문이 종료 조건 역할을 수행한다.

#### 재귀 함수 예제(5-3.py)
```python
def recursive_function(i):
    #100번째 출력했을 때 종료되도록 종료 조건 명시
    if i == 100:
        return
    print(i, '번째 재귀 함수에', i+1, '번째 재귀 함수를 호출합니다.')
    recursive_function(i+1)
    print(i, '번째 재귀 함수를 종료합니다.')

recursive_function(1)
```

<br/>

재귀 함수를 이용하는 대표적 예제로는 팩토리얼 문제가 있다.
팩토리얼을 반복적으로 구현한 방식과 재귀적으로 구현한 두 방식을 비교해보자.
```python
#반복적으로 구현한 n!
def factorial_iterative(n):
    result = 1
    #1부터 n까지의 수를 차례대로 곱하기
    for i in range(1, n+1):
        result *= i
    return result

#재귀적으로 구현한 n!
def factorial_recursive(n):
    if n <= 1:    #n이 1 이하인 경우 1을 반환
        return 1
    #n! = n*(n-1)!를 그대로 코드 작성하기
    return n*factorial_recursive(n-1)

print('반복적으로 구현:', factorial_iterative(5))
print('재귀적으로 구현', factorial_recursive(5))
````
반복문 대신에 재귀 함수를 사용했을 때, 코드거 더 간결한 것을 알 수 있다.
재귀 함수가 수학의 점화식(재귀식)을 그대로 소스 코드로 옮겼기 때문이다.

<br/>
<br/>

## 탐색 알고리즘 DFS/BFS

### DFS
***DFS*** 는 Depth-First Search, 깊이 우선 탐색이라고도 부르며, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘이다.
DFS를 이해하기 위해서는 그래프의 기본 구조를 알아야한다.

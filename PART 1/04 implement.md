# chapter04 구현   
## 아이디어를 코드로 바꾸는 구현
구현 유형의 문제는 '풀이를 떠올리는 것은 쉽지만 소스코드로 옮기기 어려운 문제'를 의미한다.  
피지컬('피지컬이 좋다' = '문법에 능숙하고 코드 작성 타자 속도가 빠름')을 요구하는 문제들이다.
문제가 긴 편이나, 입력 조건 등을 문제에 명시해주므로 문법에 익숙하다면 오히려 쉽게 풀 수 있는 문제들이다.

<br/>

### 예제 4-1 상하좌우   
#### 답안 예시(4-1.py)
```python
n = int(input())
x, y = 1, 1
plans = input().split()

dx = [0, 0, -1, 1]
dy = [-1, 1, 0, 0]
move_types = ['L', 'R', 'U', 'D']

#이동 계획을 하나씩 확인
for plan in plans:
    #이동 후 좌표 구하기
    for i in range(len(move_types)):
        if plan == move_types[i]:
            nx = x + dx[i]
            ny = y + dy[i]
    #공간을 벗어나는 경우 무시
    if nx < 1 or ny < 1 or nx > n or ny > n:
        continue
    x, y = nx, ny

print(x,y)
```
***plans = map(int, input().split())*** 가 아니라 ***plans = python input().split()*** 인 이유는 그 요소들이 L, R, U, D 등의 문자이기 때문이다.
```python
a, b = map(int, input().split()) #이 코드를 쪼개보면

x = input().split()    # input().split()의 결과는 문자열 리스트
m = map(int, x)        # 리스트의 요소를 int로 변환, 결과는 맵 객체
a, b = m               # 맵 객체는 변수 여러 개에 저장할 수 있음
````
***continue*** 를 사용하면 그 아래의 코드를 수행하지 않고 for문의 조건을 판단하는 곳으로 점프하게 된다.
```python
for plan in plans:                            #for문 차례가 더 남았다면 여기로 다시 돌아오게 된다 
    for i in range(len(move_types)):          
        if plan == move_types[i]:
            nx = x + dx[i]
            ny = y + dy[i]
    if nx < 1 or ny < 1 or nx > n or ny > n:  #위에서 구한 nx값이나 ny의 값이 1미만이거나 n이상인 경우
        continue                              #continue에 걸렸다!! > 아래 내용은 실행하지 않고 이번 차례의 for문을 빠져나간다
    x, y = nx, ny                             #이것을 실행하지 않고, for문의 조건을 판단하는 곳으로 점프 or 아예 for문 빠져나가기
```

<br/>
<br/>

### 예제 4-2 시각   
#### 답안 예시(4-2.py)
```python
h = int(input())

count = 0
for i in range(h+1):
    for j in range(60):
        for k in range(60):
            if '3' in str(i) + str(j) + str(k):
                count += 1
print(count)
````

<br/>
<br/>

## 왕실의 나이트   
#### 내가 적어본 답안(틀림)
```python
move_types = [(2,1), (2,-1), (-2,1), (-2,-1), (1,2), (1,-2), (-1,2), (-1,-2)]

a = input()
x = ord(a[0])-96
y = int(a[1])

result = 0


for move in move_types:
    x += move[0]
    y += move[1]
    if 1 <= x <= 8 and 1 <= y <= 8:
        result += 1
    else:
        continue

print(result)
````
***x*** , ***y*** 값에 모든 ***move_types*** 들이 누적되어서 틀렸음.
#### 내가 적어본 답안(맞음)
```python
move_types = [(2,1), (2,-1), (-2,1), (-2,-1), (1,2), (1,-2), (-1,2), (-1,-2)]

a = input()
x = ord(a[0])-96
y = int(a[1])

result = 0


for move in move_types:
    x += move[0]
    y += move[1]
    if 1 <= x <= 8 and 1 <= y <= 8:
        result += 1
        x = ord(a[0]) - 96
        y = int(a[1])
    else:
        x = ord(a[0]) - 96
        y = int(a[1])

print(result)
````
답은 맞는데, 뭔가 코드 지저분..

#### 답안 예시(4-3.py)
```python
input_data = input()
row = int(input_data[1])
column = int(ord(input_data[0])) - int(ord('a')) + 1   #이렇게 하면 아스키코드 안 외우고도 알파벳 숫자로 변환 가능하다!

steps = [(2,1), (2,-1), (-2,1), (-2,-1), (1,2), (1,-2), (-1,2), (-1,-2)]

result = 0
for step in steps:
    next_row = row + step[0]                           #누적시키지 않으려고 +=를 사용하지  for문 안에서 next_row를 선언해줌! 
    next_column = column + step[1]                     #마찬가지로 누적시키지 않으려고 next_colum을 선언해줌!
    if next_row >= 1 and next_row <= 8 and next_column >= 1 and next_column <= 8:
        result += 1

print(result)
```


<br/>
<br/>

## 게임 개발
***내가 적어본 답안(망함)***
```python
n, m = map(int, input().split())
x, y, face = map(int, input().split())

dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]
face = [0, -1, -2, -3]

row = []
for i in range(n):
    line = list(map(int, input().split()))
    row.append(line)

for i in face:
    nx = x - 1 + dx[i]
    ny = y - 1 + dy[i]
    next_step = row[nx][ny]
    if 0 <= nx <= n-1 and 0 <= ny <= m-1 and next_step == 0:
        row[nx][ny] += 1
        break
    else:
        face
````
***답안 예시(4-4.py)***
```python
n, m = map(int, input().split())

#방문할 위치를 저장하기 위한 맵을 생성하여 0으로 초기화
d = [[0]*m for _ in range (n)]
#현재 캐릭터의 X좌표, Y좌표, 방향을 입력받기
x, y, direction = map(int, input().split())
d[x][y] = 1    #현재 좌표 방문 처리

#전체 맵 정보를 입력받기
array = []
for i in range(n):
    array.append(list(map(int, input().split())))
    
#북, 동, 남, 서 방향 정의
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

#왼쪽으로 회전
def turn_left():
    global direction
    direction -= 1
    if direction == -1:
        direction = 3
        
#시뮬레이션 시작
count = 1
turn_time = 0
while True:
    #왼쪽으로 회전
    turn_left()
    nx = x + dx[direction]
    ny = y + dy[direction]
    #회전한 이후 정면에 가보지 않은 칸이 존재하는 경우 이동
    if d[nx][ny] == 0 and array[nx][ny] == 0:
        d[nx][ny] = 1
        x = nx
        y = ny
        count += 1
        turn_time = 0
        continue
    #회전한 이후 정면에 가보지 않은 칸이 없거나 바다인 경우
    else:
        turn_time += 1
    #네 방향 모두 갈 수 없는 경우
    if turn_time == 4:
        nx = x - dx[direction]
        ny = y - dy[direction]
        #뒤로 갈 수 있다면 이동하기
        if array[nx][ny] == 0:
            x = nx
            y = ny
        #뒤가 바다로 막혀있는 경우
        else:
            break
            turn_time = 0
            
print(count)
````

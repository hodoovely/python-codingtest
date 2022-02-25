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


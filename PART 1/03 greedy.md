# chapter03 그리디   
## 당장 좋은 것만 선택하는 그리디   
### 예제 3-1 거스름돈   
#### 내가 적어본 답안
```python
n = int(input())
count = 0

coin_types = [500, 100, 50, 10]

for coin in coin_types:
    count += n//coin
    n %= coin

print(count)
```
이 알고리즘은 큰 단위의 동전이 작은 단위의 동전들의 합보다 크기때문에 가능한 것

<br/>
<br/>

## 큰 수의 법칙
#### 단순하게 푸는 답안 예시(3-1.py)
```python
n, m, k = map(int, input().split())
data = list(map(int, input().split()))

data.sort()
first = data[n-1]
second = data[n-2]

result = 0

while True:
    for i in range(k):
        if m == 0:
            break
        result += first
        m -= 1
    if m == 0:
        break
    result += second
    m -= 1

print(result)
```
#### 반복되는 수열을 통해 파악하는 답안 예시(3-2.py)
```python
n, m, k = map(int, input().split())
data = list(map(int, input().split()))

data.sort()
first = data[n-1]
second = data[n-2]

result = 0

count = m//(k+1)*k
count += m%(k+1)

result += count*first
result += (m-count)*second
print(result)
```

<br/>
<br/>

## 숫자 카드 게임
#### min()함수를 이용하는 답안 예시(3-3.py)
```python
n, m = map(int, input().split())

result = 0

for i in range(n):
    data = list(map(int, input().split()))
    min_value = min(data)
    result = max(result, min_value)

print(result)
```
#### 2중 반복문 구조를 이용하는 답안 예시(3-4.py)
```python
n, m = map(int, input().split())

result = 0

for i in range(n):
    data = list(map(int, input().split()))
    min_value = 10001
    for a in data:
        min_value = min(min_value, a)
    result = max(result, min_value)

print(result)
```
<br/>
<br/>

## 1이 될 때까지
#### 내가 적어본 답안
```python
n, k = map(int, input().split())
result = 0

while n>= k:
    if n%k != 0:
        result += (n%k)
        n -= (n%k)
    elif n%k == 0:
        result += 1
        n //= k

result -= (n-1)

print(result)
```
#### 단순하게 푸는 답안 예시(3-5.py)
```python
n, k = map(int, input().split())
result = 0

while n >= k:
    while n%k != 0:
        n-= 1
        result += 1
    n //= k
    result += 1

while n > 1 :
    n -= 1
    result += 1

print(result)
```
#### 답안 예시(3-6.py)
```python
n, k = map(int, input().split())
result = 0

while True:
    target = (n//k)*k
    result += (n-target)
    n = target
    if n <k :
        break
    result += 1
    n //= k
result += (n-1)
print(result)
```
내가 푼거랑 뭐가 다르지..? 굳이 target을 지정해야하는 이유랑 break를 써야하는 이유가 뭐지?


---
title: "[이코테 강좌] Implementation(구현)과 Brute Force(완전 탐색)"
excerpt_separator: <!--more-->
categories:
  - "Python for Coding Test"
tags:
  - "implementation"
  - "brute force"
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [(이코테 2021 강의 몰아보기) 2. 그리디 & 구현](https://youtu.be/2zjoKjt97vQ?list=PLRx0vPvlEmdAghTr5mXQxGpHjWqSz0dgC)

## 구현 (Implementation)

- 머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정
- 풀이를 떠올리는 것은 쉽지만 소스코드로 옮기기 어려운 문제를 지칭
- 예시:
  - 알고리즘은 간단한데 코드가 지나칠 만큼 길어지는 문제
  - 실수 연산을 다루고, 특정 소수점 자리까지 출력해야하는 문제
  - 문자열을 특정한 기준에 따라서 끊어 처리해야하는 문제
  - 적절한 라이브러리를 찾아서 사용해야하는 문제
- 2차원 배열인 행렬(Matrix)가 많이 쓰임

  - 아래와 같이 방향 벡터를 활용해 상하좌우로 이동할 수 있다.

  ```python
  # 상하좌우
  dx = [0, 0, -1, 1]
  dy = [-1, 1, 0, 0]

  x, y = 2, 2

  for i in range(4):
    nx = x + dx[i]
    ny = y + dy[i]
    print(nx, ny)
  ```

## 문제: 상하좌우

- N\*N의 정사각형 공간
- 가장 왼쪽 위 좌표는 (1, 1), 가장 오른쪽 아래 좌표는 (N, N)
- 상(U), 하(D), 좌(L), 우(R) 로 이동 가능
- 최종적으로 도착할 지점의 좌표 구하기
  - 1 <= N <= 100
  - 1 <= 이동 횟수 <= 100

### 풀이

- 요구사항대로 충실히 구현
- 명령에 따라 개체를 차례대로 움직이는 시뮬레이션(Simulation) 유형

### 코드

```python
n = int(input())
x, y = 1, 1
plans = input().split()

# L, R, U, D에 따른 이동 방향
dx = [0, 0, -1, 1]
dy = [-1, 1, 0, 0]
move_types = ['L', 'R', 'U', 'D']

# 이동 계획을 하나씩 확인
for plan in plans:
    # 이동 후 좌표 구하기
    for i in range(len(move_types)):
        if plan == move_types[i]:
            nx = x + dx[i]
            ny = y + dy[i]
    # 공간을 벗어나는 경우 무시
    if nx < 1 or ny < 1 or nx > n or ny > n:
        continue
    # 이동 수행
    x, y = nx, ny

print(x, y)
```

## 문제: 시각

- 정수 N이 입력되면 00시 00분 00초 부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수 구하기
  - 0 <= N <= 23

### 풀이

- 가능한 모든 경우의 수를 모두 검사해보는 완전 탐색(Brute Forcing) 유형

### 코드

```python
h = int(input())

count = 0
for i in range(h + 1):
    for j in range(60):
        for k in range(60):
            # 매 시각 안에 '3'이 포함되어 있다면 카운트 증가
            if '3' in str(i) + str(j) + str(k):
                count += 1

print(count)
```

## 문제: 왕실의 나이트

- 8 \* 8 체스판에 나이트의 좌표가 주어지고, 나이트는 다음 두 가지 경우로 이동할 수 있다.
  - 수평으로 2칸 이동 후 수직으로 1칸
  - 수직으로 2칸 이동 후 수평으로 1칸
- 나이트가 이동할 수 있는 경우의 수 구하기

### 풀이

- 요구사항대로 구현
- 8가지의 경로의 방향 벡터를 활용

### 코드

```python
# 현재 나이트의 위치 입력받기
input_data = input()
row = int(input_data[1])
column = int(ord(input_data[0])) - int(ord('a')) + 1

# 나이트가 이동할 수 있는 8가지 방향 정의
steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1, 2), (-2, 1)]

# 8가지 방향에 대하여 각 위치로 이동이 가능한지 확인
result = 0
for step in steps:
    # 이동하고자 하는 위치 확인
    next_row = row + step[0]
    next_column = column + step[1]
    # 해당 위치로 이동이 가능하다면 카운트 증가
    if next_row >= 1 and next_row <= 8 and next_column >= 1 and next_column <= 8:
        result += 1

print(result)
```

## 문제: 문자열 재정렬

- 알파벳 대문자와 숫자(0~9)로만 구성된 문자열 S
- 알파벳을 오름차순으로 정렬하여 출력 후, 모든 숫자를 더한 값을 이어서 출력
  - 1 <= S의 길이 <= 10,000

### 풀이

- 요구사항대로 구현
- 문자를 하나씩 확인하며 숫자면 합계를 계산, 알파벳이면 별도의 리스트에 저장

### 코드

```python
data = input()
result = []
value = 0

for x in data:
  if x.isalpha():
    result.append(x)
  else:
    value += int(x)

result.sort()

if value:
  result.append(str(value))

print("".join(result))
```

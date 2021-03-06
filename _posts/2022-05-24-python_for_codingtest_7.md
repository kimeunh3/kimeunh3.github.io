---
title: '[이코테 강좌] 다이나믹 프로그래밍'
excerpt_separator: <!--more-->
categories:
  - 'Python for Coding Test'
tags:
  - 'dynamic programming'
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [(이코테 2021 강의 몰아보기) 6. 다이나믹 프로그래밍](https://youtu.be/5Lu34WIx2Us?list=PLRx0vPvlEmdAghTr5mXQxGpHjWqSz0dgC)

## 다이나믹 프로그래밍

- 메모리를 적절히 사용하여 수행 시간 효율성을 비약적으로 향상시키는 방법
- 이미 계산한 결과(작은 문제)를 별도의 메모리에 저장하여 같은 계산을 반복하지 않게 함
- 탑다운, 바텀업의 두 가지의 대표적인 방식
- 동적 계획법이라고도 불리지만 프로그래밍에서 쓰이는 동적(Dynamic)의 의미와는 다르게 별다른 의미 없이 사용된 단어
  - 동적 할당(Dynamic Allocation): 프로그램이 실행되는 도중에 실행에 필요한 메모리를 할당하는 기법

## 조건

- 최적 부분 구조 (Optimal Substructure)
  - 큰 문제를 작은 문제로 나눌 수 있고, 작은 문제의 답을 모아서 큰 문제가 해결 가능
- 중복되는 부분 문제 (Overlapping Subproblem)
  - 동일한 작은 문제를 반복적으로 해결해야 함

## 메모이제이션 (Memoization)

- DP를 구현하는 방법 중 하나
- 한 번 계산한 결과를 메모리 공간에 메모하는 기법
  - 같은 문제를 다시 호출하면 메모했던 결과를 그대로 가져옴
  - 값을 기록해 놓는다는 점에서 **캐싱(Caching)** 이라고도 함
- 이전에 계산된 결과를 일시적으로 기록해 놓는 넓은 개념이라 DP에 국한된 개념이 아님

## 탑다운(Top-Down) VS 바텀업(Bottom-Up)

- 탑다운(Top-Down): 하향식
- 바텀업(Bottom-Up): 상향식
  - 전형적인 형태

## 피보나치 수열

- DP로 구현할 수 있는 대표적인 수열
- **점화식** : 인접한 항들 사이의 관계식
  - 피보나치 수열의 점화식: $a_n = a_{n-1}+a_{n-2}, a_1 = 1, a_2 = 1$
    ![image](https://user-images.githubusercontent.com/59808674/169986151-d87beea8-1712-40d0-98ca-3a3c5e054c2e.png)
- 단순 재귀 구현

  ```python
  def fibo(x):
    if x == 1 or x == 2::
      return 1
    return fibo(x-1) + fibo(x-2)

  print(fibo(4)) # 3
  ```

  - 지수(exponential) 시간 복잡도를 가지게 됨
  - 아래와 같이 f(2)가 여러번 호출 됨 (**중복되는 부분 문제**)
    ![image](https://user-images.githubusercontent.com/59808674/169985979-fbd787bd-b829-4768-a84d-93a59b00e36e.png)

- DP 구현

  - 탑다운

    ```python
    # 한 번 계산된 결과를 메모이제이션(Memoization)하기 위한 리스트 초기화
    d = [0] * 100

    # 피보나치 함수(Fibonacci Function)를 재귀함수로 구현 (탑다운 다이나믹 프로그래밍)
    def fibo(x):
        # 종료 조건(1 혹은 2일 때 1을 반환)
        if x == 1 or x == 2:
            return 1
        # 이미 계산한 적 있는 문제라면 그대로 반환
        if d[x] != 0:
            return d[x]
        # 아직 계산하지 않은 문제라면 점화식에 따라서 피보나치 결과 반환
        d[x] = fibo(x - 1) + fibo(x - 2)
        return d[x]

    print(fibo(99)) # 218922995834555169026
    ```

  - 바텀업

    ```python
    # 앞서 계산된 결과를 저장하기 위한 DP 테이블 초기화
    d = [0] * 100

    # 첫 번째 피보나치 수와 두 번째 피보나치 수는 1
    d[1] = 1
    d[2] = 1
    n = 99

    # 피보나치 함수(Fibonacci Function) 반복문으로 구현(보텀업 다이나믹 프로그래밍)
    for i in range(3, n + 1):
        d[i] = d[i - 1] + d[i - 2]

    print(d[n]) # 218922995834555169026
    ```

  - DP 구현의 경우 아래의 색칠된 노드만 계산하여 처리하기 때문에 시간 복잡도가 O(N)이 된다
    ![image](https://user-images.githubusercontent.com/59808674/169989165-71f90e03-1814-41bc-85c2-19f42dcb1623.png)

## 다이나믹 프로그래밍 VS 분할 정복

- 둘 다 최적 부분 구조를 가질 때 사용 가능함
  - 큰 문제를 작은 문제로 나눌 수 있으며 작은 문제의 답을 모아 큰 문제를 해결 가능한 상황
- DP와 분할 정복의 차이는 **부분 문제의 중복**
  - DP: 중복되는 작은 문제의 계산을 한 번만 하게끔 하여 효율성 개선
  - 분할 정복: 작은 문제들이 반복되지않아 모두 계산해야됨
    - 퀵 정렬: 한 번 기준 원소(pivot)가 자리를 잡으면 그 기준 원소의 위치가 바뀌지 않음
      - 해당 pivot을 다시 처리하는 부분 문제는 호출 되지 않음

## 접근 방법

- DP문제 유형임을 파악하는 것이 중요
- 먼저 그리디, 구현, 완전 탐색 등으로 문제가 해결가능한지 검토 후, 다른 풀이 방법이 떠오르지 않다면 DP를 고려
- 재귀 함수로 구현한 비효율적인 완전 탐색 프로그램으로 해결한 작은 문제의 답이 큰 문제의 해결로 이어진다면 DP로 개선 가능

## 문제: 개미 전사

- 식량창고는 일직선으로 이어져 있음
- 서로 인접한 식량창고가 공격받으면 메뚜기 정찰병들이 알아채기 때문에 최소한 한 칸 이상 떨어진 식량창고를 약탈
- [1, 3, 1, 5] 일 때 두 번째와 네 번째를 선택했을 때 최대값인 총 8개의 식량 약탈 가능
- 식량창고가 N개일 때 약탈할 수 있는 식량의 최댓값을 구하는 문제
  - 3 <= N <= 100 (식량창고의 개수)
  - 1 <= K <= 1,000 (저장된 식량의 개수)

### 풀이

- N = 4일 때, 식량을 선택할 수 있는 경우의 수는 다음과 같음
  ![image](https://user-images.githubusercontent.com/59808674/170014294-656200a4-f44f-4e28-afe2-8b2c78b4c733.png)
  - 7번째의 경우가 최적의 해
- $a_i$ = i번째 식량창고까지의 최적의 해라면 DP 테이블의 값은 다음과 같이 정의 가능
  ![image](https://user-images.githubusercontent.com/59808674/170016098-e37abc06-1a9c-4d67-ac42-eff6916dbe5f.png)
- 왼쪽부터 차례대로 식량창고를 약탈하면, 다음과 같은 두 가지의 경우 중에 선택
  1. 현재(i) 식량창고를 털지 않고 바로 직전(i-1)의 식량창고를 터는 경우
  2. 현재(i) 식량창고를 털면서 두 칸 전(i-2)의 식량창고를 터는 경우
- 위의 조건을 만족하는 점화식은 다음과 같음
  - $a_i = max(a_{i-1}, a_{i-2} + k_i)$
    - $a_i$ = i번째 식량창고까지의 최적의 해
    - $k_i$ = i번째 식량창고에 있는 식량의 양

### 코드

```python

# 정수 N을 입력 받기
n = int(input())
# 모든 식량 정보 입력 받기
array = list(map(int, input().split()))

# 앞서 계산된 결과를 저장하기 위한 DP 테이블 초기화
d = [0] * 100

# 다이나믹 프로그래밍(Dynamic Programming) 진행 (보텀업)
d[0] = array[0]
d[1] = max(array[0], array[1])
for i in range(2, n):
    d[i] = max(d[i - 1], d[i - 2] + array[i])

# 계산된 결과 출력
print(d[n - 1])
```

## 문제: 1로 만들기

- 정수 X가 주어지면 가능한 연산은 다음과 같이 4가지
  1. 5로 나누어 떨어지면, 5로 나눔
  2. 3로 나누어 떨어지면, 3로 나눔
  3. 2로 나누어 떨어지면, 2로 나눔
  4. X에서 1을 뺌
- 연산을 사용하여 X를 1로 만드는 최소 연산 횟수를 구하는 문제
  - 1 <= X <= 30,000

### 풀이

- $a_i$ = i를 1로 만들기 위한 최소 연산 횟수
- 점화식: $a_i = min(a_{i-1}, a_{i/2}, a_{i/3}, a_{i/5}) + 1$
  - 해당 수로 나누어 떨어질 때에 한해 점화식을 적용 가능

### 코드

```python
# 정수 X를 입력 받기
x = int(input())

# 앞서 계산된 결과를 저장하기 위한 DP 테이블 초기화
d = [0] * 1000001

# 다이나믹 프로그래밍(Dynamic Programming) 진행(보텀업)
for i in range(2, x + 1):
    # 현재의 수에서 1을 빼는 경우
    d[i] = d[i - 1] + 1
    # 현재의 수가 2로 나누어 떨어지는 경우
    if i % 2 == 0:
        d[i] = min(d[i], d[i // 2] + 1)
    # 현재의 수가 3으로 나누어 떨어지는 경우
    if i % 3 == 0:
        d[i] = min(d[i], d[i // 3] + 1)
    # 현재의 수가 5로 나누어 떨어지는 경우
    if i % 5 == 0:
        d[i] = min(d[i], d[i // 5] + 1)

print(d[x])
```

## 문제: 효율적인 화폐 구성

- N가지 종류의 화폐
- M원을 만들 수 있는 최소한의 화폐 개수를 구하는 문제
- 불가능할 경우 -1 출력
  - 1 <= N <= 100
  - 1 <= M <= 10,000

### 풀이

- $a_i$ = 금액 i를 만들 수 있는 최소 화폐 개수
- $k$ = 각 화폐의 단위
- 점화식: 각 화폐 단위인 k를 하나씩 확인하며
  - $a_{i-k}$를 만드는 방법이 존재하는 경우: $a_i = min(a_i, a_{i-k}+1)$
  - $a_{i-k}$를 만드는 방법이 존재하지 않는 경우: $a_i = INF$

### 코드

```python
# 정수 N, M을 입력 받기
n, m = map(int, input().split())
# N개의 화폐 단위 정보를 입력 받기
array = []
for i in range(n):
    array.append(int(input()))

# 한 번 계산된 결과를 저장하기 위한 DP 테이블 초기화
d = [10001] * (m + 1)

# 다이나믹 프로그래밍(Dynamic Programming) 진행(보텀업)
d[0] = 0
for i in range(n):
    for j in range(array[i], m + 1):
        if d[j - array[i]] != 10001: # (i - k)원을 만드는 방법이 존재하는 경우
            d[j] = min(d[j], d[j - array[i]] + 1)

# 계산된 결과 출력
if d[m] == 10001: # 최종적으로 M원을 만드는 방법이 없는 경우
    print(-1)
else:
    print(d[m])
```

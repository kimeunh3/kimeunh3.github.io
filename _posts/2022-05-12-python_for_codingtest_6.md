---
title: '[이코테 강좌] 이진 탐색'
excerpt_separator: <!--more-->
categories:
  - 'Python for Coding Test'
tags:
  - 'binary search'
use_math: true
---

## 출처

- 이 포스팅은 아래의 강좌를 진행하며 정리한 글입니다.
  > [(이코테 2021 강의 몰아보기) 5. 이진 탐색](https://youtu.be/94RC-DsGMLo?list=PLRx0vPvlEmdAghTr5mXQxGpHjWqSz0dgC)

## 이진 탐색

- 순차탐색: 리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 확인하는 방법
- 정렬되어 있는 리스트에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법
  - 시작점, 끝점, 중간점을 이용하여 탐색 범위를 설정
- 시간복잡도
  - 단계마다 탐색 범위를 2로 나누므로 연산횟수는 $\log_{2}{N}$ 에 비례
  - 따라서 시간 복잡도는 O(log N)

## 구현

- 재귀적 구현

```python
def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    # 찾은 경우 중간점 인덱스 반환
    if array[mid] == target:
        return mid
    # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
    else:
        return binary_search(array, target, mid + 1, end)
```

- 반복문 구현

```python
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        # 찾은 경우 중간점 인덱스 반환
        if array[mid] == target:
            return mid
        # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
        elif array[mid] > target:
            end = mid - 1
        # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
        else:
            start = mid + 1
    return None
```

- 파이썬 이진 탐색 라이브러리
  - bisect_left(a, x): 정렬된 순서를 유지하며 배열 a에 x를 삽입할 가장 왼쪽 인덱스 반환
  - bisect_right(a, x): 정렬된 순서를 유지하며 배열 a에 x를 삽입할 가장 오른쪽 인덱스 반환

```python
from bisect import bisect_left, bisect_right

a = [1, 2, 4, 4, 8]
x = 4

bisect_left(a, x) # 2
bisect_right(a, x) # 4
```

## 파라메트릭 서치 (Parametric Search)

- 최적화 문제를 결정 문제(yes or no)로 바꾸어 해결하는 기법
  - ex) 특정한 조건을 만족하는 가장 알맞은 값을 빠르게 찾는 최적화 문제
- 일반적으로 파라메트릭 서치 문제는 이진 탐색을 이용하여 해결 가능

## 문제: 떡볶이 떡 만들기

- 높이 H를 지정
- 높이 H보다 긴 떡만 잘림
- [19, 14, 10, 17] 일 때 H가 15라면 높이는 [15, 14, 10, 15] 가 되고 잘린 떡은 [4, 0, 0, 2]가 된다.
  - 손님이 가져가는 길이는 4+2=6cm
- 손님이 요청하는 총 길이가 M일때 적어도 M만큼의 떡을 얻기 위한 H의 최댓값을 구하는 문제
  - 1 <= N <= 1000000 (떡의 개수)
  - 1 <= M <= 2000000000 (요청한 떡의 길이)

### 풀이

- 적절한 높이를 찾을 때까지 이진 탐색을 수행하며 H를 조정
- 현재 높이 H로 자르면 조건이 만족 되는지 아닌지(yes or no)에 따라서 탐색 범위를 좁혀나가며 해결 가능
- 중간점의 값은 시간이 지날수록 **최적화된 값**이 되기 때문에, 크거나 같을 때마다 **중간점의 값을 기록**

### 코드

```python
# 떡의 개수(N)와 요청한 떡의 길이(M)을 입력
n, m = list(map(int, input().split(' ')))
# 각 떡의 개별 높이 정보를 입력
array = list(map(int, input().split()))

# 이진 탐색을 위한 시작점과 끝점 설정
start = 0
end = max(array)

# 이진 탐색 수행 (반복적)
result = 0
while(start <= end):
    total = 0
    mid = (start + end) // 2
    for x in array:
        # 잘랐을 때의 떡볶이 양 계산
        if x > mid:
            total += x - mid
    # 떡볶이 양이 부족한 경우 더 많이 자르기 (오른쪽 부분 탐색)
    if total < m:
        end = mid - 1
    # 떡볶이 양이 충분한 경우 덜 자르기 (왼쪽 부분 탐색)
    else:
        result = mid # 최대한 덜 잘랐을 때가 정답이므로, 여기에서 result에 기록
        start = mid + 1

# 정답 출력
print(result)
```

## 문제: 정렬된 배열에서 특정 수의 개수 구하기

- 길이가 N이고 오름차순으로 정렬된 수열이 제공
- 수열에서 x가 등장하는 횟수를 계산
- O(log N)으로 해결하지 않으면 시간 초과
  - 0 <= N <= 1000000
  - -10e9 <= x <= 10e9
  - -10e9 <= 각 원소의 값 <= 10e9

### 풀이

- 파이썬 이진 탐색 라이브러리를 이용하여 구해준다.

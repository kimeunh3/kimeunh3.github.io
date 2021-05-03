---
title: "퀵 정렬(quick sort)"
excerpt_separator: <!--more-->
categories: 
    - "Sorting"
tags: 
    - "quick sort"
use_math: true
---

퀵 정렬은 배열을 단순하게 가운데에서 쪼개는 대신, 병합 과정이 필요 없도록 한쪽의 배열에 포함된 수가 다른 쪽 배열의 수보다 항상 작도록 배열을 분할합니다.  

이를 위해 퀵 정렬은 **파티션(partition)**이라고 부르는 단계를 도입하는데, 이는 배열에 있는 수 중 임의의 '**기준 수(pivot)**'를 지정한 후 기준보다 작거나 같은 숫자를 왼쪽, 더 큰 숫자를 오른쪽으로 보내는 과정입니다.  

퀵 정렬의 경우 대부분의 시간을 차지하는 것은 주어진 문제를 두 개의 부분 문제로 나누는 파티션 과정입니다. 파티션에는 주어진 수열의 길이에 비례하는 시간이 걸리므로, 기준으로 택한 원소가 최소 원소나 최대 원소인 경우 부분 문제의 크기가 하나씩만 줄어들 수도 있습니다. 이러한 최악의 경우에는 $O(N^2)$의 시간 복잡도를 가지게 됩니다. 평균적으로 부분 문제가 절반에 가깝게 나눠질 때 병합정렬과 같은 $O(NlogN)$이 됩니다.  

대부분의 퀵 정렬 구현은 가능한 절반에 가까운 분할을 얻기 위해 좋은 기준 수를 뽑는 다양한 방법들을 사용합니다.  

## 시간 복잡도  

|                     |   Best     |  Average   |  Worst     |
|---------------------|------------|------------|------------|
| 퀵 정렬(Quick Sort) | $O(NlogN)$ | $O(NlogN)$ | $O(N^2)$ |  


## 코드
```python
def quickSort(A, lo, hi):
    if lo < hi:
        pivotLocation - partition(A, lo, hi)
        quickSort(A, lo, pivotLocation-1)
        quickSort(A, pivotLocation+1, hi)
def partition(A, lo, hi):
    # 첫번째를 pivot으로 골랐을 경우
    pivot = A[lo]
    leftwall = lo
    for i in range(lo+1, hi):
        if A[i] < pivot:
            leftwall += 1
            # swap
            temp = A[i]
            A[i] = A[leftwall]
            A[leftwall] = temp
    # swap
    temp = A[lo]
    A[lo] = A[leftwall]
    A[leftwall] = temp
    return leftwall 
```  

## 동작 과정


---
title: "병합 정렬(merge sort)"
excerpt_separator: <!--more-->
categories: 
    - "Sorting"
tags: 
    - "merge sort"
use_math: true
---

병합 정렬 알고리즘은 주어진 수열을 가운데에서 쪼개 비슷한 크기의 수열 두 개로 만든 뒤 이들을 재귀 호출을 이용해 각각 정렬합니다.  

그 후 정렬된 배열을 하나로 합침으로써 정렬된 전체 수열을 얻습니다.  

병합 정렬은 각 단계마다 반으로 나눈 부분 문제를 재귀 호출을 이용해 해결한 뒤, 이들의 결과 수열을 합쳐 전체 문제의 답을 계산합니다. 정렬된 두 부분 수열을 합치는 데는 두 수열의 길이 합만큼 반복문을 수행해야 하기 때문에, 병합 정렬의 수행 시간은 이 병합 과정에 의해 지배됩니다.  

한 단계 내에서 모든 병합에 필요한 총 시간은 $O(N)$으로 항상 일정하며, 문제의 크기는 항상 거의 절반으로 나누어 지기 때문에 필요한 단계의 수는 $O(lgN)$이 됩니다. 따라서 병합 정렬의 시간 복잡도는 항상 $O(NlogN)$이라는 사실을 알 수 있습니다.

## 시간 복잡도  

|                       |   Best     |  Average   |  Worst     |
|-----------------------|------------|------------|------------|
| 병합 정렬(Merge Sort) | $O(NlogN)$ | $O(NlogN)$ | $O(NlogN)$ |  


## 코드
```python
def mergeSort(A):
    n = len(A)
    if n < 2:
        return
    
    mid = n // 2
    A1 = A[0:mid]
    A2 = A[mid:n]

    mergeSort(A1)
    mergeSort(A2)
    
    merge(A1, A2, A)
def merge(A1, A2, A):
    i = j = 0
    while i+j < len(A):
        if j == len(A2) or (i < len(A1) and A1[i] < A2[j]):
            A[i+j] = A1[i]
            i += 1
        else:
            A[i+j] = A2[j]
            j += 1
```  

## 동작 과정  
![merge-sort-execution](https://user-images.githubusercontent.com/59808674/117253367-1cf91700-ae82-11eb-9e40-e656747e4925.png)  

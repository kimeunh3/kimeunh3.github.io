---
title: "선택 정렬(selection sort)"
excerpt_separator: <!--more-->
categories: 
    - "Sorting"
tags: 
    - "selection sort"
use_math: true
---

선택 정렬은 모든 $i$에 대해 $A[i...N-1]$에서 가장 작은 원소를 찾은 뒤, 이것을 $A[i]$에 넣는 것을 반복합니다.  

|            	   		   |   Best   |  Average |   Worst  |
|--------------------------|:--------:|:--------:|:--------:|
| 선택 정렬(Selection Sort) | $O(N^2)$ | $O(N^2)$ | $O(N^2)$ |

## 코드
```python
def selectionSort(A):
    for i in range(len(A)):
        minIndex = i
        for j in range(i+1, len(A)):
            if A[minIndex] > A[j]:
                minIndex = j
        # swap
        temp = A[i]
        A[i] = A[minIndex]
        A[minIndex] = temp
```

## 동작 과정
![selection-sort-execution](https://user-images.githubusercontent.com/59808674/114840536-dd568680-9e11-11eb-91e0-3cc5a754ddac.png)

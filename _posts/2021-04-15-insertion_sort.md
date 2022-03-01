---
title: "삽입 정렬(insertion sort)"
excerpt_separator: <!--more-->
categories: 
    - "Sorting"
tags: 
    - "insertion sort"
use_math: true
---

삽입 정렬은 전체 배열 중 정렬되어 있는 부분 배열에 새 원소를 끼워넣는 일을 반복하는 방식으로 동작합니다.  

삽입 정렬의 최선의 경우는 처음부터 이미 정렬된 배열이 주어지는 경우입니다. 모든 원소는 처음부터 제자리에 있기 때문에 j에 대한 while문은 매번 즉시 종료하게 됩니다. 이 경우 while문의 시간 복잡도를 $O(1)$로 보아야 하기 때문에 전체 수행 시간은 선형 시간, 즉 $O(N)$이 됩니다.  

반면 최악의 경우는 역순으로 정렬된 배열이 주어지는 경우입니다. 이 경우 모든 while문은 j를 0까지 줄여가며 숫자들을 맨 앞으로 끌고 가야 합니다. 때문에 while문의 시간 복잡도는 $O(N)$이 되고, 전체 시간 복잡도는 $O(N^2)$이 됩니다.  

삽입 정렬은 흔히 사용하는 $O(N^2)$ 정렬 중 가장 빠른 알고리즘으로 알려져 있습니다.  

## 시간 복잡도  

|                           | Best   | Average  | Worst    |
|---------------------------|--------|----------|----------|
| 삽입 정렬(Insertion Sort) | $O(N)$ | $O(N^2)$ | $O(N^2)$ |  


## 코드
```python
def insertionSort(A):
    for i in range(len(A)):
        j = i
        while j > 0 and A[j-1] > A[j]:
            # swap
            temp = A[j-1]
            A[j-1] = A[j]
            A[j] = temp
            j -= 1
```

## 동작 과정
![insertion-sort-execution-1](https://user-images.githubusercontent.com/59808674/114851499-dd0fb880-9e1c-11eb-83c5-b1589003969c.png)  

![insertion-sort-execution-2](https://user-images.githubusercontent.com/59808674/114851505-ded97c00-9e1c-11eb-87b9-4ba253b5d07b.png)

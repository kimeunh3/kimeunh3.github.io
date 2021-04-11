---
title: "보글 게임 (문제 ID: BOGGLE, 난이도: 하) Python 풀이"
excerpt_separator: <!--more-->
categories: 
    - "Problem Solving"
tags: 
    - "recursion"
    - "brute force"
---



출처: [Algospot 온라인 저지](https://algospot.com/judge/problem/read/BOGGLE)

![](http://algospot.com/media/judge-attachments/09ee7a6e752f07b0d99b82a010938ab4/boggle.png)

보글(Boggle) 게임은 그림 (a)와 같은 5x5 크기의 알파벳 격자인 게임판의 한 글자에서 시작해서 펜을 움직이면서 만나는 글자를 그 순서대로 나열하여 만들어지는 영어 단어를 찾아내는 게임입니다. 펜은 상하좌우, 혹은 대각선으로 인접한 칸으로 이동할 수 있으며 글자를 건너뛸 수는 없습니다. 지나간 글자를 다시 지나가는 것은 가능하지만, 펜을 이동하지않고 같은 글자를 여러번 쓸 수는 없습니다.

예를 들어 그림의 (b), (c), (d)는 각각 (a)의 격자에서 PRETTY, GIRL, REPEAT을 찾아낸 결과를 보여줍니다.

보글 게임판과 알고 있는 단어들의 목록이 주어질 때, 보글 게임판에서 각 단어를 찾을 수 있는지 여부를 출력하는 프로그램을 작성하세요.

> **Note:** 지금부터 작성하는 솔루션은 재귀함수와 완전탐색만을 사용하여 풀은 솔루션입니다.

```python
board = [["U", "R", "L", "P", "M"], 
        ["X", "P", "R", "E", "T"], 
        ["G", "I", "A", "E", "T"], 
        ["X", "T", "N", "Z", "Y"], 
        ["X", "O", "Q", "R", "S"]]

# 상하좌우 / 대각선으로 움직이는 방향을 저장
dx = [-1, -1, -1, 1, 1, 1, 0, 0]
dy = [-1, 0, 1, -1, 0, 1, -1, 1]

# 시작 위치가 범위 내에 있는지 확인하는 함수
def isRange(y, x):
    if y < 0 or y >= len(board):
        return False
    if x < 0 or x >= len(board[0]):
        return False
    return True

def hasWord(y, x, word):
    # 시작 위치가 범위 내에 없다면 무조건 실패
    if not isRange(y, x):
        return False
    # 첫 글자가 일치하지 않으면 무조건 실패
    if board[y][x] != word[0]:
        return False
    # 단어 길이가 1이면 성공
    if len(word) == 1:
        return True
    # 인접한 8칸을 검사
    for i in range(8):
        nextX, nextY = x + dx[i], y + dy[i]
        # 위의 기저사례에서 다음 칸이 범위 내에 있는지, 
        # 첫 글자가 일치하는지 확인하므로 바로 재귀호출
        if hasWord(nextY, nextX, word[1:]):
            return True
    return False
    
print(hasWord(1, 1, "PRETTY"))
print(hasWord(2, 0, "GIRL"))
print(hasWord(1, 2, "REPEAT"))
```
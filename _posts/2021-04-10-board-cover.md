---
title: "게임판 덮기 (문제 ID: BOARDCOVER, 난이도: 하) Python 풀이"
excerpt_separator: <!--more-->
categories: 
    - "Problem Solving"
tags: 
    - "recursion"
    - "exhaustive search"
---
출처: [Algospot 온라인 저지](https://algospot.com/judge/problem/read/BOARDCOVER)

H*W 크기의 게임판이 있습니다. 게임판은 검은 칸과 흰 칸으로 구성된 격자 모양을 하고 있는데 이 중 모든 흰 칸을 3칸짜리 L자 모양의 블록으로 덮고 싶습니다. 이 때 블록들은 자유롭게 회전해서 놓을 수 있지만, 서로 겹치거나, 검은 칸을 덮거나, 게임판 밖으로 나가서는 안 됩니다. 위 그림은 한 게임판과 이를 덮는 방법을 보여줍니다.

게임판이 주어질 때 이를 덮는 방법의 수를 계산하는 프로그램을 작성하세요.

__입력__  
력의 첫 줄에는 테스트 케이스의 수 C (C <= 30) 가 주어집니다. 각 테스트 케이스의 첫 줄에는 2개의 정수 H, W (1 <= H,W <= 20) 가 주어집니다. 다음 H 줄에 각 W 글자로 게임판의 모양이 주어집니다. # 은 검은 칸, . 는 흰 칸을 나타냅니다. 입력에 주어지는 게임판에 있는 흰 칸의 수는 50 을 넘지 않습니다.

__출력__  
한 줄에 하나씩 흰 칸을 모두 덮는 방법의 수를 출력합니다.

__예제 입력__  
```
3 
3 7 
#.....# 
#.....# 
##...## 
3 7 
#.....# 
#.....# 
##..### 
8 10 
########## 
#........# 
#........# 
#........# 
#........# 
#........# 
#........# 
########## 
```
__예제 출력__
```
0
2
1514
```
__풀이__

```python
"""
ㄱ자 블럭이 커버할 수 있는 4가지의 경우의 수
블럭을 구성하는 세 칸의 상대적 위치 (dy, dx)의 목록 (아래 그림 참조)
"""
coverType = [[[0, 0], [0, 1], [1, 0]],  # (a)
            [[0, 0], [0, 1], [1, 1]],   # (b)
            [[0, 0], [1, 0], [1, 1]],   # (c)
            [[0, 0], [1, -1], [1, 0]]]  # (d)
```
![](https://user-images.githubusercontent.com/59808674/114264322-1b743480-9a25-11eb-9b25-06e7bf732081.jpg)
```python
"""
board의 (y, x)를 block_type번 방법으로 덮거나, 
덮었던 블록을 없앱니다.
delta = 1이면 덮고, -1이면 덮었던 블록을 없앱니다.
블록이 제대로 덮이지 못하는 경우 False를, 
제대로 덮일 경우 True를 반환합니다.
"""
def set_block(board, y, x, block_type, delta):
    ok = 1
    for i in range(3):
        ny = y + coverType[block_type][i][0]
        nx = x + coverType[block_type][i][1]
        # ny와 nx가 board의 범위를 벗어날 경우 False
        if ny < 0 or ny >= len(board) or nx < 0 or nx >= len(board[0]):
            ok = 0
        # 나중에 블록을 치울 때 다른 블록을 치우지 않게끔
        # 블록이 이미 놓여져 있는 상태에도 그냥 delta를 더해줌으로써
        # delta가 1일 경우는 블록이 2개 겹쳐있음을 표시해둡니다.
        elif board[ny][nx] + delta > 1:
            board[ny][nx] += delta
            ok = 0
        else:
            board[ny][nx] += delta
    return ok
"""
board의 모든 빈 칸을 덮을 수 있는 방법의 수를 반환합니다.
"""
def cover(board):
    # 가장 윗쪽이며 가장 왼쪽인 비어있는 칸을 찾아줍니다.
    y, x = -1, -1
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j] == 0:
                y, x = i, j
                break # 칸을 찾은 즉시 inner for loop break
        if y != -1:
            break # Outer for loop 또한 y값이 바뀌는 즉시 break
    # 기저 사례: 비어있는 칸을 못찾았을 시 방법을 하나 찾은거니 
    # 1을 반환해줍니다.
    if y == -1:
        return 1
    ret = 0
    for block_type in range(4):
        # 만약 board[y][x]에 block_type 형태로 놓을 수 있으면 
        # 재귀호출을 해줍니다.
        if (set_block(board, y, x, block_type, 1)):
            ret += cover(board)
        # 덮었던 블록을 치워줍니다.
        set_block(board, y, x, block_type, -1)
    return ret

C = int(input())
for _ in range(C):
    H, W = map(int, input().split())
    board = [[0] * W for _ in range(H)]
    # 비어있는 칸의 수
    count = H * W
    for a in range(H):
        for i, c in enumerate(input()):
            if c == "#": 
                board[a][i] = 1
                count -= 1
    # 비어있는 칸의 수가 3의 배수로 떨어지지 않을 경우
    # ㄱ블럭으로 커버할 수 없습니다.
    if count % 3 != 0:
        print(0) 
    else:
        print(cover(board))
```
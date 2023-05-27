---
layout: post
title: "[BOJ #17298] 오큰수 Python3"
categories: Algorithm
author: kkiyya
excerpt: 일단, 가장 단순하게 든 방법은 해당 비교하려는 인덱스 기준 오른쪽에 있는 부분을 하나씩 비교해서 찾는 방법이었다. 하지만 예상 가능하게도 이 알고리즘으로 풀게 된다면, 1부터 n까지의 합 `n(n+1)/2`으로 O(n^2)이된다. 
---

### 링크
https://www.acmicpc.net/problem/17298


<br>

### 처음 시도
일단, 가장 단순하게 든 방법은 해당 비교하려는 인덱스 기준 오른쪽에 있는 부분을 하나씩 비교해서 찾는 방법이었다.

<img src="https://github.com/codelyst-blog/codelyst-blog.github.io/assets/55094745/75261824-6632-4c5f-8ad2-430120ef70d3" width="300"/>

하지만 예상 가능하게도 이 알고리즘으로 풀게 된다면, 1부터 n까지의 합 `n(n+1)/2`으로 O(n^2)이된다. 

해당 문제가 1,000,000개의 수가 올 수 있기 때문에, 이 알고리즘이라면 1초를 넘길 수 있겠다 판단되어 시도하지 않았다.

<br>

### 풀이 과정
굳이 바로 오른쪽 것만 비교하면 되지 않을까?라는 생각으로 풀어봤다.

바로 오른쪽에 있는 수들을 검사해준다. 바로 아래처럼!

<img src="https://github.com/codelyst-blog/codelyst-blog.github.io/assets/55094745/c43c023f-58e3-4634-84ea-8603f416f023" width="300"/>

분홍색으로 칠한 부분은 아직 자신보다 큰 값을 못 만났다라는 의미다! 그래서 나중에 오른쪽에 더 큰게 온다면 그 수랑 다시 비교할 가능성이 있는 것들이다. 

오른쪽에 더 큰 수가 왔을 때의 예시는 아래와 같다.

<img src="https://github.com/codelyst-blog/codelyst-blog.github.io/assets/55094745/7f1a80e2-05cc-4dcb-9e07-e3865e8c5a0b" width="350"/>

이때, 만약 큰 수가 나타났을 때 아직 큰수를 만나보지 못한 요소들과 비교를 해보기 위해서, 이 요소도 아직 비교가 필요해요!라는 것을 표시하기 위해 스택을 사용했다. 

<img src="https://github.com/codelyst-blog/codelyst-blog.github.io/assets/55094745/d63b4b7f-187e-4f38-b8b3-b1cf7829c425" width="350"/>


### 코드

```python

N = int(input())
array = list(map(int, input().split()))
answer = [-1] * N
stack = []

for i in range(N):
    while stack and array[stack[-1]] < array[i]:
        answer[stack.pop()] = array[i]
    stack.append(i);
    
print(*answer)


``` 


<br>

### 궁금 한 점

1초가 빡세게는 1억~3억 연산량으로 생각하고 있는데, 이 문제의 입력값이 1,000,000까지 올 수 있으므로, 끽해봤자 300 * N 정도의 시간복잡도여야지만 괜찮다고 생각을 했습니다. 

사실 저 코드도 작성하면서, 이미 오큰수가 있는 수는 굳이 다시 비교하지 않는다는 점과, 바로 오른쪽에 있는 값만 비교하고, 그게 아니면 stack에 있는 값들을 보고 비교하기 때문에 기존에 생각했던 방법보다 복잡도가 확실히 줄어들거라고 생각은 합니다.

그럼에도 불구하고, 이게 어떻게 최대 연산량 수가 300 * N보다 적은지,, 계산이 잘 서지 않네요

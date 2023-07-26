---
layout: post
title: "[BOJ #14570] 나무 위의 구슬 C++"
category: algorithm
author: kritias
create_date: "2023-07-27"
preview: 트리? 그리디? 수학? 패턴 파악이 필요한 그래프 문제
image_url: "https://github.com/codelyst-blog/codelyst-blog-deprecate.github.io/assets/img/2023-07-27-algorithm/page_thumbnail.png"
---

### 링크

<a href= "https://www.acmicpc.net/problem/14570">https://www.acmicpc.net/problem/14570</a>

<br>

### 문제

좌우가 구분되어 있고 각 노드 별로 번호가 매겨진 이진트리가 있다.  
루트 노드의 번호는 1이고, 루트부터 구슬을 하나씩 흘려보낼 수 있다.  
구슬은 루트 노드에서 시작해 아래와 같은 과정을 거쳐 떨어진다.  

1. 현재 구슬이 놓인 노드의 자식이 없다면 그 자리에서 멈춘다.  
2. 1을 만족하지 않으며, 만일 현재 구슬이 놓인 노드의 자식 노드가 한 개라면 해당 자식 노드로 떨어진다.
3. 1, 2를 만족하지 않으며, 만일 현재 구슬이 놓인 노드의 자식 노드가 두 개라면,  
    1. 현재 노드의 왼쪽 서브트리에 담긴 모든 구슬의 수 <= 오른쪽 서브트리에 담긴 모든 구슬의 수일 경우, 왼쪽 자식 노드로 떨어진다.  
    2. 그 외의 경우에는 오른쪽 자식 노드로 떨어진다.  
4. 1~3번의 조건을 다시 체크하고 되풀이한다.  

구슬은 위와 같은 과정을 거쳐 결국 리프 노드에 쌓이게 된다.  
예를 들어, 위의 그림과 같은 트리에 구슬을 떨어뜨릴 경우,  
첫 다섯 개의 구슬은 2번, 4번, 2번, 5번, 2번 노드에 차례대로 떨어지게 된다.  
위처럼 트리가 충분히 작거나 구슬의 수가 충분히 적을 경우엔 직접 시뮬레이션을 통해  
구슬이 떨어지는 순서를 유추할 수가 있다.  
하지만, 우리가 관심있는 것은 큰 트리에서 많은 수의 구슬을 떨어뜨리는 과정이다.  
임의의 이진 트리가 주어지고, K가 주어졌을 때  
K번째 구슬이 어느 노드에서 멈추게 될 지 충분히 빠르게 계산해낼 수 있을까?  

<br>

### 입력

첫 줄에 이진 트리의 노드의 수 N이 주어진다. (1 ≤ N ≤ 200000)  
둘째 줄부터 N개의 줄에 걸쳐, U V가 주어진다.

i번째 줄에 주어지는 U, V는 각각 i번 노드의 왼쪽 자식이 U, 오른쪽 자식이 V임을 의미한다.

만약 U = -1 또는 V = -1이라면, 해당 위치에 자식 노드가 존재하지 않는다는 것이다.

그 외의 경우엔 항상 2 ≤ U, V ≤ N을 만족한다.

이어 마지막 줄에 문제에서 설명한 K가 주어진다. (1 ≤ K ≤ 1018)

주어지는 트리는 항상 올바른 이진 트리임이 보장되며, 루트는 항상 1번 노드이다.

<br>

### 출력

K번째 구슬이 떨어지는 노드의 번호를 출력한다.  
<br>

### 풀이

구슬이 떨어지는 과정을 요약하면,  

누적된 구슬이 더 적은 서브트리의 방향으로 떨어지고, 누적된 구슬의 개수가 같다면 왼쪽으로 떨어진다.  

구슬을 S개 떨어뜨린다고 했을 때,  

S가 짝수인 경우 왼쪽에 S/2개, 오른쪽에 S/2개.  

S가 홀수인 경우 왼쪽에 S/2+1개, 오른쪽에 S/2개가 떨어진다. 

본 과정을 K-1개의 구슬에 대해 재귀적으로 리프노드까지 처리한 뒤, 마지막으로 남은 1개의 구슬을 떨어뜨리고 그 경과를 보면 된다.  

```c++

// 나무 위의 구슬 / 14570.cpp
// 알고리즘 분류: 그래프 이론, 그래프 탐색, 트리, 깊이 우선 탐색

#include <iostream>
#include <cstdlib>
#include <vector>
#include <algorithm>
using namespace std;
typedef long long int ll;

void init()
{
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);
}

vector<ll> child_count;
vector<pii> tree;

// k-1개의 구슬을 일괄적으로 흘려보내는 함수. 각 노드 별로 누적 구슬을 전역 배열에 저장해주므로 리턴값이 필요없다.
void dfs(ll node, ll flow)
{
	child_count[node] = flow;
	ll left = flow / 2 + flow % 2;
	ll right = flow / 2;

    // -1이면 없는 노드임을 처리해준다.
	bool valid_first = tree[node].first != -1;
	bool valid_second = tree[node].second != -1;

    // 있는 자식 노드에만 흘려보내주고, 한쪽이 없다면 다른 한쪽에 모든 구슬을 흘려보낸다.
	if(valid_first) dfs(tree[node].first, valid_second ? left : flow);
	if(valid_second) dfs(tree[node].second, valid_first ? right : flow);
}

// 마지막 구슬을 흘려보내는 함수. 하나만 흘려보내기 때문에 개수 변수인 flow가 필요하지 않다.
ll dfs(ll node)
{
	if(tree[node].first == -1 && tree[node].second == -1) return node;
	if(tree[node].first == -1) return dfs(tree[node].second);
	if(tree[node].second == -1) return dfs(tree[node].first);
	if(child_count[tree[node].first] <= child_count[tree[node].second]) return dfs(tree[node].first);
	return dfs(tree[node].second);
}

int main(void) {
	init();
	ll n;
	cin >> n;
	tree = vector<pii>(n+1);
	child_count = vector<ll>(n+1, 0);
	for(ll i = 1; i <= n; i++)
	{
        // first는 왼쪽 노드 번호, second는 오른쪽 노드 번호
		ll left, right;
		cin >> tree[i].first >> tree[i].second;
	}
	ll k;
	cin >> k;
    // 먼저 루트 노드에 k-1개를 흘려보낸다.
	dfs(1, k-1);
    // 마지막 구슬을 루트 노드에 흘려보낸다.
	ll ans = dfs(1);
	cout << ans << '\n';
	return 0;
}

```

<br>

### 풀고 나서

구슬을 한꺼번에 흘려보내는 방법을 알면 쉽게 풀 수 있을 것 같다.  

특정 방향의 노드가 없을 때 다른 방향의 노드로 모든 구슬을 흘려보내는 처리,  

구슬의 개수가 long long int 범위임을 고려하지 못해서 3번 만에 맞았는데,  

난이도 기여를 보니 발상 자체가 떠올리기 어려운 문제였던 것 같다.  
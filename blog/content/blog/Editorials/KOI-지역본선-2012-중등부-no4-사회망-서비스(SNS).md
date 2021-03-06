---
title: KOI 지역본선 2012 중등부, no.4 사회망 서비스(SNS)
date: 2014-06-11 21:17:00
category: Editorials
---

* [BOJ 2533 사회망 서비스(SNS)](http://acmicpc.net/problem/2533)

## 문제요약

친구 관계는 아래와 같이 그래프 형태로 나타낼 수 있다. 아래 그래프는 철수와 영희, 철수와 만수, 영희와 순희가 친구임을 나타내는 그래프이다.  

![p25331](../images/p25331.png)

이런 친구 관계 그래프를 보면 사회망 서비스(SNS)를 쉽게 이해할 수 있는데, 사회망 서비스에는 얼리어답터인 사람과 얼리어답터가 아닌 사람으로 나뉜다. 어느 한 사람이 얼리어답터가 되려면, 그 사람의 모든 친구들이 얼리어답터일 때 얼리어답터가 될 수 있다. 이런 친구 관계 그래프가 주어졌을 때, 그래프의 모든 노드들이 얼리어답터가 될 수 있도록 최소의 얼리어답터를 배치하는 문제이다. 일반적인 그래프에서 이 문제를 푸는 것은 굉장히 어려우므로 친구 관계 그래프가 트리인 경우에만 고려한다. 아래와 같은 친구 관계 그래프가 주어진다면 표시한 정점 세 노드가 얼리어답터이면 된다.

![p25332](../images/p25332.png)


## 해법

트리 다이나믹 문제이다. 트리는 각 노드들을 루트로 하는 SubTree를 만들 수 있다. 아래와 같은 그래프를 예로 들어보자. 

![p25333](../images/p25333.png)

이 때, 2를 루트로 하는 서브그래프를 빨간색으로, 그리고 3을 루트로 하는 서브그래프를 하늘색으로 표시하면 아래와 같은 그래프가 되고, 1은 다시 빨간색 서브트리와 하늘색 서브트리를 자식노드로 하는 루트가 된다. 이 때, 서브트리리에서의 해를 이용하여 루트로 올라가면서 해를 구하면 된다.

![p25334](../images/p25334.png)

점화식은 

* $d[i][state]$ : $i$ 정점을 $root$로 갖는 서브트리에서의 최소 얼리어답터 수<br>($state$ 는 0일 때 $i$ 정점이 얼리어답터가 아닌 경우이고, 1일 때 $i$정점이 얼리어답터인 경우이다.)

로 둘 수 있고,

* $d[i][0] = 0, d[i][1] = 1$
* $d[i][0] += d[child][1]$
* $d[i][1] += min(d[child][0],d[child][1])$

로 해결할 수 있다. 


```cpp
#include <cstdio>
#include <cstring>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;
int d[1111111][2];
int degree[1111111];
bool vis[1111111];
int n;
vector<vector<int> > v;
int main() {
    scanf("%d",&n);
    if ( n == 2 ) return puts("1"),0;
    v.resize(n);
    for ( int i = 0 ; i < n-1 ; i++ ) {
        int a,b;
        scanf("%d%d",&a,&b);
        a--;b--;
        v[a].push_back(b);
        v[b].push_back(a);
        degree[a]++;degree[b]++;
    }
    queue<int> q;
    for ( int i = 0 ; i < n ; i++ )
        d[i][1] = 1;
    int root = -1;
    for ( int i = 0 ; i < n ; i++ )
        if ( degree[i] == 1 ) {
            q.push(i);
            root = i;
        }
    while ( !q.empty() ) {
        int now = q.front();q.pop();
//        printf("%d : d[%d][0] = %d , d[%d][1] = %d\n",now+1,now+1,d[now][0],now+1,d[now][1]);
        vis[now] = true;
        for ( int i = 0 ; i < (int)v[now].size() ; i++ ) {
            if ( vis[v[now][i]] ) continue;
            d[v[now][i]][0] += d[now][1];
            d[v[now][i]][1] += min(d[now][0],d[now][1]);
            degree[v[now][i]]--;
            if ( degree[v[now][i]] == 1 ) {
                q.push(v[now][i]);
                root = v[now][i];
            }
        }
    }
hell:
    printf("%d\n",min(d[root][0],d[root][1]));
    return 0;
}
```

> 엄청 오랜만에 쓰니까 어색하다.


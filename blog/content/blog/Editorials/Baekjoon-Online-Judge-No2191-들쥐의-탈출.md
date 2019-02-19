---
title: Baekjoon Online Judge No.2191 들쥐의 탈출
date: 2014-03-17 16:17:00
category: Editorials
---

* [BOJ 2191 들쥐의 탈출](http://acmicpc.net/problem/2191)

## 문제요약

$N$ 마리의 들쥐와 $M$ 개의 땅굴이 있다 $(1\leq{}N\leq{}100,1\leq{}M\leq{}100)$. 그리고 각 들쥐와 땅굴의 위치는 2차원 평면상에 위치해 있고, 각각의 좌표는 절대값이 $1,000$이 넘지 않는 실수형으로 이루어져 있다. 땅굴에는 최대 한마리의 들쥐만 들어갈 수 있고, 매는 들어올 수 없다.  

이 때, 매가 $S$초가 지난 후에 지상으로 내려오고, 들쥐들은 매 초 $V$의 속도로 땅굴을 향하여 도망친다고 했을 때, 매한테 먹히는 들쥐가 최소가 되도록 땅굴에 매칭해주는 문제이다$(1\leq{}S\leq{}100,1\leq{}V\leq{}100)$.

## 해법

전형적인 이분매칭 문제이다. 하나의 그룹을 들쥐로 하고, 다른 그룹을 땅굴로 하여 이분 그래프를 구성한 다음, 가장 많은 쥐가 땅굴로 들어갈 수 있도록 매칭해주면 된다. 이 때, 매를 피해야 하므로 이분 그래프를 구성할 때, 매를 피해 땅굴로 들어갈 수 있도록 간선을 연결해주면 된다. 


```cpp
#include <cstdio>
#include <cmath>
#include <cstring>
#include <vector>
#include <algorithm>
using namespace std;
int N,M,S,V;
int backMatch[222];
bool visited[222];
vector<vector<int> > vv;
bool dfs(int now) {
    if ( visited[now] ) return false;
    visited[now] = true;
    for ( int i = 0 ; i < vv[now].size(); i++ ) {
        int next = vv[now][i];
        if ( backMatch[next] == -1 || dfs(backMatch[next]) ) {
            backMatch[next] = now;
            return true;
        }
    }
    return false;
}
int matching() {
    memset(backMatch,-1,sizeof(backMatch));
    int matched = 0;
    for ( int i = 1 ; i <= N ; i++ ) {
        memset(visited,false,sizeof(visited));
        if ( dfs(i) ) matched++;
    }
    return matched;
}
typedef pair<double,double> dd;
double sq(double a){return a*a;}
bool ok(dd a,dd b) {
    double dist=sqrt(sq(a.first-b.first)+sq(a.second-b.second));
    if ( dist <= S*V ) return true;
    return false;
}
int main() {
    scanf("%d%d%d%d",&N,&M,&S,&V);
    vector<dd> mouse,cave;
    for ( int i = 0 ; i < N ; i++ ) {
        double a,b;
        scanf("%lf%lf",&a,&b);
        mouse.push_back(dd(a,b));
    }
    for ( int i = 0 ; i < M ; i++ ) {
        double a,b;
        scanf("%lf%lf",&a,&b);
        cave.push_back(dd(a,b));
    }
    vv.resize(N+1);
    for ( int i = 0 ; i < N ; i++ )
        for ( int j = 0 ; j < M ; j++ )
            if ( ok(mouse[i],cave[j]) )
                vv[i+1].push_back(j+1);
    printf("%d\n",N-matching());
    return 0;
}
```

> 매칭매칭

---
title: Northwestern European Regional Contest > Nordic Collegiate Programming Contest 2008, no.A Aspen Avenue
date: 2013-10-24 11:26:00
category: Editorials
---

* [BOJ 5042 나무 옮기기](http://acmicpc.net/problem/5042)

## 문제요약

백준이형네 집의 입구에서부터 분수까지의 길이가 $L$미터이고, 그 사이의 도로의 폭이 $W$미터이다$(1\leq{}L\leq{}10,000, 1\leq{}W\leq{}20)$. 이 도로의 양쪽 끝에 나무를 심어야하고 그 사이의 거리가 전무 일정해야하는데 도로의 왼쪽에만 나무를 심어서 옮겨야 한다. 규칙에 맞게 이동시켜야하는 나무 이동 거리의 최소값을 구하는 문제이다. 

## 해법

문제의 규칙에 따르면 나무 사이의 거리 $d=L/(N/2-1)$가 되고 이 문제는 다이나믹 프로그래밍으로 해결할 수 있다. 먼저 입력으로 주어진 $a_i$ 배열을 오름차순으로 정렬한 뒤 $D[i][j]$를 왼쪽 도로에 $i$개를 심고 오른쪽 도로에 $j$개를 심는다고 정의를 하면,  

$D[i][j]=min(D[i][j],D[i-1][j]+dist(0,d*(i-1)-a[l+r-1]))$<br>
$D[i][j]=min(D[i][j],D[i][j-1]+dist(W,d*(j-1)-a[l+r-1]))$

로 해결할 수 있다.

```cpp
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <string>
#include <vector>
#include <cmath>
using namespace std;
typedef long long ll;
#define inf 1e30
double d[1111][1111],now;
int a[3333];
int N,L,W;
double dist(double a,double b) {
    return sqrt(a*a+b*b);
}
double go(int l,int r) {
    if ( l < 0 || r < 0 ) return inf;
    if ( l+r == 0 ) return 0;
    double& ret = d[l][r];
    if ( ret ) return ret;
    ret = inf;
    ret = min(ret,go(l-1,r)+dist(0,now*(l-1)-a[l+r-1]));
    ret = min(ret,go(l,r-1)+dist(W,now*(r-1)-a[l+r-1]));
    return ret;
}
int main() {
    scanf("%d%d%d",&N,&L,&W);
    now = (double)L/(N/2-1);
    for ( int i = 0 ; i < N ; i++ )
        scanf("%d",&a[i]);
    sort(a,a+N);
    memset(d,0,sizeof(d));
    printf("%.6lf\n",go(N/2,N/2));
    return 0;
}
```

> 디피디피
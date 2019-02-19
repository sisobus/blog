---
title: Croatian Highschool 2008, National Competition #1 no.2 KUPUS
date: 2013-09-23 19:46:00
category: Editorials
---

* [BOJ 3123 배추](http://acmicpc.net/problem/3123)

## 문제요약

건물 옥상의 크기 $X, Y$ $(1\leq{}X,Y\leq{}1000)$가 주어지고, 반지름이 1인 원만큼 물을 뿌릴 수 있는 $N$개의 스프링쿨러 $(1\leq{}N\leq{}10000)$ 가 주어졌을 때, 물을 뿌릴 수 있는 영역의 넓이를 구하는 문제이다.


## 해법

$X,Y$의 크기가 작기 때문에 옥상 크기만큼 2차원 배열을 잡아서 스프링쿨러들이 겹치는지 안겹치는지를 잘 판단해서 해결하면 쉽게 풀 수 있다.




```cpp
#include <cstdio>
#include <algorithm>
#include <vector>
#include <set>
#include <cmath>
using namespace std;
#define pi 3.1415926535
const double sec1=sqrt(3)/4-pi/12;
const double sec2=1-(pi/4);
const double sec3=pi;
const double sec4=pi/6+sqrt(3)/4;
const double sec5=pi/4;
bool a[1111][1111];
  int main() {
    int X,Y;
    scanf("%d%d",&X,&Y);
    int n;
    scanf("%d",&n);
    for (int i = 0; i < n; i++) {
        int nx,ny;
        scanf("%d%d",&nx,&ny);
        a[nx][ny]=true;
    }
    double ans=0;
    for (int x1 = 0; x1 < X; x1++)
        for (int y1 = 0; y1 < Y; y1++) {
            int x2=x1+1, y2=y1+1;
            if ((a[x1][y1] && a[x2][y2]) || (a[x1][y2] && a[x2][y1])) ans++;
            else if (a[x1][y1]+a[x1][y2]+a[x2][y1]+a[x2][y2] == 2) ans += sec4;
            else if (a[x1][y1]+a[x1][y2]+a[x2][y1]+a[x2][y2] == 1) ans += sec5;
        }
    printf("%.2lf\n",ans);
    return 0;
}
```



>맨 처음에 각 스프링쿨러마다 $8$방향 안에 있는 모든 스프링쿨러를 본다고 생각해서 막혔었다.
>앞으로 $N$제한이나 $X,Y$제한을 먼저 보고 생각한다면 쉽게 접근할 수 있을 것 같다.








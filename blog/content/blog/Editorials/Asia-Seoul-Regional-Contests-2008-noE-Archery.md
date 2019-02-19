---
title: Asia Seoul Regional Contests 2008, no.E Archery
date: 2013-09-25 08:27:00
category: Editorials
---

문제요약

아래와 같이 X축$(\leq{}W)$에서 선분을 그었을 때, N개의 다른 모든 선분을 관통할 수 있는지 아닌지를 보는 문제이다. 선분들은 L,R로 표시할 수 있다.$(2\leq{}W\leq{}10,000,000,1\leq{}N\leq{}5,000,0\leq{}L,R\leq{}W)$ 

아래와 같이 X축$(\leq{}W)$에서 선분을 그었을 때, N개의 다른 모든 선분을 관통할 수 있는지 아닌지를 보는 문제이다. 선분들은 L,R로 표시할 수 있다.$(2\leq{}W\leq{}10,000,000,1\leq{}N\leq{}5,000,0\leq{}L,R\leq{}W)$ 





해법

정답이 있다고 가정했을 때, 이 정답 선분을 왼쪽으로 평행이동 시키게 되면, 반드시 어느 한 선분의 왼쪽 점에 닿게 된다. 따라서 임의의 선분 왼쪽점에서 모든 선분을 관통하는 선분을 그을 수 있는지 없는지의 문제가 된다. 이 때, 어느 선분 왼쪽점에서 다른 선분의 왼쪽점과 오른쪽점을 연결해서 그은 선분들의 범위가 하나라도 겹치지 않는다면 관통할 수 있는 선분이 없는 것이고, 모든 범위가 겹치게 된다면 관통할 수 있는 선분이 있다. 범위를 Line Sweeping으로 겹치는지 안겹치는지 확인할 수 있고, 왼쪽 선분과 오른쪽 선분을 좁혀나가면서 범위가 튀지 않는지를 검사해도 된다. $O(N^2)$

정답이 있다고 가정했을 때, 이 정답 선분을 왼쪽으로 평행이동 시키게 되면, 반드시 어느 한 선분의 왼쪽 점에 닿게 된다. 따라서 임의의 선분 왼쪽점에서 모든 선분을 관통하는 선분을 그을 수 있는지 없는지의 문제가 된다. 이 때, 어느 선분 왼쪽점에서 다른 선분의 왼쪽점과 오른쪽점을 연결해서 그은 선분들의 범위가 하나라도 겹치지 않는다면 관통할 수 있는 선분이 없는 것이고, 모든 범위가 겹치게 된다면 관통할 수 있는 선분이 있다. 범위를 Line Sweeping으로 겹치는지 안겹치는지 확인할 수 있고, 왼쪽 선분과 오른쪽 선분을 좁혀나가면서 범위가 튀지 않는지를 검사해도 된다. $O(N^2)$


```
<![CDATA[ #include <cstdio>#include <vector>#include <cmath>#include <algorithm>using namespace std;
 typedef pair<double,double> dd;
 struct line{     int D,L,R;
     line(){}     line(int D,int L,int R):         D (D),L (L), R(R){} };
 #define eps 1e-9 int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         int W,N;
         scanf("%d%d",&W,&N);
         vector<line> v;
         for ( int i = 0 ;
 i < N ;
 i++ ) {             int D,L,R;
             scanf("%d%d%d",&D,&L,&R);
             v.push_back(line(D,L,R));
         }         bool ok = true;
         for ( int i = 0 ;
 i < N ;
 i++ ) {             ok = true;
             double l=-987654321,r=987654321;
             for ( int j = 0 ;
 j < N ;
 j++ ) {                 if ( i == j ) continue;
                 double nowL=((double)v[i].L-(double)v[j].L)/((double)v[i].D-(double)v[j].D);
                 double nowR=((double)v[i].L-(double)v[j].R)/((double)v[i].D-(double)v[j].D);
                 if ( nowL > nowR ) swap(nowL,nowR);
                 l = l>nowL?l:nowL;
                 r = r<nowR?r:nowR;
                 if ( l>r && fabs(l-r)>eps ) {                     ok = false;
                     break;
                 }             }             if ( ok ) break;
         }         if ( ok ) printf("YES\n");
         else printf("NO\n");
     }      return 0;
 } ]]>
```
Comment


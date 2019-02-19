---
title: German Collegiate Programming Contest 2013, no.D Kastenlauf
date: 2013-09-26 15:04:00
category: Editorials
---

문제요약

맥주 20병을 들고 50미터마다 맥주를 한병씩 마셔야만 하는 상황에서 목적지에 갈 수 있는지 없는지를 판단하는 문제이다. 맥주는 추가로 더 구매할 수 있지만 20병을 넘길 수는 없다. 편의점의 개수 $N$과 목적지의 위치 $X,Y$가 주어진다. $(0\leq{}N\leq{}100, -32768\leq{}X,Y\leq{}32767)$ 

맥주 20병을 들고 50미터마다 맥주를 한병씩 마셔야만 하는 상황에서 목적지에 갈 수 있는지 없는지를 판단하는 문제이다. 맥주는 추가로 더 구매할 수 있지만 20병을 넘길 수는 없다. 편의점의 개수 $N$과 목적지의 위치 $X,Y$가 주어진다. $(0\leq{}N\leq{}100, -32768\leq{}X,Y\leq{}32767)$ 

해법

영어해석이 잘 안되서 어려웠던 문제였다. 해석만 하면 쉽게 풀 수 있다. 

영어해석이 잘 안되서 어려웠던 문제였다. 해석만 하면 쉽게 풀 수 있다. 

한번에 최대 이동할 수 있는 거리가 $20 * 50m$ 이므로 내가 갈 수 있는 편의점일 때에 반드시 가면 된다. 따라서 매 순간마다 갈 수 있는 편의점을 맨하탄 거리로 $1,000$을 넘지 않는 모든 지점을 방문하면 된다.

한번에 최대 이동할 수 있는 거리가 $20 * 50m$ 이므로 내가 갈 수 있는 편의점일 때에 반드시 가면 된다. 따라서 매 순간마다 갈 수 있는 편의점을 맨하탄 거리로 $1,000$을 넘지 않는 모든 지점을 방문하면 된다.


```
<![CDATA[ #include <cstdio>#include <cstring>#include <vector>#include <algorithm>using namespace std;
 typedef pair<int,int> ii;
 bool b[1111];
 int n;
 vector<ii> v;
 void go(int now) {     if ( b[now] ) return;
     b[now] = true;
     for ( int i= 0 ;
 i < n ;
 i++ )         if ( abs(v[now].first-v[i].first)+abs(v[now].second-v[i].second)<=1000)             go(i);
 } int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         scanf("%d",&n);
         n+=2;
         memset(b,false,sizeof(b));
         v.clear();
         for ( int i = 0 ;
 i < n ;
 i++ ) {             int x,y;
             scanf("%d%d",&x,&y);
             v.push_back(ii(x,y));
         }         go(0);
         printf("%s\n",b[n-1]?"happy":"sad");
     }     return 0;
 } ]]>
```
Comment


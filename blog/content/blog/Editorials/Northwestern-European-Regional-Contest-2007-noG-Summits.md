---
title: Northwestern European Regional Contest 2007, no.G Summits
date: 2013-10-24 12:38:00
category: Editorials
---

문제요약

$d$-정상의 수를 구하는 문제이다. $d$-정상이란 높이가 $h$인 어떤점이 $d$-정상이 되려면 그 점에서 출발해서 높이가 $h$-$d$보다 작거나 같은 곳을 방문하지 않고 더 높은 곳을 갈 수 없어야 한다$(1\leq{}h,w\leq{}500, 1\leq{}d\leq{}1,000,000,000, 0\leq{}h\leq{}1,000,000,000)$.

$d$-정상의 수를 구하는 문제이다. $d$-정상이란 높이가 $h$인 어떤점이 $d$-정상이 되려면 그 점에서 출발해서 높이가 $h$-$d$보다 작거나 같은 곳을 방문하지 않고 더 높은 곳을 갈 수 없어야 한다$(1\leq{}h,w\leq{}500, 1\leq{}d\leq{}1,000,000,000, 0\leq{}h\leq{}1,000,000,000)$.





해법

각 점들을 높이 기준으로 내림차순 정렬을 하면 $BFS$를 돌 때, 중복해서 같은 점을 방문하지 않고 자신보다 높은 곳으로 올라 갈 수 있는지 확인할 수 있다.

각 점들을 높이 기준으로 내림차순 정렬을 하면 $BFS$를 돌 때, 중복해서 같은 점을 방문하지 않고 자신보다 높은 곳으로 올라 갈 수 있는지 확인할 수 있다.


```
<![CDATA[ #include <cstdio>#include <queue>#include <cstring>#include <vector>#include <algorithm>#include <string>using namespace std;
 struct point{     int y,x,h;
     point(){}     point(int y,int x,int h):         y(y),x(x),h(h){} };
 int h,w,D,a[555][555],d[555][555];
 vector<point> v;
 int dx[]={1,0,-1,0};
 int dy[]={0,1,0,-1};
 int go(int now) {     int ret=1;
     bool ok = true;
     if ( ~d[v[now].y][v[now].x] ) return false;
     d[v[now].y][v[now].x] = v[now].h;
     queue<point> q;
     q.push(v[now]);
     while ( !q.empty() ) {         point nq=q.front();
q.pop();
         for ( int i = 0 ;
 i < 4;
 i++ ) {             int ny=nq.y+dy[i],nx=nq.x+dx[i];
             if ( ny < 0 || ny >= h || nx < 0 || nx >= w ) continue;
             if ( a[ny][nx] <= v[now].h-D ) continue;
             if ( ~d[ny][nx] ) {                 if ( d[ny][nx] != v[now].h ) ok = false;
                 continue;
             }             if ( a[ny][nx] > v[now].h-D ) {                 ret += (a[ny][nx] == v[now].h);
                 d[ny][nx] = v[now].h;
                 q.push(point(ny,nx,nq.h));
             }         }     }     return ok?ret:ok;
 } bool cmp(const point& e1,const point& e2) {     return e1.h > e2.h;
 } int main() {     int tc;
     for ( scanf("%d",&tc) ;
 tc-- ;
 ) {         scanf("%d%d%d",&h,&w,&D);
         memset(a,0,sizeof(a));
         memset(d,-1,sizeof(d));
         v.clear();
         for ( int i = 0 ;
 i <h ;
 i++ )             for ( int j = 0 ;
 j < w;
 j++ ) {                 scanf("%d",&a[i][j]);
                 v.push_back(point(i,j,a[i][j]));
             }         sort(v.begin(),v.end(),cmp);
         int ans=0;
         for ( int i = 0 ;
 i < v.size() ;
 i++ )             ans += go(i);
         printf("%d\n",ans);
     }     return 0;
 } ]]>
```
Comment


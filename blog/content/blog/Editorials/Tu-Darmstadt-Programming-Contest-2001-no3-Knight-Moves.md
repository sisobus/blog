---
title: Tu-Darmstadt Programming Contest 2001, no.3 Knight Moves
date: 2013-10-07 23:22:00
category: Editorials
---

문제요약

나이트가 한 번에 이동할 수 있는 위치가 아래와 같이 있고, 현재 위치와 목적지 위치가 주어졌을 때 현재 위치에서 목적지까지의 최단거리를 구하는 문제이다. 체스판은 정사각형이며 한 변의 길이 $L$이 주어진다$(1\leq{}L\leq{}300)$.

나이트가 한 번에 이동할 수 있는 위치가 아래와 같이 있고, 현재 위치와 목적지 위치가 주어졌을 때 현재 위치에서 목적지까지의 최단거리를 구하는 문제이다. 체스판은 정사각형이며 한 변의 길이 $L$이 주어진다$(1\leq{}L\leq{}300)$.









해법

한 변의 길이도 $300$밖에 안되고 그래서 여러 솔루션이 존재할 것 같다. 나는 그냥 $BFS$를 돌면서 범위를 점차 넓혀가며 목적지까지 도착을 하였고, 결국 최단경로를 찾는 문제이므로 $Dijkstra$를 돌려도 되지 않을까 싶다. 하지만 그냥 쉽게 $BFS$돌리는게 더 나을 것 같다. 

한 변의 길이도 $300$밖에 안되고 그래서 여러 솔루션이 존재할 것 같다. 나는 그냥 $BFS$를 돌면서 범위를 점차 넓혀가며 목적지까지 도착을 하였고, 결국 최단경로를 찾는 문제이므로 $Dijkstra$를 돌려도 되지 않을까 싶다. 하지만 그냥 쉽게 $BFS$돌리는게 더 나을 것 같다. 






```
<![CDATA[ #include <cstdio>#include <cstring>#include <algorithm>#include <vector>#include <map>#include <queue>using namespace std;
 typedef pair<int,int> ii;
 int dy[]={-2,-1,1,2,2,1,-1,-2};
 int dx[]={1,2,2,1,-1,-2,-2,-1};
 int n;
 int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         scanf("%d",&n);
         ii in,goal;
         scanf("%d%d%d%d",&in.first,&in.second,&goal.first,&goal.second);
         queue<ii> q;
         q.push(ii(in));
         bool vis[333][333]={};
         int d[333][333]={};
         while ( !q.empty() ) {             ii now = q.front();
q.pop();
             if ( now.first == goal.first && now.second == goal.second ) break;
             for ( int i = 0 ;
 i < 8 ;
 i++ ) {                 int ny=now.first+dy[i],nx=now.second+dx[i];
                 if ( ny < 0 || nx < 0 || ny >= n || nx >= n || vis[ny][nx] ) continue;
                 d[ny][nx] = d[now.first][now.second]+1;
                 vis[ny][nx] = true;
                 q.push(ii(ny,nx));
             }         }         printf("%d\n",d[goal.first][goal.second]);
     }     return 0;
 } ]]>
```
Comment


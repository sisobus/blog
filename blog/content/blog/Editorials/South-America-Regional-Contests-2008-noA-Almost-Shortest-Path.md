---
title: South America Regional Contests 2008, no.A Almost Shortest Path
date: 2013-09-24 15:13:00
category: Editorials
---

문제요약

최단경로에 해당하는 경로를 제외한 최단경로를 찾는 문제이다. 최단경로가 여러개가 있을경우 그러한 간선들도 전부 제외한다. $(2\leq{}N\leq{}500,1\leq{}M\leq{}10^4)$

최단경로에 해당하는 경로를 제외한 최단경로를 찾는 문제이다. 최단경로가 여러개가 있을경우 그러한 간선들도 전부 제외한다. $(2\leq{}N\leq{}500,1\leq{}M\leq{}10^4)$

해법

시작점에서 dijkstra를 돌린 결과 배열을 d1[]이라 하고, 끝점에서 dijkstra를 돌린 결과 배열을 d2[]라고 하면 $d1[i]+d2[i]==d1[끝점]$ 인 점 $i$가 최단경로에 있는 점에 해당한다. 따라서 dijkstra를 한번 더 돌리는데, 최단경로에 있는 간선이면 제외하고 돌리면 된다.

시작점에서 dijkstra를 돌린 결과 배열을 d1[]이라 하고, 끝점에서 dijkstra를 돌린 결과 배열을 d2[]라고 하면 $d1[i]+d2[i]==d1[끝점]$ 인 점 $i$가 최단경로에 있는 점에 해당한다. 따라서 dijkstra를 한번 더 돌리는데, 최단경로에 있는 간선이면 제외하고 돌리면 된다.


```
<![CDATA[ #include <cstdio>#include <cstring>#include <queue>#include <vector>#include <algorithm>using namespace std;
 typedef pair<int,int> ii;
 vector<vector<ii> > v;
 vector<vector<ii> > backV;
 vector<int> d;
 vector<int> backD;
 vector<bool> b;
 const int inf=0x7FFFFFFF;
 /*  * v.resize(V+!),d.resize(V+1);
  */ int dijkstra(int s,int e,vector<int>& d,vector<vector<ii> > v) {     priority_queue<ii,vector<ii>,greater<ii> > pq;
     fill(d.begin(),d.end(),inf);
     d[s]=0;
     pq.push(ii(d[s],s));
     while ( !pq.empty() ){         ii now=pq.top();
pq.pop();
         int cur=now.second;
         if ( d[cur] < now.first ) continue;
         for ( int i = 0 ;
 i < v[cur].size() ;
 i++ ) {             ii next=v[cur][i];
             if ( d[next.first] > d[cur]+next.second ) {                 d[next.first] = d[cur]+next.second;
                 pq.push(ii(d[next.first],next.first));
             }         }     }     return d[e];
 } int dijkstra2(int s,int e,vector<bool> b,vector<vector<ii> > v) {     priority_queue<ii,vector<ii>,greater<ii> > pq;
     fill(d.begin(),d.end(),inf);
     d[s]=0;
     pq.push(ii(d[s],s));
     while ( !pq.empty() ){         ii now=pq.top();
pq.pop();
         int cur=now.second;
         if ( d[cur] < now.first ) continue;
         for ( int i = 0 ;
 i < v[cur].size() ;
 i++ ) {             ii next=v[cur][i];
             if ( b[cur] && b[next.first] ) continue;
             if ( d[next.first] > d[cur]+next.second ) {                 d[next.first] = d[cur]+next.second;
                 pq.push(ii(d[next.first],next.first));
             }         }     }     return d[e]==inf?-1:d[e];
 } int main() {     for ( int n,m;
 scanf("%d%d",&n,&m)==2 && n&&m ;
 ) {         int st,ed;
         scanf("%d%d",&st,&ed);
         v.resize(n+1);
d.resize(n+1);
b.resize(n+1);
         backV.resize(n+1);
backD.resize(n+1);
         for ( int i = 0 ;
 i < m ;
 i++ ) {             int from,to,cost;
             scanf("%d%d%d",&from,&to,&cost);
             v[from].push_back(ii(to,cost));
             backV[to].push_back(ii(from,cost));
         }         dijkstra(st,ed,d,v);
         dijkstra(ed,st,backD,backV);
         for ( int i = 0 ;
 i < n ;
 i++ )             if ( d[i]+backD[i] == d[ed] )                 b[i] = true;
         int ans=dijkstra2(st,ed,b,v);
         printf("%d\n",ans);
         v.clear(),d.clear(),b.clear(),backV.clear(),backD.clear();
     }     return 0;
 } ]]>
```
Comment


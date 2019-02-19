---
title: Strongly Connected Component (SCC)
date: 2013-09-27 15:08:00
category: Algorithm
---

문제요약

방향 그래프에서 $SCC$를 구하는 문제이다. $SCC$란 방향그래프에서 정점의 최대 부분집합을 뜻하며 부분집합의 서로 다른 임의의 두 정점 $u$에서 $v$로 가는 경로와, $v$에서 $u$로 가는 경로가 모두 존재하는 경우를 말한다. 아래와 같은 그래프에서는 $(1,4,5)$ , $(6)$ ,$(2,3,7)$의 세 $SCC$가 만들어진다. 

방향 그래프에서 $SCC$를 구하는 문제이다. $SCC$란 방향그래프에서 정점의 최대 부분집합을 뜻하며 부분집합의 서로 다른 임의의 두 정점 $u$에서 $v$로 가는 경로와, $v$에서 $u$로 가는 경로가 모두 존재하는 경우를 말한다. 아래와 같은 그래프에서는 $(1,4,5)$ , $(6)$ ,$(2,3,7)$의 세 $SCC$가 만들어진다. 









해법

먼저 입력으로 들어오는 방향 그래프의 정보로 그래프를 구성하면서, 역방향의 그래프도 구성한다. 그리고 $DFS$를 돌면서 탐색한 노드를 차례로 저장한다. 그리고 다시한번 역방향을 돌면서 만들어지는 그래프들이 $SCC$가 된다.

먼저 입력으로 들어오는 방향 그래프의 정보로 그래프를 구성하면서, 역방향의 그래프도 구성한다. 그리고 $DFS$를 돌면서 탐색한 노드를 차례로 저장한다. 그리고 다시한번 역방향을 돌면서 만들어지는 그래프들이 $SCC$가 된다.


```
<![CDATA[ #include <cstdio>#include <cstring>#include <vector>#include <algorithm>#include <queue>using namespace std;
 int V,E;
 vector<vector<int> > v,G,scc;
 vector<int> tmp,tmp1;
 bool b[11111];
 void go(int now) {     b[now] = true;
     for ( int i = 0 ;
 i < v[now].size() ;
 i++ )         if ( !b[v[now][i]] )             go(v[now][i]);
     tmp.push_back(now);
 } void go1(int now) {     b[now] = true;
     tmp1.push_back(now);
     for ( int i = 0 ;
 i < G[now].size() ;
 i++ )         if ( !b[G[now][i]] )             go1(G[now][i]);
 } int main() {     scanf("%d%d",&V,&E);
     v.resize(V+1);
     G.resize(V+1);
     for ( int i = 0 ;
 i < E;
 i++ ) {         int st,ed;
         scanf("%d%d",&st,&ed);
         v[st].push_back(ed);
         G[ed].push_back(st);
     }     memset(b,false,sizeof(b));
     for ( int i = 1 ;
 i <= V ;
 i++ )         if ( !b[i] )             go(i);
     memset(b,false,sizeof(b));
     for ( int i = (int)tmp.size()-1 ;
 i >= 0 ;
 i-- ) {         if ( !b[tmp[i]] ){             go1(tmp[i]);
             sort(tmp1.begin(),tmp1.end());
             tmp1.push_back(-1);
             scc.push_back(tmp1);
             tmp1.clear();
         }     }     sort(scc.begin(),scc.end());
     printf("%d\n",(int)scc.size());
     for ( int i = 0 ;
 i < scc.size() ;
 i++ ) {         for ( int j = 0 ;
 j < scc[i].size() ;
 j++ )             printf("%d ",scc[i][j]);
         puts("");
     }     return 0;
 } ]]>
```
Comment


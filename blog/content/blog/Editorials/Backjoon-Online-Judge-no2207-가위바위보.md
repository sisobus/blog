---
title: Backjoon Online Judge no.2207 가위바위보
date: 2013-09-30 13:34:00
category: Editorials
---

문제요약

$N$명의 학생이 벌을 받아야 하는데, 원장 선생님이 $M$번의 가위바위보를 할 때, 무엇을 낼 지 맞추면 살 수 있고, 못 맞추면 죽는 문제이다$(1\leq{}N,M\leq{}10,000)$. 각 학생에게는 두 번의 기회가 있고 두 번 중 한번만 맞춰도 살 수 있다. 

$N$명의 학생이 벌을 받아야 하는데, 원장 선생님이 $M$번의 가위바위보를 할 때, 무엇을 낼 지 맞추면 살 수 있고, 못 맞추면 죽는 문제이다$(1\leq{}N,M\leq{}10,000)$. 각 학생에게는 두 번의 기회가 있고 두 번 중 한번만 맞춰도 살 수 있다. 

해법

전형적인 $2-SAT$문제이다. 먼저 $-A$에서 $B$로 간선을 만들고, $-B$에서 $A$로 간선을 만든 그래프와 그 역방향 그래프를 만든다. 그리고 $SCC$를 구성한 뒤, 그 안에서 가능한지 안가능한지를 확인하면 되는 해결할 수 있다. 

전형적인 $2-SAT$문제이다. 먼저 $-A$에서 $B$로 간선을 만들고, $-B$에서 $A$로 간선을 만든 그래프와 그 역방향 그래프를 만든다. 그리고 $SCC$를 구성한 뒤, 그 안에서 가능한지 안가능한지를 확인하면 되는 해결할 수 있다. 


```
<![CDATA[ #include <cstdio>#include <vector>#include <cstring>#include <string>#include <algorithm>#include <stack>using namespace std;
 int N,V;
 vector<vector<int> > v,G,scc;
 vector<int> tmp1,tmp2;
 vector<bool> vis,vis2;
 int RetIndex(int a) {     return a>0?((a-1)*2):((a+1)*-2+1);
 } void go(int now) {     vis[now] = true;
     for ( int i = 0 ;
 i < v[now].size() ;
 i++ )         if ( !vis[v[now][i]] )             go(v[now][i]);
     tmp1.push_back(now);
 } bool ok(int now) {     int prev = now&1?(now-1):(now+1);
     if ( vis2[prev] ) return false;
     vis[now] = vis2[now] = true;
     for ( int i = 0;
 i < G[now].size() ;
 i++ )         if ( !vis[G[now][i]] )             if ( !ok(G[now][i]) )                 return false;
     return true;
 } int main() {     scanf("%d %d",&N,&V);
     V *= 2;
     v.resize(V+1),G.resize(V+1),scc.resize(V+1);
     for ( int i = 0 ;
 i < N ;
 i++ ) {         int a,b;
         scanf("%d%d",&a,&b);
         int A,B,minusA,minusB;
         A = RetIndex(a);
         minusA = RetIndex(-a);
         B = RetIndex(b);
         minusB = RetIndex(-b);
         v[minusA].push_back(B);
         v[minusB].push_back(A);
         G[B].push_back(minusA);
         G[A].push_back(minusB);
     }     vis.resize(V,false);
     vis2.resize(V,false);
     for ( int i = 0 ;
 i < V ;
 i++ )         if ( !vis[i] )             go(i);
     fill(vis.begin(),vis.end(),false);
     for ( int i = (int)tmp1.size()-1 ;
 i >= 0 ;
 i-- )         if ( !vis[i] ) {             fill(vis2.begin(),vis2.end(),false);
             if ( !ok(tmp1[i]) )                 return printf("OTL\n"),0;
         }     printf("^_^\n");
     return 0;
 } ]]>
```
Comment


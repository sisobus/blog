---
title: Sogang Programming Contest Champion 2013 토렌트
date: 2014-01-04 15:46:00
category: Editorials
---

문제요약

$ACM$토렌트는 시드가 접속하는 시간에 시드로부터 조각을 전송받아 파일을 완성하는 방법으로 공유된다. $N$개의 조각으로 나누어진 파일을 공유받으려 할 때, 모든 파일 조각을 받을 최소 시간을 구하는 문제이다. $M$명의 사람이 조각을 가지고 있고, 각 시드가 접속하는 시간과 나가는 시간, 가지고 있는 조각의 개수 $a$와 조각의 번호가 주어진다$(1\leq{}N,M\leq{}100)$.

$ACM$토렌트는 시드가 접속하는 시간에 시드로부터 조각을 전송받아 파일을 완성하는 방법으로 공유된다. $N$개의 조각으로 나누어진 파일을 공유받으려 할 때, 모든 파일 조각을 받을 최소 시간을 구하는 문제이다. $M$명의 사람이 조각을 가지고 있고, 각 시드가 접속하는 시간과 나가는 시간, 가지고 있는 조각의 개수 $a$와 조각의 번호가 주어진다$(1\leq{}N,M\leq{}100)$.

  

  

해법

조각의 번호를 하나의 집합으로 하고, 접속하는 시간을 다른 하나의 집합으로 본다면 이 두 집합은 독립이며 이 두 집합을 연결하여 Bipartite Graph를 구성할 수 있다. 따라서 시간을 기준으로 그래프를 구성한 뒤, 매칭된 수가 총 조각의 개수 $N$보다 같거나 크다면 기준 시간에 모든 조각을 받을 수 있다.

조각의 번호를 하나의 집합으로 하고, 접속하는 시간을 다른 하나의 집합으로 본다면 이 두 집합은 독립이며 이 두 집합을 연결하여 Bipartite Graph를 구성할 수 있다. 따라서 시간을 기준으로 그래프를 구성한 뒤, 매칭된 수가 총 조각의 개수 $N$보다 같거나 크다면 기준 시간에 모든 조각을 받을 수 있다.


```
<![CDATA[ #include <cstdio>#include <cstring>#include <algorithm>#include <vector>#include <cstdlib>#include <queue>using namespace std;
 int N,M;
 int backMatch[222];
 bool visited[222];
 vector<vector<int> > vv;
 bool dfs(int now) {     if ( visited[now] ) return false;
     visited[now] = true;
     for ( int i = 0 ;
 i < vv[now].size();
 i++ ) {         int next = vv[now][i];
         if ( backMatch[next] == -1 || dfs(backMatch[next]) ) {             backMatch[next] = now;
             return true;
         }     }     return false;
 } int matching() {     memset(backMatch,-1,sizeof(backMatch));
     int matched = 0;
     for ( int i = 1 ;
 i <= N ;
 i++ ) {         memset(visited,false,sizeof(visited));
         if ( dfs(i) ) matched++;
     }     return matched;
 } int main() {     int  tc;
     scanf("%d",&tc);
     while ( tc-- ) {         scanf("%d%d",&N,&M);
         vector<vector<int> > v;
         v.resize(N+1);
         for ( int i = 0 ;
 i < M ;
 i++ ) {             int t1,t2,a;
             scanf("%d%d%d",&t1,&t2,&a);
             for ( int j = 0 ;
 j < a ;
 j++ ) {                 int t;
                 scanf("%d",&t);
                 for ( int k = t1 ;
 k < t2 ;
 k++ )                     v[t].push_back(k);
             }         }         int ans = 2147483647;
         bool ok = false;
         int l=0,r=101;
         while ( l <= r ) {             int mid = (l+r)>>1;
             vv.resize(N+1);
             for ( int i = 1 ;
 i <= N ;
 i++ )                 for ( int j = 0 ;
 j < v[i].size() ;
 j++ )                     if ( v[i][j] < mid ) vv[i].push_back(v[i][j]);
             if ( matching() >= N ) {                 ok = true;
                 ans = min(ans,mid);
                 r = mid-1;
             } else l = mid +1;
             for ( int i = 0 ;
 i < vv.size() ;
 i++ )                 vv[i].clear();
             vv.clear();
         }         printf("%d\n",ok?ans:-1);
         for ( int i = 0 ;
 i < v.size() ;
 i++ )             v[i].clear();
         v.clear();
     }     return 0;
 } ]]>
```
Comment


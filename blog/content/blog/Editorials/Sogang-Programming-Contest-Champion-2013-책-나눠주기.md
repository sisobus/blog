---
title: Sogang Programming Contest Champion 2013 책 나눠주기
date: 2014-01-03 18:43:00
category: Editorials
---

문제요약

백준이 형이 $N$개의 전공서적을 가지고 있는데$(1\leq{}N\leq{}1,000)$, 너무 많아서 후배 학부생들에게 나눠주려고 한다. 물려받기를 원하는 후배 학부생들이 $M$명이 있고$(1\leq{}M\leq{}1,000)$, 각 후배 학부생들은 원하는 책을 $a$번부터 $b$번까지 신청할 수 있다$(1\leq{}a,b\leq{}N)$. 이 때, 책을 줄 수 있는 최대 학부생의 수를 구하는 문제이다. 

백준이 형이 $N$개의 전공서적을 가지고 있는데$(1\leq{}N\leq{}1,000)$, 너무 많아서 후배 학부생들에게 나눠주려고 한다. 물려받기를 원하는 후배 학부생들이 $M$명이 있고$(1\leq{}M\leq{}1,000)$, 각 후배 학부생들은 원하는 책을 $a$번부터 $b$번까지 신청할 수 있다$(1\leq{}a,b\leq{}N)$. 이 때, 책을 줄 수 있는 최대 학부생의 수를 구하는 문제이다. 





해법

학생들을 하나의 집합으로 보고, 나눠 줄 수 있는 책을 다른 하나의 집합으로 생각한다면 Bipartite Graph가 된다. $N$제한이 1,000밖에 안되기 때문에 이분 매칭으로 해결하면 된다. 실제 원 문제는 $N$제한이 $100,000$인데, 이 때에는 $Greedy$하게 해결하면 풀 수 있다고 한다. 

학생들을 하나의 집합으로 보고, 나눠 줄 수 있는 책을 다른 하나의 집합으로 생각한다면 Bipartite Graph가 된다. $N$제한이 1,000밖에 안되기 때문에 이분 매칭으로 해결하면 된다. 실제 원 문제는 $N$제한이 $100,000$인데, 이 때에는 $Greedy$하게 해결하면 풀 수 있다고 한다. 






```
<![CDATA[ #include <cstdio>#include <cstring>#include <cstdlib>#include <map>#include <set>#include <vector>#include <algorithm>#include <stack>#include <queue>#include <string>using namespace std;
 #define MAX_V 1004 const int inf = 987654321;
 vector<vector<int> > v;
 int N,M;
 int backMatch[MAX_V*2+5];
 bool visited[MAX_V*2+5];
 bool dfs(int now) {     if ( visited[now] ) return false;
     visited[now] = true;
     for ( int i = 0 ;
 i < v[now].size() ;
i ++ ) {         int next = v[now][i];
         if ( backMatch[next] == -1 || dfs(backMatch[next] ) ) {             backMatch[next] = now;
             return true;
         }     }     return false;
 } int matching() {     memset(backMatch,-1,sizeof(backMatch));
     int matched = 0;
     for ( int i = 0 ;
 i < v.size() ;
 i++ ) {         memset(visited,false,sizeof(visited));
         if ( dfs(i) ) matched++;
     }     return matched;
 } int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         scanf("%d%d",&N,&M);
         v.resize(M+10);
         for ( int i = 0 ;
 i < M ;
 i++ ) {             int a,b;
             scanf("%d%d",&a,&b);
             a--;
 b--;
             for ( int j = M+a ;
 j <= M+b ;
 j++ )                 v[i].push_back(j);
         }         printf("%d\n",matching());
         for ( int i = 0 ;
 i < v.size() ;
 i++ )             v[i].clear();
         v.clear();
     }     return 0;
 } ]]>
```
Comment


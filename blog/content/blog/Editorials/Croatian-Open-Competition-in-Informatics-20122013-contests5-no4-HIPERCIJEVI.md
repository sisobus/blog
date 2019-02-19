---
title: Croatian Open Competition in Informatics 2012/2013, contests#5 no.4 HIPERCIJEVI
date: 2013-10-14 01:16:00
category: Editorials
---

문제요약

미래에 하이퍼 튜브라는 시스템이 있다. 하이퍼 튜브는 $K$개의 역을 서로 연결해주는 역할을 하고, 총 $M$개의 하이퍼 튜브가 존재한다. 이 때, 1번 역에서 $N$번째 역까지 이동하는 데 방문하는 최소 역의 수를 출력하는 문제이다.

미래에 하이퍼 튜브라는 시스템이 있다. 하이퍼 튜브는 $K$개의 역을 서로 연결해주는 역할을 하고, 총 $M$개의 하이퍼 튜브가 존재한다. 이 때, 1번 역에서 $N$번째 역까지 이동하는 데 방문하는 최소 역의 수를 출력하는 문제이다.





해법

보통 그래프를 구성할 때, $vector<vector<int> >$의 자료구조를 이용하여 구성할텐데, 입력받을 때 $K$개 만큼의 역을 입력받으면서, 이 세 역을 동시에 이어줘야하는데 어려움이 있을 수 있다. 이 때, 각 $K$개 만큼의 역을 새로운 더미 노드에 연결시켜주면 더미노드를 거쳐가는 길이 되겠지만 $K$개의 역은 연결되어진 상태가 된다. 따라서 이 작업으로 모든 노드를 연결한 그래프를 구성하면 $N+M$개의 노드가 만들어지고 $M*K$의 간선이 만들어 진다. 이 그래프로 최소 역을 구한다음, 그 값을 $d[N]$이라 했을 때, 더미노드를 제외한 최소 방문 역은 $d[N]/2+1$이 된다. 

보통 그래프를 구성할 때, $vector<vector<int> >$의 자료구조를 이용하여 구성할텐데, 입력받을 때 $K$개 만큼의 역을 입력받으면서, 이 세 역을 동시에 이어줘야하는데 어려움이 있을 수 있다. 이 때, 각 $K$개 만큼의 역을 새로운 더미 노드에 연결시켜주면 더미노드를 거쳐가는 길이 되겠지만 $K$개의 역은 연결되어진 상태가 된다. 따라서 이 작업으로 모든 노드를 연결한 그래프를 구성하면 $N+M$개의 노드가 만들어지고 $M*K$의 간선이 만들어 진다. 이 그래프로 최소 역을 구한다음, 그 값을 $d[N]$이라 했을 때, 더미노드를 제외한 최소 방문 역은 $d[N]/2+1$이 된다. 






```
<![CDATA[ #include <cstdio>#include <cstring>#include <string>#include <vector>#include <queue>#include <algorithm>using namespace std;
 typedef pair<int,int> ii;
 const int inf = 987654321;
 int N,K,M;
 vector<vector<int> > v;
 int d[111111];
 int main() {     scanf("%d%d%d",&N,&K,&M);
     v.resize(N+M+K+1);
     for ( int i = 1 ;
 i <= M ;
 i++ )         for ( int j = 0 ;
 j < K ;
 j++ ) {             int t ;
             scanf("%d",&t);
             v[t].push_back(i+N);
             v[i+N].push_back(t);
         }     memset(d,-1,sizeof(d));
     d[1] = 0;
     queue<int> q;
     q.push(1);
     while ( !q.empty() && !(~d[N]) ) {         int now=q.front();
q.pop();
         for ( int i = 0 ;
 i < v[now].size() ;
 i++ )             if ( !(~d[v[now][i]]) ) {                 d[v[now][i]] = d[now]+1;
                 q.push(v[now][i]);
             }     }     printf("%d\n",d[N]<0?-1:(d[N]/2+1));
     return 0;
 } ]]>
```
Comment


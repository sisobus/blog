---
title: University of Ulm Local Contest 1996, no.F Lotto
date: 2013-10-07 22:45:00
category: Editorials
---

문제요약

독일 로또는 $(1-49)$까지의 49개의 숫자 중에서 6개를 뽑는 방식이다. 로또 번호를 선택하는 좋은 전략 중에 하나는 $K$개의 숫자를 골라서 그 중에 6개를 뽑는 전략이다$(7\leq{}K\leq{}12)$. $K$와 숫자가 주어질 때, 선택할 수 있는 모든 경우를 출력하는 문제이다. 

독일 로또는 $(1-49)$까지의 49개의 숫자 중에서 6개를 뽑는 방식이다. 로또 번호를 선택하는 좋은 전략 중에 하나는 $K$개의 숫자를 골라서 그 중에 6개를 뽑는 전략이다$(7\leq{}K\leq{}12)$. $K$와 숫자가 주어질 때, 선택할 수 있는 모든 경우를 출력하는 문제이다. 





해법

선택할 수 있는 경우의 수가 최대 $12\choose{6}$밖에 안되므로 백트랙킹으로 모든 경우를 출력해 주면 된다. 

선택할 수 있는 경우의 수가 최대 $12\choose{6}$밖에 안되므로 백트랙킹으로 모든 경우를 출력해 주면 된다. 


```
<![CDATA[ #include <cstdio>#include <cstring>#include <string>#include <vector>#include <algorithm>using namespace std;
 vector<int> v;
 void go(int idx,vector<int> now) {     if ( idx >= (int)v.size() ) return;
     if ( now.size() == 6 ) {         for ( int i = 0 ;
 i < now.size() ;
 i++ )             printf("%d ",now[i]);
         puts("");
     }     for ( int i = idx ;
 i < v.size() && (int)now.size() < 6;
 i++ ) {         now.push_back(v[i]);
         go(i+1,now);
         now.pop_back();
     } } int main() {     bool first=true;
     for ( int n;
scanf("%d",&n)==1 && n ;
 )  {         if ( !first ) puts("");
         v.resize(n+1,0);
         for ( int i = 0 ;
 i < n ;
 i++ )             scanf("%d",&v[i]);
         vector<int> ans;
         go(0,ans);
         first = false;
     }     return 0;
 } ]]>
```
Comment


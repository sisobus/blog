---
title: Baekjoon Online Judge no.1450 Knapsack Problem
date: 2013-11-21 13:37:00
category: Editorials
---

문제요약

$N$개의 물건을 가지고 있고, 최대 C만큼의 무게를 넣을 수 있는 가방이 있을 때, $N$개의 물건을 가방에 넣을 수 있는 경우의 수를 구하는 문제이다. $(N\leq{}30)$, $(0\leq{}C,Weight\leq{}10^9)$

$N$개의 물건을 가지고 있고, 최대 C만큼의 무게를 넣을 수 있는 가방이 있을 때, $N$개의 물건을 가방에 넣을 수 있는 경우의 수를 구하는 문제이다. $(N\leq{}30)$, $(0\leq{}C,Weight\leq{}10^9)$





해법

일반적인 냅색 문제의 변형이다. $C$가 굉장히 큰 반면에 $N$이 30밖에 되지 않으므로 이를 이용하여 해결할 수 있다. 떠올릴 수 있는 아이디어는 비트를 이용하는 아이디어지만 $2^{30}=1073741824$ 이므로 또 너무 크다. 따라서 30이라는 큰 $N$을 반으로 나눈 다음 반을 계산한 뒤 나머지 반을 찾으면 된다. 

일반적인 냅색 문제의 변형이다. $C$가 굉장히 큰 반면에 $N$이 30밖에 되지 않으므로 이를 이용하여 해결할 수 있다. 떠올릴 수 있는 아이디어는 비트를 이용하는 아이디어지만 $2^{30}=1073741824$ 이므로 또 너무 크다. 따라서 30이라는 큰 $N$을 반으로 나눈 다음 반을 계산한 뒤 나머지 반을 찾으면 된다. 






```
<![CDATA[ #include <cstdio>#include <vector>#include <cstring>#include <algorithm>using namespace std;
 typedef long long ll;
 int main() {     int N,C;
     scanf("%d%d",&N,&C);
     vector<ll> v;
     vector<ll> d;
     for ( int i = 0 ;
 i < N ;
 i++ ) {         ll t;
         scanf("%lld",&t);
         v.push_back(t);
     }     int mid = N/2,n=0;
     for ( int i = 0,j=N-mid ;
 i < (1<<j) ;
 i++ ) {         ll s = 0;
         for ( int k = 0 ;
 k < j ;
 k++ )             if ( i&(1<<k) ) s += v[mid+k];
         d.push_back(s);
     }     sort(d.begin(),d.end());
     ll ans=0;
     for ( int i = 0 ;
 i < (1<<mid) ;
 i++ ) {         ll s =0 ;
         for ( int k = 0 ;
 k < mid ;
 k++ )             if ( i&(1<<k) ) s += v[k];
         ans += upper_bound(d.begin(),d.end(),C-s)-d.begin();
     }printf("%lld\n",ans);
     return 0;
 } ]]>
```
Comment


---
title: Croatian Open Competition in Informatics 2013/2014, contests#1 no.5 ORGANIZATOR
date: 2013-10-04 18:49:00
category: Editorials
---

문제요약

$N$개의 학교에서 $K$명의 학생이 대회에 출전한다$(2\leq{}N\leq{}200,000,1\leq{}K\leq{}2,000,000)$. 홍준이가 대회의 팀 구성원을 몇 명으로 할지 정하는 것이 문제이다. 팀 구성원으로 나누어 떨어지지 않는 학교는 대회에 참가할 수 없고, 예선에서 1등한 팀만 본선에 나갈 수 있으며 본선에는 최소 두 학교가 진출해야 한다. 이 때, 본선에서 볼 수 있는 최대 학생 수를 구하는 문제이다. 

$N$개의 학교에서 $K$명의 학생이 대회에 출전한다$(2\leq{}N\leq{}200,000,1\leq{}K\leq{}2,000,000)$. 홍준이가 대회의 팀 구성원을 몇 명으로 할지 정하는 것이 문제이다. 팀 구성원으로 나누어 떨어지지 않는 학교는 대회에 참가할 수 없고, 예선에서 1등한 팀만 본선에 나갈 수 있으며 본선에는 최소 두 학교가 진출해야 한다. 이 때, 본선에서 볼 수 있는 최대 학생 수를 구하는 문제이다. 

해법

본선에 최소 두 학교가 진출해야 하는 조건이 있으므로 출전하는 학생의 약수가 2개이상 같아야 조건을 만족할 수 있다. 또, 본선에서 볼 수 있는 학생은 약수의 개수* 약수 만큼이므로 모든 출전 학교의 학생수의 약수의 개수를 구하면 해결할 수 있다. 하지만 $N$만큼 $K$의 모든 약수를 구한다면 $O(N\sqrt{K})$의 시간이 결리므로 시간 내에 구할 수가 없다. 따라서 $map$같은 자료구조를 이용하여 학생수 $K$의 개수를 센 다음, 약수의 개수를 구할 때 학생 수의 개수만큼 더해주면 빠르게 약수의 개수를 구할 수 있다. 

본선에 최소 두 학교가 진출해야 하는 조건이 있으므로 출전하는 학생의 약수가 2개이상 같아야 조건을 만족할 수 있다. 또, 본선에서 볼 수 있는 학생은 약수의 개수* 약수 만큼이므로 모든 출전 학교의 학생수의 약수의 개수를 구하면 해결할 수 있다. 하지만 $N$만큼 $K$의 모든 약수를 구한다면 $O(N\sqrt{K})$의 시간이 결리므로 시간 내에 구할 수가 없다. 따라서 $map$같은 자료구조를 이용하여 학생수 $K$의 개수를 센 다음, 약수의 개수를 구할 때 학생 수의 개수만큼 더해주면 빠르게 약수의 개수를 구할 수 있다. 


```
<![CDATA[ #include <cstdio>#include <algorithm>#include <vector>#include <cmath>using namespace std;
 typedef long long ll;
 ll c[2000001];
 int cnt[2000001];
 vector<int> v;
 int main() {     int n;
     scanf("%d",&n);
     for ( int i = 0 ;
 i < n ;
 i++ ) {         int t;
         scanf("%d",&t);
         if ( cnt[t] == 0 ) v.push_back(t);
         cnt[t]++;
     }     for ( int i = 0 ;
 i < v.size() ;
 i++ ) {         int now=v[i],val = cnt[v[i]];
         for ( int i = 1 ;
 i <= sqrt(now) ;
 i++ )         if ( !(now%i) ) {             c[i]+=val;
             if ( i != sqrt(now) )                 c[now/i]+=val;
         }     }     ll ans=0;
     for ( int i = 1 ;
 i <= 2000000 ;
 i++ )         if ( c[i] >= 2 )             ans = max(ans,c[i]*(ll)i);
     printf("%lld\n",ans);
     return 0;
 } ]]>
```
Comment


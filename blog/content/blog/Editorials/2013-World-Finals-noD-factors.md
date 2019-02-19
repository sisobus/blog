---
title: 2013 World Finals, no.D factors
date: 2013-11-17 15:36:00
category: Editorials
---

문제요약

1보다 큰 모든 자연수는 하나 또는 그 이상의 곱으로 표시할 수 있다. 예를 들면, 

1보다 큰 모든 자연수는 하나 또는 그 이상의 곱으로 표시할 수 있다. 예를 들면, 

$10 = 2*5 = 5*2$

$10 = 2*5 = 5*2$

$20 = 2*2*5 = 2*5*2 = 5*2*2$ 

$20 = 2*2*5 = 2*5*2 = 5*2*2$ 

와 같이 나타낸다.

와 같이 나타낸다.

$k$를 소인수의 곱으로 나타낼 때, $f(k)$를 서로다른 $Arrangement$의 개수라고 하자. 따라서 $f(10)=2$, $f(20)=3$이 된다. 양의 정수 $n$이 주어질 때, $f(k)=n$인 k는 적어도 하나 이상 존재한다. 이 때, 이를 만족하는 가장 작은 $k$를 구하는 문제이다.

$k$를 소인수의 곱으로 나타낼 때, $f(k)$를 서로다른 $Arrangement$의 개수라고 하자. 따라서 $f(10)=2$, $f(20)=3$이 된다. 양의 정수 $n$이 주어질 때, $f(k)=n$인 k는 적어도 하나 이상 존재한다. 이 때, 이를 만족하는 가장 작은 $k$를 구하는 문제이다.





해법

$k = a^b+c^d+e^f+...$로 나타낼 때, $Different$ $Arrangement$는 $\dfrac{(b+d+f..)!}{b!d!f!...!}$이다. 가장 작은 수를 구하라고 했으므로 $a,c,e...$는 작은 소수부터 차례대로 증가시키면서 $Backtracking$을 돌리면 된다. 

$k = a^b+c^d+e^f+...$로 나타낼 때, $Different$ $Arrangement$는 $\dfrac{(b+d+f..)!}{b!d!f!...!}$이다. 가장 작은 수를 구하라고 했으므로 $a,c,e...$는 작은 소수부터 차례대로 증가시키면서 $Backtracking$을 돌리면 된다. 






```
<![CDATA[ #include <cstdio>#include <vector>#include <string>#include <cstring>#include <algorithm>#include <map>using namespace std;
 typedef unsigned long long ll;
 #define inf 9223372036854775807ll int prime[]={2,3,5,7,11,13,17,19,23,29,31,37,41,43,47};
 ll nCm[111][111];
 map<ll,ll> ans;
 void go(int idx,ll n,ll k, int sum) {     if ( k <= 0 || n <= 0 || !prime[idx] ) return ;
     if ( sum > 0 ) ans[n]=ans[n]==0?k:min(ans[n],k);
     int t =1;
ll now =k;
     while ( prime[idx] <= inf/now ) {         now *= prime[idx];
         if ( nCm[sum+t][t] <= inf/n )             go(idx+1,nCm[sum+t][t]*n,now,sum+t);
         t++;
     } } int main() {     for ( int i = 0 ;
 i < 111 ;
 i++ )         nCm[i][0] = nCm[i][i] = 1;
     for ( int i = 2 ;
 i < 111 ;
 i++ )         for ( int j = 1 ;
 j < i;
 j++ )             nCm[i][j] = nCm[i-1][j-1]+nCm[i-1][j];
     ll a;
     go(0,1,1,0);
     while ( scanf("%llu",&a) != EOF ) {         printf("%llu %llu\n",a,ans[a]);
     }     return 0;
 } ]]>
```
Comment


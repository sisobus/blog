---
title: Backjoon Online Judge no.2023 신기한 소수
date: 2013-10-04 00:25:00
category: Editorials
---

문제요약

절단가능소수(Truncatable prime)란 왼쪽이나 오른쪽자리부터 차례대로 절단해서 만들어지는 수가 소수인 수를 뜻한다. 각각을 왼쪽 절단가능소수, 오른쪽 절단가능소수라 하는데 이 문제는 $N$자리 수의 오른쪽 절단가능소수를 구하는 문제이다$(1\leq{}N\leq{}8)$. 예를 들면, 7331은 그 자체도 소수이고, 733, 73, 7 모두 소수이기 때문에 오른쪽 절단가능소수이다.

절단가능소수(Truncatable prime)란 왼쪽이나 오른쪽자리부터 차례대로 절단해서 만들어지는 수가 소수인 수를 뜻한다. 각각을 왼쪽 절단가능소수, 오른쪽 절단가능소수라 하는데 이 문제는 $N$자리 수의 오른쪽 절단가능소수를 구하는 문제이다$(1\leq{}N\leq{}8)$. 예를 들면, 7331은 그 자체도 소수이고, 733, 73, 7 모두 소수이기 때문에 오른쪽 절단가능소수이다.

해법

일의자리 소수부터 시작해서 자리수를 늘려가며 $DFS$로 가능한 소수를 찾으면 해결할 수 있다. 

일의자리 소수부터 시작해서 자리수를 늘려가며 $DFS$로 가능한 소수를 찾으면 해결할 수 있다. 


```
<![CDATA[ #include <cstdio>#include <cstring>#include <string>#include <algorithm>using namespace std;
 int N;
 int prime[]={2,3,5,7};
 bool ok(int now) {     if ( now == 1 ) return false;
     for ( int i = 2 ;
 i*i <= now ;
 i++ )         if ( !(now%i) ) return false;
     return true;
 } void go(int now,int len) {     if ( len == N ) {         printf("%d\n",now);
         return ;
     }     int next = now*10;
     for ( int i = 1;
  i<= 9 ;
 i+=2 )         if ( ok(next+i) )             go(next+i,len+1);
 } int main() {     scanf("%d",&N);
     for ( int i = 0 ;
 i <4 ;
 i++ )         go(prime[i],1);
     return 0;
 } ]]>
```
Comment


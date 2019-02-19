---
title: Northwestern European Regional Contest 2008, no.D Disgruntled Judge
date: 2013-10-24 10:14:00
category: Editorials
---

문제요약

총 $2T$개의 랜덤 숫자를 만드는 문제인데, $x_{2*i-1}$이 입력으로 주어지고, 나머지 숫자를 구하는 문제이다$(1\leq{}T\leq{}100)$. 랜덤 숫자를 만드는 방법은 세 정수 $x_i$,$a$,$b$를 임의로 구했을 때, $i$를 $2$부터 $2T$까지 증가시키면서 $x_i=(a*x_{i-1}+b)$ $mod$ $10001$의 식을 이용하여 만들 수 있다.

총 $2T$개의 랜덤 숫자를 만드는 문제인데, $x_{2*i-1}$이 입력으로 주어지고, 나머지 숫자를 구하는 문제이다$(1\leq{}T\leq{}100)$. 랜덤 숫자를 만드는 방법은 세 정수 $x_i$,$a$,$b$를 임의로 구했을 때, $i$를 $2$부터 $2T$까지 증가시키면서 $x_i=(a*x_{i-1}+b)$ $mod$ $10001$의 식을 이용하여 만들 수 있다.





해법

$O(N^2)$만큼 다 돌면서 매치가 되지않을 때까지 돌려주면 된다. 

$O(N^2)$만큼 다 돌면서 매치가 되지않을 때까지 돌려주면 된다. 






```
<![CDATA[ #include <cstdio>#include <algorithm>using namespace std;
 const int mod = 10001;
 int T;
 int x[1111];
 int f(int a,int x,int b) {     return (a*x+b)%mod;
 } bool go(int a,int b) {     for ( int i = 2 ;
 i <= T ;
 i++ )         if ( !(i&1) ) x[i]=f(a,x[i-1],b);
         else if ( f(a,x[i-1],b) != x[i] )             return false;
     return true;
 } int main() {     scanf("%d",&T);
     T*=2;
     for ( int i = 1 ;
 i <= T ;
 i+=2 )         scanf("%d",&x[i]);
     for ( int i = 0 ;
 i < mod ;
 i++ )         for ( int j = 0 ;
 j < mod ;
 j++ )             if ( go(i,j) ) goto heaven;
 heaven:;
        for ( int i = 2 ;
 i <= T ;
 i+=2 )            printf("%d\n",x[i]);
     return 0;
 } ]]>
```
Comment


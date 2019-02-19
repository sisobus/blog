---
title: Latin America Regional Contests 2012, no.I Interval Product
date: 2013-09-24 16:02:00
category: Editorials
---

문제요약

Ballmer's Peak Theory)에 따르면, 프로그래머의 혈중 알콜 농도가 0.129% 와 0.138% 사이일 때 초인적인 코딩 실력을 발휘하기 때문이다.

문제는 총 N개의 정수로 이루어진 수열 $X_i$가 있고, K번의 쿼리가 있을 예정이다 $(1\leq{}N,K\leq{}10^5,-100\leq{}X_i\leq{}100)$. 각 쿼리는 $X_i$의 값을 변경하거나, 구간의 곱셈을 하여 음수인지 0인지 양수인지를 판단하여 출력해주는 문제이다.

문제는 총 N개의 정수로 이루어진 수열 $X_i$가 있고, K번의 쿼리가 있을 예정이다 $(1\leq{}N,K\leq{}10^5,-100\leq{}X_i\leq{}100)$. 각 쿼리는 $X_i$의 값을 변경하거나, 구간의 곱셈을 하여 음수인지 0인지 양수인지를 판단하여 출력해주는 문제이다.

해법

N과 K가 $10^5$ 이나 되기 때문에, 전부다 해보는 O($N*K$) 솔루션은 시간초과가 난다. 따라서 효율적으로 쿼리를 수행하기 위하여 Fenwick Tree 자료구조를 이용하여 해결한다.곱셈을 전부다 수행한다면 long long으로도 못담기 때문에, 우리가 알아야 할 것을 음수와 0으로 줄여서 생각해 봐야 한다. 따라서 마이너스의 개수와 0의 개수를 tree에 담아서 음수와 0의 개수를 곱셈 쿼리를 할 때마다 세서 출력해 주면 된다.그리고 변경 쿼리가 들어올 때에는, tree에서 기존에 있던 숫자를 하나씩 빼주고, 새로 집어넣을 숫자를 하나씩 증가시켜주면 된다.

N과 K가 $10^5$ 이나 되기 때문에, 전부다 해보는 O($N*K$) 솔루션은 시간초과가 난다. 따라서 효율적으로 쿼리를 수행하기 위하여 Fenwick Tree 자료구조를 이용하여 해결한다.곱셈을 전부다 수행한다면 long long으로도 못담기 때문에, 우리가 알아야 할 것을 음수와 0으로 줄여서 생각해 봐야 한다. 따라서 마이너스의 개수와 0의 개수를 tree에 담아서 음수와 0의 개수를 곱셈 쿼리를 할 때마다 세서 출력해 주면 된다.그리고 변경 쿼리가 들어올 때에는, tree에서 기존에 있던 숫자를 하나씩 빼주고, 새로 집어넣을 숫자를 하나씩 증가시켜주면 된다.


```
<![CDATA[ #include <cstdio>#include <cstring>int n = 111111;
 int _0[1<<20],minus[1<<20],N,K;
 bool select;
 int read(int *tree,int idx) {     int sum = 0;
     while ( idx > 0 ) {         sum += tree[idx];
         idx -= ( idx & -idx);
     }     return sum;
 } void update(int *tree,int idx,int val) {     while ( idx <= n ) {         tree[idx] +=val;
         idx += (idx & -idx);
     } } int x[111111];
 int main() {     for ( ;
 scanf("%d%d",&N,&K) == 2 ;
 ) {         memset(_0,0,sizeof(_0));
         memset(minus,0,sizeof(minus));
         memset(x,0,sizeof(x));
         for ( int i = 1;
  i <= N ;
 i++ ) {             scanf("%d",&x[i]);
             if ( x[i] < 0 ) update(minus,i,1);
             else if ( x[i] == 0 ) update(_0,i,1);
         }         for ( int i = 0 ;
 i < K ;
 i++ ) {             char tmp[111];
int a,b;
             scanf("%s %d %d",tmp,&a,&b);
             if ( tmp[0] == 'C' ) {                 if ( x[a] < 0 ) update(minus,a,-1);
                 else if ( x[a] == 0 ) update(_0,a,-1);
                 x[a] = b;
                 if ( x[a] < 0 ) update(minus,a,1);
                 else if ( x[a] == 0 ) update(_0,a,1);
             }             else {                 int zeroCount=read(_0,b) - read(_0,a-1);
                 int minusCount = read(minus,b)-read(minus,a-1);
                 printf("%c",zeroCount?'0':minusCount%2==1?'-':'+');
             }         }         puts("");
     }     return 0;
 } ]]>
```
Comment


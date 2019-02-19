---
title: Asia Daejeon Regional Contests 2011, no.D Equipment
date: 2013-10-03 23:24:00
category: Editorials
---

* [Live Archive 5842 Equipment](https://icpcarchive.ecs.baylor.edu/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3853)

## 문제요약

$N$개의 장비가 있고, 이중에 $K$개의 장비를 선택할 수 있다. 각 장비는 5개의 능력치가 있는데 선택한 장비들의 각 능력치들 중 최대값들의 합의 최대를 구하는 문제이다$(1\leq{}N\leq{}10,000,1\leq{}K\leq{}N,0\leq{}r_i\leq{}10,000)$. 

## 해법

$K$가 5이상일 경우에는 각 능력치들의 최대값을 선택하면 되므로, $K$가 5보다 작을 경우에만 처리하면 된다. 그런데 $K$가 5보다 작은 경우 $N$이 $(1<<5)-1$만큼으로 줄어드므로 모든 경우를 다 보는 백트랙킹으로 해결할 수 있다. 


```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int d[1<<5],N,K;
int go(int len,int mask) {
    if ( len == K ) return 0;
    int ret = 0;
    for ( int i = 0 ; i < (1<<5) ;i++ ) {
        if ( i & mask ) continue;
        ret = max(ret,go(len+1,mask | i)+d[i]);
    }
    return ret;
}
int main() {
    int tc;
    scanf("%d",&tc);
    while ( tc-- ) {
        memset(d,0,sizeof(d));
        scanf("%d%d",&N,&K);
        if ( K < 5 ) {
            for ( int i = 0 ; i < N ; i++ ) {
                int a[5];
                for ( int j = 0 ; j < 5; j++ ) 
                    scanf("%d",&a[j]);
                for ( int j = 0 ; j < (1<<5) ; j++ ) {
                    int t=0;
                    for ( int k = 0 ; k < 5; k++ ) 
                        if ( j&(1<<k) ) 
                            t += a[k];
                    d[j] = max(d[j],t);
                }
            }
            printf("%d\n",go(0,0));
        }
        else {
            int mx[5]={};
            for ( int i = 0 ; i < N ; i++ ) {
                int a[5];
                for ( int j = 0 ; j < 5; j++ ) {
                    scanf("%d",&a[j]);
                    mx[j] = max(mx[j],a[j]);
                }
            }
            int sum=0;
            for ( int i = 0 ; i < 5 ; i++ ) 
                sum += mx[i];
            printf("%d\n",sum);
        }
    }
    return 0;
}
```

>노코멘트


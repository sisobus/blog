---
title: Northwestern European Regional Contest 2012, no.J Joint Venture
date: 2013-10-05 18:29:00
category: Editorials
---

* [BOJ 3649 로봇 프로젝트](http://acmicpc.net/problem/3649)

## 문제요약

$X$ 길이의 구멍을 $N$개의 블럭 중 두개를 선택해서 막는 문제이다$(1\leq{}N\leq{}1,000,000)$. 구멍과의 길이가 정확히 일치해야 한다. 

## 해법

결국 두 수의 합이 $X$와 같은 두 수를 찾는 문제이다. $N$이 $1,000,000$이기 때문에 이분 탐색으로 찾으면 된다. 


```cpp
#include <cstdio>
#include <algorithm>
#include <vector>
using namespace std;
int block[11111111];
bool go(int x,int n,int& l1,int& l2) {
    int s = 10000000*x;
    for ( int i = 0  ; i < n && block[i] < s ; i++ ) {
        l1 = block[i],l2 = s-l1;
        if ( binary_search(block+i+1,block+n,l2)  ) return true;
    }
    return false;
}
int main() {
    for ( int x ; scanf("%d",&x) ==1; ) {
        int n;
        scanf("%d",&n);
        for ( int i = 0 ; i < n ; i++ )
            scanf("%d",&block[i]);
        sort(block,block+n);
        int l1,l2;
        if ( go(x,n,l1,l2) ) printf("yes %d %d\n",l1,l2);
        else printf("danger\n");
    }
    return 0;
}
```

> 유사한 문제로 아래와 같은 문제가 있다. 
> * [BOJ 3273 두 수의 합](http://acmicpc.net/problem/3273)


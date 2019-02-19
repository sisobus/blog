---
title: South America Regional Contests 2008, no.K Shrinking Polygons
date: 2013-09-26 12:53:00
category: Editorials
---

* [BOJ 5729 수축하는 다각형](http://acmicpc.net/problem/5729)

## 문제요약

다각형의 모든 꼭지점이 한 원 위에 존재하는 것을 내접하는 다각형이라 한다. $N$개의 꼭지점을 최소로 제거하여 정다각형을 만드는 것이 문제이다$(3\leq{}N\leq{}10^4)$. 아래 그림은 $N$이 10일 때 예시이다. 꼭지점 $X_i$은 하나의 꼭지점을 기준으로의 호의 길이로 주어진다$(1\leq{}X_i\leq{}10^3)$. 


## 해법

$X_i * N$ 의 최대값이 $10^4 * 10^3 = 10^7$이므로 원 위의 모든 정수점에 대하여 꼭지점이 있는지 없는지를 배열에 저장할 수 있다. 따라서 점을 지워나가면서 현재 있는 점들이 모두 같은 거리에 존재하는지를 확인하면 해결할 수 있다.


```cpp
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
bool d[11111111];
int main() {
    for ( int n ; scanf("%d",&n) && n ; ) {
        bool fail=true;
        memset(d,false,sizeof(d));
        vector<int> v;
        for ( int i = 0 ; i < n ; i++ ) {
            int t;
            scanf("%d",&t);
            v.push_back(t);
        }
        int sum =0;
        d[0]=true;
        for ( int i = 0 ; i < n ; i++ )
            d[sum+=v[i]] = true;
        for ( int i = n ; i >= 3 ; i-- ) {
            if ( !(sum%i) ) {
                int now=sum/i;
                for ( int j = 0 ; j < now ; j++ ) {
                    bool ok=true;
                    for ( int k = j ; k < sum && ok ; k+=now )
                        ok &= d[k];
                    if ( ok ) {
                        printf("%d\n",n-i);
                        fail = false;
                        goto hell;
                    }
                }
            }
        }
hell:;
        if ( fail ) printf("-1\n");
    }
    return 0;
}
```

> 되...되나? 했는데 됐다.


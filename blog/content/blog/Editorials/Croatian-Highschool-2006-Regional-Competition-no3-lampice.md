---
title: Croatian Highschool 2006, Regional Competition no.3 lampice
date: 2013-09-24 15:37:00
category: Editorials
---

* [BOJ 3159 램프](http://acmicpc.net/problem/3159)

## 문제요약

$2*N$ 크기의 전구 $(1\leq{}N\leq{}10,000)$ 가 놓여있다. 전구는 행이나 열이 연속된 전구들만 끄거나 켤 수 있고(켜진건 꺼지고 꺼진건 켜짐) 전구가 모두 꺼져있는 상태일 때, 입력받은 전구 패턴을 만들 수 있는 최소 껏다킴 횟수를 구하는 문제이다.

## 해법

column기준으로 전구에 작업을 했을 때, row로 연속된 전구의 부분이 많아지도록 만들어 주면 된다. 그렇게 되는 기준은 현재 column $i$라 한다면 $i-1$의 전구와 $i+1$의 전구가 같고, 좌우의 전구와 $i$의 전구는 달라야 한다. 이 기준이 위 행과 아래 행이 동시에 적용된다면 row로 연속된 전구의 부분이 많아지게 되므로 답이 된다.


```cpp
#include <cstdio>
#include <cstring>
char a[11111],b[11111];
int main() {
    int n;
    scanf("%d",&n);
    memset(a,'0',sizeof(a));
    memset(b,'0',sizeof(b));
    scanf("%s %s",a+1,b+1);
    bool A=false,B=false;
    int ans=0;
    for ( int i = 1 ; i <= n ; i++ ) {
        if ( ((a[i] != a[i-1] && a[i] != a[i+1] && a[i-1]==a[i+1]) &&
                (b[i] != b[i-1] && b[i] != b[i+1]&&b[i-1]==b[i+1]) ) ) {
            if ( a[i] == '0' ) a[i] = '1';
            else if ( a[i] == '1' ) a[i] = '0';
            if ( b[i] == '0' ) b[i] = '1';
            else if ( b[i] == '1' ) b[i] = '0';
            ans++;
        }
        if ( a[i-1]=='0' && a[i]=='1' ) ans++;
        if ( b[i-1]=='0' && b[i]=='1' ) ans++;
    }
    printf("%d\n",ans);
    return 0;
}
```

> 설마 될까 했는데 됬다.

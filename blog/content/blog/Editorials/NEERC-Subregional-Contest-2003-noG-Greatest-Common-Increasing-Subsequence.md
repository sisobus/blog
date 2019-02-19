---
title: NEERC Subregional Contest 2003, no.G Greatest Common Increasing Subsequence
date: 2013-09-30 13:43:00
category: Editorials
---

* [BOJ 7476 최대 공통 증가 수열](http://acmicpc.net/problem/7476)

## 문제요약

정수로 이루어진 두 수열의 공통 증가 수열중에서 가장 긴 것의 길이를 구하는 문제이다$(1\leq{}N\leq{}500)$. 


## 해법

$LCS$처럼 $O(N^2)$ $DP$를 돌리면 된다. 그 부분 수열도 구해야 하므로, 역추적을 위한 배열에다가 현재 상태가 어디서부터 왔는지를 체크해 주어야 한다. 


```cpp
#include <cstdio>
#include <cstring>
#include <string>
#include <algorithm>
#include <map>
#include <set>
#include <vector>
#include <stack>
#include <queue>
#include <sstream>
#include <cmath>
using namespace std;
int a[555],b[555];
int d[555][555];
int back[555][555];
int bi,bj;
int n,m;
int main() {
  
    scanf("%d",&n);
    for ( int i = 1 ; i <= n ; i++ )
        scanf("%d",&a[i]);
    scanf("%d",&m);
    for ( int i = 1 ; i<=  m ; i++ )
        scanf("%d",&b[i]);
    memset(d,0,sizeof(d));
    int ans=0;
    for ( int i = 1 ; i<= n ; i++ )
        for ( int j = 1 ; j<= m ; j++ ) {
            int tmp=0,tmpi=0;
            if ( a[i]==b[j] ) {
                for ( int k = 1 ; k < j ; k++ )
                    if ( b[k] < b[j]&&d[i][k] > tmp )
                        tmp = d[i][k],tmpi=k;
                if ( tmp>d[i][j]-1 )
                    d[i][j]=tmp+1,back[i][j]=tmpi;
            }
            else d[i][j]=d[i-1][j];
            if ( d[i][j] > ans )
                ans=d[i][j],bi=i,bj=j;
        }
    ans=0;
    for ( int i = 1 ; i <= m ; i++ )
        ans = max(ans,d[n][i]);
    printf("%d\n",ans);
    stack<int> st;
    for ( int i = ans ; i >= 1 ; i-- ) {
        st.push(b[bj]);
        bj = back[bi][bj];
        bi--;
        while ( bi && a[bi] != b[bj] ) bi--;
    }
    while ( !st.empty() ) {
        int now=st.top();st.pop();
        printf("%d ",now);
    }
    puts("");
    return 0;
}
```

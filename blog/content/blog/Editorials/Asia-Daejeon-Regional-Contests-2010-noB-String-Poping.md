---
title: Asia Daejeon Regional Contests 2010, no.B String Poping
date: 2013-10-07 19:54:00
category: Editorials
---

* [BOJ 8913 문자열 뽑기](http://acmicpc.net/problem/8913)

## 문제요약

$a$와 $b$로만 이루어진 문자열이 주어졌을 때, 연속된 같은 문자열의 최소 길이가 2만 되면 문자열에서 삭제할 수 있다. 이러한 작업을 문자열이 사라지거나, 더이상 문자열을 분해할 수 없을 때까지 지속한다고 했을 때, 이 문자열은 전부 뽑아질 수 있을지, 아니면 남아있을지 구하는 문제이다. 예를들어, $babbbaaabb$가 주어졌을 때, 뒤의 $bb$를 삭제하고 $aaa$,$bbb$를 순서대로 삭제하면 결국 $ba$가 남으므로 분해할 수 없는 경우이다. 하지만 $bbb$, $aaaa$, $bbb$를 삭제하는 경우 모든 문자가 뽑아질 수 있다$(1\leq{}N\leq{}25)$. 

## 해법

삭제할 수 있는 모든 경우를 $DFS$돌면서 지워보면 된다. 한 번이라도 다 지워지는 경우에는 지워지는 경우이다.


```cpp
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <vector>
#include <stack>
#include <queue>
#include <map>
#include <set>
#include <string>
#include <algorithm>
#include <climits>
using namespace std;
bool ok;
void go(string now) {
    if ( ok ) return;
    if ( now.empty() ) {
        ok = true;
        return ;
    }
    for ( int i = 0,j ; i < now.length() && !ok ; i++ ) {
        for ( j = i ; j < now.length() && now[i] == now[j]  ; j++ );
        if ( j-i >= 2 )
            go(now.substr(0,i)+now.substr(j));
        i = j-1;
    }
}
int main() {
    int tc;
    scanf("%d",&tc);
    while ( tc-- ) {
        char in[1111];
        scanf("%s",in);
        string s = in;
        ok = false;
        go(s);
        printf("%d\n",ok);
    }
    return 0;
}
```

>집에 갈래!~#!#

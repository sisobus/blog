---
title: Northwestern European Regional Contest 2010, no.C High Score
date: 2013-10-05 20:33:00
category: Editorials
---

* [BOJ 3663 고득점](http://acmicpc.net/problem/3663)

## 문제요약

입력받은 이름을 조이스틱으로 선택할 때 최소 조이스틱 움직인 횟수를 구하는 문제이다. 

## 해법

그리디 하게 해결하면 된다. 이 때, A에서 Z로 가는 경우랑 Z에서 A로 가는 경우를 모듈러 연산을 이용해서 잘처리해주면 쉽게 풀 수 있다. 


```cpp
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
using namespace std;
#define charSize 26
int main() {
    int tc;
    scanf("%d",&tc);
    while ( tc-- ) {
        char in[1111];
        scanf("%s",in);
        string s = in;
  
        int ans=0;
        for ( int i = 0 ; i < s.size() ; i++ )
            ans += min((s[i]-'A'+charSize)%charSize,('A'-s[i]+charSize)%charSize);
        int ans2=(int)s.size();
        for ( int i = 0 ; i < 2 ; i++ ) {
            for ( int j = 0,k ; j < s.size() ; j++ ) {
                for ( k = j+1 ; k != j && s[k]=='A' ; k=(k+1)%((int)s.size()));
                ans2 = min(ans2,j+((int)s.size()+j-k)%((int)s.size()));
            }
            reverse(s.begin()+1,s.end());
        }
        printf("%d\n",ans+ans2);
    }
    return 0;
}
```


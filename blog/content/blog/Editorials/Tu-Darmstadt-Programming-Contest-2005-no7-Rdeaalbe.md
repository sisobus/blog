---
title: Tu-Darmstadt Programming Contest 2005, no.7 Rdeaalbe
date: 2014-01-04 17:01:00
category: Editorials
---

* [BOJ 1501 영어 읽기](http://acmicpc.net/problem/1501)

## 문제요약

첫 문자와 끝 문자가 일치한다면 그 가운데의 문자들의 순열과는 상관없이 그것에 해당하는 단어로 인식할 수 있다. 예를들어 abcda같은 경우에는 abcda, abdca, acbda, acdba, adbca, adcba로 읽을 수 있다. 대신 이렇게 읽을 수 있으려면 사전에 존재하는 단어여야 한다. 사전에 $N$개의 단어가 존재하고, 길이가 $10,000$보다 길지 않은 $M$개의 문장이 주어질 때, 그 문장을 해석할 수 있는 경우의 수를 구하는 문제이다. 

## 해법

한 문장에서 각 단어는 독립적인 경우의 수를 가질 수 있다. 또, 순서가 중요하지 않으므로 결국 첫 문자와 끝문자를 제외한 나머지 문자들의 개수만 일치한다면 같은 단어로 인식할 수 있다. 따라서 맨 처음에 사전의 단어를 저장할 때, 문자의 순열과 관계없이 같은 단어로 인식할 수 있다면 같은 단어로 치고 개수를 세준다. 그 후 문장을 탐색하면서 각 단어에서 셀 수 있는 경우의 수를 곱해주면 답이된다. 


```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <string>
#include <map>
#include <algorithm>
using namespace std;
typedef long long ll;
typedef pair<int,int> ii;
int N,M;
map<string,int> mp;
int main() {
    scanf("%d\n",&N);
    char t[111];
    for ( int i = 0 ; i < N ; i++ ) {
        scanf("%s",t);
        string now = t;
        if ( now.length() > 2 )
            sort(now.begin()+1,now.end()-1);
        mp[now]++;
    }
    scanf("%d\n",&M);
    for ( int i = 0 ; i < M ; i++ ) {
        ll ans=1;
        char s[11111];
        gets(s);
        for ( char* p=strtok(s," ") ; p ; p=strtok(NULL," ") ) {
            string now = p;
            if ( now.length() > 2 )
                sort(now.begin()+1,now.end()-1);
            ans *= mp[now];
        }
        printf("%lld\n",ans);
    }
    return 0;
}
```
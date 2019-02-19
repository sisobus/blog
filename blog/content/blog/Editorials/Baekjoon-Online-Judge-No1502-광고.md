---
title: Baekjoon Online Judge No.1502 광고
date: 2014-01-04 17:27:00
category: Editorials
---

* [BOJ 1502 광고](http://acmicpc.net/problem/1502)

## 문제요약

최대 $L$만큼의 길이를 가진 광고를 보여주는 전광판에 광고를 넣는다$(1\leq{}L\leq{}1,000,000)$. 이 때, 광고판을 효율적으로 이용하기 위하여 $L$ - 광고의 길이만큼의 같은 광고를 맨처음부터 붙여서 보여준다. 이 광고판은 1초가 지날 때마다 왼쪽으로 한칸씩 이동한다. 광고판에 보여지는 광고가 주어졌을 때, 광고의 길이 중 가장 짧은 길이를 구하는 문제이다. 

## 해법

$KMP$ Algorithm 의 실패 함수는 문자열에서 접미사와 일치하는 최장 접두사의 위치를 나타낸다. 따라서 실패함수를 구한 다음, 총 길이에서 실패함수 F[총 길이-1]-1 만큼 빼준 값이 답이 된다.


```cpp
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <string>
#include <vector>
#include <algorithm>
using namespace std;
namespace kmp{
    typedef string seq_t;
    void calculate_pi(vector<int>& pi, const seq_t& str) {
        pi[0] = -1;
        int j = -1;
        for (int i = 1 ; i < str.length() ; i++) {
            while (j >= 0 && str[i] != str[j + 1]) j = pi[j];
            if (str[i] == str[j + 1])
                pi[i] = ++j;
            else
                pi[i] = -1;
        }
    }
    /* returns all positions matched */
    vector<int> match(seq_t text, seq_t pattern) {
        vector<int> pi(pattern.size());
        vector<int> ans;
        if (pattern.size() == 0) return ans;
        calculate_pi(pi, pattern);
        int j = -1;
        for (int i = 0 ; i < text.size() ; i++) {
            while (j >= 0 && text[i] != pattern[j + 1]) j = pi[j];
            if (text[i] == pattern[j + 1]) {
                j++;
                if (j + 1 == pattern.size()) {
                    ans.push_back(i - j);
                    j = pi[j];
                }
            }
        }
        return ans;
    }
}
char s[1111111];
int main() {
    int L;
    scanf("%d",&L);
    vector<int> pi;
    pi.resize(L+1);
    scanf("%s",s);
    kmp::calculate_pi(pi,string(s));
    printf("%d\n",L-pi[L-1]-1);
    return 0;
}
```
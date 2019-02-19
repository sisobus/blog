---
title: Baekjoon Online Judge no.1168 Josephus Problem 2
date: 2013-09-25 12:42:00
category: Editorials
---

* [BOJ 1168 조세퍼스 문제 2](http://acmicpc.net/problem/1168)

## 문제요약

$N$명의 사람이 빙 둘러서 서 있고 양의 정수 $M$이 주어질 때$(1\leq{}N\leq{}100,000,M\leq{}N)$, 순서대로 매 $M$번 째 사람을 제거한다. 이 때, 죽은 순서를 조세퍼스 순열이라고 하는데, 이를 출력하는 문제이다. 


## 해법

$(5,000\leq{}N)$ 정도의 문제라면 Queue에 넣고 빼는식으로 해결할 수 있지만, 주어진 $N$제한이 100,000이므로 $O(N log N)$정도의 시간으로 해결해야 한다. Fenwick Tree로 해결할 수 있다. 트리를 구성하는데 $O(N log N)$이 걸리고, $N$명의 사람들을 차례로 보면서 죽여아 하는 사람을 바이너리 서치로 찾으면 된다. 이 때도 $O(N log N)$가 걸린다.

 


```cpp
#include <cstdio>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;
namespace FenwickTree {
    typedef long long ll;
    int MAX;
    vector<ll> tree;
    void Init(int size) {
        MAX=size;
        tree.resize(MAX+1);
    }
    ll Read(int idx) {
        ll ret=0;
        while ( idx > 0 ) {
            ret += tree[idx];
            idx -= (idx & -idx);
        }
        return ret;
    }
    void Update(int idx,int val) {
        while ( idx <= MAX ) {
            tree[idx] += val;
            idx += (idx & -idx);
        }
    }
    int Find(int now) {
        int l=1,r=MAX;
        int ret=1;
        while ( l <= r ) {
            int mid=(l+r)>>1;
            if ( Read(mid) >= now ) ret = mid,r=mid-1;
            else l=mid+1;
        }
        return ret;
    }
}
int main() {
    int N,M;
    scanf("%d%d",&N,&M);
    FenwickTree::Init(N);
    for ( int i = 1 ; i <= N ; i++ )
        FenwickTree::Update(i,1);
    int now=M;
    for ( int i = N ; i > 0 ; i-- ) {
        now=now%i?now%i:i;
        int ans = FenwickTree::Find(now);
        if ( i == N ) printf("<");
        printf("%d",ans);
        if ( i == 1 ) printf(">\n");
        else printf(", ");
        now+=(M-1);
        FenwickTree::Update(ans,-1);
    }
    return 0;
}
```
Comment


---
title: Benelux Algorithm Programming Contest 2005, no.E North-Western Winds
date: 2013-09-26 15:27:00
category: Editorials
---

* [BOJ 5419 북서풍](http://acmicpc.net/problem/5419)

## 문제요약

$Y$ 좌표가 증가하는 쪽이 북쪽이고, $X$좌표가 증가하는 쪽이 동쪽이라고 했을 때$(-10^9\leq{}X,Y\leq{}10^9)$, 북서풍이 불기 때문에 배가 북쪽이나 서쪽으로 운항할 수 없다. 이 때, 북서풍을 타고 이동할 수 있는 섬의 쌍을 구하는 문제이다. 총 $N$개의 섬이 존재한다 $(1\leq{}N\leq{}75,000)$.

## 해법

$N$이 굉장히 크기 때문에, 모든 쌍을 다 보는 $O(N^2)$의 솔루션은 시간초과가 난다. 따라서 이 문제를 잘 바꿔보면, 어느 한 점을 기준으로 사분면을 나눌 때, 2사분면에 있는 점들의 개수를 세면 된다. Fenwick Tree같은 자료구조를 이용하여 세면 $O(N log N)$만에 해결할 수 있다. 


```cpp
#include <cstdio>
#include <cstring>
#include <vector>
#include <algorithm>
#include <map>
#include <set>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> ii;
bool cmp(const ii& e1,const ii& e2) {
    if ( e1.first != e2.first ) return e1.first > e2.first;
    else return e1.second < e2.second;
}
ll tree[777778];
int n;
ll read(int idx) {
    ll sum =0;
    while ( idx > 0 ) {
        sum += tree[idx];
        idx -= (idx&-idx);
    }
    return sum;
}
void update(int idx,int y) {
    while ( idx <= n ) {
        tree[idx]+=y;
        idx += (idx&-idx);
    }
}
int main(){
    int tc;
    scanf("%d",&tc);
    while ( tc-- ) {
        memset(tree,0,sizeof(tree));
        scanf("%d",&n);
        vector<ii> v(n);
        set<ll> s;
        for ( int i = 0 ; i < n ; i++ ) {
            scanf("%lld %lld",&v[i].first,&v[i].second);
            s.insert(v[i].second);
        }
        sort(v.begin(),v.end(),cmp);
        int t[777778]={};
        int size = (int)s.size();
        for ( int i = 0 ; i < size ; i++ ) {
            t[i] = *s.begin();
            s.erase(t[i]);
        }
        ll ans=0;
        for ( int i = 0 ; i < n ; i++ ) {
            int now=int(lower_bound(t,t+size,v[i].second)-t)+1;
            ans += read(now);
            update(now,1);
        }
        printf("%lld\n",ans);
    }
    return 0;
}
```

> 비슷한 문제로 [Annie and Tibber](https://algospot.com/judge/problem/read/ANNIETIBBER) 가 있다. 이 문제가 시계방향으로, 반시계방향으로 하는 트리 두 개를 써야 되는 문제라 조금 더 어렵다. 

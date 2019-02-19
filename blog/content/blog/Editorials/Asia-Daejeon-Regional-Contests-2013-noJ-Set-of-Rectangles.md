---
title: Asia Daejeon Regional Contests 2013, no.J Set of Rectangles
date: 2013-11-16 15:53:00
category: Editorials
---

* [BOJ 9464 직사각형 집합](http://acmicpc.net/problem/9464)

## 문제요약

피타고리안 트리플은 $a^2+b^2=c^2$를 만족하는 양의 정수 $(a,b,c)$ 집합을 말한다. 이 피타고리안 트리플 집합을 만들기 위하여 두 정수 $x,y$를 이용한다.
$a = 2*x*y$, $b = x^2-y^2$, $c = x^2+y^2$ 로 놓고 식을 잘 정리하면 피타고리안 트리플을 구할 수 있다. 

입력으로 주어지는 총 L길이의 철사를 가지고 직사각형들을 만들 예정이다$(14\leq{}L\leq{}1,000,000)$. 이 때, 만드는 직사각형의 집합은 반드시 피타고리안 트리플 집합이어야 한다. 직사각형 $R_i$를 만드는 데에 필요한 철사의 길이는 $2*(W_i+H_i)$이다. 이 때의 $W_i$와 $H_i$가 피타고리안 집합이면 된다. 꼭 철사를 다 이용할 필요없이, 만들 수 있는 최대의 직사각형 집합의 개수를 구하는 문제이다.





## 해법

먼저 피타고리안 트리플 집합을 구하는 것이 중요하다. 문제의 조건에 해당하는 피타고리안 트리플 집합이 최대 몇 개인지 모르므로 분석하기 쉽지 않지만, 몇 번의 시뮬레이션을 해보면 그다지 많은 집합이 존재하지 않음을 알 수 있다. 왜냐하면 최대로 들어올 수 있는 입력 $L$이 $1,000,000$이고 하나의 $R_i$를 만드는 데 드는 총 길이가 $2*(W_i+H_I)$이므로 피타고리안 트리플 집합의 두 숫자를 더한 값이 $500,000$을 넘지 않는 집합만 구하면 되기 때문이다. 

그렇게 해서 피타고리안 트리플 집합을 구했다면 $Greedy$하게 $W_i+H_i$가 작은 집합부터 사용해서 직사각형을 만들면 최대 직사각형을 구할 수 있다. 

```cpp
#include <cstdio>
#include <cstring>
#include <vector>
#include <algorithm>
#include <set>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> llll;
ll square(ll now) {
    return now*now;
}
ll gcd(ll a,ll b) {
    return b?gcd(b,a%b):a;
}
bool cmp(const llll& e1,const llll& e2) {
    return e1.first+e1.second < e2.first+e2.second;
}
int main() {
    set<llll> s;
    for ( ll y = 1 ; y < 500 ; y++ )
        for ( ll x = y+1 ; x < 500 ; x++ ) {
            ll a = 2*x*y,b = x*x-y*y,c = x*x+y*y;
            if ( square(a)+square(b) == square(c) ) {
                vector<ll> v;
                v.push_back(a);v.push_back(b);v.push_back(c);
                sort(v.begin(),v.end());
                ll now = gcd(v[0],gcd(v[1],v[2]));
                for ( int i = 0 ; i < 3; i ++ )
                    v[i] /= now;
                s.insert(llll(v[0],v[1]));
            }
        }
    vector<llll> v;
    for ( set<llll>::iterator it = s.begin();it!=s.end() ; it++ )
        v.push_back(llll(it->first,it->second));
    sort(v.begin(),v.end(),cmp);
    int tc;
    scanf("%d",&tc);
    while ( tc-- ) {
        ll L,ans=0;
        scanf("%lld",&L);
        for ( int i = 0 ; i < v.size() ; i++ ) {
            ll now = 2*(v[i].first+v[i].second);
            if ( L >= now ) {
                L -= now;
                ans++;
            }
        }
        printf("%lld\n",ans);
    }
    return 0;
}
```
> 맨 처음에는 냅색문제인 줄 알았는데 철사를 다 쓰지 않아도 됨을 알고 그리디하게 접근했다.

---
title: Northwestern European Regional Contest > Nordic Collegiate Programming Contest 2012, no.K Kindergarten
date: 2013-10-24 12:25:00
category: Editorials
---

* [BOJ 5009 유치원](http://acmicpc.net/problem/5009)

## 문제요약

총 $3$명의 선생님이 근무하는 유치원에 $N$명의 유치원생이 있다$(1\leq{}N\leq{}200)$. 모든 학생들은 같은 반이 되고 싶어하는 순서를 적어서 냈고, 반을 배정할 때 작년에 맡았던 선생님의 반으로 배정하면 안된다. 따라서 모든 학생을 작년과 선생님이 다른 반에 배정하면서 각 반에 있는 모든 학생이 원하는 순서에서 모두 상위 $T$위 안에 드는 방법을 찾으려고 한다. 이 때, $T$는 가능한 작아야 한다. 

## 해법

$2-SAT$문제이다. 같은 반에 존재하면 안되는 쌍이 다른 반에 존재하고, 작년과 다른 반에 배정할 수 있으면 가능한 방법이다. 따라서 $BinarySearch$로 $T$를 먼저 정해놓고 가능한 답인지를 찾아가면 된다. 


```cpp
#include <cstdio>
#include <vector>
#include <cstring>
#include <algorithm>
using namespace std;
typedef long long ll;
typedef pair<int,int> ii;
vector<int> v[555];
vector<int> vv[555];
int n;
int a[555];
int back[555];
bool dfs(int now,int prev) {
    back[now] = prev;
    for ( int i =0 ;  i < vv[now].size() ; i++ ) {
        int next = vv[now][i];
        if ( back[next] == prev ) return false;
        if ( back[next] != -1 ) continue;
        if ( a[next] != prev && !dfs(next,3-prev-a[next]) )
            return false;
    }
    return true;
}
bool go(int now) {
    memset(back,-1,sizeof(back));
    for ( int i = 1; i <= n ; i++ )
        vv[i].clear();
    for ( int i = 1; i <= n ; i++ )
        for ( int j = now ; j < n-1 ; j++ ) {
            vv[i].push_back(v[i][j]);
            vv[v[i][j]].push_back(i);
        }
    for ( int i = 1 ; i <= n ; i++ )
        if ( back[i] == -1 ) {
            int tmp[555]={};
            memcpy(tmp,back,sizeof(back));
            if ( !dfs(i,(a[i]+1)%3) ) {
                memcpy(back,tmp,sizeof(back));
                if ( !dfs(i,(a[i]+2)%3) ) return false;
            }
        }
    return true;
}
int main() {
    scanf("%d",&n);
    for ( int i = 1 ; i <= n ; i++ ) {
        scanf("%d",&a[i]);
        for ( int j = 0 ; j < n-1; j++ ) {
            int t;
            scanf("%d",&t);
            v[i].push_back(t);
        }
    }
    int l = -1,r = n-1;
    while ( l < r-1 ) {
        int mid= (l+r)/2;
        if ( go(mid) ) r = mid;
        else l = mid;
    }
    printf("%d\n",r);
    return 0;
}
```
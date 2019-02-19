---
title: Northwestern European Regional Contest 2011, no.C Movie Collection
date: 2013-09-26 15:51:00
category: Editorials
---

문제요약

상근이가 가지고 있는 $N$개의 DVD에서 $M$번의 보고싶은 영화를 찾는다$(1\leq{}N,M\leq{}100,000)$. $i$ 번 째 DVD의 위에 몇 개의 DVD가 있는지 주어질 때, 보고싶은 영화의 위에 몇개의 DVD가 있었는지 구하는 문제이다. 꺼낸 DVD는 모든 DVD의 맨 위에 올려 놓는다.

상근이가 가지고 있는 $N$개의 DVD에서 $M$번의 보고싶은 영화를 찾는다$(1\leq{}N,M\leq{}100,000)$. $i$ 번 째 DVD의 위에 몇 개의 DVD가 있는지 주어질 때, 보고싶은 영화의 위에 몇개의 DVD가 있었는지 구하는 문제이다. 꺼낸 DVD는 모든 DVD의 맨 위에 올려 놓는다.

해법

$M$번의 쿼리가 있을 예정이고, 꺼낸 DVD를 매번 맨 위로 Update시켜 주는걸로 봐서 Fenwick Tree를 이용하는 문제임을 알 수 있다. 입력받는 정보로 트리를 구성한 다음, 매번 쿼리를 수행할 때, 찾고난 다음 업데이트를 시키면서 현재의 정보를 지워주고 현재의 DVD를 맨 위로 올리면서 다시한번 업데이트를 시켜준다. 

$M$번의 쿼리가 있을 예정이고, 꺼낸 DVD를 매번 맨 위로 Update시켜 주는걸로 봐서 Fenwick Tree를 이용하는 문제임을 알 수 있다. 입력받는 정보로 트리를 구성한 다음, 매번 쿼리를 수행할 때, 찾고난 다음 업데이트를 시키면서 현재의 정보를 지워주고 현재의 DVD를 맨 위로 올리면서 다시한번 업데이트를 시켜준다. 


```
<![CDATA[ #include <cstdio>#include <vector>#include <algorithm>#include <cstring>using namespace std;
 typedef long long ll;
 typedef pair<int,int> ii;
 ll tree[333333];
 int n,m;
 ll read(int idx) {     ll sum=0;
     while ( idx > 0 ) {         sum += tree[idx];
         idx -= (idx&-idx);
     }     return sum;
 } void update(int idx,int val) {     while ( idx <= 333332 ) {         tree[idx]+=val;
         idx += (idx&-idx);
     } } int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         memset(tree,0,sizeof(tree));
         scanf("%d %d",&n,&m);
         vector<int> v(2*n+1);
         for ( int i = 1 ;
 i <= n ;
 i++ ) {             update(i,1);
             v[i] = n-i+1;
         }         int mov=n+1;
         while ( m-- ) {             int t;
             scanf("%d",&t);
             printf("%lld ",read(333330)-read(v[t]));
             update(v[t],-1);
             v[t]=mov++;
             update(v[t],1);
         }         puts("");
     }     return 0;
 } ]]>
```
Comment


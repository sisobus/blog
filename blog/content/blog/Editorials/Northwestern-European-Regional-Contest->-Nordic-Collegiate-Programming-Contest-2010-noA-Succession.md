---
title: Northwestern European Regional Contest > Nordic Collegiate Programming Contest 2010, no.A Succession
date: 2013-10-24 10:46:00
category: Editorials
---

문제요약

유토피아의 법에는 왕위 계승자가 없을 경우, 나라를 세운 사람과 혈통이 가까운 사람이 왕위 계승을 하는 조항이 있다. 나라를 세운 사람과 혈통이 가까운 사람은 그의 자손이 아닌 사람과 피가 덜 섞인 사람이다. 모든 사람의 피는 아버지의 혈통과 어머니의 혈통의 절반을 받게 되고, 나라를 세운 사람의 자식은 $1/2$왕족이고 그 아들이 왕족이 아닌 사람과 결혼하면 $1/4$왕족이 된다. 이 때, 왕위 계승자를 찾는 것이 문제이다. $N$개의 족보가 있고 $M$명의 왕위 계승을 하고자 하는 사람이 주어진다$(2\leq{}N,M\leq{}50)$.

유토피아의 법에는 왕위 계승자가 없을 경우, 나라를 세운 사람과 혈통이 가까운 사람이 왕위 계승을 하는 조항이 있다. 나라를 세운 사람과 혈통이 가까운 사람은 그의 자손이 아닌 사람과 피가 덜 섞인 사람이다. 모든 사람의 피는 아버지의 혈통과 어머니의 혈통의 절반을 받게 되고, 나라를 세운 사람의 자식은 $1/2$왕족이고 그 아들이 왕족이 아닌 사람과 결혼하면 $1/4$왕족이 된다. 이 때, 왕위 계승자를 찾는 것이 문제이다. $N$개의 족보가 있고 $M$명의 왕위 계승을 하고자 하는 사람이 주어진다$(2\leq{}N,M\leq{}50)$.





해법

왕이 가지고 있는 피를 $2^N$으로 생각하면 $double$연산을 안하고 코딩할 수 있다. 

왕이 가지고 있는 피를 $2^N$으로 생각하면 $double$연산을 안하고 코딩할 수 있다. 






```
<![CDATA[ #include <cstdio>#include <string>#include <vector>#include <map>using namespace std;
 typedef long long ll;
 const ll inf = 1ll<<55;
 int N,M;
 map<string,ll> mp;
 char t[111],t1[111],t2[111];
 int main() {     scanf("%d%d",&N,&M);
     scanf("%s",t);
     string s=t;
     mp[s] = inf;
     vector<string> v[3];
     for ( int i = 0 ;
 i < N ;
 i++ ) {         scanf("%s%s%s",t,t1,t2);
         v[0].push_back(t);
         v[1].push_back(t1);
         v[2].push_back(t2);
     }     for ( int i = 0 ;
 i < N ;
 i++ )         for ( int j = 0 ;
 j < N ;
 j++ )             mp[v[0][j]] = (mp[v[1][j]]+mp[v[2][j]])/2;
     ll max=0;
     string ans="";
     for ( int i = 0 ;
 i < M ;
 i++ ) {         scanf("%s",t);
         string now = t;
         if ( mp[now] > max ) {             max = mp[now];
             ans = now;
         }     }     printf("%s\n",ans.c_str());
     return 0;
 } ]]>
```
Comment


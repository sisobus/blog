---
title: Northwestern European Regional Contest 2008, no.C Cat vs. Dog
date: 2013-10-24 10:35:00
category: Editorials
---

문제요약

고양이와 개 프로그램이 있는데, 시청자는 다음 라운드에 진출시킬 동물과 이번 라운드에서 탈락시킬 동물을 한마리씩 선택할 수 있다. 모든 시청자는 개를 좋아하거나 고양이를 좋아한다. 자신이 투표한 결과가 이루어졌을 때, 시청자는 프로그램을 계속 보고 반영되지 않았다면 더이상 프로그램을 보지 않는다. 이 때, 다음 라운드를 시청할 시청자 수의 최대 값을 구하는 문제이다.

고양이와 개 프로그램이 있는데, 시청자는 다음 라운드에 진출시킬 동물과 이번 라운드에서 탈락시킬 동물을 한마리씩 선택할 수 있다. 모든 시청자는 개를 좋아하거나 고양이를 좋아한다. 자신이 투표한 결과가 이루어졌을 때, 시청자는 프로그램을 계속 보고 반영되지 않았다면 더이상 프로그램을 보지 않는다. 이 때, 다음 라운드를 시청할 시청자 수의 최대 값을 구하는 문제이다.





해법

고양이를 좋아하거나 개를 좋아한다는 상반된 전제 때문에, 시청자들을 둘로 나눌 수 있다(시청자를 정점으로 이분 그래프를 만들 수 있다). 개를 좋아하는 사람과 고양이를 좋아하는 사람 사이에는 결국 둘중 하나만 다음 라운드의 시청자가 될 수 있으므로 $Minimum$ $Vertex$ $Cover$문제와 같아진다. 따라서 전체 시청자 수에서 $Maximum$ $Flow$를 뺀 값이 해답이 된다. 

고양이를 좋아하거나 개를 좋아한다는 상반된 전제 때문에, 시청자들을 둘로 나눌 수 있다(시청자를 정점으로 이분 그래프를 만들 수 있다). 개를 좋아하는 사람과 고양이를 좋아하는 사람 사이에는 결국 둘중 하나만 다음 라운드의 시청자가 될 수 있으므로 $Minimum$ $Vertex$ $Cover$문제와 같아진다. 따라서 전체 시청자 수에서 $Maximum$ $Flow$를 뺀 값이 해답이 된다. 






```
<![CDATA[ #include <cstdio>#include <cstring>#include <vector>#include <algorithm>using namespace std;
 #define MAX_V 1000 vector<vector<int> > v;
 int backMatch[MAX_V*2+5];
 bool visited[MAX_V*2+5];
 bool dfs(int now) {     if ( visited[now] ) return false;
     visited[now] = true;
     for ( int i = 0 ;
 i < v[now].size() ;
 i++ ) {         int next = v[now][i];
         if ( backMatch[next] == -1 || dfs(backMatch[next]) ) {             backMatch[next] = now;
             return true;
         }     }     return false;
 } int BipartiteMatching() {     memset(backMatch,-1,sizeof(backMatch));
     int matched =0;
     for ( int i = 0 ;
 i < v.size() ;
 i++ ) {         memset(visited,false,sizeof(visited));
         if ( dfs(i) ) matched++;
     }     return matched;
 } int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         v.clear();
         int c,d,V;
         scanf("%d%d%d",&c,&d,&V);
         v.resize(V*2);
         char s[2][555][11]={};
         for ( int i = 0 ;
 i < V;
 i++ )             for ( int j = 0 ;
 j < 2 ;
 j++ )                 scanf("%s",s[j][i]);
         for ( int i = 0 ;
 i < V;
 i++ )             for ( int j = 0 ;
 j < V;
 j++ )                 if ( !strcmp(s[0][i],s[1][j]) || !strcmp(s[1][i],s[0][j]) )                     v[i].push_back(j);
         printf("%d\n",V-BipartiteMatching()/2);
     }     return 0;
 } ]]>
```
Comment


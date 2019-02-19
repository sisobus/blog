---
title: Northeastern European Regional Contest 2012, no.A Addictive Bubbles
date: 2013-10-24 11:49:00
category: Editorials
---

문제요약

플레이어는 선택한 구슬과 인접한 같은 색상의 구슬을 제거할 수 있다. 제거한 구슬의 빈자리는 아래와 같은 규칙으로 채워지고 제거한 구슬의 개수의 제곱승만큼의 점수를 얻는다. 구슬의 개수와 게임판의 크기가 주어졌을 때, 최고 점수를 받을 수 있는 설계도를 출력하는 문제이다. 

플레이어는 선택한 구슬과 인접한 같은 색상의 구슬을 제거할 수 있다. 제거한 구슬의 빈자리는 아래와 같은 규칙으로 채워지고 제거한 구슬의 개수의 제곱승만큼의 점수를 얻는다. 구슬의 개수와 게임판의 크기가 주어졌을 때, 최고 점수를 받을 수 있는 설계도를 출력하는 문제이다. 









해법

알고보면 굉장히 쉬운 문제이다. 같은 색의 구슬을 위쪽부터 채우고, 너비가 다 채워지지 않았다면 꼬리를 물고 아래칸에 채워주면 된다. 아래 그림같이. 

알고보면 굉장히 쉬운 문제이다. 같은 색의 구슬을 위쪽부터 채우고, 너비가 다 채워지지 않았다면 꼬리를 물고 아래칸에 채워주면 된다. 아래 그림같이. 






```
<![CDATA[ #include <cstdio>#include <cstring>#include <algorithm>#include <vector>using namespace std;
 int h,w,c;
 char ans[11][11];
 int main() {     scanf("%d%d%d",&h,&w,&c);
     vector<int> v;
     for ( int i = 0 ;
 i < c ;
 i++ ) {         int t;
         scanf("%d",&t);
         for ( int j = 0 ;
 j < t ;
 j++ )             v.push_back(i+1);
     }     int dir=1,mov=0;
     for ( int i = 0 ;
 i < h ;
 i++,dir*=-1 )         for ( int j = dir==1?0:(w-1) ;
 dir==1?(j<w):(j>=0) ;
 j+=dir )             ans[i][j] = v[mov++]+'0';
     for ( int i = 0 ;
 i < h ;
 i++ )         printf("%s\n",ans[i]);
           return 0;
 } ]]>
```
Comment


---
title: German Collegiate Programming Contest 2013, no.B Booking
date: 2013-09-26 13:09:00
category: Editorials
---

문제요약

호텔의 입실시간과 퇴실시간이 있는 $B$개의 예약정보와 방을 정리하는데 걸리는 시간 $C$가 주어졌을 때 $(1\leq{}B\leq{}5,000, 0\leq{}C\leq{}360)$, 방을 배정하는 최소의 방 개수를 구하는 문제이다. 예약날짜는 2013년부터 2016년까지 존재한다. 

호텔의 입실시간과 퇴실시간이 있는 $B$개의 예약정보와 방을 정리하는데 걸리는 시간 $C$가 주어졌을 때 $(1\leq{}B\leq{}5,000, 0\leq{}C\leq{}360)$, 방을 배정하는 최소의 방 개수를 구하는 문제이다. 예약날짜는 2013년부터 2016년까지 존재한다. 

해법

단순한 스케쥴링 문제인데, 입력이 해괴하게 들어온다. 따라서 이 요상망측한 입력데이터를 전부 분으로 바꿔서 해결하는 것이 편하다. 따라서 입력을 분으로 바꾼 뒤, $LineSweeping$으로 괄호 열닫는 문제 풀듯이 해결하면 된다. 

단순한 스케쥴링 문제인데, 입력이 해괴하게 들어온다. 따라서 이 요상망측한 입력데이터를 전부 분으로 바꿔서 해결하는 것이 편하다. 따라서 입력을 분으로 바꾼 뒤, $LineSweeping$으로 괄호 열닫는 문제 풀듯이 해결하면 된다. 


```
<![CDATA[ #include <cstdio>#include <vector>#include <map>#include <algorithm>using namespace std;
 int B,C;
 int calendar[2222][13]={     {0,31,28,31,30,31,30,31,31,30,31,30,31},     {0,31,28,31,30,31,30,31,31,30,31,30,31},     {0,31,28,31,30,31,30,31,31,30,31,30,31},     {0,31,29,31,30,31,30,31,31,30,31,30,31} };
 typedef long long ll;
 struct reserve{     ll time;
     bool b;
     reserve(){}     reserve(ll time,bool b):         time (time),b (b){} };
 ll makeMinute(int Y,int M,int D,int h,int m) {     ll now=(Y-2013)*365;
     ll day=now+D;
     for ( int j = 1 ;
 j < M ;
 j++ )         day+=calendar[Y-2013][j];
     ll minute=h*60+m;
     minute += day*24*60;
     return minute;
 } bool cmp(const reserve& e1,const reserve& e2) {     if ( e1.time != e2.time )         return e1.time<e2.time;
     else return e1.b<e2.b;
 } int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         vector<reserve> v;
         scanf("%d%d",&B,&C);
         for ( int i = 0 ;
 i < B;
 i++ ) {             scanf("%*s");
             int Y,M,D,h,m;
             scanf("%d-%d-%d",&Y,&M,&D);
             scanf("%d:%d",&h,&m);
             reserve a(makeMinute(Y,M,D,h,m),true);
             scanf("%d-%d-%d",&Y,&M,&D);
             scanf("%d:%d",&h,&m);
             reserve b(C+makeMinute(Y,M,D,h,m),false);
             v.push_back(a);
             v.push_back(b);
         }         sort(v.begin(),v.end(),cmp);
         ll ans=0,now=0;
         for ( int i = 0 ;
 i < v.size() ;
 i++ ) {             if ( v[i].b ) {                 now++;
                 ans = max(ans,now);
             }             else now--;
         }         printf("%lld\n",ans);
     }     return 0;
 } ]]>
```
Comment


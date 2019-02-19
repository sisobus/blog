---
title: Baekjoon Online Judge No.2261 The closest pair of points
date: 2013-12-12 23:00:00
category: Editorials
---

문제요약

2차원 평면상에 $N$개의 점이 주어졌을 때, 가장 가까운 두 점을 찾는 문제이다. 

2차원 평면상에 $N$개의 점이 주어졌을 때, 가장 가까운 두 점을 찾는 문제이다. 

$(1\leq{}N\leq{}100,000)$





해법

가장 쉽게 떠오르는 해법은 $N^2$의 모든 점들의 쌍의 최소거리를 찾으면 된다. 하지만 $O(N^2)$의 시간이 걸리며 시간초과가 난다. 따라서 $O(N log N)$으로 해결해야 한다. 가장 가까운 두 점을 구하는 방법은 여러가지가 있는데, 하나는 divide & conquer이고 다른 하나는 line sweeping이다.  

가장 쉽게 떠오르는 해법은 $N^2$의 모든 점들의 쌍의 최소거리를 찾으면 된다. 하지만 $O(N^2)$의 시간이 걸리며 시간초과가 난다. 따라서 $O(N log N)$으로 해결해야 한다. 가장 가까운 두 점을 구하는 방법은 여러가지가 있는데, 하나는 divide & conquer이고 다른 하나는 line sweeping이다.  





divide & conquer 로 해결하는 방법은 일단 $X$좌표 기준으로 반씩 쪼갠다음 합치면서 답을 구하는 방식이다. 이 때, 가장 큰 문제가 합칠 때 생기게 된다. 반으로 쪼갰을 때, 왼쪽에서의 답과 오른쪽에서의 답은 각각의 공간에서 답임이 자명하다. 하지만 반으로 쪼갰을 때, 왼쪽에 있는 점과 오른쪽에 있는 점이 가장 가까운 두 점이 된다면 문제가 된다. 이를 해결하는 방법이 중요한데, 아래 그림과 같이 왼쪽에서 구해진 답과 오른쪽에서 구해진 답의 최소값 사이의 점들중에 가장 가까운 두 점이 있다고 가정할 수 있다. 여기까지만 생각을 한다면 결국 그 사이의 점들을 모두 비교해야 하므로 똑같이 $O(N^2)$이 된다.

divide & conquer 로 해결하는 방법은 일단 $X$좌표 기준으로 반씩 쪼갠다음 합치면서 답을 구하는 방식이다. 이 때, 가장 큰 문제가 합칠 때 생기게 된다. 반으로 쪼갰을 때, 왼쪽에서의 답과 오른쪽에서의 답은 각각의 공간에서 답임이 자명하다. 하지만 반으로 쪼갰을 때, 왼쪽에 있는 점과 오른쪽에 있는 점이 가장 가까운 두 점이 된다면 문제가 된다. 이를 해결하는 방법이 중요한데, 아래 그림과 같이 왼쪽에서 구해진 답과 오른쪽에서 구해진 답의 최소값 사이의 점들중에 가장 가까운 두 점이 있다고 가정할 수 있다. 여기까지만 생각을 한다면 결국 그 사이의 점들을 모두 비교해야 하므로 똑같이 $O(N^2)$이 된다.

여기서 한가지 아이디어를 떠올리면 해결할 수 있다. 왼쪽 오른쪽에서 구한 정답을 d라고 하고, 그 사이에 있는 점들을 Sy라 하며 y좌표를 기준으로 정렬되어 있다면, 각 점으로부터 최대 16개의 점만을 비교한다면 모든 경우의 가까운 점들을 볼 수 있다(이유는 생략한다...). 따라서 16개의 점을 비교하는 작업을 상수시간으로 본다면 $O(N log N)$의 시간복잡도를 갖는다.

여기서 한가지 아이디어를 떠올리면 해결할 수 있다. 왼쪽 오른쪽에서 구한 정답을 d라고 하고, 그 사이에 있는 점들을 Sy라 하며 y좌표를 기준으로 정렬되어 있다면, 각 점으로부터 최대 16개의 점만을 비교한다면 모든 경우의 가까운 점들을 볼 수 있다(이유는 생략한다...). 따라서 16개의 점을 비교하는 작업을 상수시간으로 본다면 $O(N log N)$의 시간복잡도를 갖는다.


```
<![CDATA[ #include <cstdio>#include <cstdlib>#include <vector>#include <algorithm>#define inf 1000000000 #define sqr(x) ((x)*(x)) using namespace std;
 typedef pair < int,int > point;
 typedef pair <point,point> pp;
   bool cmp_y(const point&, const point &);
 inline int dist(const point &, const point &);
 pp go(int left, int right);
   point *p;
 int n, run2;
 pp ans;
   int main() { //#define FILENAME "rand_pts.txt" #ifdef FILENAME     freopen(FILENAME, "r", stdin);
 #endif     scanf("%d",&n);
     p = (point*)malloc(sizeof(point)*n);
     for ( int i = 0 ;
 i < n ;
 i++ )         scanf("%d%d",&p[i].first, &p[i].second);
           sort(p, p+n);
     pp x_ans = go(0, n-1);
     int x_ans_dist = dist(x_ans.first, x_ans.second);
           printf("%d\n",x_ans_dist);
 #ifdef FILENAME     printf("[%d,%d] & [%d,%d]\n",ans.first.first, ans.first.second,         ans.second.first,ans.second.second);
 #endif     free(p);
     return 0;
 } pp go(int left, int right) {     pp ret;
     if ( right - left +1 < 5 ) {         int tmp = inf;
         for ( int i = left ;
 i <= right ;
 i++ )             for ( int j = i+1 ;
 j <= right ;
 j++ )                 if ( tmp > dist(p[i],p[j])) {                     tmp = dist(p[i],p[j]);
                     ret = pp(p[i], p[j]);
                 }         return ret;
     }     pp q = go(left, (left+right)/2);
     pp r = go((left+right)/2 +1, right);
     int q_dist = dist(q.first, q.second);
     int r_dist = dist(r.first, r.second);
     ret = q_dist<r_dist?q:r;
           point *Sy, mid = p[(left+right)/2];
     Sy = (point*)malloc(sizeof(point)*(right-left+1));
       int walk = 0, eps = dist(ret.first,ret.second);
     for ( int i = left ;
 i<= right ;
i++ ) {         if ( !run2 ) {             if ( sqr(p[i].first-mid.first) < eps )                 Sy[walk++] = p[i];
         }         else {             if ( sqr(p[i].second -mid.second) < eps )                 Sy[walk++] = p[i];
         }     }     sort(Sy,Sy+walk,cmp_y);
     for ( int i = 0 ;
 i < walk ;
 i++ ) {         for ( int j = i+1;
 j < i+16 && j < walk;
 j++ ) {             if ( dist(Sy[i],Sy[j]) < eps ) {                 eps = dist(Sy[i], Sy[j]);
                 ret = pp(Sy[i], Sy[j]);
                 break;
             }         }     }     free(Sy);
     return ret;
 } bool cmp_y(const point& a,const point& b){     return a.second < b.second;
 }   inline int dist(const point& a,const point& b){     return sqr(a.first-b.first)+sqr(a.second-b.second);
 } ]]>
```
Line Sweeping 으로 해결하는 방법은 말그대로 각 점들을 X좌표를 기준으로(Y좌표를 기준으로 해도 된다.) 탐색하면서 조건에 맞지않는 점들을 제거해가는 방식으로 한다. 조건에 맞지 않는 점이란, 현재 가지고 있는 답보다 X좌표를 기준으로 더 멀리있는 점을 뜻한다. X좌표만을 봤는데도 답보다 거리가 멀다면 Y좌표까지 고려한다면 더 먼 것이 자명하기 때문이다. 이렇게 조건에 맞지 않는 점들을 제외하면서 보는데, 이 집합 안에서 비교하는 과정도 위의 방법과 똑같이 $O(N^2)$의 시간복잡도를 갖는다. 따라서 전부 탐색하지 않기 위해서 Balanced Binary Tree가 필요하다. STL 중에 set이 그런 자료구조를 가지므로 set에 Y좌표를 기준으로 넣어준 뒤, 비교할 때 upper_bound와 lower_bound를 이용하여 답보다 거리가 적은 점들만을 $O(log N)$만에 비교하면 $O(N log N)$으로 해결이 된다.

Line Sweeping 으로 해결하는 방법은 말그대로 각 점들을 X좌표를 기준으로(Y좌표를 기준으로 해도 된다.) 탐색하면서 조건에 맞지않는 점들을 제거해가는 방식으로 한다. 조건에 맞지 않는 점이란, 현재 가지고 있는 답보다 X좌표를 기준으로 더 멀리있는 점을 뜻한다. X좌표만을 봤는데도 답보다 거리가 멀다면 Y좌표까지 고려한다면 더 먼 것이 자명하기 때문이다. 이렇게 조건에 맞지 않는 점들을 제외하면서 보는데, 이 집합 안에서 비교하는 과정도 위의 방법과 똑같이 $O(N^2)$의 시간복잡도를 갖는다. 따라서 전부 탐색하지 않기 위해서 Balanced Binary Tree가 필요하다. STL 중에 set이 그런 자료구조를 가지므로 set에 Y좌표를 기준으로 넣어준 뒤, 비교할 때 upper_bound와 lower_bound를 이용하여 답보다 거리가 적은 점들만을 $O(log N)$만에 비교하면 $O(N log N)$으로 해결이 된다.


```
<![CDATA[ #include <cstdio>#include <cstring>#include <algorithm>#include <set>#include <vector>#include <cmath>using namespace std;
 typedef pair<int,int> ii;
 #define inf 100000000 double dist(ii a, ii b) {     return (double)((a.first-b.first)*(a.first-b.first)         +(a.second-b.second)*(a.second-b.second));
 } int main() { //  freopen("input.txt","r",stdin);
     int n;
     scanf("%d",&n);
     vector < ii > v;
     for ( int i =0 ;
i < n;
 i++ ) {         int t1,t2;
         scanf("%d%d",&t1,&t2);
         v.push_back(ii(t1,t2));
     }     set < ii > s;
     sort(v.begin(), v.end());
     s.insert(ii(v[0].second,v[0].first));
     s.insert(ii(v[1].second,v[1].first));
     double ans = dist(v[0], v[1]);
     int last=0;
     for ( int i = 2 ;
 i < n ;
 i++ ) {         while ( last < i ) {             int d = v[i].first - v[last].first;
             if ( d*d <= ans ) break;
             else s.erase(ii(v[last].second,v[last].first)), last++;
         }         double limit = sqrt(ans);
         set<ii>::iterator end=s.upper_bound(ii(v[i].second+limit,inf));
         for ( set<ii>::iterator it=s.lower_bound(ii(v[i].second-limit,-inf));
it!=end;
it++)             ans = min(ans, dist(ii((*it).second,(*it).first), v[i]));
         s.insert(ii(v[i].second,v[i].first));
     }     printf("%d\n",(int)ans);
     return 0;
 } ]]>
```
Comment


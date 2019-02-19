---
title: Topcoder SRM 439 Round 1 Division ll, level.Three PalindromeFactory
date: 2013-10-07 10:56:00
category: Editorials
---

문제요약

팰린드롬을 만들 때에는 보통 아래 네가지 연산을 이용한다. 

팰린드롬을 만들 때에는 보통 아래 네가지 연산을 이용한다. 

1. 문자열의 어떤 위치에 문자를 삽입한다.

1. 문자열의 어떤 위치에 문자를 삽입한다.

2. 어떤 위치의 문자를 삭제한다.

2. 어떤 위치의 문자를 삭제한다.

3. 어떤 위치의 문자를 교환한다.

3. 어떤 위치의 문자를 교환한다.

4. 서로 다른 문자를 교환한다.

4. 서로 다른 문자를 교환한다.

[1-3]번 연산은 무한히 사용해도 되지만 4번 연산은 한번밖에 사용을 못한다고 할 때, 길이가 최대 $N$인 입력받은 문자열을 팰린드롬으로 만들 때, 이용되는 최소 연산 횟수를 구하는 문제이다$(1\leq{}N\leq{}50)$.

[1-3]번 연산은 무한히 사용해도 되지만 4번 연산은 한번밖에 사용을 못한다고 할 때, 길이가 최대 $N$인 입력받은 문자열을 팰린드롬으로 만들 때, 이용되는 최소 연산 횟수를 구하는 문제이다$(1\leq{}N\leq{}50)$.





해법

$2$차원 $DP$로 해결할 수 있다. 현재 상태를 $d[i][j]$라고 할 때, 

$2$차원 $DP$로 해결할 수 있다. 현재 상태를 $d[i][j]$라고 할 때, 

$d[i+1][j-1]$ = 다른 문자로 교환하거나 팰린드롬

$d[i][j-1]$ = 오른쪽 문자를 삽입하거나 삭제

$d[i][j-1]$ = 오른쪽 문자를 삽입하거나 삭제

$d[i+1][j]$ = 왼쪽 문자를 삽입하거나 삭제

$d[i+1][j]$ = 왼쪽 문자를 삽입하거나 삭제

라고 나타낼 수 있다. $N$제한이 $50$밖에 되지 않으므로 원래의 문자열로 $DP$를 한번 돌리고, $N^2$만큼 돌면서 모든 문자를 한 번씩 $Swap$시켜주고 $DP$를 돌리면 최소 연산 횟수를 구할 수 있다.

라고 나타낼 수 있다. $N$제한이 $50$밖에 되지 않으므로 원래의 문자열로 $DP$를 한번 돌리고, $N^2$만큼 돌면서 모든 문자를 한 번씩 $Swap$시켜주고 $DP$를 돌리면 최소 연산 횟수를 구할 수 있다.


```
<![CDATA[ #include <cstdio>#include <cstring>#include <string>#include <algorithm>using namespace std;
 char a[55];
 int d[55][55];
 int ans=0x74747474,n;
 void dp(int l,int r,string s) {     if ( d[l][r] != -1 ) return;
     if ( l == r || r < l ) {         d[l][r] =0;
         return;
     }     dp(l+1, r-1, s);
     dp(l+1, r  , s);
     dp(l  , r-1, s);
     int ret = d[l+1][r-1]+(s[l]!=s[r]);
     ret = min(ret, d[l+1][r]+1);
     ret = min(ret, d[l][r-1]+1);
     d[l][r] =ret;
 } int go(string s) {     if ( s.length() <= 1 ) return 0;
     memset(d,-1,sizeof(d));
     dp(0,s.length()-1,s);
     return d[0][s.length()-1];
 } int main() {     scanf("%s",a);
     string s=a;
     int ans = go(s);
     for ( int i = 0 ;
 i < s.length() ;
 i++ )         for ( int j = i+1 ;
 j < s.length() ;
 j++ ) {             string tmp = s;
             swap(tmp[i],tmp[j]);
             ans = min(ans,go(tmp)+1);
         }     printf("%d\n",ans);
     return 0;
 } ]]>
```
Comment


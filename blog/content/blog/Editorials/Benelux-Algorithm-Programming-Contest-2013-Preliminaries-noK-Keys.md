---
title: Benelux Algorithm Programming Contest 2013 Preliminaries, no.K Keys
date: 2013-11-21 13:28:00
category: Editorials
---

문제요약

내가 1층 빌딩에 침입해 문따는 문제이다. 평면도가 주어지고 문을 열 수 있는 열쇠를 가지고 있으면 문을 딸 수 있다. 알파벳 소문자가 열쇠이고 알파벳 대문자가 문이다. 열쇠는 여러번 쓸 수 있다. $(2\leq{}h,w\leq{}100)$

내가 1층 빌딩에 침입해 문따는 문제이다. 평면도가 주어지고 문을 열 수 있는 열쇠를 가지고 있으면 문을 딸 수 있다. 알파벳 소문자가 열쇠이고 알파벳 대문자가 문이다. 열쇠는 여러번 쓸 수 있다. $(2\leq{}h,w\leq{}100)$









해법

열쇠를 여러번 쓸 수 있기 때문에, 열 수 있는 문은 전부 열어서 더이상 열 수 있는 문이 없을 때까지 돌린다. 더이상 열지 못하면 얻을 수 있는 문서의 개수를 세면 풀 수 있다. 

열쇠를 여러번 쓸 수 있기 때문에, 열 수 있는 문은 전부 열어서 더이상 열 수 있는 문이 없을 때까지 돌린다. 더이상 열지 못하면 얻을 수 있는 문서의 개수를 세면 풀 수 있다. 






```
<![CDATA[ #include <cstdio>#include <cstring>#include <algorithm>#include <string>#include <map>#include <set>#include <queue>using namespace std;
 typedef pair<int,int> ii;
 int dx[]={1,-1,0,0};
 int dy[]={0,0,1,-1};
 int h,w,ans;
 char s[111][111];
 char key[111111];
 bool vis[111][111];
 bool ok[128];
 map<char,vector<ii> > mp;
 vector<ii> start;
 queue<ii> q;
 bool isLower(char now) {return (now>='a' && now <= 'z');
} bool isUpper(char now) {return (now>='A' && now <= 'Z');
} bool isDollar(char now) {return (now == '$');
} void go(int y,int x) {     if ( s[y][x] == '*' ) return;
     if ( isLower(s[y][x]) ) {         ok[s[y][x]-'a'] = true;
         for ( int i = 0 ;
 i < mp[s[y][x]-'a'+'A'].size() ;
 i++ )             if ( !vis[mp[s[y][x]-'a'+'A'][i].first][mp[s[y][x]-'a'+'A'][i].second] )                 go(mp[s[y][x]-'a'+'A'][i].first,mp[s[y][x]-'a'+'A'][i].second);
         s[y][x] = '.';
     }     if ( isUpper(s[y][x]) ) {         if ( ok[s[y][x]-'A'] ) s[y][x] = '.';
         else mp[s[y][x]].push_back(ii(y,x));
     }     if ( isDollar(s[y][x]) ) {         ans++;
         s[y][x] = '.';
     }     if ( s[y][x] != '.' ) return;
     vis[y][x] = true;
     for ( int i = 0 ;
 i < 4;
 i++ ) {         int ny = y+dy[i];
         int nx = x+dx[i];
         if ( ny < 0 || ny >= h || nx < 0 || nx >= w|| vis[ny][nx] ) continue;
         go(ny,nx);
     } } int main() {     int tc;
     scanf("%d",&tc);
     while ( tc-- ) {         for ( map<char,vector<ii> >::iterator it=mp.begin();
it!=mp.end();
it++ )             it->second.clear();
         mp.clear();
         ans=0;
         memset(s,0,sizeof(s));
         memset(key,0,sizeof(key));
         memset(vis,0,sizeof(vis));
         memset(ok,0,sizeof(ok));
         scanf("%d%d",&h,&w);
         for ( int i = 0 ;
 i < h;
 i++ )             scanf("%s",s[i]);
         scanf("%s",key);
         int n = strlen(key);
         if ( !(n == 1 && key[0] == '0') )             for ( int i = 0 ;
 i < n ;
 i++ )                 ok[key[i]-'a'] = true;
         start.clear();
         for ( int i = 0 ;
 i < h ;
 i++ ) {             if ( s[i][0] != '*' ) start.push_back(ii(i,0));
             if ( s[i][w-1] != '*' ) start.push_back(ii(i,w-1));
         }         for ( int i = 1 ;
 i < w-1 ;
 i++ ) {             if ( s[0][i] != '*' ) start.push_back(ii(0,i));
             if ( s[h-1][i] != '*' ) start.push_back(ii(h-1,i));
         }         for ( int i = 0 ;
 i < start.size() ;
 i++ )             if ( !vis[start[i].first][start[i].second] )                 go(start[i].first,start[i].second);
         printf("%d\n",ans);
     }     return 0;
 } ]]>
```
Comment


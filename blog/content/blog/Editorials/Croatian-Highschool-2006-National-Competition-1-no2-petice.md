---
title: Croatian Highschool 2006, National Competition #1 no.2 petice
date: 2013-09-24 04:31:00
category: Editorials
---

* [BOJ 3165 5](http://acmicpc.net/problem/3165)

## 문제요약

$N$과 $K$가 주어졌을 때 $(1\leq{}N\leq{}10^{15},1\leq{}K\leq{}15)$, $N$보다 크면서 $5$가 적어도 $K$번 포함되는 가장 작은 수를 구하는 문제이다.


## 해법

backtracking 으로 해결할 수 있다.<br>
$N$제한이 크므로 문자열로 처리하는 것이 좋고, 함수 $f$(index, string)라고 하고, 입력받은 숫자를 inputString이라고 했을 때, 이 inputString으로 만들 수 있는 모든 문자열을 만들어서 최종 결과를 찾는다.<br>
이 때, 어떤 상태에 대하여 세 부분으로 나눌 수가 있는데, 하나는 현재 string을 가져가는 것이고, 하나는 string[index]가 5보다 작을 경우, 다른 하나는 string[index]가 5보다 클 경우이다.<br>
5보다 작을 경우에는 string[index]를 5로 바꿔주고 index 보다 오른쪽에 있는 현재까지 만들어 놓은 string을 5로 놔두거나 0으로 갱신해 줘야 하고, 5보다 클 경우에는 index보다 왼쪽에 있는 현재까지 만들어 놓은 string을 1씩 증가시켜줘야 한다.<br>
이렇게 만들 수 있는 모든 string을 만들어 최종비교하여 결과를 출력하면 된다.

```cpp
#include <cstdio>
#include <cstring>
#include <vector>
#include <algorithm>
#include <map>
#include <string>
#include <set>
#include <queue>
#include <stack>
#include <sstream>
using namespace std;
typedef long long ll;
char s[22];
ll n, k;
string a;
string ans="9999999999999999";
ll s2ll(string now) {
    ll ret;
    stringstream ss; ss << now; ss >> ret;
    return ret;
}
int ret_cnt(string now) {
    int ret = 0;
    for (int i = 0; i < now.length(); i++)
        if (now[i] == '5') ret++;
    return ret;
}
void go(int idx, string now) {
    ll llans = s2ll(ans), llnow = s2ll(now), lla = s2ll(a);
    if (ret_cnt(now) == k) {
        if (llnow >= lla && llans>llnow)
            ans = now;
    }
    if (idx == -1) {
        if (ret_cnt(now) == k) {
            if (llnow >= lla && llans>llnow)
                ans = now;
        }
        return ;
    }
    go(idx-1, now);
    int t = now[idx];
    now[idx] = '5';
    if (t < '5') {
        for (int i = idx+1; i < now.length(); i++)
            now[i] = now[i] == '5'?'5':'0';
        go(idx-1, now);
    }
    if (t > '5') {
        int carry =1;
        for (int j = idx-1; j >= 0; j--) {
            if (now[j] == '9' && carry) now[j]='0';
            else if (carry) now[j]++, carry=0;
        }
        go(idx-1, now);
    }
}
int main() {
    scanf("%lld%lld", &n, &k);
    n++;
    sprintf(s, "%lld", n);
    a = s;
    go(a.length()-1, a);
    if (ans == "9999999999999999"){
        ans="";
        for (int i = 0; i < k; i++)
            ans += '5';
    }
    string t = s;
    ll llans = s2ll(ans), llt = s2ll(t);
    if (llans < llt) ans.insert(ans.begin(), '1');
    printf("%s\n", ans.c_str());
    return 0;
}
```

>비슷한 문제로 BOJ 1040 정수 가 있다. 정수가 더 어렵다.
맞은 사람을 보니까 나만 시간이 느렸다. 나처럼 전체를 다 보면 안되나보다. 
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
typedef long long ll;
  
ll N, ans, now;
bool d[99][99][2][3];
int buf[25];
int K, L;
  
void go(int len, int five, int match, int start)
{
    if (d[len][five][match][start]) return;
  
    if (len == L) {
        if (five >= K && now > N) {
            if (!ans) ans = now;
            else ans = min(ans, now);
        }
        return;
    }
  
    d[len][five][match][start] = 1;
  
    if (match) {
        start = max(start, buf[len]);
    }
  
    for (int k = start; k < 10; k++) {
        now = now * 10 + k;
        go(len + 1, (k == 5) + five,  match & (k == buf[len]), 0);
        now /= 10;
    }
}
  
int main()
{
    scanf("%lld%d", &N, &K);
    char buf1[256] = "";
  
    L = sprintf(buf1, "%lld", N);
    int fL = L;
  
    for (int i = 0; i < L; i++) {
        buf[i] = buf1[i] - 48;
    }
    while (!ans) {
        go(0, 0, (L == fL), 1);
        memset(d, 0, sizeof d);
        L++;
    }
  
    printf("%lld\n", ans);
    return 0;
}
```


---
title: Disjoint-Set using Union Find Tree
date: 2013-10-07 09:21:00
category: Data Structure
---

```cpp
/*
 * UnionFind::Init(size); // set initializing
 * UnionFind::Find(node); // find parent
 * UnionFind::MakeUnion(x,y); // union(x,y)
 */
#include <vector>
#include <algorithm>
using namespace std;
namespace UnionFind{
    vector<int> rank;
    vector<int> u;
    void Init(int size) {
        rank.resize(size+1,0);
        u.resize(size+1,0);
        for ( int i = 0 ; i <= size ; i++ )
            u[i] = i;
    }
    int Find(int now) {
        return (u[now]==now)?now:(u[now]=Find(u[now]));
    }
    void MakeUnion(int x,int y) {
        x = Find(x); y = Find(y);
        if ( x == y ) return;
        if ( rank[x] < rank[y] ) u[x] = y;
        else {
            u[y] = x;
            rank[x]+=(rank[x]==rank[y]);
        }
    }
}
```

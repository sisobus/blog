---
title: Segment Tree
date: 2013-09-24 05:13:00
category: Data Structure
---

```cpp
#include <cstdio>
#include <vector>
#include <algorithm>
using namespace std;
namespace SegmentTree{
    typedef long long ll;
    vector<int> segmentTree;
    vector<int> v;
    ll Combine(ll a,ll b) {
        // const complexity function to Combine two solutions
        // +, -, ^, min, max
        return a+b;
    }
    ll Build(int left,int right,int node) {
        if ( left == right )
            return segmentTree[node] = v[left];
        int mid = (left+right)>>1;
        ll ansLeft = Build(left, mid ,node*2);
        ll ansRight = Build(mid+1, right ,node*2+1);
        return segmentTree[node] = Combine(ansLeft,ansRight);
    }
    void Initializing(vector<int> vv) {
        segmentTree.resize((int)vv.size()*4+1,0);
        v = vv;
        Build(0,(int)vv.size()-1,1);
    }
    ll Query(int left,int right,int node,int nodeLeft,int nodeRight) {
        if ( right < nodeLeft || nodeRight < left ) return 0;
        if ( left <= nodeLeft && nodeRight <= right ) 
            return segmentTree[node];
        int mid = (nodeLeft+nodeRight)>>1;
        return Combine(Query(left,right,node*2,nodeLeft,mid),
                Query(left,right,node*2+1,mid+1,nodeRight));
    }
    ll Update(int idx,int val,int node,int nodeLeft,int nodeRight) {
        if ( idx < nodeLeft || nodeRight < idx ) return segmentTree[node];
        if ( nodeLeft == nodeRight ) return segmentTree[node] = val;
        int mid=(nodeLeft+nodeRight)>>1;
        return segmentTree[node] = Combine(
                Update(idx,val,node*2,nodeLeft,mid),
                Update(idx,val,node*2+1,mid+1,nodeRight));
    }
}
int main() {
    vector<int> v;
    for ( int i = 0 ; i < 10 ; i++ ) 
        v.push_back(i+1);
    SegmentTree::Initializing(v);
    SegmentTree::Build(0,9,1);
    for ( int i = 0 ; i < 10 ; i++ ) 
        for ( int j = i ; j < 10 ; j++ ) 
            printf("%d %d %lld\n",i,j,SegmentTree::Query(i,j,1,0,9));
    SegmentTree::Update(3,5,1,0,9);
    for ( int i = 0 ; i < 10 ; i++ ) 
        for ( int j = i ; j < 10 ; j++ ) 
            printf("%d %d %lld\n",i,j,SegmentTree::Query(i,j,1,0,9));
    return 0;
}
```

/**
 *     author: brownfox2k6
 *    created: Thu 29 Jun 2023 20:37:41 Hanoi, Vietnam
**/

// In this code, Segment Tree which find the min value
// of the elements between l and r will be implemented

#include <bits/stdc++.h>
using namespace std;

const int maxn = 2e5;
const int inf = 0x3f3f3f3f;
int a[maxn];
int st[4*maxn];  // segment tree

void buildTree(int i, int s, int e) {
    if (s == e) {
        st[i] = a[s];
        return;
    }
    int mid = (s + e) / 2;
    buildTree(2*i, s, mid);
    buildTree(2*i+1, mid+1, e);
    st[i] = min(st[2*i], st[2*i+1]);
}

int retrieve(int i, int ts, int te, int qs, int qe) {
    if (qs > te || qe < ts) { // completely outside
        return inf;
    }
    if (qs <= ts && qe >= te) { // completely inside
        return st[i];
    }
    int mid = (ts + te) / 2;
    int l = retrieve(2*i, ts, mid, qs, qe);
    int r = retrieve(2*i+1, mid+1, te, qs, qe);
    return min(l, r);
}

void update(int i, int s, int e, int qi, int qv) {
    if (s == e) {
        st[i] = qv;
        return;
    }
    int mid = (s + e) / 2;
    if (qi <= mid) { // update left child
        update(2*i, s, mid, qi, qv);
    } else { // update right child
        update(2*i+1, mid+1, e, qi, qv);
    }
    st[i] = min(st[2*i], st[2*i+1]);
}

int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; ++i) {  // 1-indexed is used
        cin >> a[i];
    }
    buildTree(1, 1, n);

    // Examples:
    retrieve(1, 1, n, 2, 4);  // get the value from segment a[2]..a[4]
    update(1, 1, n, 2, 100);  // update a[2] to 100
}

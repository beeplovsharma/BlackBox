# 🎯 Segment Tree with Point Update

This file provides the **boilerplate code** for implementing a Segment Tree that supports:

- Building the tree
- Point updates
- Range sum queries

---

## 🛠 Boilerplate Code

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5;
int seg[4 * N], arr[N];

// 📦 Build Segment Tree
void build(int i, int l, int r) {
    if (l == r) {
        seg[i] = arr[l];
        return;
    }
    int mid = (l + r) / 2;
    build(2 * i + 1, l, mid);
    build(2 * i + 2, mid + 1, r);
    seg[i] = seg[2 * i + 1] + seg[2 * i + 2];
}

// 🎯 Point Update
void updatePoint(int i, int l, int r, int idx, int val) {
    if (l == r) {
        seg[i] = val;
        return;
    }
    int mid = (l + r) / 2;
    if (idx <= mid)
        updatePoint(2 * i + 1, l, mid, idx, val);
    else
        updatePoint(2 * i + 2, mid + 1, r, idx, val);
    seg[i] = seg[2 * i + 1] + seg[2 * i + 2];
}

// 📊 Range Query (Sum)
int query(int i, int l, int r, int start, int end) {
    if (end < l || start > r) return 0;

    if (start <= l && r <= end) return seg[i];

    int mid = (l + r) / 2;
    int left = query(2 * i + 1, l, mid, start, end);
    int right = query(2 * i + 2, mid + 1, r, start, end);
    return left + right;
}
```

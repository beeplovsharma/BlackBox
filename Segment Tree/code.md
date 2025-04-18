# 🌲 Segment Tree in C++ - Your Go-To Guide

A **Segment Tree** is a powerful data structure for answering **range queries** and **updating elements** efficiently. It’s like having a Swiss army knife for problems on arrays involving sum, min, max, GCD, etc. 🔥

---

## 📌 1. What is a Segment Tree?

A Segment Tree is a **binary tree** where each node represents an **interval or segment** of an array. It allows:

- ✅ Efficient **range queries**
- ✅ Efficient **point or range updates**

It's widely used in competitive programming & CP problems.

---

## ⏱ 2. Time & Space Complexity

| Operation           | Time Complexity | Space Complexity |
| ------------------- | --------------- | ---------------- |
| Build               | `O(n)`          | `O(4n)`          |
| Point Update        | `O(log n)`      |                  |
| Range Update (Lazy) | `O(log n)`      |                  |
| Query (Sum/Min/Max) | `O(log n)`      |                  |

---

## 🚀 3. Applications

- 📊 **Range Sum / Min / Max Queries**
- 🔄 **Dynamic Array Updates**
- 🧮 **Counting Inversions**
- 🧠 **Lazy Propagation for Range Updates**
- 🎯 **Interval Tree Use Cases**

---

## 🛠 4. C++ Boilerplate Code

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5;
int seg[4 * N], lazy[4 * N]; // 🧠 SegTree + Lazy Array
int arr[N];

// 📦 Build Segment Tree
void build(int idx, int low, int high) {
    if (low == high) {
        seg[idx] = arr[low];
        return;
    }
    int mid = (low + high) / 2;
    build(2 * idx + 1, low, mid);
    build(2 * idx + 2, mid + 1, high);
    seg[idx] = seg[2 * idx + 1] + seg[2 * idx + 2];
}

// 🎯 Point Update
void updatePoint(int idx, int low, int high, int i, int val) {
    if (low == high) {
        seg[idx] = val;
        return;
    }
    int mid = (low + high) / 2;
    if (i <= mid)
        updatePoint(2 * idx + 1, low, mid, i, val);
    else
        updatePoint(2 * idx + 2, mid + 1, high, i, val);
    seg[idx] = seg[2 * idx + 1] + seg[2 * idx + 2];
}

// 💤 Lazy Range Update
void updateRange(int idx, int low, int high, int l, int r, int val) {
    if (lazy[idx] != 0) {
        seg[idx] += (high - low + 1) * lazy[idx];
        if (low != high) {
            lazy[2 * idx + 1] += lazy[idx];
            lazy[2 * idx + 2] += lazy[idx];
        }
        lazy[idx] = 0;
    }

    if (r < low || l > high) return;

    if (l <= low && high <= r) {
        seg[idx] += (high - low + 1) * val;
        if (low != high) {
            lazy[2 * idx + 1] += val;
            lazy[2 * idx + 2] += val;
        }
        return;
    }

    int mid = (low + high) / 2;
    updateRange(2 * idx + 1, low, mid, l, r, val);
    updateRange(2 * idx + 2, mid + 1, high, l, r, val);
    seg[idx] = seg[2 * idx + 1] + seg[2 * idx + 2];
}

// 📊 Range Query (Sum)
int query(int idx, int low, int high, int l, int r) {
    if (lazy[idx] != 0) {
        seg[idx] += (high - low + 1) * lazy[idx];
        if (low != high) {
            lazy[2 * idx + 1] += lazy[idx];
            lazy[2 * idx + 2] += lazy[idx];
        }
        lazy[idx] = 0;
    }

    if (r < low || l > high) return 0;

    if (l <= low && high <= r) return seg[idx];

    int mid = (low + high) / 2;
    int left = query(2 * idx + 1, low, mid, l, r);
    int right = query(2 * idx + 2, mid + 1, high, l, r);
    return left + right;
}
```

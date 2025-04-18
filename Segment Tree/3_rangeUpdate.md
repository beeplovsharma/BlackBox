---

### 📁 `README-3-LazyPropagation.md`

```markdown
# 💤 Segment Tree with Lazy Propagation (Range Update)

When frequent **range updates** are needed, Lazy Propagation helps:
- Avoid unnecessary updates to all elements
- Postpone and batch updates when actually needed

---

## 🛠 Boilerplate Code (Lazy Propagation)

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5;
int seg[4 * N], lazy[4 * N], arr[N];

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

// 💤 Lazy Range Update
void updateRange(int i, int l, int r, int start, int end, int val) {
    if (lazy[i] != 0) {
        seg[i] += (r - l + 1) * lazy[i];
        if (l != r) {
            lazy[2 * i + 1] += lazy[i];
            lazy[2 * i + 2] += lazy[i];
        }
        lazy[i] = 0;
    }

    if (end < l || start > r) return;

    if (start <= l && r <= end) {
        seg[i] += (r - l + 1) * val;
        if (l != r) {
            lazy[2 * i + 1] += val;
            lazy[2 * i + 2] += val;
        }
        return;
    }

    int mid = (l + r) / 2;
    updateRange(2 * i + 1, l, mid, start, end, val);
    updateRange(2 * i + 2, mid + 1, r, start, end, val);
    seg[i] = seg[2 * i + 1] + seg[2 * i + 2];
}

// 📊 Lazy Query
int queryLazy(int i, int l, int r, int start, int end) {
    if (lazy[i] != 0) {
        seg[i] += (r - l + 1) * lazy[i];
        if (l != r) {
            lazy[2 * i + 1] += lazy[i];
            lazy[2 * i + 2] += lazy[i];
        }
        lazy[i] = 0;
    }

    if (end < l || start > r) return 0;

    if (start <= l && r <= end) return seg[i];

    int mid = (l + r) / 2;
    int left = queryLazy(2 * i + 1, l, mid, start, end);
    int right = queryLazy(2 * i + 2, mid + 1, r, start, end);
    return left + right;
}
```

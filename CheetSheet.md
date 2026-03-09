# ⚔️ Grandmaster Python CP Arsenal
### Every Built-in, Library & Pattern You Need — Codeforces Ready

> **10 years distilled.** This document covers every major Python tool for competitive programming — from fast I/O to segment trees. Use this as your go-to reference before a contest.

---

## Table of Contents

1. [Fast I/O](#1-fast-io)
2. [collections Module](#2-collections-module)
3. [heapq — Priority Queue](#3-heapq--priority-queue)
4. [bisect — Binary Search](#4-bisect--binary-search)
5. [itertools](#5-itertools)
6. [math & Number Theory](#6-math--number-theory)
7. [functools](#7-functools)
8. [sortedcontainers — SortedList](#8-sortedcontainers--sortedlist)
9. [Built-in Power Tools](#9-built-in-power-tools)
10. [DSU / Union-Find](#10-dsu--union-find)
11. [Segment Tree](#11-segment-tree)
12. [Dynamic Programming Patterns](#12-dynamic-programming-patterns)
13. [Graph Algorithms](#13-graph-algorithms)
14. [Two Pointers & Sliding Window](#14-two-pointers--sliding-window)
15. [Binary Search on Answer](#15-binary-search-on-answer)
16. [Miscellaneous One-Liners](#16-miscellaneous-one-liners)
17. [Complexity Quick Reference](#17-complexity-quick-reference)

---

## 1. Fast I/O

> **Rule #1 of CP in Python:** Never use raw `input()` for large inputs. Always override it with `sys.stdin.readline`.

```python
import sys
input = sys.stdin.readline        # 3–5x faster than input()

# For bulk input — read everything at once (fastest possible)
data = sys.stdin.read().split()
idx = 0
def rd():  global idx; idx += 1; return data[idx - 1]
def ri():  return int(rd())
def rf():  return float(rd())

# Fast output — batch writes are faster than multiple print()
out = []
out.append(str(answer))
sys.stdout.write("\n".join(out) + "\n")
```

**Common patterns:**

```python
n = int(input())
a = list(map(int, input().split()))          # read array
x, y = map(int, input().split())             # read two ints
grid = [list(map(int, input().split())) for _ in range(n)]   # 2D grid
```

> 💡 `sys.stdin.read().split()` + index pointer is the fastest technique for data-heavy problems. Batch all output into a list and write once.

---

## 2. collections Module

### 2.1 deque

O(1) append and pop from both ends. Python's `list` is O(n) for left operations.

```python
from collections import deque

dq = deque()
dq.append(1)         # right push  — O(1)
dq.appendleft(0)     # left push   — O(1)
dq.pop()             # right pop   — O(1)
dq.popleft()         # left pop    — O(1)
dq.rotate(k)         # rotate right by k
len(dq), dq[0], dq[-1]
```

**BFS template:**

```python
from collections import deque

def bfs(graph, start):
    q = deque([start])
    visited = {start}
    dist = {start: 0}
    while q:
        node = q.popleft()
        for nb in graph[node]:
            if nb not in visited:
                visited.add(nb)
                dist[nb] = dist[node] + 1
                q.append(nb)
    return dist
```

**Monotonic deque (sliding window max/min):**

```python
from collections import deque

def sliding_max(arr, k):
    dq = deque()   # stores indices, decreasing order of values
    result = []
    for i, x in enumerate(arr):
        while dq and arr[dq[-1]] <= x:
            dq.pop()
        dq.append(i)
        if dq[0] == i - k:
            dq.popleft()           # out of window
        if i >= k - 1:
            result.append(arr[dq[0]])
    return result
```

> 💡 Use `deque` for BFS, sliding window, and anywhere needing O(1) on both ends. Never use `list` as a queue.

---

### 2.2 defaultdict

```python
from collections import defaultdict

graph  = defaultdict(list)          # adjacency list
freq   = defaultdict(int)           # frequency map
groups = defaultdict(list)          # group by key
sets   = defaultdict(set)           # group into sets

# No KeyError — missing keys return the default
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)

# Nested defaultdict
matrix = defaultdict(lambda: defaultdict(int))
```

> 💡 Eliminates `if key not in d` boilerplate. Essential for graphs and frequency problems.

---

### 2.3 Counter

```python
from collections import Counter

c = Counter("aabbbcc")        # {'b':3, 'a':2, 'c':2}
c = Counter([1, 2, 2, 3])

c['x']                        # returns 0, no KeyError
c.most_common(3)              # [(val, count), ...] top 3
c.total()                     # sum of all counts (Python 3.10+)

# Anagram check
Counter(s) == Counter(t)

# Multiset operations
c1 & c2    # intersection — min of each
c1 | c2    # union — max of each
c1 + c2    # add counts
c1 - c2    # subtract (keeps only positive)
```

> 💡 Best tool for frequency problems, anagrams, and multiset arithmetic. Much faster than `dict` + manual counting.

---

### 2.4 namedtuple & OrderedDict

```python
from collections import namedtuple, OrderedDict

# Lightweight, immutable struct — faster than class
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)
p.x, p.y         # access by name
p._replace(x=10) # returns new tuple with x=10

# OrderedDict — useful for LRU-style problems
od = OrderedDict()
od.move_to_end(key)           # O(1) move to end
od.move_to_end(key, last=False)  # move to front
od.popitem(last=True)         # O(1) remove last inserted
```

---

## 3. heapq — Priority Queue

Python only provides a **min-heap**. Negate values for max-heap.

```python
import heapq

h = []
heapq.heappush(h, val)        # O(log n)
heapq.heappop(h)              # O(log n) — removes and returns min
h[0]                          # peek min — O(1), no removal
heapq.heapify(arr)            # convert list to heap in O(n)

# Max-heap: negate values
heapq.heappush(h, -val)
max_val = -heapq.heappop(h)

# Tuples: sorted by first element
heapq.heappush(h, (priority, item))

# K smallest / K largest
heapq.nsmallest(k, arr)       # O(n log k)
heapq.nlargest(k, arr)        # O(n log k)

# Merge sorted iterables lazily
heapq.merge(sorted_iter1, sorted_iter2)
```

### Dijkstra Template

```python
import heapq
from collections import defaultdict

def dijkstra(graph, src, n):
    INF = float('inf')
    dist = [INF] * n
    dist[src] = 0
    pq = [(0, src)]          # (cost, node)

    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue          # stale entry — skip
        for v, w in graph[u]:
            nd = dist[u] + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist
```

> 💡 The `if d > dist[u]: continue` check is critical — without it, you'll process stale entries and get wrong answers or TLE.

---

## 4. bisect — Binary Search

Works on any **sorted** list. All operations are O(log n).

```python
import bisect

a = [1, 3, 5, 7, 9]

bisect.bisect_left(a, 5)     # 2 — index of first element >= 5
bisect.bisect_right(a, 5)    # 3 — index of first element >  5
bisect.insort_left(a, 6)     # insert 6 maintaining sorted order — O(n) due to shift
bisect.insort_right(a, 6)

# C++ equivalents
# lower_bound(a, x) = bisect_left(a, x)
# upper_bound(a, x) = bisect_right(a, x)

# Count elements equal to x
lo = bisect.bisect_left(a, x)
hi = bisect.bisect_right(a, x)
count = hi - lo

# Count elements in range [lo_val, hi_val]
def count_range(a, lo_val, hi_val):
    return bisect.bisect_right(a, hi_val) - bisect.bisect_left(a, lo_val)

# Floor (largest element <= x)
idx = bisect.bisect_right(a, x) - 1
floor_val = a[idx] if idx >= 0 else None

# Ceiling (smallest element >= x)
idx = bisect.bisect_left(a, x)
ceil_val = a[idx] if idx < len(a) else None
```

> 💡 Use for coordinate compression, offline range queries, and any "find nearest" problem on a sorted structure.

---

## 5. itertools

### 5.1 Combinatorics

```python
import itertools

# Permutations
list(itertools.permutations([1,2,3]))         # all 3! = 6 orderings
list(itertools.permutations([1,2,3], 2))      # P(3,2) = 6

# Combinations
list(itertools.combinations([1,2,3], 2))              # C(3,2) = 3, no repeats
list(itertools.combinations_with_replacement([1,2], 2))  # with repeats

# Cartesian product (replaces nested loops)
for i, j, k in itertools.product(range(3), repeat=3):
    pass                                         # 3^3 = 27 iterations

for a, b in itertools.product(arr1, arr2):
    pass
```

### 5.2 Accumulate (Prefix Operations)

```python
import itertools, operator

arr = [1, 2, 3, 4, 5]

list(itertools.accumulate(arr))                       # prefix sum:  [1,3,6,10,15]
list(itertools.accumulate(arr, max))                  # running max: [1,2,3,4,5]
list(itertools.accumulate(arr, min))                  # running min: [1,1,1,1,1]
list(itertools.accumulate(arr, operator.mul))         # prefix product
list(itertools.accumulate(arr, lambda a,b: a^b))      # prefix XOR
list(itertools.accumulate(arr, initial=0))            # starts with 0 (Python 3.8+)
```

### 5.3 Other Useful Tools

```python
# Chain: flatten one level
itertools.chain([1,2], [3,4], [5])             # [1,2,3,4,5]
itertools.chain.from_iterable([[1,2],[3,4]])   # same

# Groupby: group consecutive equal elements (input must be sorted!)
for key, group in itertools.groupby(sorted(arr)):
    print(key, list(group))

# Islice: lazy slicing of any iterable
itertools.islice(iterable, start, stop, step)

# Cycle and repeat
itertools.cycle([1,2,3])     # 1,2,3,1,2,3,...
itertools.repeat(x, n)       # x, x, x, ... (n times)
```

> 💡 `accumulate` is the cleanest way to compute prefix sums, prefix XOR, and running extremes — no extra array needed.

---

## 6. math & Number Theory

### 6.1 math Module

```python
import math

math.gcd(a, b)               # GCD — O(log min(a,b))
math.lcm(a, b)               # LCM (Python 3.9+)
math.gcd(*arr)               # GCD of entire array (Python 3.9+)
math.isqrt(n)                # exact integer square root
math.comb(n, k)              # C(n,k) — exact, arbitrary precision
math.perm(n, k)              # P(n,k)
math.factorial(n)            # n!
math.log(x, base)
math.log2(n), math.log10(n)
math.ceil(x), math.floor(x)
math.inf                     # infinity

# Check perfect square
math.isqrt(n) ** 2 == n

# Number of digits
math.floor(math.log10(n)) + 1
```

### 6.2 Modular Arithmetic

```python
MOD = 10**9 + 7

# Fast modular exponentiation — built-in C speed
pow(base, exp, MOD)          # O(log exp), THE most important CP built-in

# Modular inverse (requires MOD to be prime)
inv_a = pow(a, MOD - 2, MOD)             # Fermat's little theorem
inv_a = pow(a, -1, MOD)                  # Python 3.8+ shorthand

# Precompute factorials + inverse factorials for nCr mod p
N = 2 * 10**6
fact = [1] * (N + 1)
for i in range(1, N + 1):
    fact[i] = fact[i-1] * i % MOD

inv_fact = [1] * (N + 1)
inv_fact[N] = pow(fact[N], MOD - 2, MOD)
for i in range(N - 1, -1, -1):
    inv_fact[i] = inv_fact[i+1] * (i+1) % MOD

def comb(n, r):
    if r < 0 or r > n: return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n-r] % MOD
```

### 6.3 Sieve of Eratosthenes

```python
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, n+1, i):
                is_prime[j] = False
    return [i for i in range(2, n+1) if is_prime[i]]

# Smallest prime factor (for fast factorization)
def spf_sieve(n):
    spf = list(range(n + 1))
    for i in range(2, int(n**0.5) + 1):
        if spf[i] == i:                   # i is prime
            for j in range(i*i, n+1, i):
                if spf[j] == j:
                    spf[j] = i
    return spf

def factorize(n, spf):
    factors = {}
    while n > 1:
        p = spf[n]
        while n % p == 0:
            factors[p] = factors.get(p, 0) + 1
            n //= p
    return factors
```

> 💡 `pow(b, e, mod)` is implemented in C and is dramatically faster than manual modular exponentiation. Always use it.

---

## 7. functools

### 7.1 @cache / @lru_cache — Memoization

```python
from functools import cache, lru_cache

@cache                              # Python 3.9+, unbounded, fastest
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

@lru_cache(maxsize=None)            # equivalent, works on older Python
def dp(i, j, state):
    # any recursive function becomes memoized automatically
    ...

# IMPORTANT: clear cache between test cases
fib.cache_clear()

# Works with any hashable arguments: ints, tuples, strings, frozensets
@cache
def solve(node, parent, mask):
    ...
```

### 7.2 reduce & cmp_to_key

```python
from functools import reduce, cmp_to_key
import operator

reduce(operator.add, arr)             # sum
reduce(operator.or_, sets)            # union of sets
reduce(lambda a, b: a & b, sets)      # intersection

# Custom multi-key comparator
def compare(a, b):
    # return negative if a should come first
    if a[0] != b[0]: return a[0] - b[0]
    return b[1] - a[1]

arr.sort(key=cmp_to_key(compare))

# Largest number problem
nums = [3, 30, 34, 5, 9]
nums.sort(key=cmp_to_key(lambda a,b: 1 if str(a)+str(b) < str(b)+str(a) else -1))
```

> 💡 `@cache` is the single most powerful CP tool in Python. Convert any recursive brute-force into memoized DP in one decorator line.

---

## 8. sortedcontainers — SortedList

Python's equivalent of C++ `std::set` / `std::multiset` / `std::map`. **O(log n)** insertions and deletions on a sorted structure.

```python
from sortedcontainers import SortedList, SortedDict, SortedSet

sl = SortedList()
sl.add(x)              # O(log n) insert
sl.remove(x)           # O(log n) remove (raises if not found)
sl.discard(x)          # O(log n) remove if exists
sl[0]                  # O(1) min
sl[-1]                 # O(1) max
sl[k]                  # O(log n) kth smallest element
len(sl)

sl.bisect_left(x)      # index of first element >= x
sl.bisect_right(x)     # index of first element > x

# Count elements in range [a, b]
count = sl.bisect_right(b) - sl.bisect_left(a)

# Ordered set (no duplicates)
ss = SortedSet([1, 2, 3])

# Ordered dict (keys kept sorted)
sd = SortedDict()
sd[key] = val
sd.peekitem(0)         # (min_key, val)
sd.peekitem(-1)        # (max_key, val)
```

> 💡 Install: `pip install sortedcontainers`. It's allowed on Codeforces. Use this wherever you'd use a TreeSet in Java or `std::set` in C++.

---

## 9. Built-in Power Tools

### 9.1 Sorting

```python
arr.sort()                                  # in-place, O(n log n), stable (Timsort)
sorted(arr)                                 # new list
arr.sort(key=lambda x: (-x[1], x[0]))       # multi-key: desc by x[1], then asc by x[0]
arr.sort(key=lambda x: x.val, reverse=True)

# Sort strings by frequency then lexicographically
words.sort(key=lambda w: (-freq[w], w))
```

### 9.2 Aggregates

```python
sum(arr)
min(arr), max(arr)
min(arr, key=lambda x: abs(x - target))    # min by custom key
sum(1 for x in arr if x > 0)              # conditional count
any(x > 5 for x in arr)                   # short-circuits at first True
all(x > 0 for x in arr)                   # short-circuits at first False
```

### 9.3 zip & enumerate

```python
for i, val in enumerate(arr, start=1):    # 1-indexed
    pass

for a, b in zip(arr1, arr2):
    pass

# Unzip / transpose
rows, cols = zip(*pairs)

# Transpose matrix
transposed = list(zip(*matrix))
```

### 9.4 List & String Tricks

```python
# 2D array initialization — CORRECT way
grid = [[0] * m for _ in range(n)]
# WRONG: [[0]*m]*n  (all rows point to same object!)

# Flatten
flat = [x for row in matrix for x in row]

# Reverse
arr[::-1]
s[::-1]

# String operations
s.count(sub), s.find(sub), s.rfind(sub)
s.split(sep), s.strip(), s.lstrip(), s.rstrip()
''.join(arr)
s.startswith(prefix), s.endswith(suffix)
s.isdigit(), s.isalpha(), s.isalnum()

# ord / chr
ord('a')          # 97
chr(97)           # 'a'
ord(c) - ord('a') # 0-indexed alphabet position
```

### 9.5 Bit Tricks

```python
n.bit_length()             # floor(log2(n)) + 1
bin(n).count('1')          # popcount — number of set bits
n & (n - 1)                # clear lowest set bit (0 if power of 2)
n & (-n)                   # isolate lowest set bit (LSB)
n | (n + 1)                # set lowest unset bit
~n & (n - 1)               # isolate trailing ones (rare)

# Power of 2 check
n > 0 and (n & (n - 1)) == 0

# Set / clear / toggle bit i
n | (1 << i)               # set
n & ~(1 << i)              # clear
n ^ (1 << i)               # toggle
(n >> i) & 1               # check
```

> 💡 Python integers are arbitrary precision — **no overflow ever**. This is a major advantage in problems with large exponents or factorials.

---

## 10. DSU / Union-Find

Near O(1) amortized per operation with path compression + union by rank.

```python
class DSU:
    def __init__(self, n):
        self.parent     = list(range(n))
        self.rank       = [0] * n
        self.size       = [1] * n
        self.components = n

    def find(self, x):
        # Path halving (iterative, no recursion limit issues)
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]   # path compression
            x = self.parent[x]
        return x

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py: return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        self.size[px] += self.size[py]
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        self.components -= 1
        return True

    def connected(self, x, y): return self.find(x) == self.find(y)
    def get_size(self, x):     return self.size[self.find(x)]
```

**Weighted DSU (for difference array problems):**

```python
class WeightedDSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.weight = [0] * n          # weight[x] = w(x → root)

    def find(self, x):
        if self.parent[x] == x: return x, 0
        root, w = self.find(self.parent[x])
        self.weight[x] += w
        self.parent[x] = root
        return root, self.weight[x]
```

---

## 11. Segment Tree

### 11.1 Iterative Segment Tree (Point Update, Range Query)

```python
class SegTree:
    def __init__(self, n, func=lambda a, b: a + b, identity=0):
        self.n        = n
        self.func     = func
        self.identity = identity
        self.tree     = [identity] * (2 * n)

    def update(self, i, val):
        i += self.n
        self.tree[i] = val
        while i > 1:
            i >>= 1
            self.tree[i] = self.func(self.tree[2*i], self.tree[2*i+1])

    def query(self, l, r):     # range [l, r) exclusive
        res = self.identity
        l += self.n; r += self.n
        while l < r:
            if l & 1:
                res = self.func(res, self.tree[l]); l += 1
            if r & 1:
                r -= 1; res = self.func(res, self.tree[r])
            l >>= 1; r >>= 1
        return res

# Usage examples
st_sum = SegTree(n)                                    # range sum
st_min = SegTree(n, min, float('inf'))                 # range min
st_max = SegTree(n, max, float('-inf'))                # range max
st_xor = SegTree(n, lambda a, b: a ^ b, 0)            # range XOR
```

### 11.2 Binary Indexed Tree (Fenwick Tree)

Simpler, faster constant — use when only prefix queries are needed.

```python
class BIT:
    def __init__(self, n):
        self.n    = n
        self.tree = [0] * (n + 1)

    def update(self, i, delta):          # 1-indexed
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)

    def query(self, i):                  # prefix sum [1..i]
        s = 0
        while i > 0:
            s += self.tree[i]
            i -= i & (-i)
        return s

    def range_query(self, l, r):         # sum [l..r] 1-indexed
        return self.query(r) - self.query(l - 1)
```

---

## 12. Dynamic Programming Patterns

### 12.1 LIS — O(n log n)

```python
import bisect

def lis_length(arr):
    sub = []                           # patience sorting
    for x in arr:
        pos = bisect.bisect_left(sub, x)
        if pos == len(sub):
            sub.append(x)
        else:
            sub[pos] = x
    return len(sub)

# LIS with actual sequence reconstruction
def lis(arr):
    n = len(arr)
    tails, parent, idx_in_sub = [], [-1]*n, [0]*n
    for i, x in enumerate(arr):
        pos = bisect.bisect_left(tails, x)
        if pos == len(tails): tails.append(x)
        else: tails[pos] = x
        idx_in_sub[i] = pos
        parent[i] = -1
    # ... backtrack for actual sequence
    return len(tails)
```

### 12.2 Knapsack

```python
# 0/1 Knapsack — iterate backwards
dp = [0] * (W + 1)
for weight, value in items:
    for w in range(W, weight - 1, -1):
        dp[w] = max(dp[w], dp[w - weight] + value)

# Unbounded Knapsack — iterate forwards
dp = [0] * (W + 1)
for weight, value in items:
    for w in range(weight, W + 1):
        dp[w] = max(dp[w], dp[w - weight] + value)
```

### 12.3 Bitmask DP

```python
n = len(items)
dp = [[float('inf')] * n for _ in range(1 << n)]
dp[0][0] = 0

for mask in range(1 << n):
    for last in range(n):
        if not (mask >> last & 1): continue
        if dp[mask][last] == float('inf'): continue
        for nxt in range(n):
            if mask >> nxt & 1: continue
            new_mask = mask | (1 << nxt)
            dp[new_mask][nxt] = min(dp[new_mask][nxt], dp[mask][last] + cost[last][nxt])

# TSP answer
ans = min(dp[(1<<n)-1][i] + cost[i][0] for i in range(n))
```

### 12.4 Interval DP

```python
# Matrix chain multiplication / merge stones
dp = [[0] * n for _ in range(n)]
for length in range(2, n + 1):
    for i in range(n - length + 1):
        j = i + length - 1
        dp[i][j] = float('inf')
        for k in range(i, j):
            dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + cost(i, k, j))
```

---

## 13. Graph Algorithms

### 13.1 DFS (Iterative — Safer in Python)

```python
import sys
sys.setrecursionlimit(300000)      # MUST SET before any deep recursion

def dfs_iterative(start, graph):
    stack   = [start]
    visited = {start}
    order   = []
    while stack:
        node = stack.pop()
        order.append(node)
        for nb in graph[node]:
            if nb not in visited:
                visited.add(nb)
                stack.append(nb)
    return order
```

### 13.2 Topological Sort (Kahn's Algorithm)

```python
from collections import defaultdict, deque

def topo_sort(n, edges):
    indeg = [0] * n
    adj   = defaultdict(list)
    for u, v in edges:
        adj[u].append(v)
        indeg[v] += 1

    q = deque(i for i in range(n) if indeg[i] == 0)
    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    return order if len(order) == n else []    # empty list = cycle detected
```

### 13.3 Bellman-Ford

```python
def bellman_ford(n, edges, src):
    dist = [float('inf')] * n
    dist[src] = 0
    for _ in range(n - 1):
        for u, v, w in edges:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    # Detect negative cycle
    for u, v, w in edges:
        if dist[u] + w < dist[v]:
            return None    # negative cycle exists
    return dist
```

### 13.4 Floyd-Warshall (All-Pairs Shortest Path)

```python
def floyd_warshall(n, edges):
    INF = float('inf')
    dist = [[INF]*n for _ in range(n)]
    for i in range(n): dist[i][i] = 0
    for u, v, w in edges: dist[u][v] = min(dist[u][v], w)

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
    return dist
```

### 13.5 Strongly Connected Components (Kosaraju)

```python
def kosaraju(n, graph, rev_graph):
    visited = [False] * n
    order   = []

    def dfs1(u):
        visited[u] = True
        for v in graph[u]:
            if not visited[v]: dfs1(v)
        order.append(u)

    for i in range(n):
        if not visited[i]: dfs1(i)

    comp    = [-1] * n
    num_scc = 0
    def dfs2(u, c):
        comp[u] = c
        for v in rev_graph[u]:
            if comp[v] == -1: dfs2(v, c)

    for u in reversed(order):
        if comp[u] == -1:
            dfs2(u, num_scc); num_scc += 1

    return comp, num_scc
```

---

## 14. Two Pointers & Sliding Window

### 14.1 Two Pointers

```python
# Sorted array pair sum
a.sort()
l, r = 0, len(a) - 1
while l < r:
    s = a[l] + a[r]
    if s == target:   return (l, r)
    elif s < target:  l += 1
    else:             r -= 1

# Partition around pivot (three-way)
lo = mid = 0; hi = len(arr) - 1
while mid <= hi:
    if arr[mid] < pivot:   arr[lo], arr[mid] = arr[mid], arr[lo]; lo += 1; mid += 1
    elif arr[mid] == pivot: mid += 1
    else:                  arr[mid], arr[hi] = arr[hi], arr[mid]; hi -= 1
```

### 14.2 Sliding Window

```python
from collections import defaultdict

# Fixed window
k = 3
window_sum = sum(arr[:k])
best = window_sum
for i in range(k, len(arr)):
    window_sum += arr[i] - arr[i - k]
    best = max(best, window_sum)

# Variable window — longest subarray with at most k distinct values
def longest_at_most_k_distinct(arr, k):
    freq = defaultdict(int)
    l = best = 0
    for r, val in enumerate(arr):
        freq[val] += 1
        while len(freq) > k:
            freq[arr[l]] -= 1
            if freq[arr[l]] == 0: del freq[arr[l]]
            l += 1
        best = max(best, r - l + 1)
    return best
```

> 💡 The left pointer `l` only ever moves right — O(n) amortized total. Never reset it inside the inner loop.

---

## 15. Binary Search on Answer

Convert "find the optimal value" → "is this value achievable?" (feasibility check).

```python
def can_achieve(mid):
    # Write this function — check if answer = mid is feasible
    return True   # or False

# Minimize answer
lo, hi = 0, 10**18
while lo < hi:
    mid = (lo + hi) // 2
    if can_achieve(mid):
        hi = mid
    else:
        lo = mid + 1
answer = lo

# Maximize answer
lo, hi = 0, 10**18
while lo < hi:
    mid = (lo + hi + 1) // 2      # +1 avoids infinite loop when lo+1 == hi
    if can_achieve(mid):
        lo = mid
    else:
        hi = mid - 1
answer = lo
```

> 💡 BSOA converts many hard optimization problems (minimum time, maximum min-distance, etc.) into a simple greedy/simulation feasibility check.

---

## 16. Miscellaneous One-Liners

```python
# Coordinate compression
vals       = sorted(set(arr))
compress   = {v: i for i, v in enumerate(vals)}
compressed = [compress[x] for x in arr]

# 2D prefix sum
ps = [[0]*(m+1) for _ in range(n+1)]
for i in range(1, n+1):
    for j in range(1, m+1):
        ps[i][j] = grid[i-1][j-1] + ps[i-1][j] + ps[i][j-1] - ps[i-1][j-1]

def rect_sum(r1, c1, r2, c2):     # inclusive, 0-indexed in grid
    return ps[r2+1][c2+1] - ps[r1][c2+1] - ps[r2+1][c1] + ps[r1][c1]

# Matrix rotation 90° clockwise
rotated = [list(row) for row in zip(*matrix[::-1])]

# Transpose
transposed = [list(row) for row in zip(*matrix)]

# GCD of list
from math import gcd
from functools import reduce
total_gcd = reduce(gcd, arr)

# Fast set operations
union        = set_a | set_b
intersection = set_a & set_b
difference   = set_a - set_b
is_subset    = set_a <= set_b

# Check if string is palindrome
s == s[::-1]

# All divisors of n — O(sqrt(n))
def divisors(n):
    divs = []
    for i in range(1, int(n**0.5) + 1):
        if n % i == 0:
            divs.append(i)
            if i != n // i: divs.append(n // i)
    return sorted(divs)

# Power set
def power_set(arr):
    return [
        [arr[j] for j in range(len(arr)) if mask >> j & 1]
        for mask in range(1 << len(arr))
    ]

# Next permutation
import itertools
sorted_perms = sorted(set(itertools.permutations(arr)))
```

---

## 17. Complexity Quick Reference

| Operation | Data Structure / Method | Time Complexity |
|---|---|---|
| Push/pop both ends | `collections.deque` | O(1) |
| Heap push/pop | `heapq` | O(log n) |
| Heap build | `heapq.heapify` | O(n) |
| Binary search | `bisect` | O(log n) |
| Sorted insert/remove | `SortedList` | O(log n) |
| Memoized recursion | `@cache` | O(1) per cached call |
| Modular exponentiation | `pow(b, e, mod)` | O(log e) |
| GCD | `math.gcd` | O(log min) |
| DSU find/union | `DSU` (path compression) | O(α(n)) ≈ O(1) |
| Segment tree update/query | `SegTree` iterative | O(log n) |
| BIT update/query | `BIT` | O(log n) |
| Sorting | `list.sort()` | O(n log n) |
| LIS | `bisect` + patience sort | O(n log n) |
| Dijkstra | `heapq` | O((V + E) log V) |
| Floyd-Warshall | nested loops | O(V³) |
| Sieve | `is_prime` array | O(n log log n) |
| nCr mod p | precomputed factorials | O(1) per query, O(n) preprocess |

---

## Quick Contest Checklist

```
□  import sys; input = sys.stdin.readline
□  sys.setrecursionlimit(300000)  — before any DFS
□  MOD = 10**9 + 7
□  Use pow(b, e, MOD) for modular exponentiation
□  Use @cache on recursive functions
□  Initialize 2D arrays with list comprehension, NOT [[val]*m]*n
□  Use deque for BFS, NOT list with .pop(0)
□  Use heapq for Dijkstra / any greedy with priority
□  Negate values for max-heap
□  Clear @cache between test cases if needed
```

---

*Compiled from 10 years of competitive programming experience. Good luck on your next contest!*
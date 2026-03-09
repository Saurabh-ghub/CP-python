# ☕ Java Grandmaster CP & DSA Cheatsheet
### 20 Years Distilled — Codeforces · LeetCode · OA · Interviews

> Every pattern, template, and trick you need. Written from 20 years of competitive programming at Grandmaster level.  
> **Java advantages over C++/Python:** Strong typing catches bugs, rich standard library, fast I/O with BufferedReader, no overflow surprises with `long`, BigInteger built-in.

---

## Table of Contents

1. [Fast I/O & Setup](#1-fast-io--setup)
2. [Java Collections Arsenal](#2-java-collections-arsenal)
3. [Sorting & Custom Comparators](#3-sorting--custom-comparators)
4. [Arrays & Strings](#4-arrays--strings)
5. [Math & Number Theory](#5-math--number-theory)
6. [Bit Manipulation](#6-bit-manipulation)
7. [Two Pointers & Sliding Window](#7-two-pointers--sliding-window)
8. [Binary Search](#8-binary-search)
9. [Prefix Sums & Difference Arrays](#9-prefix-sums--difference-arrays)
10. [Stacks & Monotonic Stack](#10-stacks--monotonic-stack)
11. [Queues & Monotonic Deque](#11-queues--monotonic-deque)
12. [Linked List Patterns](#12-linked-list-patterns)
13. [Trees & Binary Trees](#13-trees--binary-trees)
14. [Binary Search Tree (BST)](#14-binary-search-tree-bst)
15. [Heaps & Priority Queue](#15-heaps--priority-queue)
16. [Hashing Techniques](#16-hashing-techniques)
17. [Graph Representations & Traversal](#17-graph-representations--traversal)
18. [Shortest Path Algorithms](#18-shortest-path-algorithms)
19. [Minimum Spanning Tree](#19-minimum-spanning-tree)
20. [Union-Find (DSU)](#20-union-find-dsu)
21. [Dynamic Programming Patterns](#21-dynamic-programming-patterns)
22. [Greedy Patterns](#22-greedy-patterns)
23. [Backtracking Templates](#23-backtracking-templates)
24. [Segment Tree & BIT (Fenwick Tree)](#24-segment-tree--bit-fenwick-tree)
25. [Trie](#25-trie)
26. [String Algorithms](#26-string-algorithms)
27. [Geometry Basics](#27-geometry-basics)
28. [Interview Patterns Cheatsheet](#28-interview-patterns-cheatsheet)
29. [Complexity Quick Reference](#29-complexity-quick-reference)
30. [Contest Checklist](#30-contest-checklist)

---

## 1. Fast I/O & Setup

### Standard CP Template

```java
import java.util.*;
import java.io.*;

public class Main {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static PrintWriter out   = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));
    static StringTokenizer st;

    static String next()      throws IOException { while (st == null || !st.hasMoreTokens()) st = new StringTokenizer(br.readLine()); return st.nextToken(); }
    static int    nextInt()   throws IOException { return Integer.parseInt(next()); }
    static long   nextLong()  throws IOException { return Long.parseLong(next()); }
    static double nextDouble()throws IOException { return Double.parseDouble(next()); }

    public static void main(String[] args) throws IOException {
        int t = nextInt();
        while (t-- > 0) {
            solve();
        }
        out.flush();
    }

    static void solve() throws IOException {
        int n = nextInt();
        int[] a = new int[n];
        for (int i = 0; i < n; i++) a[i] = nextInt();

        // your solution here

        out.println(answer);
    }
}
```

### Key I/O Rules
```java
// NEVER use Scanner in CP — it's 10–50x slower than BufferedReader
// Scanner sc = new Scanner(System.in);  ← DO NOT USE

// Read whole line
String line = br.readLine();

// Read int array in one line
int[] arr = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

// Fast output: use PrintWriter, flush at end
// NEVER call System.out.println in a loop — use out.println then out.flush() once

// For interactive problems
out.flush();   // flush after each query
```

---

## 2. Java Collections Arsenal

### The Big Picture

| Use Case | Java Class | Complexity |
|---|---|---|
| Resizable array | `ArrayList` | O(1) amortized add |
| Queue / Deque | `ArrayDeque` | O(1) all ends |
| Priority queue | `PriorityQueue` | O(log n) |
| Hash map | `HashMap` | O(1) avg |
| Sorted map | `TreeMap` | O(log n) |
| Hash set | `HashSet` | O(1) avg |
| Sorted set | `TreeSet` | O(log n) |
| Fixed array | `int[]` | O(1) |

### ArrayList

```java
List<Integer> list = new ArrayList<>();
list.add(x);                         // append O(1) amortized
list.add(i, x);                      // insert at i O(n)
list.get(i);                         // O(1)
list.set(i, x);                      // O(1)
list.remove(Integer.valueOf(x));     // remove by value O(n)
list.remove(i);                      // remove by index O(n)
list.size();
Collections.sort(list);
Collections.reverse(list);
Collections.fill(list, 0);
list.contains(x);                    // O(n) — use HashSet for O(1)
```

### ArrayDeque (Use Instead of Stack and LinkedList)

```java
Deque<Integer> dq = new ArrayDeque<>();

// Stack operations
dq.push(x);          // addFirst — O(1)
dq.pop();            // removeFirst — O(1)
dq.peek();           // peekFirst — O(1)

// Queue operations
dq.offer(x);         // addLast — O(1)
dq.poll();           // removeFirst — O(1)
dq.peekFirst();      // O(1)

// Both ends
dq.addFirst(x);  dq.addLast(x);
dq.pollFirst();  dq.pollLast();
dq.peekFirst();  dq.peekLast();

dq.isEmpty();  dq.size();
```

> ⚠️ **Never use `Stack<T>` or `LinkedList<T>` in CP** — `ArrayDeque` is faster for both stack and queue operations.

### PriorityQueue

```java
// Min-heap (default)
PriorityQueue<Integer> minPQ = new PriorityQueue<>();

// Max-heap
PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Collections.reverseOrder());

// Custom comparator (e.g., sort by second element descending)
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] != b[1] ? a[1] - b[1] : a[0] - b[0]);

minPQ.offer(x);    // O(log n)
minPQ.poll();      // O(log n) — removes and returns min
minPQ.peek();      // O(1)    — view min without removing
minPQ.size();
minPQ.isEmpty();

// Build from collection — O(n)
PriorityQueue<Integer> pq2 = new PriorityQueue<>(list);
```

### HashMap & HashSet

```java
Map<Integer, Integer> map = new HashMap<>();
map.put(k, v);
map.get(k);                          // null if missing
map.getOrDefault(k, 0);             // safe get with default
map.containsKey(k);
map.containsValue(v);
map.remove(k);
map.size();
map.merge(k, 1, Integer::sum);      // increment counter elegantly

// Frequency map pattern
for (int x : arr) map.merge(x, 1, Integer::sum);

// Iterate
for (Map.Entry<Integer,Integer> e : map.entrySet()) { e.getKey(); e.getValue(); }
for (int k : map.keySet()) { }
for (int v : map.values()) { }

// HashSet
Set<Integer> set = new HashSet<>();
set.add(x);
set.contains(x);    // O(1)
set.remove(x);
set.size();

// Set operations
set1.retainAll(set2);   // intersection (modifies set1)
set1.addAll(set2);      // union
set1.removeAll(set2);   // difference
```

### TreeMap & TreeSet (Sorted)

```java
// TreeMap = sorted map — O(log n) all operations
TreeMap<Integer, Integer> tm = new TreeMap<>();
tm.put(k, v);
tm.get(k);
tm.firstKey();          // min key
tm.lastKey();           // max key
tm.floorKey(x);         // largest key <= x
tm.ceilingKey(x);       // smallest key >= x
tm.lowerKey(x);         // largest key < x
tm.higherKey(x);        // smallest key > x
tm.headMap(x);          // keys < x
tm.tailMap(x);          // keys >= x
tm.pollFirstEntry();    // remove and return min entry
tm.pollLastEntry();     // remove and return max entry

// TreeSet = sorted set — O(log n) all operations
TreeSet<Integer> ts = new TreeSet<>();
ts.add(x);
ts.first();   ts.last();
ts.floor(x);  ts.ceiling(x);
ts.lower(x);  ts.higher(x);
ts.headSet(x);           // elements < x
ts.tailSet(x);           // elements >= x
ts.subSet(lo, hi);       // [lo, hi)
ts.pollFirst();          // remove and return min
ts.pollLast();           // remove and return max
```

---

## 3. Sorting & Custom Comparators

```java
// Primitive arrays — uses dual-pivot quicksort
int[] arr = {3,1,4,1,5};
Arrays.sort(arr);                      // ascending
Arrays.sort(arr, 2, 5);               // sort subarray [2,5)

// Object arrays / collections — uses Timsort (stable)
Integer[] arr2 = {3,1,4,1,5};
Arrays.sort(arr2, Collections.reverseOrder());

List<Integer> list = new ArrayList<>();
Collections.sort(list);
list.sort(Comparator.reverseOrder());

// Custom comparator — lambda
int[][] intervals = {{1,3},{2,6},{8,10}};
Arrays.sort(intervals, (a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);

// Multi-key sort
Arrays.sort(people, (a, b) -> {
    if (a[1] != b[1]) return b[1] - a[1];   // sort by height desc
    return a[0] - b[0];                        // then by k asc
});

// Comparator chaining
list.sort(Comparator.comparingInt((int[] x) -> x[1])
                    .thenComparingInt(x -> x[0])
                    .reversed());

// Sort strings by length then lexicographically
strs.sort(Comparator.comparingInt(String::length).thenComparing(Comparator.naturalOrder()));

// ⚠️ DANGER: Integer overflow in comparators — never write (a - b) for large values
// WRONG: (a, b) -> a - b      (can overflow if a is large negative, b is large positive)
// RIGHT: (a, b) -> Integer.compare(a, b)   or   Long.compare(a, b)
```

### Coordinate Compression

```java
int[] arr = {100, 3, 7, 100, 3};
int[] sorted = Arrays.stream(arr).distinct().sorted().toArray();
Map<Integer, Integer> compress = new HashMap<>();
for (int i = 0; i < sorted.length; i++) compress.put(sorted[i], i);
int[] compressed = Arrays.stream(arr).map(compress::get).toArray();
// Result: [2, 0, 1, 2, 0]
```

---

## 4. Arrays & Strings

### Arrays Utility

```java
int[] a = new int[n];
Arrays.fill(a, -1);                      // fill with value
Arrays.fill(a, lo, hi, -1);             // fill range [lo, hi)
int[] copy = Arrays.copyOf(a, n);        // copy first n elements
int[] copy2 = Arrays.copyOfRange(a, l, r); // copy [l, r)
Arrays.equals(a, b);                     // element-wise equality
int idx = Arrays.binarySearch(a, x);    // on sorted array, returns index or negative

// 2D arrays
int[][] grid = new int[n][m];
for (int[] row : grid) Arrays.fill(row, 0);

// Convert array ↔ list
List<Integer> list = new ArrayList<>(Arrays.asList(boxedArr));
Integer[] boxed = list.toArray(new Integer[0]);
int[] unboxed = list.stream().mapToInt(Integer::intValue).toArray();
```

### String Operations

```java
String s = "hello world";

// Core methods
s.length();
s.charAt(i);
s.substring(l, r);           // [l, r) exclusive
s.indexOf(sub);              // first occurrence, -1 if not found
s.lastIndexOf(sub);
s.contains(sub);
s.startsWith(pre);  s.endsWith(suf);
s.replace(old, new_s);       // all occurrences (literal)
s.replaceAll(regex, new_s);  // regex version
s.split(" ");                // split by space
s.trim();                    // remove leading/trailing whitespace
s.toLowerCase();  s.toUpperCase();
s.toCharArray();             // char[]
String.valueOf(x);           // int/char/... to String

// Char operations
Character.isDigit(c);
Character.isLetter(c);
Character.isUpperCase(c);
Character.toLowerCase(c);
(int)(c - 'a');              // 0-indexed position in alphabet

// StringBuilder — O(1) amortized append vs String concatenation O(n)
StringBuilder sb = new StringBuilder();
sb.append(x);
sb.insert(i, x);
sb.delete(l, r);             // [l, r)
sb.reverse();
sb.toString();
sb.length();
sb.charAt(i);
sb.setCharAt(i, c);
sb.deleteCharAt(i);

// String comparison
s.equals(t);                 // content equality
s.compareTo(t);              // lexicographic: negative/0/positive
s.equalsIgnoreCase(t);

// String to int, int to String
int n = Integer.parseInt(s);
long n2 = Long.parseLong(s);
String str = Integer.toString(n);
String str2 = String.valueOf(n);
```

---

## 5. Math & Number Theory

### Core Math

```java
Math.abs(x);
Math.max(a, b);   Math.min(a, b);
Math.pow(a, b);                    // double — avoid for integers
Math.sqrt(x);                      // double
Math.floor(x);  Math.ceil(x);  Math.round(x);
Math.log(x);    Math.log10(x);
Math.PI;        Math.E;

// Integer square root (exact)
long isqrt(long n) {
    long r = (long) Math.sqrt(n);
    while (r * r > n) r--;
    while ((r+1)*(r+1) <= n) r++;
    return r;
}

// Safe overflow check
long result = Math.multiplyExact(a, b);   // throws on overflow
```

### GCD, LCM, and Modular Arithmetic

```java
// GCD — Euclidean algorithm
static long gcd(long a, long b) { return b == 0 ? a : gcd(b, a % b); }
static long lcm(long a, long b) { return a / gcd(a, b) * b; }    // divide first to prevent overflow

// GCD of array
long g = arr[0];
for (int x : arr) g = gcd(g, x);

// ===== MODULAR ARITHMETIC =====
static final long MOD = 1_000_000_007L;

// Modular exponentiation — O(log exp)
static long powMod(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    while (exp > 0) {
        if ((exp & 1) == 1) result = result * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}

// Modular inverse (mod must be prime) — Fermat's little theorem
static long inv(long a, long mod) { return powMod(a, mod - 2, mod); }

// Precompute factorials for nCr mod p
static long[] fact, inv_fact;
static void precompute(int n) {
    fact = new long[n+1];
    inv_fact = new long[n+1];
    fact[0] = 1;
    for (int i = 1; i <= n; i++) fact[i] = fact[i-1] * i % MOD;
    inv_fact[n] = inv(fact[n], MOD);
    for (int i = n-1; i >= 0; i--) inv_fact[i] = inv_fact[i+1] * (i+1) % MOD;
}

static long comb(int n, int r) {
    if (r < 0 || r > n) return 0;
    return fact[n] * inv_fact[r] % MOD * inv_fact[n-r] % MOD;
}
```

### Sieve of Eratosthenes

```java
static boolean[] sieve(int n) {
    boolean[] isPrime = new boolean[n+1];
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; (long)i*i <= n; i++)
        if (isPrime[i])
            for (int j = i*i; j <= n; j += i)
                isPrime[j] = false;
    return isPrime;
}

// Smallest prime factor (for fast factorization)
static int[] spf(int n) {
    int[] spf = new int[n+1];
    for (int i = 0; i <= n; i++) spf[i] = i;
    for (int i = 2; (long)i*i <= n; i++)
        if (spf[i] == i)
            for (int j = i*i; j <= n; j += i)
                if (spf[j] == j) spf[j] = i;
    return spf;
}

static Map<Integer,Integer> factorize(int n, int[] spf) {
    Map<Integer,Integer> factors = new HashMap<>();
    while (n > 1) {
        int p = spf[n];
        while (n % p == 0) { factors.merge(p, 1, Integer::sum); n /= p; }
    }
    return factors;
}
```

### BigInteger

```java
import java.math.BigInteger;

BigInteger a = new BigInteger("123456789012345678901234567890");
BigInteger b = BigInteger.valueOf(42);

a.add(b);
a.subtract(b);
a.multiply(b);
a.divide(b);
a.mod(b);
a.pow(n);
a.gcd(b);
a.modPow(exp, mod);          // fast modular exponentiation
a.isProbablePrime(certainty); // Miller-Rabin primality
a.compareTo(b);              // -1, 0, 1
BigInteger.ZERO;  BigInteger.ONE;  BigInteger.TWO;
```

---

## 6. Bit Manipulation

```java
// Core operations
int a = 0b1010;
a & b;     // AND
a | b;     // OR
a ^ b;     // XOR
~a;        // NOT
a << k;    // left shift (multiply by 2^k)
a >> k;    // arithmetic right shift (divide by 2^k, preserves sign)
a >>> k;   // logical right shift (fills with 0)

// Common tricks
Integer.bitCount(n);          // popcount — number of set bits
Integer.highestOneBit(n);     // isolate highest set bit
Integer.numberOfTrailingZeros(n);  // count trailing zeros
Integer.numberOfLeadingZeros(n);
Integer.toBinaryString(n);    // binary string representation

n & (n-1);                    // clear lowest set bit
n & (-n);                     // isolate lowest set bit (LSB)
n | (n+1);                    // set lowest unset bit
(n >> i) & 1;                 // check bit i
n | (1 << i);                 // set bit i
n & ~(1 << i);                // clear bit i
n ^ (1 << i);                 // toggle bit i

// Power of 2
boolean isPow2 = n > 0 && (n & (n-1)) == 0;

// XOR properties
// a ^ a = 0
// a ^ 0 = a
// XOR is commutative and associative
// Find single non-duplicate: XOR all elements

// Count bits in [1..n]
static int countBits(int n) {
    int count = 0;
    for (int i = 0; (1 << i) <= n; i++) {
        int cycle = 1 << (i+1);
        count += (n+1) / cycle * (cycle/2);
        count += Math.max(0, (n+1) % cycle - cycle/2);
    }
    return count;
}

// Bitmask DP: iterate subsets
for (int mask = 0; mask < (1 << n); mask++) {
    for (int i = 0; i < n; i++) {
        if ((mask >> i & 1) == 1) {
            // bit i is set in mask
        }
    }
}

// Iterate all non-empty subsets of a mask
for (int sub = mask; sub > 0; sub = (sub-1) & mask) {
    // process sub
}
```

---

## 7. Two Pointers & Sliding Window

### Two Pointers

```java
// Two sum in sorted array
int l = 0, r = arr.length - 1;
while (l < r) {
    int sum = arr[l] + arr[r];
    if (sum == target) { /* found */ break; }
    else if (sum < target) l++;
    else r--;
}

// Remove duplicates in-place
int slow = 0;
for (int fast = 1; fast < arr.length; fast++)
    if (arr[fast] != arr[slow]) arr[++slow] = arr[fast];
// arr[0..slow] has unique elements

// Partition: Dutch national flag (3-way)
int lo = 0, mid = 0, hi = arr.length - 1;
while (mid <= hi) {
    if      (arr[mid] < pivot) { swap(arr, lo++, mid++); }
    else if (arr[mid] > pivot) { swap(arr, mid, hi--); }
    else                       { mid++; }
}

// Merge two sorted arrays
int i = 0, j = 0, k = 0;
while (i < a.length && j < b.length)
    result[k++] = (a[i] <= b[j]) ? a[i++] : b[j++];
while (i < a.length) result[k++] = a[i++];
while (j < b.length) result[k++] = b[j++];
```

### Sliding Window

```java
// Fixed-size window (max sum of k elements)
int windowSum = 0;
for (int i = 0; i < k; i++) windowSum += arr[i];
int best = windowSum;
for (int i = k; i < n; i++) {
    windowSum += arr[i] - arr[i-k];
    best = Math.max(best, windowSum);
}

// Variable window — longest subarray with at most k distinct values
Map<Integer,Integer> freq = new HashMap<>();
int l = 0, best = 0;
for (int r = 0; r < n; r++) {
    freq.merge(arr[r], 1, Integer::sum);
    while (freq.size() > k) {
        int left = arr[l++];
        freq.merge(left, -1, Integer::sum);
        if (freq.get(left) == 0) freq.remove(left);
    }
    best = Math.max(best, r - l + 1);
}

// Minimum window substring (contains all chars of t)
int[] need = new int[128], have = new int[128];
for (char c : t.toCharArray()) need[c]++;
int formed = 0, required = (int) t.chars().distinct().count();
int l = 0, minLen = Integer.MAX_VALUE, minL = 0;
for (int r = 0; r < s.length(); r++) {
    char c = s.charAt(r);
    have[c]++;
    if (need[c] > 0 && have[c] == need[c]) formed++;
    while (formed == required) {
        if (r - l + 1 < minLen) { minLen = r - l + 1; minL = l; }
        char lc = s.charAt(l++);
        have[lc]--;
        if (need[lc] > 0 && have[lc] < need[lc]) formed--;
    }
}
```

---

## 8. Binary Search

### Standard Templates

```java
// Find exact value — returns index or -1
int binarySearch(int[] arr, int target) {
    int lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;   // avoids overflow — never use (lo+hi)/2
        if      (arr[mid] == target) return mid;
        else if (arr[mid] < target)  lo = mid + 1;
        else                         hi = mid - 1;
    }
    return -1;
}

// Lower bound — first index where arr[i] >= target
int lowerBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;   // hi = length (not length-1)
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] < target) lo = mid + 1;
        else                   hi = mid;
    }
    return lo;  // lo == hi
}

// Upper bound — first index where arr[i] > target
int upperBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] <= target) lo = mid + 1;
        else                    hi = mid;
    }
    return lo;
}
```

### Binary Search on Answer (BSOA)

```java
// Minimize: find smallest mid where canAchieve(mid) is true
long lo = 0, hi = (long) 1e18;
while (lo < hi) {
    long mid = lo + (hi - lo) / 2;
    if (canAchieve(mid)) hi = mid;
    else                 lo = mid + 1;
}
long answer = lo;

// Maximize: find largest mid where canAchieve(mid) is true
long lo = 0, hi = (long) 1e18;
while (lo < hi) {
    long mid = lo + (hi - lo + 1) / 2;   // +1 to avoid infinite loop
    if (canAchieve(mid)) lo = mid;
    else                 hi = mid - 1;
}
long answer = lo;

// Example: minimize maximum — allocate n tasks to k workers
static boolean canAchieve(int[] tasks, int k, long maxLoad) {
    int workers = 1;
    long current = 0;
    for (int task : tasks) {
        if (task > maxLoad) return false;
        if (current + task > maxLoad) { workers++; current = task; }
        else current += task;
    }
    return workers <= k;
}
```

### LIS — O(n log n)

```java
// Length of Longest Increasing Subsequence
int lisLength(int[] arr) {
    List<Integer> tails = new ArrayList<>();
    for (int x : arr) {
        int pos = lowerBound(tails, x);    // bisect_left equivalent
        if (pos == tails.size()) tails.add(x);
        else tails.set(pos, x);
    }
    return tails.size();
}

// Helper: lowerBound on ArrayList
int lowerBound(List<Integer> list, int target) {
    int lo = 0, hi = list.size();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (list.get(mid) < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

---

## 9. Prefix Sums & Difference Arrays

```java
// 1D Prefix Sum
int[] prefix = new int[n+1];
for (int i = 0; i < n; i++) prefix[i+1] = prefix[i] + arr[i];
// Range sum [l, r] inclusive (0-indexed)
int rangeSum(int l, int r) { return prefix[r+1] - prefix[l]; }

// 2D Prefix Sum
int[][] ps = new int[n+1][m+1];
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
        ps[i][j] = grid[i-1][j-1] + ps[i-1][j] + ps[i][j-1] - ps[i-1][j-1];

// Rectangle sum (r1,c1) to (r2,c2) inclusive, 0-indexed in grid
int rectSum(int r1, int c1, int r2, int c2) {
    return ps[r2+1][c2+1] - ps[r1][c2+1] - ps[r2+1][c1] + ps[r1][c1];
}

// 1D Difference Array — range update in O(1), build result in O(n)
int[] diff = new int[n+1];
// Add val to range [l, r] inclusive
diff[l] += val;
diff[r+1] -= val;
// Build result
int[] result = new int[n];
int running = 0;
for (int i = 0; i < n; i++) result[i] = (running += diff[i]);

// 2D Difference Array
int[][] diff2d = new int[n+2][m+2];
// Add val to rectangle (r1,c1)-(r2,c2) inclusive
diff2d[r1][c1]     += val;
diff2d[r1][c2+1]   -= val;
diff2d[r2+1][c1]   -= val;
diff2d[r2+1][c2+1] += val;
// Build result (2D prefix sum)
```

---

## 10. Stacks & Monotonic Stack

### Monotonic Stack Patterns

```java
// Next Greater Element (to the right)
int[] nextGreater = new int[n];
Arrays.fill(nextGreater, -1);
Deque<Integer> stack = new ArrayDeque<>();  // stores indices
for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && arr[stack.peek()] < arr[i])
        nextGreater[stack.pop()] = arr[i];
    stack.push(i);
}

// Previous Smaller Element (to the left)
int[] prevSmaller = new int[n];
Arrays.fill(prevSmaller, -1);
stack.clear();
for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && arr[stack.peek()] >= arr[i])
        stack.pop();
    if (!stack.isEmpty()) prevSmaller[i] = arr[stack.peek()];
    stack.push(i);
}

// Largest Rectangle in Histogram — O(n)
int largestRect(int[] heights) {
    Deque<Integer> st = new ArrayDeque<>();
    int max = 0, n = heights.length;
    for (int i = 0; i <= n; i++) {
        int h = (i == n) ? 0 : heights[i];
        while (!st.isEmpty() && heights[st.peek()] > h) {
            int height = heights[st.pop()];
            int width  = st.isEmpty() ? i : i - st.peek() - 1;
            max = Math.max(max, height * width);
        }
        st.push(i);
    }
    return max;
}

// Sum of subarray minimums — O(n)
// contribution of arr[i] as min = (i - left[i]) * (right[i] - i)
// where left[i] = nearest index with arr[j] < arr[i]
//       right[i] = nearest index with arr[j] <= arr[i] (strict on right to avoid double count)
```

---

## 11. Queues & Monotonic Deque

```java
// BFS template
Queue<Integer> q = new ArrayDeque<>();
boolean[] visited = new boolean[n];
q.offer(src);
visited[src] = true;
int dist = 0;
while (!q.isEmpty()) {
    int size = q.size();
    while (size-- > 0) {
        int node = q.poll();
        for (int nb : graph[node]) {
            if (!visited[nb]) {
                visited[nb] = true;
                q.offer(nb);
            }
        }
    }
    dist++;
}

// Sliding Window Maximum — O(n) using monotonic deque
int[] slidingMax(int[] arr, int k) {
    int n = arr.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> dq = new ArrayDeque<>();   // stores indices, decreasing values
    for (int i = 0; i < n; i++) {
        // Remove out-of-window elements from front
        while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) dq.pollFirst();
        // Remove smaller elements from back (maintain decreasing order)
        while (!dq.isEmpty() && arr[dq.peekLast()] <= arr[i]) dq.pollLast();
        dq.addLast(i);
        if (i >= k - 1) result[i - k + 1] = arr[dq.peekFirst()];
    }
    return result;
}
```

---

## 12. Linked List Patterns

```java
// Node definition
static class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

// Reverse linked list — iterative
ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

// Find middle (slow/fast pointer)
ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;   // for even length, returns first of two middles
}

// Detect cycle (Floyd's algorithm)
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}

// Find cycle start
ListNode cycleStart(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next; fast = fast.next.next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) { slow = slow.next; fast = fast.next; }
            return slow;
        }
    }
    return null;
}

// Dummy node pattern — simplifies edge cases
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode curr = dummy;
// ... manipulate list ...
return dummy.next;

// Merge two sorted lists
ListNode merge(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0), curr = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
        else                  { curr.next = l2; l2 = l2.next; }
        curr = curr.next;
    }
    curr.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

---

## 13. Trees & Binary Trees

```java
static class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}

// DFS traversals — recursive
void inorder(TreeNode node)   { if (node==null) return; inorder(node.left); visit(node); inorder(node.right); }
void preorder(TreeNode node)  { if (node==null) return; visit(node); preorder(node.left); preorder(node.right); }
void postorder(TreeNode node) { if (node==null) return; postorder(node.left); postorder(node.right); visit(node); }

// Iterative inorder (avoids stack overflow on deep trees)
List<Integer> inorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;
    while (curr != null || !stack.isEmpty()) {
        while (curr != null) { stack.push(curr); curr = curr.left; }
        curr = stack.pop();
        result.add(curr.val);
        curr = curr.right;
    }
    return result;
}

// BFS level-order
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) return res;
    Queue<TreeNode> q = new ArrayDeque<>();
    q.offer(root);
    while (!q.isEmpty()) {
        int size = q.size();
        List<Integer> level = new ArrayList<>();
        while (size-- > 0) {
            TreeNode node = q.poll();
            level.add(node.val);
            if (node.left  != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
        res.add(level);
    }
    return res;
}

// Height / depth
int height(TreeNode node) {
    if (node == null) return 0;
    return 1 + Math.max(height(node.left), height(node.right));
}

// Diameter of binary tree
int maxDiameter = 0;
int diameter(TreeNode node) {
    if (node == null) return 0;
    int l = diameter(node.left), r = diameter(node.right);
    maxDiameter = Math.max(maxDiameter, l + r);
    return 1 + Math.max(l, r);
}

// Lowest Common Ancestor (LCA)
TreeNode lca(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left  = lca(root.left, p, q);
    TreeNode right = lca(root.right, p, q);
    return (left != null && right != null) ? root : (left != null ? left : right);
}

// Path sum
boolean hasPathSum(TreeNode node, int remaining) {
    if (node == null) return false;
    if (node.left == null && node.right == null) return node.val == remaining;
    return hasPathSum(node.left, remaining - node.val) ||
           hasPathSum(node.right, remaining - node.val);
}

// Serialize / Deserialize
String serialize(TreeNode root) {
    if (root == null) return "N";
    return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}
```

---

## 14. Binary Search Tree (BST)

```java
// Validate BST
boolean isValidBST(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return isValidBST(node.left, min, node.val) &&
           isValidBST(node.right, node.val, max);
}
// Call: isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE)

// Inorder of BST = sorted array — use for kth smallest
int k, kthVal;
void kthSmallest(TreeNode node) {
    if (node == null) return;
    kthSmallest(node.left);
    if (--k == 0) { kthVal = node.val; return; }
    kthSmallest(node.right);
}

// BST Insert
TreeNode insert(TreeNode node, int val) {
    if (node == null) return new TreeNode(val);
    if      (val < node.val) node.left  = insert(node.left, val);
    else if (val > node.val) node.right = insert(node.right, val);
    return node;
}

// BST Delete
TreeNode delete(TreeNode node, int val) {
    if (node == null) return null;
    if (val < node.val) { node.left  = delete(node.left, val);  return node; }
    if (val > node.val) { node.right = delete(node.right, val); return node; }
    if (node.left  == null) return node.right;
    if (node.right == null) return node.left;
    // Find inorder successor (min in right subtree)
    TreeNode succ = node.right;
    while (succ.left != null) succ = succ.left;
    node.val   = succ.val;
    node.right = delete(node.right, succ.val);
    return node;
}

// Use TreeMap/TreeSet for BST in interviews — O(log n) guaranteed
TreeMap<Integer, Integer> bst = new TreeMap<>();
bst.floorKey(x);    // largest key <= x
bst.ceilingKey(x);  // smallest key >= x
```

---

## 15. Heaps & Priority Queue

```java
// Min-heap — default
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max-heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// Custom: heap of int[] sorted by first element
PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));

// Kth largest element — min-heap of size k
int kthLargest(int[] arr, int k) {
    PriorityQueue<Integer> pq = new PriorityQueue<>();
    for (int x : arr) {
        pq.offer(x);
        if (pq.size() > k) pq.poll();   // evict smallest, keeps k largest
    }
    return pq.peek();   // min of top-k = kth largest
}

// Merge k sorted arrays — O(n log k) where n = total elements
int[] mergeKSorted(int[][] arrays) {
    PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
    int total = 0;
    for (int i = 0; i < arrays.length; i++) {
        if (arrays[i].length > 0) { pq.offer(new int[]{arrays[i][0], i, 0}); total += arrays[i].length; }
    }
    int[] result = new int[total];
    int idx = 0;
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        result[idx++] = curr[0];
        int i = curr[1], j = curr[2] + 1;
        if (j < arrays[i].length) pq.offer(new int[]{arrays[i][j], i, j});
    }
    return result;
}

// Median from data stream — two heaps
PriorityQueue<Integer> lo = new PriorityQueue<>(Collections.reverseOrder()); // max-heap, lower half
PriorityQueue<Integer> hi = new PriorityQueue<>();                            // min-heap, upper half

void addNum(int num) {
    lo.offer(num);
    hi.offer(lo.poll());
    if (lo.size() < hi.size()) lo.offer(hi.poll());
}
double findMedian() {
    return lo.size() > hi.size() ? lo.peek() : (lo.peek() + hi.peek()) / 2.0;
}
```

---

## 16. Hashing Techniques

```java
// Frequency map one-liner
Map<Character,Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) freq.merge(c, 1, Integer::sum);

// Group anagrams
Map<String, List<String>> groups = new HashMap<>();
for (String s : strs) {
    char[] arr = s.toCharArray();
    Arrays.sort(arr);
    String key = new String(arr);
    groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
}

// Rolling hash for substring matching — O(n)
static long rollingHash(String s, int l, int r, long base, long mod) {
    long h = 0;
    for (int i = l; i <= r; i++) h = (h * base + s.charAt(i)) % mod;
    return h;
}

// Polynomial hash (Rabin-Karp)
long BASE = 31, MOD = 1_000_000_007L;
long[] pow = new long[n+1], hash = new long[n+1];
pow[0] = 1;
for (int i = 1; i <= n; i++) {
    pow[i]  = pow[i-1] * BASE % MOD;
    hash[i] = (hash[i-1] * BASE + s.charAt(i-1) - 'a' + 1) % MOD;
}
// Substring [l,r] hash (0-indexed)
long getHash(int l, int r) {
    return (hash[r+1] - hash[l] * pow[r-l+1] % MOD + MOD * MOD) % MOD;
}

// Pair key trick — combine two ints into one long
long key = ((long) x << 32) | (y & 0xFFFFFFFFL);
// Or use: key = x * MOD + y  (if y < MOD)

// Count pairs with sum k
Map<Integer,Integer> count = new HashMap<>();
int pairs = 0;
for (int x : arr) {
    pairs += count.getOrDefault(k - x, 0);
    count.merge(x, 1, Integer::sum);
}
```

---

## 17. Graph Representations & Traversal

### Graph Setup

```java
// Adjacency list (most common in CP)
List<List<Integer>> graph = new ArrayList<>();
for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
graph.get(u).add(v);
graph.get(v).add(u);   // undirected

// Weighted adjacency list
List<List<int[]>> wgraph = new ArrayList<>();
for (int i = 0; i < n; i++) wgraph.add(new ArrayList<>());
wgraph.get(u).add(new int[]{v, w});

// Adjacency matrix (for dense graphs or Floyd-Warshall)
int[][] adj = new int[n][n];
Arrays.fill(adj[i], Integer.MAX_VALUE/2);
adj[i][i] = 0;

// 4-directional grid
int[] dr = {-1, 1, 0, 0};
int[] dc = {0, 0, -1, 1};
// 8-directional
int[] dr8 = {-1,-1,-1,0,0,1,1,1};
int[] dc8 = {-1,0,1,-1,1,-1,0,1};
```

### DFS & BFS

```java
// DFS — iterative (avoids stack overflow on n=10^5+)
void dfs(int start, List<List<Integer>> g, boolean[] visited) {
    Deque<Integer> stack = new ArrayDeque<>();
    stack.push(start);
    visited[start] = true;
    while (!stack.isEmpty()) {
        int u = stack.pop();
        for (int v : g.get(u))
            if (!visited[v]) { visited[v] = true; stack.push(v); }
    }
}

// BFS — multi-source
int[] bfsMultiSource(int n, List<List<Integer>> g, List<Integer> sources) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    Queue<Integer> q = new ArrayDeque<>();
    for (int s : sources) { dist[s] = 0; q.offer(s); }
    while (!q.isEmpty()) {
        int u = q.poll();
        for (int v : g.get(u))
            if (dist[v] == Integer.MAX_VALUE) { dist[v] = dist[u] + 1; q.offer(v); }
    }
    return dist;
}

// Topological Sort — Kahn's (BFS-based)
int[] topoSort(int n, List<List<Integer>> g) {
    int[] indeg = new int[n];
    for (int u = 0; u < n; u++) for (int v : g.get(u)) indeg[v]++;
    Queue<Integer> q = new ArrayDeque<>();
    for (int i = 0; i < n; i++) if (indeg[i] == 0) q.offer(i);
    int[] order = new int[n]; int idx = 0;
    while (!q.isEmpty()) {
        int u = q.poll(); order[idx++] = u;
        for (int v : g.get(u)) if (--indeg[v] == 0) q.offer(v);
    }
    return idx == n ? order : new int[0];   // empty = cycle
}

// Detect cycle in directed graph (DFS with coloring)
// 0=unvisited, 1=in stack, 2=done
int[] color = new int[n];
boolean hasCycle = false;
void dfsCycle(int u, List<List<Integer>> g) {
    color[u] = 1;
    for (int v : g.get(u)) {
        if (color[v] == 1) { hasCycle = true; return; }
        if (color[v] == 0) dfsCycle(v, g);
    }
    color[u] = 2;
}
```

---

## 18. Shortest Path Algorithms

### Dijkstra — O((V+E) log V)

```java
long[] dijkstra(int src, int n, List<List<int[]>> g) {
    long[] dist = new long[n];
    Arrays.fill(dist, Long.MAX_VALUE);
    dist[src] = 0;
    // {dist, node}
    PriorityQueue<long[]> pq = new PriorityQueue<>(Comparator.comparingLong(a -> a[0]));
    pq.offer(new long[]{0, src});
    while (!pq.isEmpty()) {
        long[] cur = pq.poll();
        long d = cur[0]; int u = (int) cur[1];
        if (d > dist[u]) continue;     // stale entry
        for (int[] edge : g.get(u)) {
            int v = edge[0]; long w = edge[1];
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.offer(new long[]{dist[v], v});
            }
        }
    }
    return dist;
}
```

### Bellman-Ford — O(VE), handles negative weights

```java
long[] bellmanFord(int src, int n, int[][] edges) {
    long[] dist = new long[n];
    Arrays.fill(dist, Long.MAX_VALUE);
    dist[src] = 0;
    for (int i = 0; i < n-1; i++) {
        for (int[] e : edges) {
            int u = e[0], v = e[1]; long w = e[2];
            if (dist[u] != Long.MAX_VALUE && dist[u] + w < dist[v])
                dist[v] = dist[u] + w;
        }
    }
    // Detect negative cycle
    for (int[] e : edges) {
        if (dist[e[0]] != Long.MAX_VALUE && dist[e[0]] + e[2] < dist[e[1]])
            return null;  // negative cycle
    }
    return dist;
}
```

### Floyd-Warshall — O(V³), all-pairs

```java
long[][] floydWarshall(int n, long[][] dist) {
    // dist[i][j] = edge weight, Long.MAX_VALUE/2 if no edge, 0 if i==j
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                if (dist[i][k] + dist[k][j] < dist[i][j])
                    dist[i][j] = dist[i][k] + dist[k][j];
    return dist;
}
```

---

## 19. Minimum Spanning Tree

### Kruskal — O(E log E)

```java
int kruskalMST(int n, int[][] edges) {
    Arrays.sort(edges, Comparator.comparingInt(e -> e[2]));
    DSU dsu = new DSU(n);
    int totalCost = 0, edgesUsed = 0;
    for (int[] e : edges) {
        if (dsu.union(e[0], e[1])) {
            totalCost += e[2];
            if (++edgesUsed == n-1) break;
        }
    }
    return edgesUsed == n-1 ? totalCost : -1;  // -1 if not connected
}
```

### Prim — O(E log V)

```java
int primMST(int n, List<List<int[]>> g) {
    boolean[] inMST = new boolean[n];
    PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
    pq.offer(new int[]{0, 0});   // {cost, node}
    int total = 0, count = 0;
    while (!pq.isEmpty() && count < n) {
        int[] cur = pq.poll();
        int cost = cur[0], u = cur[1];
        if (inMST[u]) continue;
        inMST[u] = true; total += cost; count++;
        for (int[] e : g.get(u))
            if (!inMST[e[0]]) pq.offer(new int[]{e[1], e[0]});
    }
    return count == n ? total : -1;
}
```

---

## 20. Union-Find (DSU)

```java
static class DSU {
    int[] parent, rank, size;
    int components;

    DSU(int n) {
        parent = new int[n]; rank = new int[n]; size = new int[n];
        for (int i = 0; i < n; i++) { parent[i] = i; size[i] = 1; }
        components = n;
    }

    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);  // path compression
        return parent[x];
    }

    boolean union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank[px] < rank[py]) { int tmp = px; px = py; py = tmp; }
        parent[py] = px;
        size[px] += size[py];
        if (rank[px] == rank[py]) rank[px]++;
        components--;
        return true;
    }

    boolean connected(int x, int y) { return find(x) == find(y); }
    int size(int x) { return size[find(x)]; }
}

// Weighted DSU (for problems with relative differences)
static class WeightedDSU {
    int[] parent;
    long[] weight;   // weight[x] = weight from x to parent[x]
    WeightedDSU(int n) { parent = new int[n]; weight = new long[n]; for (int i = 0; i < n; i++) parent[i] = i; }

    long[] find(int x) {
        if (parent[x] == x) return new long[]{x, 0};
        long[] res = find(parent[x]);
        parent[x] = (int) res[0];
        weight[x] += res[1];
        return new long[]{parent[x], weight[x]};
    }
}
```

---

## 21. Dynamic Programming Patterns

### Core DP Templates

```java
// ===== 1D DP =====
// Fibonacci / count paths
long[] dp = new long[n+1];
dp[0] = base; dp[1] = base;
for (int i = 2; i <= n; i++) dp[i] = dp[i-1] + dp[i-2];

// ===== 0/1 KNAPSACK =====
int[] dp = new int[W+1];
for (int[] item : items) {   // item = {weight, value}
    for (int w = W; w >= item[0]; w--)   // iterate backwards!
        dp[w] = Math.max(dp[w], dp[w - item[0]] + item[1]);
}

// ===== UNBOUNDED KNAPSACK (Coin Change) =====
int[] dp = new int[amount+1];
Arrays.fill(dp, Integer.MAX_VALUE); dp[0] = 0;
for (int coin : coins)
    for (int w = coin; w <= amount; w++)     // iterate forwards!
        if (dp[w-coin] != Integer.MAX_VALUE)
            dp[w] = Math.min(dp[w], dp[w-coin] + 1);

// ===== 2D DP (Grid / String) =====
// Edit distance
int[][] dp = new int[m+1][n+1];
for (int i = 0; i <= m; i++) dp[i][0] = i;
for (int j = 0; j <= n; j++) dp[0][j] = j;
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        dp[i][j] = (s.charAt(i-1) == t.charAt(j-1)) ? dp[i-1][j-1]
                   : 1 + Math.min(dp[i-1][j-1], Math.min(dp[i-1][j], dp[i][j-1]));

// LCS
int[][] dp = new int[m+1][n+1];
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        dp[i][j] = (s.charAt(i-1) == t.charAt(j-1)) ? dp[i-1][j-1] + 1
                   : Math.max(dp[i-1][j], dp[i][j-1]);

// ===== INTERVAL DP =====
// Burst Balloons / Matrix Chain
int[][] dp = new int[n][n];
for (int len = 2; len <= n; len++)
    for (int i = 0; i + len - 1 < n; i++) {
        int j = i + len - 1;
        for (int k = i; k < j; k++)
            dp[i][j] = Math.max(dp[i][j], dp[i][k] + dp[k+1][j] + cost(i,k,j));
    }

// ===== BITMASK DP (TSP / Assignment) =====
int[][] dp = new int[1<<n][n];
for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE/2);
dp[1][0] = 0;  // start at node 0
for (int mask = 1; mask < (1<<n); mask++)
    for (int u = 0; u < n; u++) {
        if ((mask >> u & 1) == 0) continue;
        for (int v = 0; v < n; v++) {
            if ((mask >> v & 1) == 1) continue;
            int nmask = mask | (1<<v);
            dp[nmask][v] = Math.min(dp[nmask][v], dp[mask][u] + dist[u][v]);
        }
    }

// ===== DIGIT DP =====
// Count numbers in [0, n] satisfying some property
// State: (position, tight, sum, ...) — memoize with @cache / HashMap
// See Digit DP section for full template
```

### Digit DP Template

```java
// Count numbers from 1 to N with digit sum divisible by k
String N_str;
int K;
Integer[][] memo;

int digitDP(int pos, int sum, boolean tight) {
    if (pos == N_str.length()) return sum % K == 0 ? 1 : 0;
    if (!tight && memo[pos][sum] != null) return memo[pos][sum];
    int limit = tight ? (N_str.charAt(pos) - '0') : 9;
    int res = 0;
    for (int d = 0; d <= limit; d++)
        res += digitDP(pos+1, (sum+d)%K, tight && d==limit);
    if (!tight) memo[pos][sum] = res;
    return res;
}
// Call: Arrays.fill(memo, null); digitDP(0, 0, true);
```

---

## 22. Greedy Patterns

```java
// Activity selection (maximum non-overlapping intervals)
Arrays.sort(intervals, Comparator.comparingInt(a -> a[1]));  // sort by end time
int count = 0, end = Integer.MIN_VALUE;
for (int[] interval : intervals) {
    if (interval[0] >= end) { count++; end = interval[1]; }
}

// Minimum platforms / meeting rooms
int[] starts = ..., ends = ...;
Arrays.sort(starts); Arrays.sort(ends);
int rooms = 0, maxRooms = 0, j = 0;
for (int i = 0; i < n; i++) {
    rooms++;
    if (starts[i] >= ends[j]) { rooms--; j++; }
    maxRooms = Math.max(maxRooms, rooms);
}

// Huffman encoding / minimum cost to join ropes — greedy with min-heap
PriorityQueue<Long> pq = new PriorityQueue<>();
for (int r : ropes) pq.offer((long)r);
long cost = 0;
while (pq.size() > 1) {
    long a = pq.poll(), b = pq.poll();
    cost += a + b; pq.offer(a + b);
}

// Jump Game II — minimum jumps
int jumps = 0, curEnd = 0, farthest = 0;
for (int i = 0; i < n-1; i++) {
    farthest = Math.max(farthest, i + nums[i]);
    if (i == curEnd) { jumps++; curEnd = farthest; }
}

// Task Scheduler — greedy
int[] freq = new int[26];
for (char c : tasks) freq[c-'A']++;
Arrays.sort(freq);
int maxFreq = freq[25];
int slots = (maxFreq - 1) * (n + 1) + 1;
for (int i = 24; i >= 0 && freq[i] == maxFreq; i--) slots++;
return Math.max(slots, tasks.length);
```

---

## 23. Backtracking Templates

```java
// Subsets
void subsets(int[] nums, int start, List<Integer> curr, List<List<Integer>> result) {
    result.add(new ArrayList<>(curr));
    for (int i = start; i < nums.length; i++) {
        curr.add(nums[i]);
        subsets(nums, i+1, curr, result);
        curr.remove(curr.size()-1);    // backtrack
    }
}

// Permutations
void permute(int[] nums, boolean[] used, List<Integer> curr, List<List<Integer>> result) {
    if (curr.size() == nums.length) { result.add(new ArrayList<>(curr)); return; }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        if (i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue;  // skip duplicates
        used[i] = true;
        curr.add(nums[i]);
        permute(nums, used, curr, result);
        curr.remove(curr.size()-1);
        used[i] = false;
    }
}

// Combinations that sum to target
void combine(int[] candidates, int target, int start, List<Integer> curr, List<List<Integer>> result) {
    if (target == 0) { result.add(new ArrayList<>(curr)); return; }
    for (int i = start; i < candidates.length; i++) {
        if (candidates[i] > target) break;       // pruning (array must be sorted)
        if (i > start && candidates[i] == candidates[i-1]) continue;  // skip duplicates
        curr.add(candidates[i]);
        combine(candidates, target - candidates[i], i+1, curr, result);
        curr.remove(curr.size()-1);
    }
}

// N-Queens
void solveNQueens(int n, int row, int[] queens, List<List<String>> result) {
    if (row == n) { result.add(buildBoard(queens, n)); return; }
    for (int col = 0; col < n; col++) {
        boolean valid = true;
        for (int r = 0; r < row; r++) {
            if (queens[r] == col || Math.abs(queens[r]-col) == Math.abs(r-row)) { valid = false; break; }
        }
        if (valid) {
            queens[row] = col;
            solveNQueens(n, row+1, queens, result);
        }
    }
}

// Sudoku Solver
boolean solveSudoku(char[][] board) {
    for (int i = 0; i < 9; i++)
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.') continue;
            for (char c = '1'; c <= '9'; c++) {
                if (isValid(board, i, j, c)) {
                    board[i][j] = c;
                    if (solveSudoku(board)) return true;
                    board[i][j] = '.';  // backtrack
                }
            }
            return false;  // no valid digit found
        }
    return true;
}
```

---

## 24. Segment Tree & BIT (Fenwick Tree)

### Iterative Segment Tree

```java
class SegTree {
    int n;
    long[] tree;

    SegTree(int n) {
        this.n = n;
        tree = new long[2 * n];
    }

    void update(int i, long val) {
        tree[i += n] = val;
        for (i >>= 1; i >= 1; i >>= 1)
            tree[i] = tree[2*i] + tree[2*i+1];  // change + for min/max
    }

    long query(int l, int r) {  // [l, r) exclusive
        long res = 0;            // identity: 0 for sum, Long.MAX_VALUE for min
        for (l += n, r += n; l < r; l >>= 1, r >>= 1) {
            if ((l & 1) == 1) res += tree[l++];
            if ((r & 1) == 1) res += tree[--r];
        }
        return res;
    }
}
// For min: res = Long.MAX_VALUE, use Math.min; For max: res = Long.MIN_VALUE, use Math.max

// Lazy Propagation Segment Tree (range update, range query)
class LazySegTree {
    int n;
    long[] tree, lazy;
    LazySegTree(int n) { this.n = n; tree = new long[4*n]; lazy = new long[4*n]; }

    void pushDown(int node, int l, int r) {
        if (lazy[node] != 0) {
            int mid = (l+r)/2;
            tree[2*node]   += lazy[node] * (mid-l+1);
            tree[2*node+1] += lazy[node] * (r-mid);
            lazy[2*node]   += lazy[node];
            lazy[2*node+1] += lazy[node];
            lazy[node] = 0;
        }
    }

    void update(int node, int l, int r, int ql, int qr, long val) {
        if (ql > r || qr < l) return;
        if (ql <= l && r <= qr) { tree[node] += val*(r-l+1); lazy[node] += val; return; }
        pushDown(node, l, r);
        int mid = (l+r)/2;
        update(2*node, l, mid, ql, qr, val);
        update(2*node+1, mid+1, r, ql, qr, val);
        tree[node] = tree[2*node] + tree[2*node+1];
    }

    long query(int node, int l, int r, int ql, int qr) {
        if (ql > r || qr < l) return 0;
        if (ql <= l && r <= qr) return tree[node];
        pushDown(node, l, r);
        int mid = (l+r)/2;
        return query(2*node, l, mid, ql, qr) + query(2*node+1, mid+1, r, ql, qr);
    }
}
```

### BIT (Fenwick Tree)

```java
class BIT {
    int n;
    long[] tree;

    BIT(int n) { this.n = n; tree = new long[n+1]; }

    void update(int i, long delta) {    // 1-indexed
        for (; i <= n; i += i & (-i)) tree[i] += delta;
    }

    long query(int i) {                 // prefix sum [1..i]
        long sum = 0;
        for (; i > 0; i -= i & (-i)) sum += tree[i];
        return sum;
    }

    long query(int l, int r) { return query(r) - query(l-1); }

    // Point query after range updates (use difference BIT)
    // Range update [l,r] += val: update(l, val); update(r+1, -val)
    // Point query at i: query(i)
}

// 2D BIT
class BIT2D {
    int n, m;
    long[][] tree;
    BIT2D(int n, int m) { this.n = n; this.m = m; tree = new long[n+1][m+1]; }

    void update(int x, int y, long v) {
        for (int i = x; i <= n; i += i&(-i))
            for (int j = y; j <= m; j += j&(-j)) tree[i][j] += v;
    }

    long query(int x, int y) {
        long s = 0;
        for (int i = x; i > 0; i -= i&(-i))
            for (int j = y; j > 0; j -= j&(-j)) s += tree[i][j];
        return s;
    }
}
```

---

## 25. Trie

```java
class Trie {
    TrieNode root = new TrieNode();

    static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEnd;
        int count;   // number of words passing through this node (for prefix count)
    }

    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) node.children[idx] = new TrieNode();
            node = node.children[idx];
            node.count++;
        }
        node.isEnd = true;
    }

    boolean search(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return false;
            node = node.children[idx];
        }
        return node.isEnd;
    }

    boolean startsWith(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return false;
            node = node.children[idx];
        }
        return true;
    }

    int countPrefix(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return 0;
            node = node.children[idx];
        }
        return node.count;
    }
}

// XOR Trie (maximum XOR of two numbers)
class XORTrie {
    int[][] trie = new int[32 * 100001][2];
    int cnt = 1;

    void insert(int x) {
        int node = 0;
        for (int i = 30; i >= 0; i--) {
            int bit = (x >> i) & 1;
            if (trie[node][bit] == 0) trie[node][bit] = cnt++;
            node = trie[node][bit];
        }
    }

    int maxXOR(int x) {
        int node = 0, result = 0;
        for (int i = 30; i >= 0; i--) {
            int bit = (x >> i) & 1, want = 1 - bit;
            if (trie[node][want] != 0) { result |= (1 << i); node = trie[node][want]; }
            else node = trie[node][bit];
        }
        return result;
    }
}
```

---

## 26. String Algorithms

### KMP — O(n+m) pattern matching

```java
// Build failure function
int[] buildKMP(String pattern) {
    int m = pattern.length();
    int[] fail = new int[m];
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) j = fail[j-1];
        if (pattern.charAt(i) == pattern.charAt(j)) j++;
        fail[i] = j;
    }
    return fail;
}

// Search: returns all starting indices of pattern in text
List<Integer> kmpSearch(String text, String pattern) {
    int[] fail = buildKMP(pattern);
    List<Integer> result = new ArrayList<>();
    int n = text.length(), m = pattern.length();
    for (int i = 0, j = 0; i < n; i++) {
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) j = fail[j-1];
        if (text.charAt(i) == pattern.charAt(j)) j++;
        if (j == m) { result.add(i - m + 1); j = fail[j-1]; }
    }
    return result;
}
```

### Z-Algorithm — O(n)

```java
int[] zFunction(String s) {
    int n = s.length();
    int[] z = new int[n];
    z[0] = n;
    int l = 0, r = 0;
    for (int i = 1; i < n; i++) {
        if (i < r) z[i] = Math.min(r - i, z[i - l]);
        while (i + z[i] < n && s.charAt(z[i]) == s.charAt(i + z[i])) z[i]++;
        if (i + z[i] > r) { l = i; r = i + z[i]; }
    }
    return z;
}
```

### Manacher's Algorithm — O(n) longest palindromic substring

```java
int longestPalindrome(String s) {
    String t = "#" + String.join("#", s.split("")) + "#";
    int n = t.length();
    int[] p = new int[n];
    int c = 0, r = 0, maxLen = 0;
    for (int i = 0; i < n; i++) {
        if (i < r) p[i] = Math.min(r - i, p[2 * c - i]);
        while (i - p[i] - 1 >= 0 && i + p[i] + 1 < n && t.charAt(i-p[i]-1) == t.charAt(i+p[i]+1)) p[i]++;
        if (i + p[i] > r) { c = i; r = i + p[i]; }
        maxLen = Math.max(maxLen, p[i]);
    }
    return maxLen;
}
```

---

## 27. Geometry Basics

```java
// Cross product of vectors OA and OB
long cross(long[] O, long[] A, long[] B) {
    return (A[0]-O[0])*(B[1]-O[1]) - (A[1]-O[1])*(B[0]-O[0]);
}
// cross > 0: counter-clockwise, < 0: clockwise, == 0: collinear

// Distance squared (avoid floating point)
long dist2(long[] A, long[] B) {
    return (A[0]-B[0])*(A[0]-B[0]) + (A[1]-B[1])*(A[1]-B[1]);
}

// Convex Hull — Graham Scan O(n log n)
long[][] convexHull(long[][] points) {
    int n = points.length;
    Arrays.sort(points, (a,b) -> a[0] != b[0] ? Long.compare(a[0],b[0]) : Long.compare(a[1],b[1]));
    long[][] hull = new long[2*n][2];
    int k = 0;
    // Build lower hull
    for (int i = 0; i < n; i++) {
        while (k >= 2 && cross(hull[k-2], hull[k-1], points[i]) <= 0) k--;
        hull[k++] = points[i];
    }
    // Build upper hull
    for (int i = n-2, t = k+1; i >= 0; i--) {
        while (k >= t && cross(hull[k-2], hull[k-1], points[i]) <= 0) k--;
        hull[k++] = points[i];
    }
    return Arrays.copyOf(hull, k-1);
}

// Point in convex polygon — O(log n)
boolean pointInConvex(long[][] polygon, long[] p) {
    int n = polygon.length;
    if (cross(polygon[0], polygon[1], p) < 0 || cross(polygon[0], polygon[n-1], p) > 0) return false;
    int lo = 1, hi = n-1;
    while (hi - lo > 1) {
        int mid = (lo+hi)/2;
        if (cross(polygon[0], polygon[mid], p) >= 0) lo = mid; else hi = mid;
    }
    return cross(polygon[lo], polygon[hi], p) >= 0;
}
```

---

## 28. Interview Patterns Cheatsheet

### Problem → Pattern Mapping

| Problem Signal | Pattern to Use |
|---|---|
| "Kth largest/smallest" | Min-heap of size k |
| "Top K frequent" | Min-heap + HashMap |
| "Subarray sum = k" | Prefix sum + HashMap |
| "Longest substring with constraint" | Sliding window |
| "Sorted array, find pair/triplet" | Two pointers |
| "Find duplicate / missing" | XOR or math |
| "Matrix path count" | 2D DP |
| "Parentheses valid/generate" | Stack / Backtracking |
| "Merge intervals" | Sort + greedy |
| "Word search in grid" | DFS + backtrack |
| "Shortest path unweighted" | BFS |
| "Shortest path weighted" | Dijkstra |
| "All paths / combinations" | Backtracking |
| "Optimal substructure" | DP |
| "Greedy choice works" | Greedy (prove it!) |
| "Range query on array" | Segment Tree / BIT |
| "Find pattern in string" | KMP / Z-algo / Trie |
| "Connected components" | DSU / BFS/DFS |
| "Schedule with constraints" | Topological sort |
| "Max XOR" | XOR Trie |
| "Count inversions" | Merge sort / BIT |
| "Palindromes" | Manacher / DP |
| "Sliding window max" | Monotonic deque |
| "Next greater element" | Monotonic stack |
| "Numbers in range with property" | Digit DP |

### Common Interview Problem Templates

```java
// ===== TWO SUM (all variants) =====
// Array: sort + two pointers, or HashMap O(n)
// Sorted: two pointers
// K-sum: sort + recursion + two pointers

// ===== MERGE INTERVALS =====
Arrays.sort(intervals, (a,b) -> a[0]-b[0]);
List<int[]> merged = new ArrayList<>();
for (int[] iv : intervals) {
    if (merged.isEmpty() || merged.get(merged.size()-1)[1] < iv[0])
        merged.add(iv);
    else
        merged.get(merged.size()-1)[1] = Math.max(merged.get(merged.size()-1)[1], iv[1]);
}

// ===== SUBARRAY SUM = k =====
Map<Long,Integer> prefixCount = new HashMap<>();
prefixCount.put(0L, 1);
long sum = 0; int ans = 0;
for (int x : arr) {
    sum += x;
    ans += prefixCount.getOrDefault(sum - k, 0);
    prefixCount.merge(sum, 1, Integer::sum);
}

// ===== CYCLE DETECTION in graph =====
// Undirected: DSU or DFS with parent tracking
// Directed: DFS with 3-color marking (0=unvisited, 1=in-stack, 2=done)

// ===== WORD LADDER (BFS on implicit graph) =====
// State = current word; neighbors = all words differing by 1 char

// ===== CLONE GRAPH =====
Map<Node,Node> visited = new HashMap<>();
Node clone(Node node) {
    if (node == null) return null;
    if (visited.containsKey(node)) return visited.get(node);
    Node copy = new Node(node.val);
    visited.put(node, copy);
    for (Node nb : node.neighbors) copy.neighbors.add(clone(nb));
    return copy;
}
```

---

## 29. Complexity Quick Reference

### Time Complexities

| Algorithm / Operation | Time |
|---|---|
| Array access | O(1) |
| HashMap get/put | O(1) avg |
| TreeMap/TreeSet get/put | O(log n) |
| PriorityQueue offer/poll | O(log n) |
| Sorting (Timsort) | O(n log n) |
| Binary search | O(log n) |
| BFS / DFS | O(V + E) |
| Dijkstra | O((V+E) log V) |
| Bellman-Ford | O(VE) |
| Floyd-Warshall | O(V³) |
| Kruskal MST | O(E log E) |
| Prim MST | O(E log V) |
| Segment tree update/query | O(log n) |
| BIT update/query | O(log n) |
| KMP / Z-algo | O(n + m) |
| Manacher | O(n) |
| Sieve of Eratosthenes | O(n log log n) |
| Modular exponentiation | O(log n) |
| Bitmask DP (TSP) | O(2ⁿ · n²) |
| DSU find/union | O(α(n)) ≈ O(1) |
| LIS (binary search) | O(n log n) |
| Convex Hull | O(n log n) |

### Space Complexities

| Structure | Space |
|---|---|
| 1D DP | O(n) or O(1) with rolling |
| 2D DP | O(nm) or O(min(n,m)) |
| Graph | O(V + E) |
| Segment Tree | O(4n) |
| Trie | O(ALPHABET_SIZE · n) |
| Recursion depth | O(depth) stack |

### Input Size → Expected Complexity

| n (input size) | Target complexity |
|---|---|
| ≤ 12 | O(2ⁿ · n) — bitmask DP |
| ≤ 20 | O(2ⁿ) — bitmask / meet-in-middle |
| ≤ 100 | O(n³) — Floyd, DP O(n³) |
| ≤ 1,000 | O(n²) — nested loops, O(n² log n) |
| ≤ 10,000 | O(n² ) — careful nested loops |
| ≤ 10⁵ | O(n log n) — sorting, trees |
| ≤ 10⁶ | O(n) or O(n log n) |
| ≤ 10⁸ | O(n) — single pass |
| ≤ 10¹⁸ | O(log n) or O(√n) — math |

---

## 30. Contest Checklist

```
PRE-CONTEST
  □  Template ready: BufferedReader + PrintWriter + helper methods
  □  MOD = 1_000_000_007L declared
  □  DSU template ready
  □  SegTree / BIT template ready

READING THE PROBLEM
  □  Note constraints — determines algorithm choice (see table above)
  □  Note if answer needs mod
  □  Note if long needed (n > ~46000 can overflow int with n²)
  □  Read examples carefully — edge cases often hidden there

CODING
  □  int vs long — when n²,  n·val, or distances can exceed 2×10⁹ → use long
  □  Comparator: use Integer.compare(a,b) not a-b (overflow!)
  □  2D array init: new int[n][m], then Arrays.fill each row
  □  Graph: initialize all adjacency lists before adding edges
  □  mod arithmetic: ((a % MOD) + MOD) % MOD for negative values
  □  powMod() instead of Math.pow() for integer exponentiation

DEBUGGING
  □  Off-by-one in binary search: check lo/hi/mid carefully
  □  BFS visited check: mark visited BEFORE adding to queue (not after poll)
  □  DP base cases: are all base cases initialized?
  □  Segment tree: is query range [l, r) or [l, r]?
  □  Overflow: trace through with max input values

COMMON MISTAKES
  □  Using Stack<T>  → Use ArrayDeque<T>
  □  Using LinkedList as queue → Use ArrayDeque<T>
  □  Integer overflow in comparator (a - b)
  □  Forgetting out.flush() at end
  □  Not handling disconnected graph (multiple components)
  □  Not resetting global/static state between test cases
  □  TreeMap.get() on missing key returns null (NullPointerException)
  □  ConcurrentModificationException: don't modify collection while iterating
```

---

*Compiled from 20 years of competitive programming at Grandmaster level.*  
*"The best code is correct code — optimize only after proving correctness."*
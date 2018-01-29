### Introduction

why study algorithms?
- impact is broad and far-reaching
- old roots, new opportunities
- solve problems that could not otherwise be addressed
- intellectual stimulation
- become a proficient programmer
- common language for understanding nature
    * mathematical model v.s.
    * computational model
- for fun and profit

quadratic algorithms don't scale

### union find

Union: to merge components containing q and q, set the id of p root to id of q root

weighted quick union with path compression makes it possible
to solve problems that could not otherwise be addressed.

ex. 10^9 unions and finds with 10^9 objects:  
* WQUPC reduces time from 30 years to 6 seconds
* supercomputer wont help much; good algorithm enables solution

in weighted quick-union (by size), we make the root of the smaller tree
(in terms of number of nodes) point to the root of the larger tree.

percolation threshold is about 0.592746 for large square lattices.  
constant known only via stimulation.  
==> fast algorithm enables accurate answer to scientific question.

### Analysis of Algorithms

running time:
- costs: depend on machine, compiler
- frequencies: depend on algorithm, input

linearithmic == N log N  
divide and conquer

need linear or linearithmic alg to keep pace with Moore's Law.

sorting-based algorithm:
- sort N (distinct) numbers
    * N^2
- for each pair of numbers a[i] and a[j], binary search for -(a[i] + a[j])
    * N^2 log N
- order of growth: N^2 log N

best case: lower bound on cost
* "easiest" input
* provides a goal for all inputs

worst case: upper bound on cost
* "most difficult" input
* provides a guarantee for all inputs

average case: "expected" cost
* "random" input
* provides a way to predict performance

goals:
* establish "difficulty" of a problem
* develop "optimal" algorithms

1 million ~ 2^20
1 billion ~ 2^30

memory usage for objects in java:
* object overhead: 16 bytes
* reference: 8 bytes
* padding: each object uses a multiple of 8 bytes

mathematical model is independent of a particular system;
applies to machines not yet built.
empirical analysis is necessary to validate mathematical models
and to make predictions.

## Stacks and Queues

* stack: examine the item most recently added.
* queue: examine the item least recently added.

Design: creates modular, reusable libraries.  
performance: use optimized implementation where it matters.

* client: program using operations defined in interface
* implementation: actual code implementing operations
* interface: description of data type, basic operations

### stack: linked-list implementation

Maintain pointer to first node in a linked list;
insert/remove both from front.

```java
public class LinkedStackOfStrings {
    private Node first = null;

    private class Node {
        String item;
        Node next;
    }

    public boolean isEmpty() {
        return first == null;
    }

    public void push(String item) {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
    }

    public String pop() {
        String item = first.item;
        first = first.next;
        return item;
    }
}
```

### stack: array implementation
```java
public class FixedCapacityStackOfStrings {
    private String[] s;
    private int N = 0;

    // require client to provide capacity!!
    // does not implement API!
    public FixedCapacityStackOfStrings(int capacity) {
        s = new String[capacity];
    }

    public boolean isEmpty() {
        return N == 0;
    }

    public void push(Stirng item) {
        // use to index into array; then increment N
        s[N++] = item;
    }

    public String pop() {
        // decrement N; then use to index into array
        return s[--N];
    }
}
```

* underflow: throw exception if pop from an empty stack
* overflow: use resizing array for array implementation

#### Loitering
Holding a reference to an object when it is no longer needed.

Garbage collector can reclaim memory only if no outstanding references

```java
// this version avoids loitering
public String pop() {
    String item = s[--N];
    s[N] = null;
    return item;
}

```

#### Delete last node in linked list  
```java
Node x = first;
// upon termination of the loop, x is a reference to the 2nd to last node.
while (x.next.next != null) {
    x = x.next
}
// final statement deletes the last node
x.next = null
```

### Resizing Arrays

too expensive: inserting first N items takes time ~ N^2/2

challenge: ensure that array resizing happens infrequently.  
==> repeated doubling:  
if array is full, create a new array twice of the size, then copy items.  
inserting first N items takes time ~N not N^2.

```java
public ResizingArrayStackOfStrings() {
    s = new String[1];
}

public void push(String item) {
    if (N == s.length) resize(2 * s.length);
    s[N++] = item;
}

private void resize(int capacity){
    String[] copy = new String[capacity];
    for (int i = 0; i < N; i++)
        copy[i] = s[i];
    s = copy;
}
```
amortized cost of inserting first N items:  
N + (2 + 4 + 8 + ... + N) = ~3N

* push(): double size of array s[] when array is full.
* pop(): halve size of array when array is `one-quarter full`.
```java
public String pop() {
    String item = s[--N];
    s[N] = null;
    if (N > 0 && N == s.length/4) resize(s.length/2);
    return item;
}
```

invariant: array is always between 25% and 100% full.

### Tradeoffs
linked-list implementation
* every operation takes constant time in the `worst case`
* use extra time and space to deal with links

resizing-array implementation
* every operation takes constant `amortized` time
* less wasted space

the `resize()` method is called only when the size of stack is power of 2.
there are ~log N powers of 2 between 1 and N.

## Queue

### APIs

```
QueueOfStrings()    create an empty queue
void enqueue(String item)   insert a new string onto queue
String dequeue()    remove and return the string least recently added
boolean isEmpty()   is queue empty?
```

### queue: linked-list implementation

Maintain pointer to first and last nodes in a linked list;
insert at end of linked list;
remove from front of linked list.

```java
public class LinkedQueueOfStrings {
    private Node first, last;

    private class Node {
        String item;
        Node next;
    }

    public boolean isEmpty() {
        return first == null;
    }

    public void enqueue(String item) {
        Node oldlast = last;
        last = new Node();
        last.item = item;
        last.next = null;
        if (isEmpty()) // special case for empty queue
            first = last;
        else
            oldlast.next = last;
    }

    public String dequeue() {
        String item = first.item;
        first = first.next;
        if (isEmpty()) last = null; //// special case for empty queue
        return item;
    }
}

```

## Parameterized stack
Java generics
* avoid casting in client
* discover type mismatch errors at compile-time instead of run-time

```java
Stack<Apple> s = new Stack<Apple>();
Apple a = new Apple();
Orange b = new Orange();
s.push(a);
s.push(b);  //compile-time error
a = s.pop();
```

Guiding principles: welcome compile-time errors; avoid run-time errors.

### generic stack: linked-list implementation

```java
public class Stack<Item> {
    private Node first = null;

    private class Node {
        Item item;
        Node next;
    }

    public boolean isEmpty() {
        return first == null;
    }

    public void push(Item item) {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
    }

    public Item pop() {
        Item item = first.item;
        first = first.next;
        return item;
    }
}
```

good code has zero cast.

### autoboxing

automatic cast between a primitive type and its wrapper.

wrapper type:
* each primitive type has a wrapper object type:
* ex: `Integer` is a wrapper type for int

Syntactic sugar: behind-the-scene casting.
```java
Stack<Integer> s = new Stack<Integer>();
s.push(17);         // s.push(new Integer(17));
int a = s.pop();    // int a = s.pop().intValue();
```

in java 6 you must specify the concrete type both in the variable declaration
(left hand side), and the constructor call (right hand side).
```java
Stack<Integer> stack = new Stack<Integer>();
```

starting in java 7, you can use the diamond operator instead:
```java
Stack<Integer> stack = new Stack<>();
```

### iterators

#### bag API

adding items to a collection and iterate (order does not matter).

```java
public class Bag<Item> implements Iterable<Item>

Bag()               creates an empty bag
void add(Item x)    insert a new item onto bag
int size()          number of items in bag
Iterable<Item> iterator()   iterator for all items in bag
```

### Function calls

how a compiler implements a function call:
* function call: `push` local environment and return address
* return: `pop` return address and local environment

recursive function: function that calls itself.  
can always use an explicit stack to remove recursion.

### arithmetic expression evaluation: Dijkstra two-stack algorithm

value: push on to the value stack  
operator: push onto the operator stack  
left parenthesis: ignore  
right parenthesis: pop operator and two values; push the result of applying
that operator to those values onto the operand stack.

## Elementary Sorts

callback = reference to executable code  

sort implementation has no dependence on the type of data.

v.compareTo(w)  
* less than: return -1
* equal to: return 0
* greater than: return +1

implement the `Comparable` interface  

Selection sort uses ~1/2 N^2 compares and N exchanges.
quadratic time even if input is sorted.  
data movement is minimal. linear number of exchanges.

Insertion sort uses ~ 1/4 N^2 compares and  ~ 1/4 N^2 exchanges on average.

best case: if array is in ascending order, insertion sort N-1 compares and 0 exchanges.  
pf.: each item (except the first) is compared against the item to its left (and no other items).
total N-1 compares.

worst case: descending order and no duplicates. ~ 1/2 N^2 compares and  ~ 1/2 N^2 exchanges

An `inversion` is a pair of keys that are out of order.  
array is `partially sorted` if the number of inversions is linear <= cN.

for partially sorted arrays, insertion sort runs in linear time.  
pf.:  
    # exchanges == # inversions
    # compares = exchanges + (N - 1)

a g-sorted array remains g-sorted after h-sorting it.

### Shellsort

Shellsort increment sequence: 3x + 1

worst case shellsort with 3x+1 increments is ~N^(3/2)  
accurate model has not yet been discovered!

tiny code  
==> hardware sort prototype in embedded systems.

open problem: best increment sequence. 50 years.

how many compares does shellsort make on an already sorted array?  

since successive increment values of h differ by at least a factor of 3,
there are ~ log_3 N increment values.
for each increment value h, the array is already h-sorted,
so it will make ~ N compares.

### Shuffle

#### Knuth Shuffle
* in iteration i, pick integer r between 0 and i uniformly at random.
* swap a[i] and a[r]
* Knuth shuffling algorithm produces a uniformly random permutation of the input array in linear time .
* common bug: between 0 and N-1, should be 0 and i. not uniform.

use a hardware random number generator

#### Graham scan
convex hull: the smallest perimeter fence enclosing a set of N points.

* choose p with smallest y coordinate
* sort pints by polar angle with p  
* consider points in order; discard unless it creates a ccw turn

sort N logN, rest linear.  

## Mergesort

any compare-based sorting algorithm must make at least ∼nlg⁡n compares in the worst case.

* divide array into two halves
* recursively sort each half.
* merge two halves

assertion: statement to test assumptions about your program.

Mergesort uses at most `N lg N` compares and 6 `N lg N` array access
to sort any array of size N.

Mergesort uses extra space proportional to N.  
aux[] for last merge.

a sorting algorithm is `in-place` if it uses <= `c logN` extra memory.  
e.g. insertion sort, selection sort, shell sort.

use insertion sort for small subarrays.
Mergesort has too much overhead for tiny subarrays.
cutoff to insertion sort for ~ 7 items.

Bottom-up Mergesort

### sorting complexity

lower bound:  
any compare-based sorting algorithm must use at least `log(N!)` ~ `N lg N`
compares in the worse case.  

model of computation: decision tree
cost model: # compares    
optimal algorithm == mergesort  

first goal of algorithm Design: optimal algorithms

mergesort is optimal wrt # compares;  
mergesort is not optimal wrt space usage.

lower bound may not hold if has info about:
* the initial order of the input  
    - partially sorted arrays:
    - insertion sort only N-1 compares if already sorted.
* the distribution of key values
    - duplicate keys: 3-way quicksort
* the representation of keys  
    - use digit/character compares instead of key compares for numbers and strings

`Comparator` supports multiple ordering of a given data type.

### Stability
a `stable` sort preserves the relative order of items with equal keys.

Insertion sort and mergesort are stable, but not selection sort or shellsort.

long-distance exchange might move an item past some equal item.

## Quicksort  
* Shuffle the array
* partition so that, for some j
    - entry a[j] is in place
    - no larger entry to the left of j
    - no smaller entry to the right of j
* sort each piece recursively

shuffling is needed for performance guarantee.  
probabilistic guarantee against worst case.  

Quicksort is faster than mergesort.

worst case # compares is `1/2 N^2`. when it is sorted already.

average # compares to an array of N distinct keys is ~ `2NlnN`.
~ `1.39 N log N`.  
* 39% more compares than mergesort  
* but faster than mergesort in practice because of less data movement.

quicksort is an in-place sorting algorithm.  
quicksort is NOT stable.

cutoff to insertion sort for ~ 10 items.  
best choice of pivot item == median.  
estimate median by taking median sample  

### Quick selection
Given an array of N items, find the kth largest.  
e.g. min(k=0), max(k=N-1), median(k=N/2)

quick-select takes linear time on average. `(2 + 2 ln 2)N`.  
worst case ~ `1/2 N^2` compares. ==> random shuffle  

use quick select when you don't need a full sort.  

### duplicate keys

quicksort goes quadratic unless partitioning stops on equal keys.

#### Dijkstra 3-way partitioning  

randomized quicksort with 3-way partitioning reduces running time
from linearithmic to linear in broad class of applications.

### System sorts

`Arrays.sort()`  
uses tuned quicksort for primitive types;  
tuned mergesort for objects.  
uses mergesort for reference types.  
for stability and `nlogn` guaranteed performance.

Tukey's ninther  
median of the median of 3 samples, each of 3 elements.  
better partitioning than random shuffle and less cost.

java System sort crashes due to overflows function call stack.  
not shuffle!!

Quicksort is in-place and typically the fastest general-purpose
sorting algorithm in practice.

## Priority Queues

Collections: insert and delete items. diffs which item to delete.

stack: remove the item most recently added.  
queue: remove the item least recently added.  
randomized queue: remove a random item.  
priority queue: remove the largest (or smallest) item.  

### APIs
```java
public class MaxPQ<Key extends Comparable<Key>> // keys must be Comparable
        MaxPQ() // creates an empty priority queue
    void insert (Key v) // insert a key to the priority queue
    key delMax() // return and remove the largest key
    boolean isEmpty()
```

applications
* event-driven stimulation
    - customer in a line, colliding particles
* statistics
    - maintain larges M values in a sequence
* operating systems
    - load balancing, interrupt handling
* bayesian spam filtering

Find the largest M items in a stream of N items.  
N huge. M large.   
not enought memory to store N  items.

sort uses space N, so out of the question.  
binary heap: time N log M, space M.  
theory: time N, space M.

Binary tree: empty or node with links to left and right binary trees.  
complete tree: perfectly balanced, except for the bottom level.

height of compete tree is `log N`.

### binary heap
array representation of a heap-ordered binary tree.

heap-ordered binary tree:
* key in nodes
* parents key no smaller than children keys.

array representation
* indices start at 1
* take nodes in level order

largest key is a[1], which is root of binary tree.

can use array indices to move through tree:
* parent of node at k is at k/2.
* children of node at k are at 2k and 2k+1.

reverse-sorted array is always a max heap.
array of all equal keys is both a max- and min-oriented heap.

promotion in a heap
* exchange key in child with key in parent.
* repeat until heap order restored.
```java
private void swim(int k) {
    while (k > 1 && less(k/2, k)) {
        exch(k, k/2); // parent of node at k is at k/2
        k = k/2;
    }
}
```

insert: add node at end, then swim it up.
cost: at most `1 + lg N` compares.
```java
public void insert(Key x) {
    pq[++N] = x;    // increment N first, then put x there.
    swim(N);
}
```


demotion in a heap
* exchange key in parent with key in `larger` child.
* repeat until heap order restored.
```java
private void sink(int k) {
    while (2*k <= N) {
        int j = 2*k;
        // children of node at k are at 2k and 2k+1
        if (j < N && less(j, j+1))  j++; // j is now larger of the two!
        if (!less(k, j))            break;
        exch(k, j);
        k = j;
    }
}
```


delete max in a heap:  
* need do 2 things: return and remove root, decrease heap size by 1.
* ==> exchange root with node at end, then sink it down.
* cost: at most `2 lgN` compares.
```java
public Key delMax() {
    Key max = pq[1];
    exch(1, N--);
    sink(1);
    pq[N+1] = null; // prevent loitering
    return max;
}
```
### Java implementation
```java
public class MaxPQ<Key extends Comparable<Key>>
{
    private Key[] pq;
    private int N;

    public MaxPQ(int capacity)
    { pq = (Key[]) new Comparable[capacity + 1]} //index start at 1

    public boolean isEmpty()
    { return N == 0; }

    public void insert(Key key);
    public Key delMax();

    // heap helper functions
    private void swim(int k);
    private void sink(int k);

    // array help functions
    private boolean less(int i, int j)
    { return pq[i].compareTo(pq[j]) }

    private void exch(int i, int j)
    { Key t = pq[i]; pq[i] = pq[j]; pq[j] = t; }
}
```

data type: set of values and operations on those values  
best practice: use immutable keys.  
immutable data type: can't change the data type values once created.

immutable: String, Integer, Double, Vector  
mutable: Stack, Counter, Java array  

advantage:  
safe to use as key in priority queue or symbol table.

disadvantage:  
must create new object for each data type value

"classes should be immutable unless there's a good reason to make them mutable."

### Heapsort
in-place sort
* create max-heap with all N keys using bottom-up method.
* repeatedly remove the maximum key.
* leave in array, instead of nulling out.

```java
public static void sort(Comparable[] pq)
{
    int N = pq.length;
    for (int k = N/2; k >= 1; k--)
        sink(pq, k, N);
    while (N > 1)
    {
        exch(pq, 1, N);
        sink(pq, 1, --N);
    }
}
```

heap construction uses <= 2N compares and exchanges.  
heapsort uses <= `2N lg N` compares and exchanges.  

in-place sorting algorithm with `N log N` worst-case.

heapsort is optimal for both time and space, but:
* inner loop longer than quicksort  
* makes poor use of cache memory.
* not stable. long distance exchange.

## Symbol Tables

key-value pair abstraction
* insert a value with specific key.
* given a key, search for the corresponding value.  

ex.
- DNS lookup
- file share: name of song --> computer ID
- file system: filename --> location on disk.

### API
associative array abstraction: associate one value with each key.
```java
public class ST<Key, Value>
-----------------------------
    ST()    // create a symbol table
        void put(Key key, Value val)    // put key-value pair into the table
                                        // remove key if val is null
        Value get(Key key)              // value paired with key
        void delete (Key key)       // remove key (and its value)
        boolean contains(Key key)   // is there are value paired with key?
        int size()
        Iterable<Key> keys()    // all the keys in the table
```

conventions:
* values are not null
* get() returns null if key not present
* method put() overwrites old value with new value.

lazy version of delete():
```java
public void delete (Key key)
{ put(key, null) }
```

value type: any generic type.

key type:
* keys are comparable, use `compareTo()`
* keys are any generic type, `use equals()` to test equality.
* keys are any generic type, `use equals()` to test equality;
use `hashCode()` to scramble key.

best practice: use immutable types for symbol table keys.

all Java classes inherit a method `equals()`.

compare each significant field:
- if field is a primitive type, use ==
- if field is an object, use `equals()`
- if field is an array, apply to each entry.  
a.equals(b) tests if they are the same object!

sequential search unordered linked list ST implementation:
* Search hit: N/2
* insert N

binary search in an ordered array of key-value pairs.  
problem: to insert, need to shift all greater keys over.  
search hit: logN  
insert: N/2
good for static situation.  
not good for dynamic.  

### ordered symbol table
```java
public class ST<Key extends Comparable<Key>, Value>
Key floor(Key key)  // largest key less than or equal to key
Key ceiling(Key key)    // smallest key great than or equal to key
int rank(Key key)   // number of keys less than key
Key select(int k)   // key or rank k
Iterable<Key> keys(Key lo, Key hi)  // keys in [lo..hi] in sorted order
```

insert/delete ~ N!

### Binary search tree
enable efficient operations for symbol table.

a BST is a binary tree in symmetric order.

a `binary tree` is either:
* empty.
* two disjoint binary trees (left and right)

`symmetric order`  
each node has a key, and every node key is:
* larger than all keys in its left subtree.
* smaller than all keys in its right subtree.

heap tree parent is larger than all children.  
bst parent is between two children.

java definition: a bst is a reference to a root node.

a node is comprised of 4 fields:
* key and value
* reference to the left and right subtree

```java
// Key and Value are generic types; Key is Comparable.
private class Node
{
    private Key key;
    private Value val;
    private Node left, right;

    public Node(Key key, Value val)
    {
        this.key = key;
        this.val = val;
    }
}

```

| key  |  val  |
|------|-------|
| left | right |

#### BST search java implementation

```java
public Value get(Key key)
{
    Node x = root;
    while (x != null)
    {
        int cmp = key.compareTo(x.key);
        if (cmp < 0)        x = x.left;
        else if (cmp > 0)   x = x.right;
        else    return x.val;
    }
    return null;
}
```

# compares is 1 + depth of tree.

put: associate value with key.  
search for key, then:
* key in tree ==> reset value.
* key not in tree ==> add new node at bottom.

* many BSTs correspond to same set of keys! (depend on how keys come in).
* \# compares for search/insert is 1 + depth of node.

best case: perfectly balanced.  
worst case: keys come in order.

BST correspond with quicksort partitioning 1-1.

N distinct keys into BST random order, expected compares for search/insert
is ~ `2 ln N`.  
expected height of tree is ~ `4.311 ln N`.

subtree counts:  
in each node, we store the number of nodes in the subtree rooted at that node;
to implement size(), return the count at root.

#### Inorder traversal

* traverse left subtree
* enqueue key
* traverse right subtree

```java
public Iterable<Key> keys()
{
    Queue<Key> q = new Queue<Key>();
    inorder(root, q);
    return q;
}

private void inorder(Node x, Queue<key> q)
{
    if (x == null)  return;
    inorder(x.left, q);
    q.enqueue(x.key);
    inorder(x.right, q);
}
```

inorder traversal of a BST yields keys in ascending order.

BST operations `h ~ logN` (height).  
delete ~ `sqrt(N)`!

Hibbard deletion unblances the tree, leading to sqrt(N) height.  
if instead of replacing the node to delete with its succesor, you randomly
replace it with either its successor or predecessor, then, in practice,
the height becomes logarithmic. but nobody has been able to prove this fact
mathematically.

red-black BST guarantee logarithmic performance for all operations.

## Balanced Search Trees

### 2-3 tree
2-node: one key, two children  
3-node: two keys, three children

perfect balance: every path from root to null link has same length.

left link: smaller than 2 keys.  
middle link: between 2 keys.  
right link: larger than 2 keys.

maintains symmetric order and perfect balance.

best case: `lg_2 n`
worst case: `lg_3 N`

the height of 2-3 tree increases only when the root node splits,
and this happends only when every node on the search path
from the root to the leaf where the new key should be inserted is a 3-node.

### Red Black BST

1-1 correspondence with 2-3 tree.

height is <= `2 lgN` in worst case.
* every path from root to null link has same number of black links.
* never two red links in a row.

height is ~ `1.00 lgN` in typical applications.

### B tree

time required for a probe is much larger than time to access data within a page.

generalize 2-3 tree by allowing up to M-1 key-link pairs per node.
* at least 2 key-link pairs at root
* at least M/2 key-link pairs in other nodes.

a search or an insertion in a B tree of order M with N keys requires
between `log_(M-1) N` and `log_(M/2) N` probes.

in practice, number of probes is at most 4.

optimization: always keep root page in memory.

widely used as system symbol table.
* java.util.TreeMap
* c++ STL: map, multimap.
* linux kernel: completely fair scheduler, linx/rbtree.h

widely used for file systems and database:
* NTFS
* HFS
* Linux: ReiserFS, XFS, Ext3FS
* DB2, PostgreSQL

### 2d tree  

recursively partition plane into two halfplanes.

### interval search tree

BST, where each node stores an interval (lo, hi)  
- use left endpoint as bst key.
- store max endpoint in subtree rooted at node

Search for any one interval that intersects query interval (lo, hi):
- if interval in node intersects query interval, return it
- else if left subtree is null, go right
- else if max endpoint in left subtree is less than lo, go right
- else go left  

```java
Node x = root;
while (x != null) {
    if (x.interval.intersects(lo, hi))  return x.interval;
    else if (x.left == null)            x = x.right;
    else if (x.left.max < lo)           x = x.right;
    else                                x = x.left;
}
return null;
```

use red-black bst to guarantee performance.

## Hash Tables

save items in a `key-indexed table` (index is a function of the key).

hash function: method for computing array index from key.  
e.g.: hash("it") = 3.

classic space-time Tradeoff:
* no space limitation ==> trivial hash function with key as index.
* no time limitation ==> trivial collision resolution with sequential search.
* space and time limitations (the real world) ==> hashing

2 requirements for hash function
* efficiently computable
* each table index equally likely for each key.

all java classes inherit a method hashCode(), which returns a 32 bit int.  
if !x.equals(y), then (x.hashCode() != y.hashCode()).  
default implementation: memory address of x.

horner's method to hash string.

bins and balls: throw balls uniformly at random into M bins.  
- birthday problem: expect two balls in the same bin after ~ sqrt(PI M / 2) tosses.  
==> can't avoid collisions unless have quadratic amount of memory
- coupon collector: expect every bin has >= 1 ball after ~ M ln M tosses.  
- load balancing: after M tosses, expect most loaded bin has ~ (log M / log log M)
balls.  
==> collisions will be evenly distributed.

one-way hash functions.  
applications: digital fingerprint, message digest, storing passwords

main reason to use hash table instead of a red-black bst:  
better performance in practice on typical inputs.

## Symbol table applications

linear-probing hash table is most suitable for dictionary clients.
because ordered ops not needed.

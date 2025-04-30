# Java Collections Framework
The Java Collections Framework (JCF) provides a unified architecture of interfaces and classes for representing and manipulating groups of objects. It defines core interfaces (Collection, List, Set, Map, etc.),along with concrete high-performance implementations, algorithms, utilities​
- Root Interface -> Collection
- Utillity Class -> Collections static methods (sort(), binarySearch(), synchronizedList()).
- JCF includes
    - General-purpose implementations (ArrayList, HashMap, etc.)
    - Legacy classes (Vector, Hashtable), 
    - Specialized classes (e.g. EnumMap, concurrent collections), 
    - Wrapper views (Collections.synchronizedList(), Collections.unmodifiableMap())
    - static algorithms​

- This layered design means you can code to the interfaces (e.g. List<String>) and swap out implementations as needed. It also provides consistent behavior (contracts on equality, ordering, concurrency, etc.) across different collection types.

## List Implementations
### ArrayList
- Growable array implementation of the List interface. 
- Internally -> dynamically resizable array of elements. 
- Common characteristics:
    - Random access: get(i) and set(i,v) are O(1) operations.
    - Amortized append: add(elem) at the end is O(1) amortized. 
- When the internal array fills, it is reallocated to a larger size (about 1.5×)
- Insert/remove at index: At an arbitrary index -> requires shifting -> TC - O(n).
- Iteration: fast (contiguous memory) but sensitive to concurrent modifications (fail-fast)
    - if structurally modified (except iterator’s remove)-> ConcurrentModificationException
- Allows null elements.
- Not synchronized -> Use Collections.synchronizedList() or CopyOnWriteArrayList.

### LinkedList
-----------------------------------------------------------------------------
- LinkedList implements List (and Deque) using a doubly-linked node structure.
- Sequential access: No random access; get(i) or add(i,elem) require traversing nodes O(n).
- Constant-time insert/delete at ends: addFirst, addLast, removeFirst, removeLast are O(1).
- Nulls and duplicates: It allows null values and duplicates​
- Memory: Each element -> separate object (Memory overhead > ArrayList -> for many elements).
- Iteration: Also fail-fast on structural changes.
- Useful when many insertions/deletions at ends or iterator-based processing. 
- Not synchronized; use external synchronization if needed.

### Other List Types
- Vector/Stack: Legacy thread-safe list (Vector) and stack (Stack extends Vector). They synchronize every method and are generally replaced by ArrayList plus external synchronization or concurrent alternatives.
- CopyOnWriteArrayList (in bonus): A concurrent List that makes a fresh copy on each mutative write, providing snapshot-style iteration.

## Set Implementations
### HashSet
HashSet implements Set using a backing HashMap. Key points:
Hash table: Internally uses a HashMap (elements are keys with dummy values)​

.
Complexity: Typical add, remove, contains are O(1) on average, assuming a good hash function​

. Worst-case can degrade to O(n) if many collisions occur (rare in Java 8+ due to treeification).
Null: Allows one null element (via the underlying HashMap that permits a null key)​

.
Order: Unordered; iteration order may seem random and can change after rehash.
Iteration: Iteration time is proportional to the number of buckets plus size​

. Therefore a large capacity with few elements wastes time.
Fail-fast: Iterators are fail-fast (throw ConcurrentModificationException on concurrent modification)​

.
Thread-safety: Not synchronized. For thread-safe use, wrap with Collections.synchronizedSet() (see below).
Performance detail: A HashSet with default load factor 0.75 will resize when size > 0.75 × capacity​

. In Java 8+, when a bucket’s chain grows beyond a threshold (typically 8 entries), it is transformed into a balanced tree to avoid O(n) lookup​
studytonight.com
. 

​
studytonight.com
LinkedHashSet
A HashSet with a linked-list running through entries to preserve insertion order​

. Key differences from HashSet:
Iteration order: Always returns elements in insertion order (or access order if configured), while still forbidding duplicates​

.
Complexity: Basic ops (add, contains, remove) are still O(1) (like HashSet), with slight overhead for maintaining the linked list​

.
Null: Permits one null (like HashSet)​

.
Iteration cost: Iterating over a LinkedHashSet is O(n) (only size matters)​

, versus a HashSet which depends on the number of buckets.
Thread-safety: Not synchronized. Iterators are fail-fast​

.

​

TreeSet
A sorted set backed by a red-black tree (TreeMap). Key points:
Sorted order: Maintains elements in ascending (natural or Comparator-defined) order.
Performance: add, remove, contains are O(log n) guaranteed​
geeksforgeeks.org
.
Nulls: Null elements are not allowed. Attempting to add null will throw NullPointerException because natural ordering cannot compare null​
geeksforgeeks.org
. (If a custom Comparator that handles nulls is provided, then nulls could be allowed.)
Duplicates: Disallowed (like all Set).
Iteration: In sorted order. Iterators are fail-fast.
Uses: Good when you need a dynamically sorted set or range queries (headSet, tailSet).
geeksforgeeks.org
​
geeksforgeeks.org
Other Sets
EnumSet (bonus): Specialized for enum types (very fast, uses bit vectors).
CopyOnWriteArraySet (bonus): A set backed by a CopyOnWriteArrayList, for concurrency.
Queue and Deque Implementations
PriorityQueue
An unbounded priority heap (min-heap by default).
Ordering: Elements ordered by natural order or a provided Comparator. The head is the least element.
Nulls: Does not permit null elements​

.
Complexity: Insertion (offer/add) and removal of the head (poll) are O(log n); retrieving the head (peek) is O(1)​

. Methods like contains(o) and remove(o) are linear time O(n) (scans the heap)​

.
Iteration: Iterators are not sorted (they traverse the underlying array). Iteration order is arbitrary​

. Iterators are fail-fast.
Uses: Priority-based scheduling, Dijkstra’s algorithm, etc.

​

ArrayDeque
A resizable-array implementation of Deque (double-ended queue).
Operations: Amortized O(1) for insert/remove at both ends (addFirst, addLast, pollFirst, pollLast), since it uses a circular buffer.
Nulls: Null elements are prohibited​

.
Capacity: Grows as needed. It is usually faster than LinkedList when used as a queue or stack​

.
Iteration: Fail-fast​

.
Thread-safety: Not synchronized.

​

LinkedList as Queue/Deque
LinkedList also implements Queue and Deque. Its queue operations (add, poll, etc.) have similar complexity to list operations (e.g. addLast is O(1), pollFirst is O(1), but remove(Object) is O(n)). It permits nulls. In many cases ArrayDeque is preferred due to lower memory overhead and better locality.
Map Implementations
HashMap
The general-purpose hash table–based Map.
Structure: Backed by an array of bins (Node<K,V>). Keys are hashed via hashCode(), and placed into buckets via index = hash & (capacity-1). Bins store entries by separate chaining (linked list or tree)​
geeksforgeeks.org
​
studytonight.com
.
Capacity & load factor: Default initial capacity is 16 and load factor is 0.75. When size exceeds capacity * loadFactor, the table is rehashed (usually doubled in capacity)​

. This ensures roughly 0.75 fill ratio.
Collision handling: In Java 8+, bins with few entries use linked nodes; if a single bin’s chain grows beyond a threshold (TREEIFY_THRESHOLD = 8), it is transformed into a balanced tree to avoid worst-case O(n) lookup​
studytonight.com
.
Operations: get, put, remove are O(1) on average, assuming a good hash function​

. In the worst case (all keys collide and no tree), they degrade to O(n).
Nulls: Permits one null key and multiple null values​

. The null key is treated specially (in bucket 0).
Iteration & order: Unordered; the order is not guaranteed and can change after rehashing. Iterating over keys/views is O(n + capacity)​

, so a large capacity (many empty buckets) slows iteration.
Fail-fast: Iterators on keySet, values, or entrySet are fail-fast: if the map is structurally modified after iterator creation (except via the iterator’s remove), a ConcurrentModificationException is thrown​

.
Thread-safety: Not synchronized. Do not use a plain HashMap from multiple threads without external synchronization. For a concurrent variant, see ConcurrentHashMap.

​

LinkedHashMap
Extends HashMap with predictable ordering.
Order: Maintains a doubly-linked list of entries, by insertion order (or access order if constructed with that mode)​

. This provides a “nearly constant” insertion-order iteration.
Performance: Basic operations are still O(1) (like HashMap)​

, but with slightly more overhead to update links.
Nulls: Permits one null key and null values (like HashMap).
Iteration: Iterates in insertion (or access) order. Iteration cost is O(n) regardless of capacity​

 (no wasted time on empty buckets).
Fail-fast: Iterators are fail-fast​

.
Uses: Useful when you need a map that remembers insertion order, or an LRU cache (by using access-order mode)​

.

​

TreeMap
A sorted map backed by a Red-Black tree.
Order: Keys are sorted by their natural ordering (via Comparable), or by a provided Comparator.
Complexity: get, put, remove take O(log n) time​

.
Nulls: Null keys are not allowed by default (inserting null throws NullPointerException)​

, because comparison to null is undefined. Null values are allowed.
NavigableMap: Implements NavigableMap, so supports range views (subMap, headMap, tailMap) and lookup methods (lowerKey, ceilingKey, etc.). All key-based view iterators are in sorted order.
Fail-fast: Iterators on a TreeMap (via its keySet, etc.) are fail-fast.
Thread-safety: Not synchronized.

​

Hashtable
A legacy synchronized map (pre-JDK 1.2).
Synchronization: All methods of Hashtable are synchronized, giving thread safety at the cost of throughput. Only one thread can operate on it at a time.
Nulls: Does not allow null keys or values​

 (unlike HashMap). Attempts to use null will throw NullPointerException.
Performance: Similar to HashMap for single-threaded use (O(1) basic ops), but significantly slower under contention.
Iteration: The iterator or enumeration of a Hashtable is fail-fast (since Java 2)​

.
Use: Hashtable is mostly obsolete; for thread-safe maps use ConcurrentHashMap or Collections.synchronizedMap(new HashMap<…>()).

​

ConcurrentHashMap
A high-performance concurrent map.
Thread-safety: All operations are thread-safe. Reads (get) do not block and can proceed concurrently with writes​

. Writes (put, remove) use fine-grained locking/CAS so that multiple updates can often proceed in parallel. (Internally, Java 8’s ConcurrentHashMap uses CAS and tree bins, not fixed segments.)
Nulls: Null keys and values are not allowed​

 (unlike HashMap). This avoids ambiguous return values.
Iteration: Iterators/spliterators are weakly consistent – they reflect the state of the map at or since creation and do not throw ConcurrentModificationException​

. They may (or may not) see updates made after they were created.
Bulk operations: Aggregate methods (size(), isEmpty(), containsValue(), etc.) are not guaranteed to be atomic under concurrent updates (they may reflect transient states)​

.
Concurrency Level: The (now-deprecated) concurrencyLevel hint in constructors is ignored in Java 8+. The map automatically resizes to maintain about 0.75 load factor​

.
Performance: Provides high concurrency; good for maps accessed by many threads.

​

Important Concepts
Fail-fast vs. Fail-safe Iterators: Most JCF collections (e.g. ArrayList, HashMap) use fail-fast iterators that detect structural modification and throw ConcurrentModificationException​

. Concurrent collections (like ConcurrentHashMap, CopyOnWriteArrayList) provide weakly consistent or fail-safe iteration: they do not throw CME and allow the underlying collection to be concurrently modified during iteration​

​
javarevisited.blogspot.com
. For example, ConcurrentHashMap’s iterator sees a snapshot of the map and continues, while a HashMap’s iterator will fail if the map is changed.
equals() and hashCode(): To use objects as keys in HashMap/HashSet, you must follow the contract: if a.equals(b) then a.hashCode()==b.hashCode(). Failing to override hashCode() when equals() is overridden will break hash-based collections​
geeksforgeeks.org
. In practice, equal objects must produce the same hash code, or lookups/containment checks will fail unexpectedly​
geeksforgeeks.org
.
Comparable vs. Comparator: Comparable<T> defines a class’s natural ordering via compareTo(). Comparator<T> is a separate object for custom orderings. A class can only have one compareTo (one natural order), but you can use many Comparators for different orders​
geeksforgeeks.org
. Sorted collections (like TreeSet) use compareTo (or a supplied Comparator) for ordering.
Synchronized vs. Concurrent Collections: “Synchronized” collections (e.g. Collections.synchronizedMap, Vector, legacy thread-safe classes) lock the entire structure on each operation. This ensures safety but severely limits concurrency. In contrast, modern concurrent classes (like ConcurrentHashMap, CopyOnWriteArrayList) use finer-grained techniques (lock striping or copy-on-write) so that multiple threads can operate concurrently without a single global lock​
javarevisited.blogspot.com
​
javarevisited.blogspot.com
. For example, ConcurrentHashMap partitions its table and only locks one part at a time, and CopyOnWriteArrayList allows unsynchronized iteration by copying the array on writes​
javarevisited.blogspot.com
​
javarevisited.blogspot.com
.
Resizing Behavior: Many collections resize automatically. E.g. ArrayList doubles capacity by ~1.5× when needed (so add is amortized O(1))​
cisc3130fa24.github.io
. HashMap doubles its bucket array when the load factor is exceeded​

. Resizing is relatively expensive, so choosing a good initial capacity (and load factor) can improve performance.
Weak References: Classes like WeakHashMap use weak references for keys. When a key becomes only weakly reachable, the GC can reclaim it and remove the entry​

. This is useful for caches: you can let keys (and their entries) disappear automatically when not in use.
Collections Utilities
Java provides utility classes for working with collections:
Algorithms (Collections class): java.util.Collections has static methods like sort(), shuffle(), binarySearch(), reverse(), etc. These operate on List or other collections​

. For example, Collections.sort(list) sorts a List in natural order.
Arrays utilities: java.util.Arrays has similar methods for arrays (e.g. Arrays.sort(), Arrays.binarySearch()) and can convert between arrays and lists (Arrays.asList(array)).
Unmodifiable and Immutable Collections: Before Java 9, one created read-only collections via wrappers: e.g. Collections.unmodifiableList(list). Since Java 9, there are convenient factory methods that return truly immutable collections. For example, List.of("a","b") creates an immutable list (nulls are not allowed)​

. Likewise Set.of() and Map.of() create unmodifiable set/map; duplicate keys or nulls cause exceptions​

​

. These immutable collections are optimized (compact, and in Java 11+ use copy-on-write copyOf methods) and use much less memory for constant data​

.
Synchronized Wrappers: The Collections class provides synchronizedList, synchronizedMap, etc., which wrap a collection so that each method is synchronized on a given mutex. For example:
Map<K,V> syncMap = Collections.synchronizedMap(new HashMap<>());
This ensures basic thread safety, but iteration must still be externally synchronized on the wrapper object to avoid CME. For example, Collections.synchronizedSet(someSet) requires:
Set<K> set = Collections.synchronizedSet(new HashSet<>());
synchronized (set) {
    for (K k : set) { … }  // lock held during iteration
}

​

.
Copy utilities: Since Java 10, List.copyOf, Set.copyOf, and Map.copyOf create immutable copies of existing collections. (E.g. List<String> snap = List.copyOf(existingList);). These help snapshotted unmodifiable views.

​

Bonus Topics
EnumMap: A map specialized for enum keys. Internally uses an array indexed by enum ordinal, so operations are extremely fast and space-efficient​

. Keys must all come from a single enum type. Iteration is in the enum’s natural order (order of declaration)​

. Null keys are not allowed​

. Basic operations are O(1) and even faster than HashMap due to the array structure​

. Useful when your keys are enum constants (and you need a map); e.g. EnumMap<DayOfWeek,String>. Its iterators are weakly consistent (no CME)​

, and the map is not thread-safe by default.
IdentityHashMap: Uses reference equality (==) instead of equals() for keys (and values)​

. It’s not for general use because it violates Map’s usual contract. But it’s useful when you want a “node table” of objects by identity (e.g. serialization cache, object graph processing). It allows null keys and values​

. Operations are constant-time (using System.identityHashCode)​

. Iteration order is unspecified. This map is unsynchronized.
WeakHashMap: A hash table with weak keys. When a key is no longer strongly referenced elsewhere, its entry is automatically removed​

. This makes it ideal for building caches: entries disappear when no one else uses the key. It permits null keys/values, has performance similar to HashMap, but its size can shrink “silently” under GC. Not synchronized.
CopyOnWriteArrayList: A thread-safe variant of ArrayList where all mutative operations (add, set, remove) are implemented by making a fresh copy of the array. This means that iteration can occur without locking on a snapshot of the data​
javarevisited.blogspot.com
. Readers see a consistent snapshot and do not encounter CME. Write operations are O(n) (copying the whole array). Useful when reads greatly outnumber writes (e.g. listener lists). It allows one null element.
BlockingQueue (and related): An interface (java.util.concurrent.BlockingQueue) for thread-safe queues that support blocking operations​

. For example, ArrayBlockingQueue is a bounded array-backed queue; LinkedBlockingQueue is (optionally bounded) linked nodes; PriorityBlockingQueue is an unbounded priority queue; DelayQueue only releases elements when their delay expires. Methods like put(e) block if the queue is full; take() blocks if empty. Null elements are not allowed​

. All implementations are thread-safe, using internal locks or advanced concurrency control​

. They are designed for producer-consumer patterns.
NavigableMap: An extension of SortedMap with methods to navigate by key. E.g. lowerKey(k), floorKey(k), ceilingKey(k), higherKey(k) (to find entries just below/above a given key) and firstEntry()/lastEntry()/pollFirstEntry()/pollLastEntry(). Also supports descending maps and inclusive/exclusive submaps​

​

. Implemented by TreeMap and ConcurrentSkipListMap. Allows efficient range queries on sorted data.
ConcurrentSkipListMap: A concurrent sorted map (implements ConcurrentNavigableMap). Internally uses a lock-free skip-list. Provides O(log n) average get/put/remove​

 and allows concurrent access by multiple threads without external locking. Iterators are weakly consistent​

. Null keys/values are disallowed​

. Useful when you need a thread-safe sorted map (for example, a concurrent priority map) with better scalability than synchronizing a TreeMap.
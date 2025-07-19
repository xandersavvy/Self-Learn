# Ultimate SDE Interview Playbook

> A single-page checklist of every technical area, concept, pattern, and canonical question that can appear in a top-tier Software Development Engineer (SDE) interview.  
> Master everything below and nothing technical should surprise you.

## 1. Core CS Foundations

| Area                | Must-Know Concepts                                     | Canonical Questions (free links)                  |
| ------------------- | ------------------------------------------------------ | ------------------------------------------------- |
| Big-O Analysis      | Time/space classes, amortized analysis, master theorem | Explain why hash-table ops are O(1) amortized [1] |
| Data Representation | Binary, hex, endianness, floating-point, UTF-8         | Why is 0.1 + 0.2 != 0.3? [2]                      |
| Number Theory       | GCD, modular arithmetic, pow-mod, prime testing        | Implement RSA key step (mod inverse)              |
| Concurrency Basics  | Threads vs processes, locks, lock-free, memory models  | Producer-consumer with bounded buffer [3]         |

## 2. Data Structures & Paradigms

### 2.1 Arrays & Strings

- Two-pointers, sliding window, prefix/suffix, difference array
- In-place rotate, subarray sum = k, longest substring w/o repeats [1]

### 2.2 Linked Lists

- Fast/slow pointer, reversal, merge k-lists, copy with random ptr
- Detect & remove cycle, lru-cache with doubly list [4]

### 2.3 Stacks & Queues

- Min-stack/O(1) get-min, monotonic stack (next greater), circular deque
- Queue via two stacks, stack via two queues, sliding-window max [4]

### 2.4 Hash Tables & Sets

- Chaining vs open-addressing, load factor, robin-hood, bloom filter
- Two-sum, longest consecutive sequence, LRU via OrderedDict

### 2.5 Trees

- Traversals (rec, iter, Morris), serialize/deserialize, diameter
- BST ops, validate BST, kth smallest, successor/predecessor
- Balanced trees: AVL vs Red-Black trade-offs [2]

### 2.6 Heaps & Priority Queues

- Binary, d-ary, Fibonacci, pairing heap use-cases
- Median stream, top-k frequent, merge k-sorted arrays [1]

### 2.7 Tries & Advanced Strings

- Prefix tree, compressed trie, ternary search tree
- Aho-Corasick, suffix array/tree, KMP, Rabin-Karp

### 2.8 Graphs

- Representations, DFS/BFS, topological sort, connected comps
- Dijkstra, A\*, Bellman-Ford, Floyd-Warshall, Union-Find (path compression + rank)
- MST (Kruskal, Prim), bipartite check, bridges & articulation

### 2.9 Segment / Fenwick / Interval Trees

- Range sum/min/max updates, lazy propagation
- Order statistics tree, sweep-line + interval merges

### 2.10 Advanced / Probabilistic

- Skip list, treap, cuckoo hash, bloom & count-min filters

## 3. Algorithmic Patterns

| Pattern              | Typical Problems                                           |
| -------------------- | ---------------------------------------------------------- |
| Divide & Conquer     | Merge sort, quickselect, closest pair of points            |
| Dynamic Programming  | LIS/LCS, 0-1 & unbounded knapsack, DP on trees, bitmask DP |
| Greedy               | Interval scheduling, Huffman coding, activity selection    |
| Backtracking         | N-queens, permutations, sudoku solver                      |
| Bit Manipulation     | Single number, subset generation, Gray code                |
| Geometry             | Convex hull (Graham/Monotone), line-sweep intersections    |
| Math / Combinatorics | Catalan numbers, permutation ranking                       |

## 4. System Design (HLD) Checklist

1. **Requirement Clarification**

   - Functional, non-functional (scale, latency, consistency, SLA)

2. **Back-of-Envelope Estimation**

   - QPS, storage, bandwidth, peak vs average

3. **High-Level Architecture**

   - Client → CDN → LB → App Tier → Data Stores
   - Read/write path, async flows, service registry, health checks

4. **Key Components to Cover**

   - API gateway / GSLB
   - Caching layers (client, edge, app, DB)
   - Data store choices (SQL vs NoSQL vs NewSQL)
   - Sharding & replication strategy
   - Messaging/stream (Kafka/PubSub)
   - Index/search service
   - Observability: logging, metrics, tracing
   - Security: authN, authZ, rate limiting, encryption

5. **Bottlenecks & Trade-offs**

   - Consistent hashing vs sticky routing
   - Strong vs eventual consistency; CAP position
   - Push vs pull for notifications
   - Data retention, GDPR deletes

6. **Evolvability**
   - Feature flags, schema migrations, blue-green / canary deploy
   - Backfill & reindex strategy

### Classic Design Prompts

- URL shortener, Twitter timeline, YouTube, Uber, WhatsApp, Payment gateway, Real-time collaborative editor, Distributed cache, Rate limiter

## 5. Low-Level Design (LLD) Essentials

| Principle       | Application                                                                     |
| --------------- | ------------------------------------------------------------------------------- |
| SOLID           | Service & class design                                                          |
| OOP Modeling    | Chess, parking lot, elevator, airline reservation                               |
| Design Patterns | Singleton, Factory, Builder, Adapter, Observer, Strategy, Decorator, Repository |
| Concurrency     | Thread-safe singleton, producer-consumer, dining philosophers                   |
| Database Schema | Normalization vs denormalization, indexing strategy, ACID vs BASE               |
| API Contract    | RESTful resource naming, idempotency, pagination, error model                   |

## 6. Language-Specific Deep Dives

- **Java**: JVM memory, GC types, Java memory model, volatile vs synchronized, CompletableFuture, Stream API
- **C++**: RAII, move semantics, smart pointers, memory order, templates, STL internals
- **Python**: GIL, asyncio, generators, descriptor protocol, typing, memory profiler
- **JavaScript/TS**: Event loop, microtask vs macrotask, prototypal inheritance, Node clustering

## 7. Runtime & OS Basics

- Process vs thread, context switch, syscall overhead
- CPU cache hierarchy, false sharing, memory alignment
- Virtual memory, page faults, mmap vs read
- Networking: TCP three-way handshake, HTTP/2 vs HTTP/3, TLS handshake
- Containerization basics: namespaces, cgroups, docker image layers

## 8. Cloud & DevOps Touch Points

| Topic         | Key Points                                                  |
| ------------- | ----------------------------------------------------------- |
| Kubernetes    | Pods vs deployment vs service, autoscaling, rolling update  |
| AWS/GCP/Azure | VPC, IAM, S3/GCS, DynamoDB/Bigtable, Lambda/Cloud Functions |
| CI/CD         | GitOps, blue-green, canary, feature toggles                 |
| Observability | Prometheus metrics, OpenTelemetry traces, ELK stack logs    |

## 9. Canonical Coding Questions by Topic (Free)

| Topic       | Representative Problems                                       |
| ----------- | ------------------------------------------------------------- |
| Arrays      | LC #53 (Max Subarray), #238 (Product of Array Except Self)    |
| Strings     | LC #3 (Longest Substring), #76 (Min Window Substring)         |
| Linked List | LC #141 (Cycle), #23 (Merge k Lists)                          |
| Trees       | LC #124 (Tree Max Path Sum), #105 (Construct from Traversals) |
| Graphs      | LC #200 (Number of Islands), #269 (Alien Dictionary)          |
| DP          | LC #72 (Edit Distance), #322 (Coin Change)                    |
| Math/Bit    | LC #50 (Pow xⁿ), #190 (Reverse Bits)                          |
| Greedy      | LC #435 (Non-overlap Intervals), #253 (Meeting Rooms II)      |
| Heap        | LC #295 (Median Data Stream), #347 (Top K Frequent)           |

Full 200-question free list: GeeksforGeeks “Must Do DSA Questions” [4]

## 10. Practice & Mock Platforms (No Paywall)

| Platform                    | Coverage                          | Link                                            |
| --------------------------- | --------------------------------- | ----------------------------------------------- |
| **LeetCode (free subset)**  | 1-2500+ coding Qs                 | leetcode.com/problemset/all                     |
| **GeeksforGeeks**           | DSA + system design articles & Qs | geeksforgeeks.org [4]                           |
| **Codeforces**              | Timed contests, CP style          | codeforces.com                                  |
| **SystemDesignSchool**      | Free design walkthroughs          | systemdesignschool.io                           |
| **Tech Interview Handbook** | Curated prep & templates          | techinterviewhandbook.org [5]                   |
| **Grokking GitHub Repos**   | Yangshun handbook, design repos   | github.com/yangshun/tech-interview-handbook [6] |

## 11. Interview Day Execution Checklist

1. Clarify requirements & constraints
2. State naive solution & complexity
3. Optimize aloud, discuss trade-offs
4. Write clean, bug-free code with tests
5. Analyze complexity again; discuss edge cases & extensions
6. For design: recap requirements → capacity → HLD diagram → components → deep dive → bottlenecks → evolution

[1] https://www.simplilearn.com/coding-interview-questions-article
[2] https://www.springboard.com/blog/software-engineering/21-software-engineering-interview-questions/
[3] https://dev.to/somadevtoo/7-essential-topics-for-software-engineering-interviews-in-2025-2cc3
[4] https://www.geeksforgeeks.org/dsa/must-do-coding-questions-for-companies-like-amazon-microsoft-adobe/
[5] https://www.techinterviewhandbook.org
[6] https://github.com/yangshun/tech-interview-handbook
[7] https://igotanoffer.com/blogs/tech/faang-interview-questions
[8] https://www.geeksforgeeks.org/software-engineering/software-engineering-interview-questions-and-answers/
[9] https://builtin.com/software-engineering-perspectives/technical-interview-questions
[10] https://www.techinterviewhandbook.org/behavioral-interview-questions/
[11] https://www.codementor.io/blog/technical-interview-questions-jqbigasqsn
[12] https://github.com/ombharatiya/FAANG-Coding-Interview-Questions
[13] https://www.datacamp.com/blog/software-engineer-interview-questions
[14] https://www.techinterviewhandbook.org/software-engineering-interview-guide/
[15] https://www.linkedin.com/pulse/big-tech-coding-interview-simple-guide-forsuccess-frances-cooper
[16] https://www.geeksforgeeks.org/interview-experiences/faang-interview-experience/
[17] https://in.indeed.com/career-advice/interviewing/software-engineer-interview-questions
[18] https://www.reddit.com/r/webdev/comments/wkpgco/most_asked_questions_at_faang_companies_must_read/
[19] https://agilemania.com/software-developer-interview-questions
[20] https://www.freecodecamp.org/news/coding-interview-prep-for-big-tech/

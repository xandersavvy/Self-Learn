# Data Structures: Zero to Fellow Engineer

_A comprehensive guide to master data structures for FAANG Fellow Engineer positions_

## Table of Contents

- [Learning Strategy](#learning-strategy)
- [Phase 1: Foundation Data Structures](#phase-1-foundation-data-structures)
- [Phase 2: Advanced Data Structures](#phase-2-advanced-data-structures)
- [Phase 3: Expert-Level Structures](#phase-3-expert-level-structures)
- [Phase 4: System Design Data Structures](#phase-4-system-design-data-structures)
- [Free Practice Platforms](#free-practice-platforms)
- [Interview Preparation Strategy](#interview-preparation-strategy)

## Learning Strategy

### Timeline for Mastery

- **Phase 1 (Weeks 1-4)**: Foundation structures
- **Phase 2 (Weeks 5-8)**: Advanced structures and algorithms
- **Phase 3 (Weeks 9-12)**: Expert-level and specialized structures
- **Phase 4 (Weeks 13-16)**: System design and distributed structures

### Study Methodology

1. **Understand the Theory**: Deep dive into structure mechanics
2. **Analyze Time/Space Complexity**: Master Big-O analysis
3. **Implement from Scratch**: Code each structure without libraries
4. **Solve Progressive Problems**: Start easy, advance to hard
5. **Pattern Recognition**: Identify when to use each structure

## Phase 1: Foundation Data Structures

### Arrays and Dynamic Arrays

**Core Concepts:**

- Contiguous memory allocation
- Fixed vs dynamic sizing
- Cache locality advantages
- Index-based random access

**Key Operations & Complexity:**

- Access: O(1)
- Search: O(n) unsorted, O(log n) sorted
- Insertion: O(n) at beginning, O(1) at end (amortized)
- Deletion: O(n) at beginning, O(1) at end

**Advanced Techniques:**

- Two-pointer technique
- Sliding window patterns
- Prefix sums and difference arrays
- Kadane's algorithm for maximum subarray

**Practice Problems:**

- **LeetCode**: 1, 26, 27, 53, 121, 122, 238, 560
- **GeeksforGeeks**: Array rotation, Maximum subarray, Two sum
- **CodeChef**: STFOOD, CLEANUP (Beginner level)

### Linked Lists

**Types and Applications:**

- **Singly Linked**: Basic linear traversal
- **Doubly Linked**: Bidirectional navigation, used in LRU cache
- **Circular Linked**: Round-robin scheduling, Josephus problem

**Memory Management:**

- Dynamic allocation reduces memory waste
- Non-contiguous storage affects cache performance
- Pointer overhead considerations

**Advanced Patterns:**

- Fast-slow pointer (Floyd's cycle detection)
- Reversal techniques
- Merging sorted lists
- Skip list concepts for probabilistic search

**Practice Problems:**

- **LeetCode**: 21, 141, 142, 206, 234, 328, 876
- **HackerRank**: Detect cycle, Merge two sorted lists
- **SPOJ**: Linked list problems in data-structure-1 tag [1]

### Stacks and Queues

**Stack Applications:**

- Function call management
- Expression evaluation and syntax parsing
- Backtracking algorithms
- Undo operations in editors

**Queue Variants:**

- **Simple Queue**: FIFO operations
- **Circular Queue**: Efficient space utilization
- **Deque**: Double-ended operations
- **Priority Queue**: Heap-based implementation

**Advanced Stack Techniques:**

- Monotonic stack for next greater/smaller element
- Stack-based iterative tree traversals
- Balanced parentheses and bracket matching

**Practice Problems:**

- **LeetCode**: 20, 155, 225, 232, 496, 739, 946
- **CodeChef**: STFOOD, MGCSET (Stack applications)
- **GeeksforGeeks**: Infix to postfix, Stock span problem

### Hash Tables

**Collision Resolution:**

- **Chaining**: Linked lists at each bucket
- **Open Addressing**: Linear/quadratic probing, double hashing
- **Robin Hood Hashing**: Advanced probing technique

**Advanced Hashing:**

- Universal hashing functions
- Perfect hashing for static data
- Bloom filters for membership testing
- Consistent hashing for distributed systems

**Load Factor and Rehashing:**

- Optimal load factor (0.7-0.75)
- Dynamic resizing strategies
- Amortized analysis of operations

**Practice Problems:**

- **LeetCode**: 1, 49, 128, 217, 242, 347, 380
- **CodeChef**: FREQ, HASHIT
- **HackerRank**: Hash table implementations

## Phase 2: Advanced Data Structures

### Trees

#### Binary Trees and Traversals

**Tree Properties:**

- Height vs depth relationships
- Complete, full, and perfect binary trees
- Balanced tree characteristics

**Traversal Techniques:**

- **Recursive**: Inorder, preorder, postorder
- **Iterative**: Stack-based implementations
- **Level-order**: Queue-based BFS
- **Morris Traversal**: O(1) space complexity

#### Binary Search Trees (BST)

**BST Properties:**

- In-order traversal yields sorted sequence
- Average O(log n) operations
- Worst-case O(n) for skewed trees

**BST Operations:**

- Insertion with parent tracking
- Deletion: three cases (leaf, one child, two children)
- Successor and predecessor finding
- Range queries and k-th element

**Practice Problems:**

- **LeetCode**: 98, 108, 173, 230, 450, 700, 701
- **GeeksforGeeks**: BST construction from traversals
- **SPOJ**: Binary tree problems [2]

#### Self-Balancing Trees

#### AVL Trees

**Balancing Mechanism:**

- Balance factor: height(left) - height(right)
- Must be -1, 0, or 1 for all nodes
- Automatic rebalancing through rotations

**Rotation Types:**

- **Single Rotations**: Left and right
- **Double Rotations**: Left-right and right-left
- Height updates during rotations

**When to Use AVL:**

- Frequent lookups with fewer insertions/deletions
- Database indexing where search speed is critical
- Real-time systems requiring guaranteed O(log n)

**Advantages over Red-Black Trees:**

- Stricter balancing ensures faster lookups
- Maximum height is 1.44 \* log n
- Better for read-heavy workloads [3]

#### Red-Black Trees

**Color Properties:**

1. Every node is red or black
2. Root is always black
3. Red nodes have black children only
4. All paths from node to leaves have same black height
5. New nodes are initially red

**Balancing Strategy:**

- Less rigid than AVL (allows height up to 2 \* log n)
- Requires fewer rotations during insertion/deletion
- Used in language libraries (C++ STL map/set) [4]

**Comparison with AVL:**

- **Red-Black**: Faster insertion/deletion, more memory efficient
- **AVL**: Faster lookups, strictly balanced [5]

**Practice Problems:**

- **LeetCode**: Implement basic BST operations first
- **GeeksforGeeks**: Red-black tree insertion/deletion
- **Study**: Visualize rotations and color changes

### Heaps

#### Binary Heaps

**Heap Properties:**

- **Max Heap**: Parent ≥ children
- **Min Heap**: Parent ≤ children
- Complete binary tree structure
- Array-based implementation

**Array Representation:**

- Parent at index i has children at 2i+1 and 2i+2
- Child at index i has parent at (i-1)/2
- Last non-leaf node at (n/2 - 1)

**Heap Operations:**

- **Heapify**: O(log n) bubble up/down
- **Build Heap**: O(n) bottom-up construction
- **Extract**: O(log n) remove root and reheapify

#### Priority Queues

**Applications:**

- Dijkstra's shortest path algorithm
- Huffman coding for compression
- Task scheduling in operating systems
- Event simulation systems

**Advanced Heap Variants:**

- **Binary Heap**: Standard implementation
- **d-ary Heap**: More children per node
- **Fibonacci Heap**: Better amortized complexities
- **Binomial Heap**: Efficient merge operations

**Practice Problems:**

- **LeetCode**: 23, 215, 295, 347, 378, 692, 703
- **HackerRank**: Heap-based problems
- **CodeChef**: Heap sort, Priority queue problems

### Graphs

#### Graph Representations

**Adjacency Matrix:**

- O(V²) space complexity
- O(1) edge existence check
- Efficient for dense graphs
- Good for graph algorithms like Floyd-Warshall

**Adjacency List:**

- O(V + E) space complexity
- Efficient for sparse graphs
- Better memory usage for most real-world graphs
- Preferred for DFS/BFS traversals

**Edge List:**

- Simple list of (u, v) pairs
- Good for algorithms like Kruskal's MST
- Easy to sort edges by weight

#### Graph Traversals

**Depth-First Search (DFS):**

- Stack-based (recursive or iterative)
- Applications: Topological sorting, cycle detection, connected components
- Time: O(V + E), Space: O(V) for recursion stack

**Breadth-First Search (BFS):**

- Queue-based traversal
- Applications: Shortest path in unweighted graphs, level-order processing
- Time: O(V + E), Space: O(V) for queue

**Advanced Traversal Techniques:**

- **Bidirectional BFS**: Meet in the middle approach
- **A\* Search**: Heuristic-based pathfinding
- **Iterative Deepening**: DFS with depth limits

#### Graph Algorithms

**Shortest Path Algorithms:**

- **Dijkstra's Algorithm**: Non-negative weights, O((V + E) log V)
- **Bellman-Ford**: Handles negative weights, O(VE)
- **Floyd-Warshall**: All pairs shortest path, O(V³)

**Minimum Spanning Tree:**

- **Kruskal's Algorithm**: Union-find based, O(E log E)
- **Prim's Algorithm**: Priority queue based, O((V + E) log V)

**Practice Problems:**

- **LeetCode**: 133, 200, 207, 210, 261, 323, 684, 785
- **CodeChef**: Graph traversal problems
- **Codeforces**: Graph theory contests [6]

## Phase 3: Expert-Level Structures

### Disjoint Set Union (Union-Find)

#### Core Concepts

**Disjoint Set Properties:**

- Maintains collection of non-overlapping sets
- Each set has a representative (root) element
- Supports union and find operations efficiently

**Operations:**

- **Find(x)**: Returns representative of set containing x
- **Union(x, y)**: Merges sets containing x and y
- **Connected(x, y)**: Checks if x and y are in same set

#### Optimizations

**Path Compression:**

- During find, point all nodes directly to root
- Flattens tree structure for future operations
- Amortizes operation cost significantly

```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])  # Path compression
    return parent[x]
```

**Union by Rank/Size:**

- Always attach smaller tree under larger tree root
- Prevents degenerate tree structures
- Maintains logarithmic height bounds

**Time Complexity:**

- With both optimizations: O(α(n)) per operation
- α(n) is inverse Ackermann function (practically constant)
- For all practical purposes, operations are O(1) [7]

#### Applications

**Graph Algorithms:**

- **Kruskal's MST**: Check if edge creates cycle [8]
- **Connected Components**: Group vertices by connectivity
- **Cycle Detection**: In undirected graphs

**Dynamic Connectivity:**

- Online algorithms for adding edges
- Network connectivity maintenance
- Social network friend groups [9]

**Computational Geometry:**

- Image segmentation algorithms
- Percolation theory problems

**Practice Problems:**

- **LeetCode**: 128, 200, 261, 323, 547, 684, 721, 990
- **GeeksforGeeks**: Union-find applications [10]
- **CodeChef**: Disjoint set problems
- **HackerEarth**: Union-find tutorial problems [9]

### Advanced Tree Structures

#### Trie (Prefix Tree)

**Structure and Properties:**

- Tree where each path represents a string
- Common prefixes share paths from root
- Leaves (or marked nodes) represent complete words

**Applications:**

- **Autocomplete Systems**: Prefix-based suggestions
- **Spell Checkers**: Dictionary word validation
- **IP Routing**: Longest prefix matching
- **Genome Analysis**: DNA sequence matching

**Advanced Trie Variants:**

- **Compressed Trie**: Merge single-child chains
- **Suffix Trie**: All suffixes of a string
- **Aho-Corasick**: Multiple pattern matching

**Practice Problems:**

- **LeetCode**: 208, 211, 212, 421, 648, 676, 720
- **GeeksforGeeks**: Trie implementation and applications
- **CodeChef**: String processing problems

#### Segment Trees

**Concept:**

- Binary tree for range query problems
- Each node represents information about a range [l, r]
- Supports updates and queries in O(log n)

**Types of Segment Trees:**

- **Sum Segment Tree**: Range sum queries
- **Min/Max Segment Tree**: Range minimum/maximum
- **Lazy Propagation**: Efficient range updates

**Applications:**

- Range sum/min/max queries
- Count inversions in array
- Persistent data structures
- Computational geometry

**Practice Problems:**

- **LeetCode**: 307, 308, 315, 327, 493
- **CodeChef**: Segment tree problems
- **SPOJ**: Range query problems

#### Fenwick Tree (Binary Indexed Tree)

**Structure:**

- Array-based tree using binary representation
- More space-efficient than segment trees
- Easier to implement for sum queries

**Key Insight:**

- Uses binary representation of indices
- Each position stores cumulative information
- Update and query both O(log n)

**Applications:**

- Prefix sum queries with updates
- Count smaller elements after self
- 2D range sum queries

**Practice Problems:**

- **LeetCode**: 307, 308, 315
- **SPOJ**: Fenwick tree tagged problems [2]
- **GeeksforGeeks**: Binary indexed tree problems

### String Algorithms and Structures

#### KMP Algorithm

**Pattern Matching:**

- Efficient string searching in O(n + m)
- Uses failure function to skip characters
- Avoids redundant comparisons

**Applications:**

- Text searching in editors
- Bioinformatics sequence matching
- Network intrusion detection

#### Suffix Arrays and LCP

**Suffix Array:**

- Sorted array of all suffixes
- Space-efficient alternative to suffix trees
- Construction in O(n log n) or O(n)

**Applications:**

- Full-text indexing
- Longest common substring
- String compression algorithms

**Practice Problems:**

- **LeetCode**: 28, 214, 459, 1392
- **GeeksforGeeks**: String matching algorithms
- **SPOJ**: String algorithm problems

## Phase 4: System Design Data Structures

### Consistent Hashing

#### Concept and Motivation

**Problem with Simple Hashing:**

- Adding/removing servers causes massive data redistribution
- hash(key) % n changes for all keys when n changes
- Results in cache misses and system instability

**Consistent Hashing Solution:**

- Maps both keys and servers to points on a ring
- Adding/removing servers affects only adjacent keys
- Minimizes data movement during scaling [11]

#### Ring Structure

**Hash Ring Properties:**

- Conceptual circular space (0 to 2³² - 1)
- Servers placed at hash(server_id) positions
- Keys assigned to first server clockwise from hash(key)

**Virtual Nodes:**

- Multiple positions per physical server
- Better load distribution
- Reduces impact of non-uniform hash distribution [12]

#### Applications

**Distributed Systems:**

- **Amazon DynamoDB**: Data partitioning across nodes
- **Apache Cassandra**: Ring-based architecture
- **Content Delivery Networks**: Server selection
- **Load Balancing**: Request distribution [13]

**Advantages:**

- Minimal remapping when nodes join/leave
- Scalable architecture
- Fault tolerance through replication
- Load balancing across nodes [14]

**Practice Understanding:**

- **System Design Interviews**: Core concept for distributed systems
- **GeeksforGeeks**: Consistent hashing implementation [11]
- **InterviewCake**: System design examples [14]

### Bloom Filters

#### Probabilistic Data Structure

**Core Properties:**

- Space-efficient set membership testing
- False positives possible, false negatives impossible
- Multiple hash functions map elements to bit array

**Applications:**

- **Database Systems**: Reduce disk lookups
- **Web Crawling**: Check if URL already visited
- **Caching**: Avoid cache lookups for non-existent keys
- **Network Routing**: Route filtering

#### Advanced Variants

**Counting Bloom Filter:**

- Supports deletions using counters instead of bits
- Higher memory usage but more flexible

**Cuckoo Filter:**

- Better space efficiency than bloom filters
- Supports deletion operations
- Deterministic lookup time

### Distributed Data Structures

#### Merkle Trees

**Structure:**

- Binary tree where leaves are data blocks
- Internal nodes contain hashes of children
- Root hash represents entire dataset integrity

**Applications:**

- **Blockchain**: Block validation and integrity
- **Git Version Control**: Commit tree structure
- **Peer-to-Peer Networks**: Data synchronization
- **Database Replication**: Consistency checking

#### Skip Lists

**Probabilistic Structure:**

- Multiple levels of linked lists
- Higher levels skip more elements
- Randomized balancing instead of rotations

**Advantages over Balanced Trees:**

- Lock-free concurrent implementations
- Simple insertion/deletion algorithms
- Good cache performance

**Applications:**

- **Redis**: Sorted sets implementation
- **Concurrent Programming**: Lock-free data structures
- **Database Indexing**: Alternative to B-trees

## Free Practice Platforms

### Primary Platforms

#### LeetCode (Free Tier)

**Strengths:**

- Extensive problem collection (2500+ problems)
- Company-specific problem sets
- Discussion forums with solutions
- Mock interview environment

**Focus Areas:**

- Data structure fundamentals
- Algorithm pattern recognition
- Time/space complexity analysis

**Free Access:**

- Most problems available without premium
- Basic submission and testing
- Community discussions and hints

#### GeeksforGeeks

**Comprehensive Resource:**

- Detailed tutorials with explanations
- Implementation examples in multiple languages
- Interview experiences from major companies
- Practice problems sorted by company and topic

**Unique Features:**

- Theory combined with practical problems
- Step-by-step algorithm explanations
- Real interview questions from FAANG companies

#### HackerRank

**Structured Learning Path:**

- Domain-specific problem categories
- Progressive difficulty levels
- Certification programs (free)
- Company-sponsored challenges [15]

**Problem Quality:**

- Well-defined input/output formats
- Comprehensive test cases
- Editorial explanations available

### Competitive Programming Platforms

#### Codeforces

**World-Class Platform:**

- Regular contests (2-3 per week)
- Problem difficulty ratings (800-3500)
- Strong community with detailed editorials
- Russian olympiad-style problems [6]

**Learning Benefits:**

- Advanced algorithmic thinking
- Time-pressure problem solving
- Complex data structure applications

#### CodeChef

**Beginner-Friendly Approach:**

- Monthly long challenges (10 days)
- Cook-off and lunchtime contests
- Learning modules with practice problems
- Indian competitive programming community [16]

**Free Features:**

- All contests and practice problems free
- Detailed editorials and discussions
- Skill-based problem recommendations [17]

#### SPOJ (Sphere Online Judge)

**Classical Problems:**

- 20,000+ problems across all topics
- No premium restrictions
- Multiple programming languages supported
- Focus on algorithmic problem solving [18]

**Categories:**

- Classical algorithmic problems
- Data structure implementations [1]
- Mathematical and computational challenges

#### AtCoder

**Japanese Platform:**

- Weekly contests with English statements
- Excellent problem quality and variety
- Strong focus on mathematical problem solving
- Friendly international community

### Specialized Resources

#### Project Euler

**Mathematical Programming:**

- 800+ mathematical/computational problems
- Requires deep thinking and optimization
- No time limits, focus on elegant solutions
- Excellent for developing mathematical insight

#### HackerEarth

**Corporate-Focused:**

- Hiring challenges from tech companies
- Domain-specific tracks (web development, ML)
- Free courses with practical projects
- Good for understanding industry applications [19]

#### Tech Interview Handbook

**Curated Problem Lists:**

- **Blind 75**: Essential LeetCode problems
- **Grind 75**: Extended problem set with schedule
- Pattern-based problem organization
- Interview preparation strategies [20][21]

## Interview Preparation Strategy

### Fellow Engineer Expectations

#### Technical Depth Requirements

**Advanced Problem Solving:**

- Design novel algorithms for complex problems
- Optimize solutions across multiple dimensions
- Handle edge cases and constraint variations
- Explain trade-offs between different approaches

**System Design Integration:**

- Choose appropriate data structures for distributed systems
- Understand consistency, availability, and partition tolerance
- Design for scale (millions of operations per second)
- Consider memory, CPU, and network constraints

#### Communication and Leadership

**Technical Communication:**

- Explain complex algorithms to non-experts
- Justify design decisions with concrete reasoning
- Present multiple solutions with trade-off analysis
- Mentor and guide junior engineers

**Architecture Decisions:**

- Evaluate and recommend data structure choices
- Design systems that can evolve over time
- Balance performance, maintainability, and simplicity
- Consider operational aspects (monitoring, debugging)

### Study Schedule for Senior Roles

#### Months 1-2: Foundation Mastery

**Week 1-2: Arrays and Hashing**

- Master all two-pointer techniques
- Solve 50+ problems across difficulty levels
- Focus on optimization and multiple approaches

**Week 3-4: Linked Lists and Stacks/Queues**

- Implement all variations from scratch
- Master advanced techniques (Floyd's cycle, monotonic stacks)
- Solve 40+ problems including hard-level challenges

**Week 5-6: Trees and Graphs**

- Deep dive into traversal algorithms
- Master shortest path and MST algorithms
- Solve 60+ problems including system design applications

**Week 7-8: Heaps and Advanced Sorting**

- Implement multiple heap variants
- Master selection algorithms and order statistics
- Solve 30+ problems focusing on optimization

#### Months 3-4: Advanced Structures

**Week 9-10: Self-Balancing Trees**

- Implement AVL and Red-Black trees from scratch
- Understand rotation mechanics deeply
- Compare performance characteristics

**Week 11-12: Union-Find and Tries**

- Master path compression and union by rank
- Implement compressed tries and suffix structures
- Solve advanced connectivity problems

**Week 13-14: Segment Trees and Range Queries**

- Build segment trees with lazy propagation
- Implement Fenwick trees for 2D problems
- Solve complex range query challenges

**Week 15-16: String Algorithms**

- Implement KMP, Z-algorithm, and suffix arrays
- Master pattern matching algorithms
- Solve bioinformatics-style problems

#### Months 5-6: Expert Level and System Design

**Week 17-18: Advanced Graph Algorithms**

- Network flow algorithms (Ford-Fulkerson, Dinic's)
- Heavy-light decomposition for trees
- Advanced shortest path algorithms

**Week 19-20: Probabilistic Structures**

- Implement bloom filters and skip lists
- Understand hash function design
- Master consistent hashing applications

**Week 21-22: Distributed System Structures**

- Design distributed hash tables
- Implement vector clocks and CRDTs
- Understand CAP theorem implications

**Week 23-24: Mock Interviews and Integration**

- Practice system design with data structure choices
- Mock interviews focusing on explanation skills
- Review and reinforce weak areas

### Problem-Solving Methodology

#### The UMPIRE Method

**U**nderstand: Clarify requirements and constraints
**M**atch: Identify similar problems and patterns  
**P**lan: Design algorithm with data structure choice
**I**mplement: Code solution with proper error handling
**R**eview: Test with edge cases and optimize
**E**valuate: Analyze time/space complexity and trade-offs

#### Pattern Recognition

**Two Pointers:**

- Array manipulation, palindromes, pair finding
- Sliding window for substring problems
- Fast-slow pointer for linked list cycles

**Divide and Conquer:**

- Tree problems, merge sort variations
- Binary search applications
- Recursive problem decomposition

**Dynamic Programming:**

- Optimal substructure identification
- Memoization vs tabulation approaches
- State space design and optimization

**Greedy Algorithms:**

- Activity selection, scheduling problems
- Huffman coding, fractional knapsack
- Graph algorithms (Dijkstra's, MST)

### Company-Specific Focus

#### Google/Alphabet

- Graph algorithms and tree traversals
- System design with global scale considerations
- Mathematical problem solving ability
- Code elegance and optimization

#### Meta/Facebook

- Social network graph problems
- Real-time system data structures
- Scalability and performance optimization
- User-facing system design

#### Amazon

- E-commerce scale system design
- Distributed system data structures
- Cost optimization and efficiency
- Leadership principle integration

#### Apple

- User experience and performance focus
- Memory-efficient implementations
- Real-time constraints (iOS/macOS)
- Privacy and security considerations

#### Microsoft

- Enterprise-scale system design
- Integration with existing platforms
- Backward compatibility considerations
- Diverse technology stack knowledge

This comprehensive guide provides the theoretical knowledge, practical skills, and strategic preparation needed to excel at Fellow Engineer level interviews at top technology companies. The combination of deep technical understanding, extensive practice, and system design integration will prepare you for the most challenging data structure problems in the industry.

[1] https://www.spoj.com/problems/tag/data-structure-1
[2] https://www.spoj.com/problems/tag/datastructures
[3] https://www.w3schools.com/dsa/dsa_data_avltrees.php
[4] https://www.geeksforgeeks.org/dsa/introduction-to-red-black-tree/
[5] https://www.baeldung.com/cs/red-black-tree-vs-avl-tree
[6] https://daily.dev/blog/choosing-competitive-programming-websites-for-beginners
[7] https://cp-algorithms.com/data_structures/disjoint_set_union.html
[8] https://www.math.umd.edu/~immortal/CMSC420/notes/disjointset.pdf
[9] https://www.hackerearth.com/practice/notes/disjoint-set-union-union-find/
[10] https://www.geeksforgeeks.org/dsa/introduction-to-disjoint-set-data-structure-or-union-find-algorithm/
[11] https://www.geeksforgeeks.org/system-design/consistent-hashing/
[12] https://www.linkedin.com/pulse/system-design-interview-all-consistent-hashing-examples-sapra
[13] https://www.hellointerview.com/learn/system-design/deep-dives/consistent-hashing
[14] https://www.interviewcake.com/concept/java/consistent-hashing
[15] https://www.reddit.com/r/codinginterview/comments/ztdf3n/coding_interview_preparation_resources/
[16] https://daily.dev/blog/competitive-programming-websites-for-beginners-a-guide
[17] https://www.youtube.com/watch?v=6iR-JX7Zysg
[18] https://www.spoj.com/problems/basics/sort=2
[19] https://www.hackerearth.com/practice/
[20] https://github.com/yangshun/tech-interview-handbook
[21] https://www.techinterviewhandbook.org/best-practice-questions/
[22] https://www.interviewbit.com/data-structure-interview-questions/
[23] https://www.geeksforgeeks.org/dsa/advanced-data-structures/
[24] https://www.geeksforgeeks.org/system-design/system-design-interviews-faang/
[25] https://www.datacamp.com/blog/top-21-data-engineering-interview-questions-and-answers
[26] https://www.techinterviewhandbook.org/algorithms/study-cheatsheet/
[27] https://www.hellointerview.com/learn/system-design/deep-dives/data-structures-for-big-data
[28] https://workat.tech/problem-solving/lists/faang-interview-questions/practice
[29] https://www.geeksforgeeks.org/complete-roadmap-to-learn-dsa-from-scratch/
[30] https://igotanoffer.com/blogs/tech/system-design-interviews
[31] https://igotanoffer.com/blogs/tech/facebook-data-engineer-interview
[32] https://www.lpude.in/SLMs/Master%20of%20Computer%20Applications/Sem_2/DECAP770_ADVANCED_DATA_STRUCTURES.pdf
[33] https://www.reddit.com/r/learnmachinelearning/comments/1glkkve/faang_ml_system_design_interview_guide/
[34] https://www.youtube.com/watch?v=4W-q6nvh3bs
[35] https://www.reddit.com/r/webdev/comments/clvw5x/how_often_do_you_use_advanced_data_structures_and/
[36] https://www.tryexponent.com/blog/system-design-interview-guide
[37] https://www.pathrise.com/guides/software-engineer-interview-questions/
[38] https://codesignal.com/learn/paths/advanced-interview-prep-for-senior-engineers-in-java
[39] https://github.com/AkashSingh3031/The-Complete-FAANG-Preparation
[40] https://igotanoffer.com/blogs/tech/google-software-engineer-interview
[41] https://outscal.com/course/data-structure-advanced
[42] https://dev.to/dfs_with_memo/leetcode-warmup-problems-13o7
[43] https://www.geeksforgeeks.org/advance-data-structure/
[44] https://github.com/ashishps1/awesome-leetcode-resources
[45] https://www.youtube.com/watch?v=xo7XrRVxH8Y
[46] https://techdevguide.withgoogle.com/paths/data-structures-and-algorithms/
[47] https://github.com/17tanya/Leetcode-Data-Structures-and-Algorithms
[48] https://www.youtube.com/watch?v=2FJukOtis70
[49] https://www.youtube.com/watch?v=lvO88XxNAzs
[50] https://www.w3schools.com/dsa/
[51] https://leetcode.com/explore/interview/card/leetcodes-interview-crash-course-data-structures-and-algorithms/703/arraystrings/
[52] https://www.youtube.com/watch?v=TFdLT7Hi2QE
[53] https://leetcode.com/problemset/
[54] https://www.youtube.com/playlist?list=PLqM7alHXFySF7JxK9E24C-ZeNAXFB1u8k
[55] https://www.hackerrank.com/domains/data-structures
[56] https://www.codechef.com/roadmap/data-structures-and-algorithms
[57] https://www.algotutor.io/assets/img/notes/dsAndAlgo/pdf/75%20Most%20Important%20LeetCode%20DSA%20Questions.pdf
[58] https://in.linkedin.com/company/geeksforgeeks
[59] https://leetcode.com/problem-list/linked-list/
[60] https://www.reddit.com/r/learnprogramming/comments/146e5f8/best_course_for_data_structures_and_algorithms/
[61] https://blogs.30dayscoding.com/blogs/dsa/advanced-data-structures/segment-trees-and-fenwick-trees/fenwick-trees/
[62] https://dev.to/mukeshkuiry/top-6-system-design-patterns-to-ace-every-system-design-interview-4388
[63] https://www.foundit.in/career-advice/data-structures-and-algorithms-interview-questions/
[64] https://beta.shawonnotes.com/computer-science/coding-interview/20-fenwick-tree.html
[65] https://towardsdatascience.com/system-design-bloom-filter-a2e19dcd4810/
[66] https://www.apollotechnical.com/data-structures-and-algorithms-interview-questions/
[67] https://www.designgurus.io/blog/advanced-data-structure-patterns-for-competitive-coding
[68] https://www.geeksforgeeks.org/system-design/bloom-filters-in-system-design/
[69] https://www.geeksforgeeks.org/dsa/fenwick-tree-vs-segment-tree/
[70] https://theawesomenayak.hashnode.dev/system-design-102-bloom-filters
[71] https://cp-algorithms.com/data_structures/fenwick.html
[72] https://systemdesign.one/bloom-filters-explained/
[73] https://www.guvi.in/blog/dsa-interview-questions-and-answers/
[74] https://www.numberanalytics.com/blog/fenwick-trees-deep-dive-advanced-data-structures
[75] https://www.pramp.com/dev/uc-data-structures-and-algorithms
[76] https://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/
[77] https://www.youtube.com/watch?v=V3pzxngeLqw
[78] https://www.reddit.com/r/learnprogramming/comments/zh9brb/how_do_you_learn_data_structures_and_algorithms/
[79] https://github.com/bollwarm/DataStructuresAlgorithms
[80] https://github.com/ombharatiya/FAANG-Coding-Interview-Questions
[81] https://www.reddit.com/r/leetcode/comments/rq724s/codeforcescodecheftopcoderkagglefirecodehackerrank/
[82] https://dev.to/naime_molla/top-10-free-resources-to-learn-data-structures-and-algorithms-in-2024-4i4j
[83] https://www.techinterviewhandbook.org
[84] https://www.sololearn.com/en/Discuss/2499579/for-beginners-which-platform-is-best-for-competitive-programming-hackerrank-hackerearth-codechef-or-codeforces
[85] https://www.hackerearth.com/practice/data-structures/arrays/1-d/practice-problems/
[86] https://capd.mit.edu/resources/coding-interview-prep-relax-start-here/
[87] https://pesto.tech/resources/top-10-websites-to-practice-coding-challenges-in-2024
[88] https://github.com/rupaai/interview-preparation-faang
[89] https://www.geeksforgeeks.org/blogs/best-coding-platform-websites/
[90] https://workat.tech/problem-solving/practice/topics
[91] https://www.freecodecamp.org/news/coding-interview-prep-for-big-tech/
[92] https://www.hackerrank.com
[93] https://wiki.python.org/moin/BeginnersGuide/NonProgrammers
[94] https://www.scaler.com/topics/data-structures/disjoint-set/
[95] https://www.geeksforgeeks.org/dsa/red-black-tree-vs-avl-tree/
[96] https://www.youtube.com/watch?v=4kd40gEKaLM
[97] https://egyankosh.ac.in/bitstream/123456789/83393/1/Unit-7.pdf
[98] https://en.wikipedia.org/wiki/Disjoint-set_data_structure
[99] https://www.tryexponent.com/courses/system-design-interviews/consistent-hashing
[100] https://www.geeksforgeeks.org/dsa/disjoint-set-data-structures/
[101] https://www.youtube.com/watch?v=EmxWC18iuSc
[102] https://www.slideshare.net/samrinriya/balanced-tree-avl-tree-redblack-tree
[103] https://dev.to/dhyaneshsiddhartha15/10-best-websites-for-practicing-data-structures-and-algorithms-dsa-3n39
[104] https://github.com/arindal1/CodeChefProblems
[105] https://www.codechef.com
[106] https://dwf.dev/docs/learning-resources/tech-interview-handbook
[107] https://www.youtube.com/watch?v=CBYHwZcbD-s
[108] https://www.codechef.com/practice/python
[109] https://www.codechef.com/practice-old
[110] https://www.techinterviewhandbook.org/coding-interview-prep/

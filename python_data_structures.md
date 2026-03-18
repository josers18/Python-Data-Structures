# Python Data Structures & Algorithms

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)
![Topics](https://img.shields.io/badge/Topics-12-blueviolet)

Personal reference guide covering core data structures and algorithms in Python — from Big O fundamentals through sorting, searching, trees, graphs, and dynamic programming.

---

## Table of Contents

- [Definitions](#definitions)
- [Big O Notation](#big-o-notation)
- [Linked Lists](#linked-lists)
- [Stacks](#stacks)
- [Queues](#queues)
- [Hash Tables](#hash-tables)
- [Trees](#trees)
- [Graphs](#graphs)
- [Recursion](#recursion)
- [Dynamic Programming](#dynamic-programming)
- [Searching Algorithms](#linear-search--binary-search)
- [Binary Search Trees](#binary-search-tree-bst)
- [Tree Traversal — DFS](#depth-first-search-dfs)
- [Tree Traversal — BFS](#breadth-first-search-bfs)
- [Heaps & Priority Queues](#heaps--priority-queues)
- [Sorting Algorithms](#sorting-algorithms)
- [Complexity Cheat Sheet](#complexity-cheat-sheet)

---

## Definitions

- **Algorithm** — a set of instructions that solve a problem
- **Data Structure** — holds and manipulates data when we execute an algorithm

> **Rule of thumb:** choose the right data structure first, then the right algorithm. The combination determines your complexity.

---

## Big O Notation

Measures the **worst-case complexity** of an algorithm as input size grows.

- **Time complexity** — how long it takes to run
- **Space complexity** — how much extra memory it needs

| Notation | Name | Description |
|---|---|---|
| O(1) | Constant | Always the same regardless of input size |
| O(log n) | Logarithmic | Halves the problem each step (binary search) |
| O(n) | Linear | One operation per input element |
| O(n log n) | Linearithmic | Efficient sorts (merge sort, quicksort average) |
| O(n²) | Quadratic | Nested loops over the same input |
| O(2ⁿ) | Exponential | Doubles with each additional input |
| O(n!) | Factorial | All permutations (travelling salesman brute force) |

> **Order of growth (best to worst):** O(1) → O(log n) → O(n) → O(n log n) → O(n²) → O(2ⁿ) → O(n!)

### Simplification Rules

1. **Drop constants** — O(4 + 2n + 2m) → O(n + m)
2. **Different inputs get different variables** — two loops over different inputs = O(n + m), not O(2n)
3. **Drop non-dominant terms** — O(n + n²) → O(n²)

### Code Examples

```python
# O(1) — constant: always accesses index 2 regardless of list size
colors = ["red", "green", "blue"]

def constant(colors):
    print(colors[2])


# O(n) — linear: one operation per element
def linear(colors):
    for color in colors:
        print(color)


# O(n²) — quadratic: nested loop over the same input
def quadratic(colors):
    for color in colors:
        for color2 in colors:
            print(color, color2)


# O(n³) — cubic: triple nested loop
def cubic(colors):
    for color in colors:
        for color2 in colors:
            for color3 in colors:
                print(color, color2, color3)


# Calculating combined complexity
colors = ["red", "green", "blue"]             # O(1)
other_colors = ["yellow", "orange", "purple"] # O(1)

def complex_algorithm(colors):
    color_count = 0                      # O(1)
    for color in colors:
        print(color)                     # O(n)
        color_count += 1                 # O(n)
    for other_color in other_colors:
        print(other_color)               # O(m)
        color_count += 1                 # O(m)
    print(color_count)                   # O(1)

# Total: O(4 + 2n + 2m) → simplify → O(n + m)
```

---

## Linked Lists

A **sequence of data elements connected through links**. Each element is called a **node**.

### Node structure
- **Data** — the value stored
- **Pointer** — reference to the next node

### Key properties
- Last node points to `None`
- First node = **Head**, last node = **Tail**
- Nodes can live at any memory address (not contiguous like arrays)
- **Singly linked list** — each node has one link (forward only)
- **Doubly linked list** — each node has two links (forward and backward)

### When to use
- Frequent insertions/deletions at the beginning or end
- Unknown or dynamic size
- Avoid if you need frequent random access by index (use a list instead)

### Real-world uses
- Web browser back/forward navigation
- Music playlist next/previous
- Undo/redo functionality
- Building blocks for stacks, queues, and graphs

### Implementation

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None


class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def insert_at_beginning(self, data):
        new_node = Node(data)
        if self.head:
            new_node.next = self.head
            self.head = new_node
        else:
            self.head = new_node
            self.tail = new_node

    def insert_at_end(self, data):
        new_node = Node(data)
        if self.head:
            self.tail.next = new_node
            self.tail = new_node
        else:
            self.head = new_node
            self.tail = new_node

    def remove_at_beginning(self):
        if not self.head:
            return None
        removed = self.head.data
        self.head = self.head.next
        if not self.head:
            self.tail = None
        return removed

    def search(self, data):
        current_node = self.head
        while current_node:
            if current_node.data == data:
                return True
            current_node = current_node.next
        return False

    def display(self):
        elements = []
        current_node = self.head
        while current_node:
            elements.append(str(current_node.data))
            current_node = current_node.next
        return " -> ".join(elements) + " -> None"


# Usage
ll = LinkedList()
ll.insert_at_end(1)
ll.insert_at_end(2)
ll.insert_at_end(3)
ll.insert_at_beginning(0)
print(ll.display())   # 0 -> 1 -> 2 -> 3 -> None
print(ll.search(2))   # True
```

| Operation | Singly Linked List |
|---|---|
| Access by index | O(n) |
| Search | O(n) |
| Insert at head | O(1) |
| Insert at tail | O(1) with tail pointer |
| Delete at head | O(1) |
| Delete at tail | O(n) singly / O(1) doubly |

---

## Stacks

**LIFO — Last In, First Out**

> Think of a stack of plates: you always add and remove from the top.

| Operation | Description |
|---|---|
| **Push** | Add an item to the top |
| **Pop** | Remove and return the top item |
| **Peek** | Read the top item without removing it |

### When to use
- Undo/redo operations
- Function call management (the call stack)
- Expression parsing and parentheses matching
- Backtracking algorithms and DFS

### Implementation

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None


class Stack:
    def __init__(self):
        self.top = None
        self.height = 0

    def push(self, value):
        new_node = Node(value)
        if self.height == 0:
            self.top = new_node
        else:
            new_node.next = self.top
            self.top = new_node
        self.height += 1

    def pop(self):
        if self.height == 0:
            return None
        temp = self.top
        self.top = self.top.next
        temp.next = None
        self.height -= 1
        return temp.value

    def peek(self):
        if self.height == 0:
            return None
        return self.top.value

    def is_empty(self):
        return self.height == 0


# Python built-in: use a list as a stack
stack = []
stack.append(1)   # push
stack.append(2)
stack.append(3)
print(stack[-1])  # peek -> 3
stack.pop()       # pop -> 3
```

| Operation | Complexity |
|---|---|
| Push | O(1) |
| Pop | O(1) |
| Peek | O(1) |
| Search | O(n) |

---

## Queues

**FIFO — First In, First Out**

> Think of a line at a restaurant: the first person in line is the first to be served.

| Operation | Description |
|---|---|
| **Enqueue** | Add an item to the end |
| **Dequeue** | Remove and return the front item |
| **Peek** | Read the front item without removing it |

### Queue variants
- **Simple Queue** — standard FIFO
- **Double-Ended Queue (deque)** — add/remove from both ends
- **Circular Queue** — tail wraps back to head
- **Priority Queue** — elements dequeued by priority, not insertion order

### When to use
- Task and job scheduling
- Print spooling
- BFS traversal
- Buffering data streams

### Implementation

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None


class Queue:
    def __init__(self):
        self.first = None
        self.last = None
        self.length = 0

    def enqueue(self, value):
        new_node = Node(value)
        if self.length == 0:
            self.first = new_node
            self.last = new_node
        else:
            self.last.next = new_node
            self.last = new_node
        self.length += 1

    def dequeue(self):
        if self.length == 0:
            return None
        temp = self.first
        if self.length == 1:
            self.first = None
            self.last = None
        else:
            self.first = self.first.next
            temp.next = None
        self.length -= 1
        return temp.value

    def peek(self):
        if self.length == 0:
            return None
        return self.first.value

    def is_empty(self):
        return self.length == 0


# Python built-in: queue module
import queue

orders_queue = queue.SimpleQueue()
orders_queue.put('Sushi')
orders_queue.put('Lasagna')
orders_queue.put('Paella')

print("Size:", orders_queue.qsize())   # 3
print(orders_queue.get())              # Sushi
print(orders_queue.get())              # Lasagna
print(orders_queue.get())              # Paella
print("Empty:", orders_queue.empty())  # True

# Python built-in: collections.deque (most efficient)
from collections import deque
dq = deque()
dq.append('a')       # enqueue right
dq.appendleft('b')   # enqueue left
dq.pop()             # dequeue right
dq.popleft()         # dequeue left
```

| Operation | Complexity |
|---|---|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Peek | O(1) |
| Search | O(n) |

---

## Hash Tables

Stores a collection of items as **key-value pairs**. A **hash function** maps each key to a memory location.

- Every language has one: Python `dict`, JavaScript `Object/Map`, Java `HashMap`
- Hash function must return the **same output for the same input every time**
- Average lookup: **O(1)**

### When to use
- Fast lookup by key
- Counting frequencies
- Caching and memoization
- Deduplication

```python
# Creating
my_menu = {'pizza': 12, 'pasta': 9, 'salad': 7}

# Reading
print(my_menu['pizza'])          # 12
print(my_menu.get('pizza', 0))   # 12 — safe, returns 0 if key missing

# Adding / Updating
my_menu['samosas'] = 13
my_menu['pizza'] = 15

# Deleting
del my_menu['samosas']
my_menu.pop('pasta', None)       # safe remove
my_menu.clear()                  # remove all

# Iterating
for key, value in my_menu.items():
    print(f"{key}: {value}")

# Checking membership
if 'pizza' in my_menu:
    print("pizza is on the menu")

# Common pattern: counting frequencies
words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple']
freq = {}
for word in words:
    freq[word] = freq.get(word, 0) + 1
print(freq)  # {'apple': 3, 'banana': 2, 'cherry': 1}
```

| Operation | Average | Worst |
|---|---|---|
| Lookup | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |

> Worst case O(n) occurs on hash collisions — rare with good hash functions.

---

## Trees

**Node-based data structures** where each node can link to more than one child node.

### Terminology

| Term | Definition |
|---|---|
| **Root** | Topmost node — has no parent |
| **Parent** | A node with children |
| **Child** | A node connected below a parent |
| **Leaf** | A node with no children |
| **Level / Depth** | Distance from the root |
| **Height** | Longest path from root to a leaf |

### Binary Tree

Each node has **0, 1, or 2 children**.

```python
class TreeNode:
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right


#       A
#      / \
#     B   C
node1 = TreeNode("B")
node2 = TreeNode("C")
root_node = TreeNode("A", node1, node2)
```

### Real-world uses
- Computer file systems
- HTML DOM structure
- Chess move trees
- Searching and sorting algorithms

---

## Graphs

A graph is a set of **vertices (nodes)** connected by **edges (links)**.

> Trees are a special type of graph — connected, acyclic, and directed.

| Property | Trees | Graphs |
|---|---|---|
| Cycles | No | Yes |
| All nodes connected | Yes | Optional |
| Direction | Implied root-down | Directed or undirected |

### Types
- **Directed** — edges have a direction (A → B)
- **Undirected** — edges are bidirectional (A — B)
- **Weighted** — edges carry a numeric value (distance, cost)

### When to use
- Social networks (followers, friends, likes)
- Maps and route optimization
- Recommendation engines
- Dependency resolution
- Graph databases (Neo4j)

### Implementation

```python
class Graph:
    def __init__(self):
        self.vertices = {}   # adjacency list

    def add_vertex(self, vertex):
        if vertex not in self.vertices:
            self.vertices[vertex] = []

    def add_edge(self, source, target):
        self.vertices[source].append(target)

    def add_undirected_edge(self, v1, v2):
        self.vertices[v1].append(v2)
        self.vertices[v2].append(v1)

    def display(self):
        for vertex, neighbors in self.vertices.items():
            print(f"{vertex} -> {neighbors}")


my_graph = Graph()
my_graph.add_vertex('David')
my_graph.add_vertex('Miriam')
my_graph.add_vertex('Martin')

my_graph.add_edge('David', 'Miriam')
my_graph.add_edge('Miriam', 'Martin')
my_graph.add_edge('Martin', 'David')

my_graph.display()
# David -> ['Miriam']
# Miriam -> ['Martin']
# Martin -> ['David']
```

---

## Recursion

A function that **calls itself** to solve a smaller version of the same problem.

Every recursive function needs:
1. **Base case** — the condition that stops the recursion
2. **Recursive case** — the call that moves toward the base case

> Without a base case, recursion becomes infinite and causes a stack overflow.

```python
# Iterative factorial
def factorial_iterative(n):
    result = 1
    while n > 1:
        result *= n
        n -= 1
    return result


# Recursive factorial
def factorial_recursive(n):
    if n <= 1:           # base case
        return 1
    return n * factorial_recursive(n - 1)   # recursive case

# Trace: factorial_recursive(5)
# 5 * factorial_recursive(4)
# 5 * 4 * factorial_recursive(3)
# 5 * 4 * 3 * factorial_recursive(2)
# 5 * 4 * 3 * 2 * factorial_recursive(1)
# 5 * 4 * 3 * 2 * 1 = 120

print(factorial_recursive(5))   # 120
```

---

## Dynamic Programming

An **optimization technique** applied to recursion that solves complex problems by breaking them into **overlapping subproblems** and **storing results** to avoid recomputation.

**Apply when:**
- The problem can be divided into smaller subproblems
- Subproblems overlap (the same subproblem appears multiple times)

This caching technique is called **memoization**.

### Two approaches

| Approach | Description |
|---|---|
| **Top-down (Memoization)** | Recursive + cache results in a dict |
| **Bottom-up (Tabulation)** | Iterative — build solution from smallest subproblems up |

### Example: Fibonacci

```python
# Naive recursion — O(2^n) — extremely slow for large n
def fib_naive(n):
    if n <= 1:
        return n
    return fib_naive(n - 1) + fib_naive(n - 2)


# Top-down memoization — O(n)
def fib_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]


# Bottom-up tabulation — O(n) time, O(n) space
def fib_tabulation(n):
    if n <= 1:
        return n
    table = [0] * (n + 1)
    table[1] = 1
    for i in range(2, n + 1):
        table[i] = table[i - 1] + table[i - 2]
    return table[n]


# Space-optimized — O(n) time, O(1) space
def fib_optimized(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b


print(fib_naive(10))        # 55
print(fib_memo(50))         # 12586269025
print(fib_tabulation(50))   # 12586269025
print(fib_optimized(50))    # 12586269025
```

### Example: Coin Change (minimum coins)

```python
def coin_change(coins, amount):
    """Returns the minimum number of coins to make amount, or -1 if impossible."""
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1

print(coin_change([1, 5, 10, 25], 36))   # 3  (25 + 10 + 1)
```

---

## Linear Search & Binary Search

### Linear Search

Loops through each element until the target is found.

```python
def linear_search(unordered_list, search_value):
    for index in range(len(unordered_list)):
        if unordered_list[index] == search_value:
            return index
    return -1

print(linear_search([15, 2, 21, 3, 12, 7, 8], 8))   # 6
```

- **Complexity:** O(n)
- Works on **unsorted** lists
- Best for small lists or one-time searches

### Binary Search

Repeatedly halves the search space. **Requires a sorted list.**

```python
def binary_search(ordered_list, search_value):
    first = 0
    last = len(ordered_list) - 1

    while first <= last:
        middle = (first + last) // 2
        if ordered_list[middle] == search_value:
            return middle
        elif search_value < ordered_list[middle]:
            last = middle - 1
        else:
            first = middle + 1
    return -1

print(binary_search([2, 5, 8, 12, 16, 23, 38, 56, 72, 91], 23))   # 5

# Python built-in
import bisect
sorted_list = [2, 5, 8, 12, 16, 23, 38]
idx = bisect.bisect_left(sorted_list, 12)   # 3
```

| | Linear Search | Binary Search |
|---|---|---|
| Complexity | O(n) | O(log n) |
| Requires sorted | No | Yes |
| Best for | Small/unsorted data | Large sorted data |

---

## Binary Search Tree (BST)

A binary tree where:
- **Left subtree** contains only values **less than** the node
- **Right subtree** contains only values **greater than** the node
- Both subtrees are also valid BSTs

```
       8
      / \
     3   10
    / \    \
   1   6    14
      / \   /
     4   7 13
```

```python
class TreeNode:
    def __init__(self, data, left=None, right=None):
        self.data = data
        self.left_child = left
        self.right_child = right


class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, data):
        if self.root is None:
            self.root = TreeNode(data)
        else:
            self._insert_recursively(self.root, data)

    def _insert_recursively(self, node, data):
        if data < node.data:
            if node.left_child is None:
                node.left_child = TreeNode(data)
            else:
                self._insert_recursively(node.left_child, data)
        else:
            if node.right_child is None:
                node.right_child = TreeNode(data)
            else:
                self._insert_recursively(node.right_child, data)

    def search(self, data):
        return self._search_recursively(self.root, data)

    def _search_recursively(self, node, data):
        if node is None:
            return False
        if node.data == data:
            return True
        elif data < node.data:
            return self._search_recursively(node.left_child, data)
        else:
            return self._search_recursively(node.right_child, data)


bst = BinarySearchTree()
for val in [8, 3, 10, 1, 6, 14, 4, 7, 13]:
    bst.insert(val)

print(bst.search(6))    # True
print(bst.search(99))   # False
```

### When to use
- Maintain sorted data dynamically
- Faster search than arrays and linked lists
- Faster insert/delete than sorted arrays
- Foundation for dynamic sets, lookup tables, priority queues

| Operation | Average | Worst (unbalanced) |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

---

## Depth First Search (DFS)

Tree/graph traversal that explores **as far as possible down each branch** before backtracking.

### DFS on Binary Trees — 3 orderings

```
       A
      / \
     B   C
    / \
   D   E
```

| Order | Pattern | Result |
|---|---|---|
| **In-order** | Left → Current → Right | D, B, E, A, C |
| **Pre-order** | Current → Left → Right | A, B, D, E, C |
| **Post-order** | Left → Right → Current | D, E, B, C, A |

```python
def in_order(self, current_node):
    """Produces sorted output when used on a BST."""
    if current_node:
        self.in_order(current_node.left_child)
        print(current_node.data)
        self.in_order(current_node.right_child)

def pre_order(self, current_node):
    """Useful for copying or serializing a tree."""
    if current_node:
        print(current_node.data)
        self.pre_order(current_node.left_child)
        self.pre_order(current_node.right_child)

def post_order(self, current_node):
    """Useful for deleting a tree."""
    if current_node:
        self.post_order(current_node.left_child)
        self.post_order(current_node.right_child)
        print(current_node.data)
```

### DFS on Graphs (iterative with stack)

```python
def dfs_graph(graph, start):
    visited = set()
    stack = [start]
    result = []
    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            result.append(vertex)
            for neighbor in graph[vertex]:
                if neighbor not in visited:
                    stack.append(neighbor)
    return result

graph = {
    'A': ['B', 'C'],
    'B': ['D', 'E'],
    'C': ['F'],
    'D': [], 'E': [], 'F': []
}
print(dfs_graph(graph, 'A'))   # ['A', 'C', 'F', 'B', 'E', 'D']
```

### Use cases

| Traversal | Use |
|---|---|
| In-order | Get BST values in ascending order |
| Pre-order | Copy/serialize a tree, prefix expressions |
| Post-order | Delete a tree, postfix expressions |
| DFS (graph) | Cycle detection, topological sort, pathfinding |

- **Complexity:** O(n) trees · O(V + E) graphs

---

## Breadth First Search (BFS)

Tree/graph traversal that visits every node **level by level**, using a queue.

> DFS goes deep first. BFS goes wide first.

### BFS on Binary Trees

```python
import queue

def bfs(self):
    if not self.root:
        return []
    visited_nodes = []
    bfs_queue = queue.SimpleQueue()
    bfs_queue.put(self.root)
    while not bfs_queue.empty():
        current_node = bfs_queue.get()
        visited_nodes.append(current_node.data)
        if current_node.left_child:
            bfs_queue.put(current_node.left_child)
        if current_node.right_child:
            bfs_queue.put(current_node.right_child)
    return visited_nodes
```

### BFS on Graphs

```python
def bfs_graph(graph, initial_vertex):
    visited_vertices = []
    bfs_queue = queue.SimpleQueue()
    bfs_queue.put(initial_vertex)
    visited_vertices.append(initial_vertex)
    while not bfs_queue.empty():
        current_vertex = bfs_queue.get()
        for neighbor in graph[current_vertex]:
            if neighbor not in visited_vertices:
                visited_vertices.append(neighbor)
                bfs_queue.put(neighbor)
    return visited_vertices
```

| | DFS | BFS |
|---|---|---|
| Data structure used | Stack (or recursion) | Queue |
| Memory usage | O(h) — tree height | O(w) — tree width |
| Finds shortest path | No | Yes (unweighted graphs) |
| Best for | Deep searches, cycle detection | Shortest path, level-order |

- **Complexity:** O(n) trees · O(V + E) graphs

---

## Heaps & Priority Queues

A **heap** is a complete binary tree with the heap property:
- **Min-heap** — parent is always ≤ children (root = minimum element)
- **Max-heap** — parent is always ≥ children (root = maximum element)

> A **priority queue** is an abstract data type best implemented with a heap.

### When to use
- Always need the min or max element quickly
- Task scheduling by priority
- Dijkstra's shortest path algorithm
- Heap sort

```python
import heapq

# Python's heapq is a MIN-heap by default
min_heap = []
heapq.heappush(min_heap, 5)
heapq.heappush(min_heap, 1)
heapq.heappush(min_heap, 8)
heapq.heappush(min_heap, 3)

print(heapq.heappop(min_heap))   # 1 — always the smallest
print(heapq.heappop(min_heap))   # 3
print(min_heap[0])                # peek at current min

# Build heap from existing list — O(n)
data = [5, 1, 8, 3, 7, 2]
heapq.heapify(data)
print(heapq.heappop(data))       # 1

# Simulate MAX-heap by negating values
max_heap = []
for val in [5, 1, 8, 3]:
    heapq.heappush(max_heap, -val)
print(-heapq.heappop(max_heap))  # 8

# N largest / N smallest
nums = [5, 1, 8, 3, 7, 2, 9, 4, 6]
print(heapq.nlargest(3, nums))    # [9, 8, 7]
print(heapq.nsmallest(3, nums))   # [1, 2, 3]
```

| Operation | Complexity |
|---|---|
| Push | O(log n) |
| Pop min/max | O(log n) |
| Peek min/max | O(1) |
| Heapify (build from list) | O(n) |

---

## Sorting Algorithms

### Comparison Overview

| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |

> **Stable** means equal elements maintain their original relative order.

---

### Bubble Sort

Repeatedly swaps adjacent elements that are out of order. The largest element "bubbles" to the end each pass.

```python
# Basic bubble sort
def bubble_sort(my_list):
    list_length = len(my_list)
    for i in range(list_length - 1):
        for j in range(list_length - 1 - i):
            if my_list[j] > my_list[j + 1]:
                my_list[j], my_list[j + 1] = my_list[j + 1], my_list[j]
    return my_list


# Optimized: exit early if no swaps occurred (already sorted)
def bubble_sort_optimized(my_list):
    list_length = len(my_list)
    is_sorted = False
    while not is_sorted:
        is_sorted = True
        for i in range(list_length - 1):
            if my_list[i] > my_list[i + 1]:
                my_list[i], my_list[i + 1] = my_list[i + 1], my_list[i]
                is_sorted = False
        list_length -= 1
    return my_list

print(bubble_sort([64, 34, 25, 12, 22, 11, 90]))
# [11, 12, 22, 25, 34, 64, 90]
```

- **Complexity:** O(n²) average/worst · O(n) best (already sorted, optimized version)

---

### Selection Sort

Finds the minimum element and places it at the front, one pass at a time.

```python
def selection_sort(my_list):
    list_length = len(my_list)
    for i in range(list_length - 1):
        lowest = my_list[i]
        index = i
        for j in range(i + 1, list_length):
            if my_list[j] < lowest:
                index = j
                lowest = my_list[j]
        my_list[i], my_list[index] = my_list[index], my_list[i]
    return my_list

print(selection_sort([64, 25, 12, 22, 11]))
# [11, 12, 22, 25, 64]
```

- **Complexity:** O(n²) always — same number of comparisons regardless of input

---

### Insertion Sort

Builds the sorted array one element at a time by inserting each element into its correct position.

```python
def insertion_sort(my_list):
    for i in range(1, len(my_list)):
        number_to_order = my_list[i]
        j = i - 1
        while j >= 0 and number_to_order < my_list[j]:
            my_list[j + 1] = my_list[j]
            j -= 1
        my_list[j + 1] = number_to_order
    return my_list

print(insertion_sort([12, 11, 13, 5, 6]))
# [5, 6, 11, 12, 13]
```

- **Complexity:** O(n²) average/worst · O(n) best (already sorted)
- Good for small lists and nearly sorted data

---

### Merge Sort

**Divide and conquer:** split list in half, sort each half recursively, merge back together.

```
[38, 27, 43, 3]  →  [38, 27] [43, 3]
                 →  [38] [27] [43] [3]
                 →  [27, 38] [3, 43]
                 →  [3, 27, 38, 43]
```

```python
def merge_sort(my_list):
    if len(my_list) <= 1:
        return my_list
    mid = len(my_list) // 2
    left_half = merge_sort(my_list[:mid])
    right_half = merge_sort(my_list[mid:])
    return merge(left_half, right_half)


def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result

print(merge_sort([38, 27, 43, 3, 9, 82, 10]))
# [3, 9, 10, 27, 38, 43, 82]
```

- **Complexity:** O(n log n) always
- **Space:** O(n) — requires extra memory for merging
- Good for large datasets, linked lists, and when stable sort is required

---

### Quick Sort

**Divide and conquer using a pivot:** elements smaller than pivot go left, larger go right, then recurse on both sides.

```python
def quick_sort(my_list, first_index, last_index):
    if first_index < last_index:
        pivot_index = partition(my_list, first_index, last_index)
        quick_sort(my_list, first_index, pivot_index)
        quick_sort(my_list, pivot_index + 1, last_index)
    return my_list


def partition(my_list, first_index, last_index):
    pivot = my_list[first_index]
    left_index = first_index + 1
    right_index = last_index

    while True:
        while left_index <= right_index and my_list[left_index] < pivot:
            left_index += 1
        while right_index >= left_index and my_list[right_index] > pivot:
            right_index -= 1
        if left_index >= right_index:
            break
        my_list[left_index], my_list[right_index] = my_list[right_index], my_list[left_index]

    my_list[first_index], my_list[right_index] = my_list[right_index], my_list[first_index]
    return right_index


my_list = [10, 7, 8, 9, 1, 5]
print(quick_sort(my_list, 0, len(my_list) - 1))
# [1, 5, 7, 8, 9, 10]
```

- **Complexity:** O(n log n) average · O(n²) worst (bad pivot choice)
- **Space:** O(log n) for the call stack
- Fastest in practice for most in-place sorting scenarios

---

## Complexity Cheat Sheet

### Data Structures

| Structure | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| Array / List | O(1) | O(n) | O(n) | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1) | O(1) | O(n) |
| Stack | O(n) | O(n) | O(1) | O(1) | O(n) |
| Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| Hash Table | N/A | O(1) avg | O(1) avg | O(1) avg | O(n) |
| BST (balanced) | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Heap | O(1) min/max | O(n) | O(log n) | O(log n) | O(n) |

### Sorting Algorithms

| Algorithm | Best | Average | Worst | Space |
|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |

### Quick decision guide

| Need | Best choice |
|---|---|
| Fast lookup by key | Hash Table |
| Sorted data + fast search | BST or sorted list + binary search |
| Always need min or max quickly | Heap / Priority Queue |
| LIFO — undo, call stack | Stack |
| FIFO — scheduling, BFS | Queue |
| Frequent head insertions/deletions | Linked List |
| Relationships between entities | Graph |
| Hierarchical / nested data | Tree |
| Overlapping subproblems | Dynamic Programming |

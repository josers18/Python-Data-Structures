# Data Structures and Algorithms

- **Algorithm**: set of instructions that solve a problem
- **Data Structures**: Hold and manipulate data when we execute and algorithm

---

## Linked Lists

- Seuqnece of data connectede through links, each element called a node
  - 2 Parts
    - Data
    - Pointer to the next node
- Last link points to Null
- First node is the Head
- Last node is the Tail
- can be in any memory address

- If each node only has 1 link, its called a singly linked list
- If each node has a link in either direction its called a double linked list

- Can be used to implement other data structures like:
  - Stacks
  - Queues
  - Graphs

- Access information by navigating backward and forward
  - web browser
  - Music playlist
  
  ```Python
  class LinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
    # methods
    insert_at_beginning()
    remove_at_beginning()
    insert_at_end()
    remove_at_end()
    insert_at()
    remove_at()
    search()

    def insert_at_beginning(self, data):
        new_node = Node(data)
        if self.head:
            new_node.next = self.head
            self.head = new_node
        else:
            self.tail = new_node
            self.head = new_node
    
    def insert_at_end(self, data):
        new_node = Node(data)
        if self.head:
            self.tail.next = new_node
            self.tail = new_node
        else:
            self.head = new_node
            self.tail = new_node
    
    def search(self, data):
        current_node = self.head
        while current_node:
            if current_node.data == data:
                return True
            else:
                current_node = current_node.next
        return False
        
    ```
---
## Big O Notation
- measures worst case complexity of an algorithm
- Time complexity: time take to run completely
- Space complexity: extra memory space
- Uses Mathematical expressions:
  - O(1) - constant time - 1
  - O(log n) - logarithmic time - 2
  - O(n) - linear time - 3
  - O(n log n) - linearithmic time - 4
  - O(n^2) - quadratic time - 5
  - O(2^n) - exponential time - 6
  - O(n!) - factorial time - 7

```Python
#Example O(1)
colors = ["red", "green", "blue"]

def constant(colors):
    print(colors[2])
    
constant(colors)

#Example O(n) - the bigger the list the more operations 1 operation per n
colors = ["red", "green", "blue"]

def linear(colors):
    for color in colors:
        print(color)

linear(colors)

#Example O(n^2) - looping over 2 elements for example

colors = ["red", "green", "blue"]

def quadratic(colors):
    for color in colors:
        for color2 in colors:
            print(color, color2)

quadratic(colors)

#Example O(n^3) - cubic pattern o of n cubed

colors = ["red", "green", "blue"]

def cubic(colors):
    for color in colors:
        for color2 in colors:
            for color3 in colors:
                print(color, color2, color3)

cubic(colors)

# Calculating Algorithm complexity

colors = ["red", "green", "blue"] #O(1)
other_colors = ["yellow", "orange", "purple"] #O(1)

def complex_algorithm(colors):
    color_count = 0 #O(1)
    
    for color in colors: 
        print(color) #O(n)
        color_count += 1 #O(n)
        
    for other_color in other_colors:
        print(other_color) #O(m)
        color_count += 1 #O(m)
        
    print(color_count) #O(1)

complex_algorithm(colors) #O(4 + 2n + 2m)

#Calculating
#Step 1 remove constants  
#O(4 + 2n + 2m) -> O(n + m)
#Step 2 different variables for different inputs, thats why there is an n and m, would be other letters for more
# Remove smaller terms
# O(n + n^2) -> O(n^2)

```

***

## Stacks
- LIFO: Last in First Out
- Last inserted item will always be the first item to be removed

- Can only add at the top 
  - called pushing onto the stack

- Can only take from the top
  - called Popping from the stack
  
- Can only read the last element
  - called peaking from the stack


***

# Queues
- FIFO: First in First Out
- First inserted item will always be the first item to be removed

- Can only add at the end 
  - called enqueueing onto the queue

- Can only take from the front
  - called dequeuing from the queue
  
- Can only read the first element
  - called peeking from the queue

- Multiple types of queues
  - Other types are: double ended queues, circular queues, and priority queues



## Implementation
```Python
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
import queue

orders_queue = queue.SimpleQueue()

orders_queue.put('Sushi')
orders_queue.put('Lasagna')
orders_queue.put('Paella')

print("the size is: ", orders_queue.qsize())

print(orders_queue.get())
print(orders_queue.get())
print(orders_queue.get())

print("Empty queue: ",orders_queue.empty())

```

***

# Hash Tables

- stores collection of items in key value pairs
- every programming language has a built in hash table: hashes, hash maps, dictionaries, associative arrays
- in Python we have dictionaries

- a Hash function maps the key with the location of the value on the hash table, must return the same value for the same input every time
- O(1)

```Python
my_empty_dictionary = {}
my_menu.items()
my_menu.keys()
my_menu.items()
my_menu['samosas'] = 13 #creating a new key value pair

#deleting
del my_menu
del my_menu['samosas']
my_menu.clear()

#iterate

for key, value in my_menu.items():
    print(f"\nkey: {key}")
    print(f"value: {value}")

```

***

# Trees and Graphs

- node based data structures
- each node can have links to more than one node
- first link is called the root
- a Node can be the parent of other nodes, called children
- trees have levels

## Binary tree

- each node has 0, 1 or 2 children

```Python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = left
        self.right = right
        
node1 = TreeNode("B")
node2 = TreeNode("C")
root_node = TreeNode("A", node1, node2)

```

## Real Uses

- computer file system
- structure of an HTML document
- chess possible moves of the rival
- searching and sorting algorithms

***

## Graphs

- set of 
  - nodes/vertices
  - links/edges
  
- trees are a type of graph

- graphs can be directed , meaning they have a specific direction
- graphs can be undirected, where edges have no direction
  - the relationship in this case is mutual
  
- weighted graphs:
  - have numeric values associated with edges
  - can be directed or undirected
  
- Trees cannot have cycles, so nodes cannot reference each other circularly
- trees all nodes must be connected
- graphs can have cycles
- graphs there can be unconnected nodes

## real uses

- social network
  - likes
  - friendship
  - follows
- locations and distances
  - optimize routes
  
- graph databases
-searching and sorting algorithms

```Python
class Graph:
    def __init__(self):
        self.nodes = {}
        
    def add_vertex(self, vertex):
        self.vertices[vertex] = []
    
    def add_edge(self, source, target):
        self.vertices[source].append(target)
    
my_graph = Graph()
my_graph.add_vertex('David')
my_graph.add_vertex('Miriam')
my_graph.add_vertex('Martin')

my_graph.add_edge('David', 'Miriam')
my_graph.add_edge('Miriam', 'Martin')
my_graph.add_edge('Martin', 'David')

print(my_graph.vertices)

```

***

# Recursion

- terms used to define a function calling itself
- almost all the situations where we use loops
  - substitute the loops using recursion
- can solve problems that seem very complex at first

```Python
def factorial(n):
    result = 1
    while n > 1:
        result = n * result
        n -= 1
    return result

factorial(5)

def factorial_recursion(n):
    if n == 1:
        return 1
    else:
        return n * factorial_recursion(n - 1)


```

***

## Dynamic Programming

- Optimizaion technique
- mainly applied to recursion
- can reduce the complexity of recursive algorithms
- Used for:
  - any problem that can be divided into smaller subproblems
  - subproblems overlap
- solutions of subproblems are saved, avoiding the need to recalculate
- **memoization**

***

# Linear Search & Binary Search

## Linear Search
- Linear search loops through each element, once found, algorithm stops, returns the result

```Python

def linear_search(unordered_list, search_value):
    for index in range(len(unordered_list)):
        if unordered_list[index] == search_value:
            return True
    return False

print(linear_search([15,2,21,3,12,7,8],8))
```

- Complexity O(n)

## Binary Search

- Only applies to ordered list
- Compares search_value with the item in the middle of the list

```Python
def binary_search(ordered_list, search_value):
    first = 0
    last = len(ordered_list) - 1
    
    while first <= last:
        middle = (first + last) // 2
        if ordered_list[middle] == search_value:
            return True
        elif search_value < ordered_list[middle]:
            last = middle - 1
        else:
            first = middle + 1
    return False

```

- Complexity is O(log n) better than linear search

***

## Binary Search Tree (BST)

- Left subtree contains only nodes with values less than the node itself
- Right containts values greater than the node itself
- Left and Right subtrees must be Binary search trees

```Python
class TreeNode:
    def __init__(self, data, left=None, right=None):
        self.data = data
        self.left_child = left
        self.right_child = right
        
    def BinarySearchTree:
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
```

### Uses

- Order lists efficiently
- Much faster at searchingv than arrays and linked lists
- much faster at inserting and deleting than arrays
- Used to implement more advanced data structures:
  - dynamic sets
  - lookup tables
  - priority queues
  

***

# Depth First Search (DFS)

- Tree/graph traversal
  - process of visiting all nodes
  - Depth First Search (DFS)
  - Breadth First Search (BFS)

DFS - Binary Trees
- In order
  - Left -> Current -> right

```Python
def in_order(self, current_node):
    if current_node:
        self.in_order(current_node.left_child)
        print(current_node.data)
        self.in_order(current_node.right_child)
        
my_tree.in_order(my_tree.root)
```
- Complexity O(n)

- Pre-Order Traversal
  - Order: Current -> left -> Right
  
- Post-Order Traversal
  - Order: Left -> Right -> Current
  
### Uses

- In-Order: use BST to obtain the node's values in acending order
- Pre-Order:
  - Create Copies of a Tree
  - Get Prefix Expressions
- Post-Order:
  - delete bindary trees
  - get post fix expressions
 
 ***
  
## Breadth First Search (BFS)

- Starts from the root
- Visits every node of every level

```Python
def bfs(self):
    if self.root:
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

- Complexity O(n)

```Python
def bfs(graph, initial_vertex):
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
- Complexity O(V + E) # Vertices plus Edges

***

# Sorting Algorightms

## Bubble Sort

- Deeply Studied
- Solve how to sort and unsorted collection in ascending/descernding order
- can reduce complexity of problems
- Some sorting algorithms:
  - Bubble Sort
  - Merge Sort
  - Quick Sort
  - Insertion Sort
  - Selection Sort
  - Heap Sort
  - Radix Sort

  ```Python
  
  def bubble_sort(my_list):
    list_length = len(my_list)
    for i in range(list_length-1):
        for j in range(list_length - 1 - i):
            if my_list[j] > my_list[j + 1]:
                my_list[j], my_list[j + 1] = my_list[j + 1], my_list[j]
    return my_list

  def bubble_sort(my_list):
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
  ```

- Complexity O(n^2)

***

## Selection & Insertion Sort

```Python
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
```

- Complexity O(n^2)

```Python
def insertion_sort(my_list):
    for i in range(1, len(my_list)):
        number_to_order = my_list[i]
        j = i - 1
        while j >= 0 and number_to_order < my_list[j]:
            my_list[j + 1] = my_list[j]
            j -= 1
        my_list[j + 1] = number_to_order
    return my_list
```
- Complexity O(n^2)


***

## Merge Sort

- Follows divide and conquer strategy
- Divide:
  - divides the problem into smaller sub-problems
- Conquer:
  - sub problems are solved recursively
- Combine:
  - solutions of sub problems are combined to achieve the final solution
  
```Python
def merge_sort(my_list):
    if len(my_list) > 1:
        mid = len(my_list) // 2
        left_half = my_list[:mid]
        right_half = my_list[mid:]

        merge_sort(left_half)
        merge_sort(right_half)

        i = j = k = 0

        while i < len(left_half) and j < len(right_half):
            if left_half[i] < right_half[j]:
                my_list[k] = left_half[i]
                i += 1
            else:
                my_list[k] = right_half[j]
                j += 1
            k += 1

        while i < len(left_half):
            my_list[k] = left_half[i]
            i += 1
            k += 1

        while j < len(right_half):
            my_list[k] = right_half[j]
            j += 1
            k += 1
    return my_list
```

- complexity O(n log n)

***

## Quick Sort

- Follows Divide and Conquer principle
- Partition technique
  - Pivot
  - items smaller than the pivot -> left
  - items greater than the pivot -> right
- elements to the left will be sorted recursively
- elements to the right will be sorted recursively
- Hoare's partition , partition as first element

```Python
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
        left_index += 1
        while my_list[left_index] < pivot: and left_index < last_index:
            left_index += 1

        right_index -= 1
        while my_list[right_index] > pivot:
            right_index -= 1

        if left_index >= right_index:
            return right_index

        my_list[left_index], my_list[right_index] = my_list[right_index], my_list[left_index]
```

# JavaScript Data Structures
 
## Table of Contents

1. Arrays
2. Linked Lists
3. Stacks
4. Queues
5. Trees
6. Graphs
7. Hash Tables
8. Heaps
9. Sets
10. Maps

---

## 1. Arrays

Arrays are one of the most basic and widely used data structures in JavaScript. They are used to store collections of elements in a linear order.

### Creating Arrays

    // Array literal
    let fruits = ['apple', 'banana', 'orange'];
    
    // Array constructor
    let numbers = new Array(1, 2, 3);
    
    // Array with specific length
    let empty = new Array(5);  // creates array with 5 undefined elements

### Basic Operations

    // Accessing elements
    console.log(fruits[0]);  // Output: 'apple'
    console.log(fruits[1]);  // Output: 'banana'

    // Modifying elements
    fruits[1] = 'grape';

    // Adding elements
    fruits.push('kiwi');           // add to end
    fruits.unshift('mango');       // add to beginning

    // Removing elements
    fruits.pop();                  // remove from end
    fruits.shift();                // remove from beginning
    
    // Array length
    console.log(fruits.length);    // Output: 4

### Array Methods

    // Iteration
    fruits.forEach(function(fruit) {
      console.log(fruit);
    });
    
    // Map - transform array
    let lengths = fruits.map(fruit => fruit.length);
    
    // Filter - select elements
    let longFruits = fruits.filter(fruit => fruit.length > 5);
    
    // Reduce - accumulate values
    let totalLength = fruits.reduce((sum, fruit) => sum + fruit.length, 0);
    
    // Find - find first match
    let found = fruits.find(fruit => fruit.startsWith('a'));
    
    // Some - check if any matches
    let hasApple = fruits.some(fruit => fruit === 'apple');
    
    // Every - check if all match
    let allLong = fruits.every(fruit => fruit.length > 3);
    
    // Sort
    fruits.sort();                    // alphabetical
    let numbers = [3, 1, 4, 1, 5];
    numbers.sort((a, b) => a - b);   // numerical
    
    // Reverse
    fruits.reverse();
    
    // Slice - copy portion
    let subset = fruits.slice(1, 3);
    
    // Splice - modify array
    fruits.splice(1, 2, 'pear', 'cherry');  // remove 2, add 2
    
    // Concat - combine arrays
    let all = fruits.concat(['mango', 'papaya']);
    
    // Join - convert to string
    let str = fruits.join(', ');  // "apple, banana, orange"
    
    // Includes - check existence
    fruits.includes('apple');  // true
    
    // IndexOf - find index
    fruits.indexOf('banana');  // 1 or -1 if not found

### Multi-dimensional Arrays

    // 2D Array
    let matrix = [
      [1, 2, 3],
      [4, 5, 6],
      [7, 8, 9]
    ];
    
    // Accessing
    console.log(matrix[0][1]);  // 2
    
    // Iterating
    for (let i = 0; i < matrix.length; i++) {
      for (let j = 0; j < matrix[i].length; j++) {
        console.log(matrix[i][j]);
      }
    }

### Time Complexity

- Access: O(1)
- Search: O(n)
- Insertion: O(1) at end, O(n) at beginning
- Deletion: O(1) at end, O(n) at beginning

---

## 2. Linked Lists

A linked list is a linear data structure where elements are stored in nodes, and each node points to the next node in the sequence. Unlike arrays, linked lists don't store elements in contiguous memory locations.

### Node Class

    class Node {
      constructor(data) {
        this.data = data;
        this.next = null;
      }
    }

### Simple Linked List Implementation

    class LinkedList {
      constructor() {
        this.head = null;
        this.size = 0;
      }
      
      // Add element at the end
      append(data) {
        const newNode = new Node(data);
        
        if (!this.head) {
          this.head = newNode;
        } else {
          let current = this.head;
          while (current.next) {
            current = current.next;
          }
          current.next = newNode;
        }
        this.size++;
      }
      
      // Add element at the beginning
      prepend(data) {
        const newNode = new Node(data);
        newNode.next = this.head;
        this.head = newNode;
        this.size++;
      }
      
      // Insert at specific position
      insertAt(data, index) {
        if (index < 0 || index > this.size) {
          return false;
        }
        
        const newNode = new Node(data);
        
        if (index === 0) {
          newNode.next = this.head;
          this.head = newNode;
        } else {
          let current = this.head;
          for (let i = 0; i < index - 1; i++) {
            current = current.next;
          }
          newNode.next = current.next;
          current.next = newNode;
        }
        this.size++;
        return true;
      }
      
      // Remove element
      remove(data) {
        if (!this.head) return false;
        
        if (this.head.data === data) {
          this.head = this.head.next;
          this.size--;
          return true;
        }
        
        let current = this.head;
        while (current.next) {
          if (current.next.data === data) {
            current.next = current.next.next;
            this.size--;
            return true;
          }
          current = current.next;
        }
        return false;
      }
      
      // Find element
      find(data) {
        let current = this.head;
        while (current) {
          if (current.data === data) {
            return current;
          }
          current = current.next;
        }
        return null;
      }
      
      // Get size
      getSize() {
        return this.size;
      }
      
      // Check if empty
      isEmpty() {
        return this.size === 0;
      }
      
      // Convert to array
      toArray() {
        const result = [];
        let current = this.head;
        while (current) {
          result.push(current.data);
          current = current.next;
        }
        return result;
      }
      
      // Print list
      print() {
        let current = this.head;
        let output = '';
        while (current) {
          output += current.data + ' -> ';
          current = current.next;
        }
        output += 'null';
        console.log(output);
      }
    }

### Usage Example

    // Creating a linked list
    let linkedList = new LinkedList();
    linkedList.append('apple');
    linkedList.append('banana');
    linkedList.append('orange');
    linkedList.prepend('mango');
    linkedList.print();  // mango -> apple -> banana -> orange -> null
    
    linkedList.insertAt('grape', 2);
    linkedList.remove('banana');
    console.log(linkedList.find('orange'));  // Node { data: 'orange', next: ... }

### Doubly Linked List

    class DoublyNode {
      constructor(data) {
        this.data = data;
        this.next = null;
        this.prev = null;
      }
    }
    
    class DoublyLinkedList {
      constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
      }
      
      append(data) {
        const newNode = new DoublyNode(data);
        
        if (!this.head) {
          this.head = newNode;
          this.tail = newNode;
        } else {
          newNode.prev = this.tail;
          this.tail.next = newNode;
          this.tail = newNode;
        }
        this.size++;
      }
      
      // Can traverse both forward and backward
    }

### Time Complexity

- Access: O(n)
- Search: O(n)
- Insertion: O(1) at beginning/end, O(n) at position
- Deletion: O(1) at beginning/end, O(n) at position

---

## 3. Stacks

A stack is a data structure that follows the Last In, First Out (LIFO) principle, where the last element added is the first to be removed.

### Stack Implementation

    class Stack {
      constructor() {
        this.items = [];
      }
      
      // Push element onto stack
      push(element) {
        this.items.push(element);
      }
      
      // Pop element from stack
      pop() {
        if (this.isEmpty()) {
          return 'Stack is empty';
        }
        return this.items.pop();
      }
      
      // Peek at top element
      peek() {
        if (this.isEmpty()) {
          return 'Stack is empty';
        }
        return this.items[this.items.length - 1];
      }
      
      // Check if empty
      isEmpty() {
        return this.items.length === 0;
      }
      
      // Get size
      size() {
        return this.items.length;
      }
      
      // Clear stack
      clear() {
        this.items = [];
      }
      
      // Print stack
      print() {
        console.log(this.items.toString());
      }
    }

### Usage Example

    // Using an array to implement a stack
    let stack = new Stack();

    // Pushing elements onto the stack
    stack.push('apple');
    stack.push('banana');
    stack.push('orange');

    // Popping elements from the stack
    let poppedElement = stack.pop();
    console.log(poppedElement);  // Output: 'orange'
    
    console.log(stack.peek());   // Output: 'banana'
    console.log(stack.size());   // Output: 2

### Practical Applications

    // 1. Balanced Parentheses Checker
    function isBalanced(str) {
      const stack = [];
      const pairs = {
        '(': ')',
        '[': ']',
        '{': '}'
      };
      
      for (let char of str) {
        if (pairs[char]) {
          stack.push(char);
        } else if (Object.values(pairs).includes(char)) {
          if (stack.length === 0 || pairs[stack.pop()] !== char) {
            return false;
          }
        }
      }
      return stack.length === 0;
    }
    
    // 2. Reverse String
    function reverseString(str) {
      const stack = [];
      for (let char of str) {
        stack.push(char);
      }
      let reversed = '';
      while (stack.length > 0) {
        reversed += stack.pop();
      }
      return reversed;
    }

### Time Complexity

- Push: O(1)
- Pop: O(1)
- Peek: O(1)
- Search: O(n)

---

## 4. Queues

A queue is a data structure that follows the First In, First Out (FIFO) principle, where the first element added is the first to be removed.

### Queue Implementation

    class Queue {
      constructor() {
        this.items = [];
      }
      
      // Enqueue (add element)
      enqueue(element) {
        this.items.push(element);
      }
      
      // Dequeue (remove element)
      dequeue() {
        if (this.isEmpty()) {
          return 'Queue is empty';
        }
        return this.items.shift();
      }
      
      // Front element
      front() {
        if (this.isEmpty()) {
          return 'Queue is empty';
        }
        return this.items[0];
      }
      
      // Check if empty
      isEmpty() {
        return this.items.length === 0;
      }
      
      // Get size
      size() {
        return this.items.length;
      }
      
      // Clear queue
      clear() {
        this.items = [];
      }
      
      // Print queue
      print() {
        console.log(this.items.toString());
      }
    }

### Usage Example

    // Using an array to implement a queue
    let queue = new Queue();

    // Enqueue (adding elements)
    queue.enqueue('apple');
    queue.enqueue('banana');
    queue.enqueue('orange');

    // Dequeue (removing elements)
    let dequeuedElement = queue.dequeue();
    console.log(dequeuedElement);  // Output: 'apple'
    
    console.log(queue.front());    // Output: 'banana'
    console.log(queue.size());     // Output: 2

### Priority Queue

    class PriorityQueue {
      constructor() {
        this.items = [];
      }
      
      enqueue(element, priority) {
        const queueElement = { element, priority };
        let added = false;
        
        for (let i = 0; i < this.items.length; i++) {
          if (queueElement.priority < this.items[i].priority) {
            this.items.splice(i, 0, queueElement);
            added = true;
            break;
          }
        }
        
        if (!added) {
          this.items.push(queueElement);
        }
      }
      
      dequeue() {
        return this.items.shift();
      }
      
      front() {
        return this.items[0];
      }
      
      isEmpty() {
        return this.items.length === 0;
      }
    }

### Circular Queue

    class CircularQueue {
      constructor(size) {
        this.items = new Array(size);
        this.front = -1;
        this.rear = -1;
        this.size = size;
      }
      
      enqueue(element) {
        if ((this.rear + 1) % this.size === this.front) {
          return 'Queue is full';
        }
        if (this.front === -1) {
          this.front = 0;
        }
        this.rear = (this.rear + 1) % this.size;
        this.items[this.rear] = element;
      }
      
      dequeue() {
        if (this.front === -1) {
          return 'Queue is empty';
        }
        const element = this.items[this.front];
        if (this.front === this.rear) {
          this.front = -1;
          this.rear = -1;
        } else {
          this.front = (this.front + 1) % this.size;
        }
        return element;
      }
    }

### Time Complexity

- Enqueue: O(1)
- Dequeue: O(1) with proper implementation, O(n) with array shift
- Front: O(1)
- Search: O(n)

---

## 5. Trees

Trees are hierarchical data structures consisting of nodes connected by edges, with a single root node at the top.

### Binary Tree Node

    class TreeNode {
      constructor(data) {
        this.data = data;
        this.left = null;
        this.right = null;
      }
    }

### Binary Tree Implementation

    class BinaryTree {
      constructor() {
        this.root = null;
      }
      
      // Insert node
      insert(data) {
        const newNode = new TreeNode(data);
        
        if (!this.root) {
          this.root = newNode;
          return;
        }
        
        this.insertNode(this.root, newNode);
      }
      
      insertNode(node, newNode) {
        if (newNode.data < node.data) {
          if (!node.left) {
            node.left = newNode;
          } else {
            this.insertNode(node.left, newNode);
          }
        } else {
          if (!node.right) {
            node.right = newNode;
          } else {
            this.insertNode(node.right, newNode);
          }
        }
      }
      
      // Search
      search(data) {
        return this.searchNode(this.root, data);
      }
      
      searchNode(node, data) {
        if (!node) return false;
        
        if (data < node.data) {
          return this.searchNode(node.left, data);
        } else if (data > node.data) {
          return this.searchNode(node.right, data);
        } else {
          return true;
        }
      }
      
      // In-order traversal (Left, Root, Right)
      inOrder(node = this.root) {
        if (node) {
          this.inOrder(node.left);
          console.log(node.data);
          this.inOrder(node.right);
        }
      }
      
      // Pre-order traversal (Root, Left, Right)
      preOrder(node = this.root) {
        if (node) {
          console.log(node.data);
          this.preOrder(node.left);
          this.preOrder(node.right);
        }
      }
      
      // Post-order traversal (Left, Right, Root)
      postOrder(node = this.root) {
        if (node) {
          this.postOrder(node.left);
          this.postOrder(node.right);
          console.log(node.data);
        }
      }
      
      // Level-order traversal (BFS)
      levelOrder() {
        if (!this.root) return;
        
        const queue = [];
        queue.push(this.root);
        
        while (queue.length > 0) {
          const node = queue.shift();
          console.log(node.data);
          
          if (node.left) queue.push(node.left);
          if (node.right) queue.push(node.right);
        }
      }
      
      // Find minimum
      findMin(node = this.root) {
        if (!node.left) return node.data;
        return this.findMin(node.left);
      }
      
      // Find maximum
      findMax(node = this.root) {
        if (!node.right) return node.data;
        return this.findMax(node.right);
      }
    }

### General Tree (Multiple Children)

    class TreeNode {
      constructor(data) {
        this.data = data;
        this.children = [];
      }

      addChild(child) {
        this.children.push(child);
      }
    }

    // Creating a tree
    let root = new TreeNode('root');
    let child1 = new TreeNode('child1');
    let child2 = new TreeNode('child2');
    let grandchild1 = new TreeNode('grandchild1');

    root.addChild(child1);
    root.addChild(child2);
    child1.addChild(grandchild1);

### Tree Traversal Methods

    // Depth-First Search (DFS)
    function dfs(node) {
      console.log(node.data);
      for (let child of node.children) {
        dfs(child);
      }
    }
    
    // Breadth-First Search (BFS)
    function bfs(root) {
      const queue = [root];
      
      while (queue.length > 0) {
        const node = queue.shift();
        console.log(node.data);
        
        for (let child of node.children) {
          queue.push(child);
        }
      }
    }

### Time Complexity

- Search: O(log n) for balanced BST, O(n) worst case
- Insert: O(log n) for balanced BST, O(n) worst case
- Delete: O(log n) for balanced BST, O(n) worst case
- Traversal: O(n)

---

## 6. Graphs

A graph is a collection of nodes (vertices) connected by edges. Graphs can be directed or undirected, weighted or unweighted.

### Graph Implementation (Adjacency List)

    class Graph {
      constructor() {
        this.vertices = [];
        this.adjList = new Map();
      }
      
      // Add vertex
      addVertex(v) {
        this.vertices.push(v);
        this.adjList.set(v, []);
      }
      
      // Add edge
      addEdge(v, w) {
        this.adjList.get(v).push(w);
        this.adjList.get(w).push(v);  // for undirected graph
      }
      
      // Print graph
      print() {
        for (let v of this.vertices) {
          let neighbors = this.adjList.get(v);
          let output = v + ' -> ';
          for (let neighbor of neighbors) {
            output += neighbor + ' ';
          }
          console.log(output);
        }
      }
      
      // BFS
      bfs(startVertex) {
        const visited = {};
        const queue = [startVertex];
        visited[startVertex] = true;
        
        while (queue.length > 0) {
          const vertex = queue.shift();
          console.log(vertex);
          
          const neighbors = this.adjList.get(vertex);
          for (let neighbor of neighbors) {
            if (!visited[neighbor]) {
              visited[neighbor] = true;
              queue.push(neighbor);
            }
          }
        }
      }
      
      // DFS
      dfs(startVertex) {
        const visited = {};
        this.dfsVisit(startVertex, visited);
      }
      
      dfsVisit(vertex, visited) {
        visited[vertex] = true;
        console.log(vertex);
        
        const neighbors = this.adjList.get(vertex);
        for (let neighbor of neighbors) {
          if (!visited[neighbor]) {
            this.dfsVisit(neighbor, visited);
          }
        }
      }
    }

### Usage Example

    const graph = new Graph();
    const vertices = ['A', 'B', 'C', 'D', 'E'];
    
    for (let v of vertices) {
      graph.addVertex(v);
    }
    
    graph.addEdge('A', 'B');
    graph.addEdge('A', 'C');
    graph.addEdge('B', 'D');
    graph.addEdge('C', 'E');
    
    graph.print();
    graph.bfs('A');
    graph.dfs('A');

### Time Complexity

- Add Vertex: O(1)
- Add Edge: O(1)
- Remove Vertex: O(V + E)
- Remove Edge: O(E)
- BFS/DFS: O(V + E)

---

## 7. Hash Tables

A hash table (hash map) is a data structure that implements an associative array, mapping keys to values using a hash function.

### Hash Table Implementation

    class HashTable {
      constructor(size = 53) {
        this.keyMap = new Array(size);
      }
      
      // Hash function
      _hash(key) {
        let total = 0;
        const WEIRD_PRIME = 31;
        for (let i = 0; i < Math.min(key.length, 100); i++) {
          const char = key[i];
          const value = char.charCodeAt(0) - 96;
          total = (total * WEIRD_PRIME + value) % this.keyMap.length;
        }
        return total;
      }
      
      // Set key-value pair
      set(key, value) {
        const index = this._hash(key);
        if (!this.keyMap[index]) {
          this.keyMap[index] = [];
        }
        this.keyMap[index].push([key, value]);
      }
      
      // Get value by key
      get(key) {
        const index = this._hash(key);
        if (this.keyMap[index]) {
          for (let pair of this.keyMap[index]) {
            if (pair[0] === key) {
              return pair[1];
            }
          }
        }
        return undefined;
      }
      
      // Get all keys
      keys() {
        const keysArr = [];
        for (let i = 0; i < this.keyMap.length; i++) {
          if (this.keyMap[i]) {
            for (let pair of this.keyMap[i]) {
              if (!keysArr.includes(pair[0])) {
                keysArr.push(pair[0]);
              }
            }
          }
        }
        return keysArr;
      }
      
      // Get all values
      values() {
        const valuesArr = [];
        for (let i = 0; i < this.keyMap.length; i++) {
          if (this.keyMap[i]) {
            for (let pair of this.keyMap[i]) {
              if (!valuesArr.includes(pair[1])) {
                valuesArr.push(pair[1]);
              }
            }
          }
        }
        return valuesArr;
      }
    }

### JavaScript Map (Built-in Hash Table)

    // Using JavaScript's built-in Map
    const map = new Map();
    
    map.set('name', 'John');
    map.set('age', 30);
    map.get('name');        // 'John'
    map.has('age');         // true
    map.delete('age');
    map.size;               // 1
    map.clear();
    
    // Iteration
    for (let [key, value] of map) {
      console.log(key, value);
    }

### Time Complexity

- Insert: O(1) average, O(n) worst case
- Search: O(1) average, O(n) worst case
- Delete: O(1) average, O(n) worst case

---

## 8. Heaps

A heap is a specialized tree-based data structure that satisfies the heap property. In a max heap, parent nodes are always greater than or equal to child nodes.

### Max Heap Implementation

    class MaxHeap {
      constructor() {
        this.heap = [];
      }
      
      // Get parent index
      getParentIndex(index) {
        return Math.floor((index - 1) / 2);
      }
      
      // Get left child index
      getLeftChildIndex(index) {
        return 2 * index + 1;
      }
      
      // Get right child index
      getRightChildIndex(index) {
        return 2 * index + 2;
      }
      
      // Swap elements
      swap(index1, index2) {
        [this.heap[index1], this.heap[index2]] = [this.heap[index2], this.heap[index1]];
      }
      
      // Insert element
      insert(value) {
        this.heap.push(value);
        this.heapifyUp();
      }
      
      // Heapify up
      heapifyUp() {
        let index = this.heap.length - 1;
        
        while (index > 0) {
          const parentIndex = this.getParentIndex(index);
          if (this.heap[parentIndex] >= this.heap[index]) {
            break;
          }
          this.swap(parentIndex, index);
          index = parentIndex;
        }
      }
      
      // Extract max
      extractMax() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();
        
        const max = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.heapifyDown();
        return max;
      }
      
      // Heapify down
      heapifyDown() {
        let index = 0;
        
        while (this.getLeftChildIndex(index) < this.heap.length) {
          let largerChildIndex = this.getLeftChildIndex(index);
          const rightChildIndex = this.getRightChildIndex(index);
          
          if (rightChildIndex < this.heap.length && 
              this.heap[rightChildIndex] > this.heap[largerChildIndex]) {
            largerChildIndex = rightChildIndex;
          }
          
          if (this.heap[index] >= this.heap[largerChildIndex]) {
            break;
          }
          
          this.swap(index, largerChildIndex);
          index = largerChildIndex;
        }
      }
      
      // Peek at max
      peek() {
        return this.heap[0];
      }
      
      // Get size
      size() {
        return this.heap.length;
      }
    }

### Time Complexity

- Insert: O(log n)
- Extract Max: O(log n)
- Peek: O(1)
- Build Heap: O(n)

---

## 9. Sets

A Set is a collection of unique values. JavaScript has a built-in Set data structure.

### JavaScript Set

    // Creating a set
    const mySet = new Set();
    
    // Adding values
    mySet.add(1);
    mySet.add(2);
    mySet.add(3);
    mySet.add(2);  // duplicate, ignored
    
    // Checking size
    console.log(mySet.size);  // 3
    
    // Checking existence
    mySet.has(2);  // true
    
    // Removing values
    mySet.delete(2);
    
    // Clearing set
    mySet.clear();
    
    // Iteration
    for (let value of mySet) {
      console.log(value);
    }
    
    // Convert to array
    const arr = Array.from(mySet);
    const arr2 = [...mySet];

### Set Operations

    // Union
    function union(setA, setB) {
      return new Set([...setA, ...setB]);
    }
    
    // Intersection
    function intersection(setA, setB) {
      return new Set([...setA].filter(x => setB.has(x)));
    }
    
    // Difference
    function difference(setA, setB) {
      return new Set([...setA].filter(x => !setB.has(x)));
    }

---

## 10. Maps

A Map is a collection of key-value pairs. JavaScript has a built-in Map data structure.

### JavaScript Map

    // Creating a map
    const myMap = new Map();
    
    // Setting values
    myMap.set('name', 'John');
    myMap.set('age', 30);
    myMap.set(1, 'one');
    
    // Getting values
    myMap.get('name');  // 'John'
    
    // Checking existence
    myMap.has('age');  // true
    
    // Size
    console.log(myMap.size);  // 3
    
    // Deleting
    myMap.delete('age');
    
    // Clearing
    myMap.clear();
    
    // Iteration
    for (let [key, value] of myMap) {
      console.log(key, value);
    }
    
    // Keys
    for (let key of myMap.keys()) {
      console.log(key);
    }
    
    // Values
    for (let value of myMap.values()) {
      console.log(value);
    }

---

## Choosing the Right Data Structure

| Use Case | Recommended Data Structure |
|----------|---------------------------|
| Fast lookup by key | Hash Table / Map |
| Ordered data with fast insertion | Linked List |
| LIFO operations | Stack |
| FIFO operations | Queue |
| Hierarchical data | Tree |
| Network/relationships | Graph |
| Unique values | Set |
| Key-value pairs | Map |
| Indexed access | Array |

---

## Best Practices

1. **Choose the right structure** for your use case
2. **Understand time complexity** of operations
3. **Use built-in structures** (Map, Set) when possible
4. **Consider memory usage** for large datasets
5. **Implement custom structures** only when needed
6. **Test edge cases** (empty structures, single element, etc.)
7. **Document your implementations** for maintainability

---

## Conclusion

Understanding different data structures is crucial for writing efficient JavaScript code. Each structure has its strengths and weaknesses, and choosing the right one can significantly impact your application's performance. Practice implementing these structures and understanding when to use each one.
 
 

#  JavaScript Data Structure 
Table of Contents
1. Arrays
2. Linked Lists
3. Stacks
4. Queues
5. Trees

Arrays
###### Arrays are one of the most basic and widely used data structures in JavaScript. They are used to store collections of elements.

    // Creating an array
    let fruits = ['apple', 'banana', 'orange'];

    // Accessing elements
    console.log(fruits[0]); // Output: 'apple'

    // Modifying elements
    fruits[1] = 'grape';

    // Adding elements
    fruits.push('kiwi');

    // Removing elements
    fruits.pop();

___
Linked Lists
###### A linked list is a linear data structure where elements are stored in nodes, and each node points to the next node in the sequence.

    // Node class for a linked list
    class Node {
      constructor(data) {
        this.data = data;
        this.next = null;
      }
    }

    // Creating a linked list
    let linkedList = new Node('apple');
    linkedList.next = new Node('banana');
    linkedList.next.next = new Node('orange');
___
Stacks
###### A stack is a data structure that follows the Last In, First Out (LIFO) principle, where the last element added is the first to be removed.

    // Using an array to implement a stack
    let stack = [];

    // Pushing elements onto the stack
    stack.push('apple');
    stack.push('banana');
    stack.push('orange');

    // Popping elements from the stack
    let poppedElement = stack.pop();
    console.log(poppedElement); // Output: 'orange'
___
Queues
###### A queue is a data structure that follows the First In, First Out (FIFO) principle, where the first element added is the first to be removed.

    // Using an array to implement a queue
    let queue = [];

    // Enqueue (adding elements)
    queue.push('apple');
    queue.push('banana');
    queue.push('orange');

    // Dequeue (removing elements)
    let dequeuedElement = queue.shift();
    console.log(dequeuedElement); // Output: 'apple'
___
Trees
 ###### Trees are hierarchical data structures consisting of nodes connected by edges, with a single root node at the top.
    // Node class for a tree
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

    root.addChild(child1);
    root.addChild(child2);

 
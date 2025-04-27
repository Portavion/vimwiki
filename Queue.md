# Queue

A queue is a linear data structure that follows the *First In First Out* principle (FIFO). Meaning the first element added to the queue is the first one to be popped.

## Terminology

* Front: position of the entry ready to be served
* Rear: position of the last entry
* Size: current numbers of elements in the queue
* Capacity: maximum number of elements the queue can hold

## Queue Operations

* Enqueue: adds an element the end / rear
* Dequeue: removes an element from the front
* Peek: returns the element at the front without removing it
* Size: returns the number of elements in the queue
* isEmpty: returns true if the queue is empty and false otherwise
* isFull: returns true if the queue is full and false otherwise

## Implementations

* [Queue - Circular Implementation](Queue - Circular Implementation.md)
* [Queue - Linked List Implementation](Queue - Linked List Implementation.md)

## Complexity

| operations | time complexity | space complexity |
| ----       | ----            | ----             |
| enqueue    | O(1)            | O(1)             |
| dequeue    | O(1)            | O(1)             |
| front      | O(1)            | O(1)             |
| size       | O(1)            | O(1)             |
| isEmpty    | O(1)            | O(1)             |
| isFull    | O(1)            | O(1)             |


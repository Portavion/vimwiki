# Queue - Linked List Implementation

```python
class Node:
    def __init__(self, new_data):
        self.data = new_data
        self.next = None

class Queue:
    def __init__(self):
        self.front = self.rear = None

    def isEmpty(self):
        return self.front is None

    def enqueue(self, new_data):
        new_node = Node(new_data)
        if self.isEmpty():
            self.front = self.rear = new_node
            return
        self.rear.next = new_node
        self.rear = new_node

    def dequeue(self):
        if self.isEmpty():
            return
        temp = self.front
        self.front = self.front.next
        if self.front is None:
            self.rear = None
        return temp
        
```

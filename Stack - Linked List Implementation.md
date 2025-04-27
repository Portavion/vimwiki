# Stack - Linked List Implementation

```python
class Node:
    def __init__(self, new_data):
        self.data = new_data
        self.next = None

# Class to implement stack using a singly linked list
class Stack:
    def __init__(self):
        self.head = None

    def is_empty(self):
        return self.head is None

    def push(self, new_data):
        new_node = Node(new_data)
        if not new_node:
            print("\nStack Overflow")
            return
        # Link the new node to the current top node
        new_node.next = self.head
        # Update the top to the new node
        self.head = new_node

    def pop(self):
        if self.is_empty():
            print("\nStack Underflow")
        else:
            # Assign the current top to a temporary variable
            temp = self.head
            # Update the top to the next node
            self.head = self.head.next
            del temp

    def peek(self):
        if not self.is_empty():
            return self.head.data
        else:
            print("\nStack is empty")
            return float('-inf')
```

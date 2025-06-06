# Linked List

A linked list is a data structure that allows efficient insertion and deletion compared to arrays.It is non-contiguous, typically allocated one-by-one and sequential.

## Basic

* Single linked list: each node reference the next node
* Doubly Linked List: each node reference next and previous node
* Circular Linked List: last node points back to start

## Operations

* traversal (__iter__)
* Length
* Print
* Insertion
* Search
* Deletion
* Delete list
* Nth node from start
* Nth node from End

## Implementation

### Node Class

```python
class Node:
  def __init__(self, data):
    self.data = data
    self.next = None
```

### Linked List Class

```python
class LinkedList:
  def __init__(self):
    self.head = None #initialise an empty list
    
  def insertAtBeginneing(self, new_data):
    new_node = Node(new_data) # creates the new node
    new_node.next = self.head # set the next node to the previous head
    self.head = new_node #sets the head as the new node
    
  def insertAtEnd(self, new_data):
    new_node = Node(new_data)
    
    #if there is no node in the list then we can set the head as the new element
    if self.head is None:
      self.head = new_node
      return
    
    # if there is at least one node, we traverse the list to find the last node
    last = self.head
    while last.next:
      last = last.next
    last.next = new_node
  
  def deleteAtBeginning(self):
    if self.head is None:
      return
    self.head= self.head.next
    
  def deleteAtEnd(self):
    if self.head is None:
      return
    if self.head.next is None:
      self.head = None
      return
    temp = self.head
    while temp.next.next:
      temp = temp.next
    temp.next = None
    
    def insertAtPosition(self, new_data, pos):
      if pos == 0:
        self.insertAtBeginneing(new_data)
        return
        
      new_node = Node(new_data)
      current = self.head
      position = 0
      while current.next:
        position += 1
        if position == pos:
          new_node.next = current.next
          current.next = new_node
          return 
      return False
    
  def remove_at_index(self, index):
    if self.head is None:
        return

    current_node = self.head
    position = 0
    
    if index == 0:
        self.remove_first_node()
    else:
        while current_node is not None and position < index - 1:
            position += 1
            current_node = current_node.next
        
        if current_node is None or current_node.next is None:
            print("Index not present")
        else:
            current_node.next = current_node.next.next
          
    def search(self, value):
      current = self.head
      position = 0
      while current:
        if current.data == value:
          return position
        current = current.next
        position += 1
    return False
        
```

# Queue - Circular Implementation

```python
class Queue:
    def __init__(self, c):
        self.arr = [0] * c
        self.capacity = c
        self.size = 0
        self.front = 0

    def getFront(self):
        if self.size == 0:
            return -1
        return self.arr[self.front]

    def getRear(self):
        if self.size == 0:
            return -1
        rear = (self.front + self.size - 1) % self.capacity
        return self.arr[rear]

    def enqueue(self, x):
        if self.size == self.capacity:
            return
        rear = (self.front + self.size) % self.capacity
        self.arr[rear] = x
        self.size += 1

    def dequeue(self):
        if self.size == 0:
            return -1
        res = self.arr[self.front]
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return res
```

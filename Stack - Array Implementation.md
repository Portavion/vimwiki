# Stack Implementation using Array

```python
class Stack:
    def __init__(self, cap):
        self.cap = cap
        self.top = -1
        self.a = [0] * cap

    def push(self, x):
        if self.top >= self.cap - 1:
            print("Stack Overflow")
            return False
        self.top += 1
        self.a[self.top] = x
        return True

    def pop(self):
        if self.top < 0:
            print("Stack Underflow")
            return 0
        popped = self.a[self.top]
        self.top -= 1
        return popped

    def peek(self):
        if self.top < 0:
            print("Stack is Empty")
            return 0
        return self.a[self.top]

    def is_empty(self):
        return self.top < 0
```


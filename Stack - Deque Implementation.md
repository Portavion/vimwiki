# Stack - Deque Implementation

```python
from collections import deque

stack = deque()
# a regular FIFO queue would use popleft()
print(f'{stack.pop()} popped from stack')
print(f'Top element is: {stack[-1]}' if stack else 'Stack is empty')

```

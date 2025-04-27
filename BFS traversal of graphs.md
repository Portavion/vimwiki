# BFS - graphs

BFS help answer two types of questions:
  * Is there a path between two nodes
  * What is the shortest path between two nodes

## BFS Implementation

  * Keep a queue of nodes to check and array of visited nodes
  * Pop a node from the queue and check if it has already been visited
  * Check if the node is the answer
    - If yes then you are done
    - If not add all its neighbours to the queue and mark the node as visited
  * Loop
  * If the queue is empty then there are no answer

## Python Implementation

```python
from collections import deque

search_queue = deque()
search_queue += graph['you'] # add all the 'you' neighbours to the search queue
searched = []

while search_queue:
  person = search_queue.popleft()
  
  if not person in searched:
    if person_is_seller(person):
      print(f'{person} is a mango seller')
      return True
    else: 
      search_queue += graph[person] # add all the person neighbours to the search queue
      searched.append(person)

return False
```

## Complexity

If we search the entire graph then the running time is at least O(number of edges).

We keep a queue of every nodes to visit. Adding to the queue takes O(1). For every node it will take O(number of people).

BFS takes O(edges+nodes) or  O(V+E) where V is Vertices and E edges.



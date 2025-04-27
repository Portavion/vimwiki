# Introduction to graphs
source: [Grokking the algorithm](https://www.manning.com/books/grokking-algorithms)

## What is a graph

A graph models a set of connections. It is made up of **nodes** and **edges**. 

A node can be directly connected to many other nodes, which are called its neighbours.

## Graph implementation

Each node is connected to other nodes. This can be represented using the hash table data structure.

```python
graph = {}
graph['you'] = ['Alice', 'Bob', 'Claire']
graph['Bob'] = ['Alice', 'Claire']
graph['Alice'] = ['Claire']
graph['Claire'] = []
```

A graph can be a _directed graph_ like in the above examples. Meaning a node can have arrows pointing to it, but no arrow from it to another. The relationship is only one way.

An undirected graph does not have any arrows and both nodes are each other's neighbours.

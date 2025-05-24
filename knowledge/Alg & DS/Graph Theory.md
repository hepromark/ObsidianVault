

**Definition:** Graph is a data structure that consists of a finite set of vertices (also called nodes), and a set of edges between the nodes.

### **Graph Representations**

**Matrix Representation**

- Graph G is defined by a set of all nodes & edges between each nodes (a 2D array)
- Let V be a set of nodes
- Let M be the matrix where M(i, j) = 1 when there is an edge between node i and j
- **Pros:** O(1) access for any M(i, j)
- **Cons:** O(n^2) memory complexity for graph with n nodes

**Adjacency List Representation**

- Graph G is defined by a set of all nodes & list of edges which originates at each node
- Let V be a set of nodes
- Let L be a **list of linked lists for all edges that originates at a node**
- **Pros:** O(n + e) memory complexity for graph with n nodes and e edges
- **Cons:** Need to traverse LL to access its elements → O(e) time complexity to view node connection worst case

Adjacency Set Representation

- Graph G is defined by a set of all nodes & set of adjacency sets

### Graph Terminology

Directed Graph: Edges have a direction
Undirected Graph: Edges don’t have a direction
Labelled Graph: Each node has a unique symbolic label
Weighed graphs: Graph where each edge has a numerical value
Connectivity: A subset of nodes are connected if there is path going from each node to every other node
Path Length: # of edges that form path between 2 nodes
Strongly Connected: Each pair of nodes are connected, node i↔ node j
Cycles: Path where a node is visited more than once
Simple cycle: Only starting node visited more than once
Complex Cycle: Any node can be visited more than once
Degree of a Node:
- Un-directed: # of edge connected to that node
- Directed: In-degree and out-degree for # edges ending at / coming out of node
Adjacency: There is an edge connecting the two nodes
Adjacency set: Set of all nodes adjacent to this node
Directed Acyclic Graphs (DAG): A directed graph w/ no cycles
Traversal: Process of searching through a graph data structure, the order of visiting determines the classification of the traversal method.

### **Graph Traversal: DFS**

**Definition:** Can determine whether nodes x and y have a path between them by looking at children of the starting node & recursively determining if a path exists.

### **Graph Traversal: BFS**

Steps:

1. Add a node from graph to be visited into the queue
2. Visit the node at front of queue, mark it as visited
3. Check the node’s neighbors to check if they were visited
4. Add any unvisited neighbours to back of the queue
5. Dequeue the node at front of the queue

Time Complexity:

- Visit n nodes + iterate through adjacency list of each node
- O(v + e) time complexity
- **Linear Algorithm**


Graph Search Algorithms

| Algorithm                | Description | Implementation | Application |
| ------------------------ | ----------- | -------------- | ----------- |
| DFS                      |             |                |             |
| BFS                      |             |                |             |
| Dijkstra's               |             |                |             |
| Greedy Best First Search |             |                |             |
| A*                       |             |                |             |
| Bellman-Ford Algorithm   |             |                |             |
| Topological Sorting      |             |                |             |
	
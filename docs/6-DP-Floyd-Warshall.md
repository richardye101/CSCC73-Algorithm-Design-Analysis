Bellman ford finds the shortest path from all nodes to some goal node $t$ with negative edges. It takes $O(n^{3})$ time in the worst case. 

>Idk why this was mentioned but:
>Dealing with sinks: A sink is a node  $t$ where all nodes are directed to it and nothing flows out of $t$

## Why Floyd-Warshall
This algorithm floyd-warshall is a way to find **all-pair shortest paths, where all-pair shortest paths finds the shortest paths between each pair of node**. It is still $O(n^{3})$ time in worst case though. You wouldn't wanna do this until you wanted access to all the pairs.

To do this with bellman ford, we would have to compute the algorithm for each vertex/node $n$ resulting in a time complexity of $O(n^{4})$ in the worst case.

## In Bellman Ford:
- Suppose a shortest path from $v_{1}\to v_{5}$ is $v_{1},v_{8},v_{2},v_{3},v_{5}$, then the shortest path from $v_{1}\to v_{2}$ and $v_{2}\to v_{5}$ are already known, because by definition bellman ford will have already taken the shortest path through theses nodes.

## Method

1. Order the vertices $v_{1}, v_{2}, \dots, v_{n}$ and consider them in order.
2. For each pair $(v_{i},v_{j})$, the initial shortest path is the weight of the edge between them, if it exists. Otherwise we set it to $\infty$. (Basically just the adjacency matrix)
3. On the first iteration $i=1$, we consider *including* vertex $v_{1}$ on the shortest path.
	1. Is there an edge $(v_{i},v_{1})$ and a $(v_{1}, v_{j})$ edge such that their sum is less than the weight between $(v_{i}, v_{j})$?
4. On the second iteration, we look if there are any shorter paths containing $v_{1}$ and $v_{2}$.

Basically,
![[Obsidian-Attachments/6-DP-Floyd-Warshall-1.png]]
>Perhaps at each iteration, when we look at each pair $i,j$, we also allow the algorithm to look at the $1,\dots ,k$  adjacency lists

We can define the recurrence relation as:
![[Obsidian-Attachments/6-DP-Floyd-Warshall.png]]

### Example
![[Obsidian-Attachments/6-DP-Floyd-Warshall-2.png]]

> For detecting cycles:
> If the weight from a given node to itself **decreases**, then we know we have found a negative cycle. AKA Just check the diagonal of the table

## Pseudocode
![[Obsidian-Attachments/6-DP-Floyd-Warshall-3.png]]

## Proving correctness

![[Obsidian-Attachments/6-DP-Floyd-Warshall-4.png]]
![[Obsidian-Attachments/6-DP-Floyd-Warshall-5.png]]
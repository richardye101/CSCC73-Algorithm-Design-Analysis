An algorithm that works on graphs with negative weights. 

From Djikstra's, we could possibly add a constant to all edges to make them all positive. However, if the shortest path (by weight) uses many edges, then the weight would have been increased for the entire path.
![[Obsidian-Attachments/5-Bellman-Ford Algorithm.png]]

Negative weight graphs with cycles could cause a cycle and endlessly loop to decrease weight.

## How it works
Build paths backward from a given node $T$.
We can start by finding the shortest (least weight) path with one edge to $T$.
Then we can repeat with two edges to $T$.
When we repeat with three edges, if we find a better path then we update the node to $T$ with that path.

This algorithm may run for $n-1$ iterations, OR until two iterations where the shortest path does not decrease (need to be more clear).

### Recursive Function
$$
OPT(i,v) = \text{length of shortest } v\to t \text{ path } P \text{ using at most } i \text{ edges.}
$$

Cases:
- The shortest $v-t$ path uses at most $i-1$ edges.
- The shortest path uses $OPT(i-1,u) + w(v,u)$ for some $u\in N_{out}(v)$ 
	- $min_{u\in N_{out}(v)}(OPT(i-1,u) + w(v,u))$
	- Where $N_{out}(v)$ is the out neighbourhood of $v$

![[Obsidian-Attachments/5-Bellman-Ford Algorithm-4.png]]

This function will end up being $O(n^{3})$ time because for every node, it loops $1\to n$ edge lengths and for each edge length it will look at every edge connected to that node which in worst case is $n$.

It will be $O(n^{2})$ space as each iteration of edge length, from $1\to (n-1)$, each $n$ nodes  have some shortest path value.

![[Obsidian-Attachments/5-Bellman-Ford Algorithm-5.png]]

## Improving Space/Time Complexity
**Improving Space Complexity**
We don't need to store every single column to make this work, just the $i-1$ iteration.

This means we only need to store:
- the adjacency list of each node (in order to determine the in and out neighbours for each node) which is $m$ edges
- They current column of the table, $M[v]$, and the successor $s[v]$ (the $N_{out}$ of $v$?) which in total is $O(n)$

Therefore, space complexity is reduced to $O(m+n)$

**Improving Time Complexity**
We don't need to check $O(n)$ nodes for every entry $M(i,v)$, just the nodes which are in the out neighbourhood of $v$ ($N_{out}[v]$)

![[Obsidian-Attachments/5-Bellman-Ford Algorithm-6.png]]

We will only compare up to $O(m)$ edges, resulting in $O(nm)$ time complexity.

## Improvement: Push values ahead

- Notice that at each step of the algorithm we perform a fetch action to retrieve $OPT(i − 1, v)$ or $OPT(i − 1, u)$. 
- If we can instead push these values ahead each time, we can skip the nodes whose values won’t change. 
- Initially when $i = 0$ only $t$ has a finite value - it is only reachable from itself. This means all paths using 1 edge must be of the form $(u, t)$.
- So the iteration when $i = 0$, need only check those vertices $u \in V$ such that $t \in N_{out}(u)$, i.e., (u, t).

>Basically, if we know a node $u$ takes less than $i$ edges to get to the goal node $t$, then we can just push it's weight to the next column for $i+1$ edges. In a subsequent iteration, if we find that we can reach $t$ taking a path from $u\to v\to t$, then we flag it as something we updated and have to check in a future iteration, and update it's optimal weight.


![[Obsidian-Attachments/5-Bellman-Ford Algorithm-7.png]]

## Negative Cycles

Since Bellman Ford works for graphs with negative edge weights, what if there exists some negative cycle?

Reasonably, that implies we could have a path longer than the number of nodes $n$ which would then be deemed the shortest/least weight path from $s\to t$. 
$$
\implies OPT(i,v) \text{ decreases as } i \geq n 
$$

All we need to do is check that $OPT(n,v) = OPT(n-1,v)$, where $n$ is the number of nodes in the graph.

>Logically, this just means that if the shortest/least weight path from $s$ to $t$ contains more than $n-1$ edges, the weight will not decrease. If it does, then we have found a negative weight cycle somewhere.

**Claim: There is no negative cycle on a path to $t \iff$ $OPT(n,v) = OPV(n-1,v)$ for all nodes $v$**

### Proof that the Bellman Ford has no cycles
Need to prove the iff

![[Obsidian-Attachments/5-Bellman-Ford Algorithm-1.png]]
The version two of the proof for the **forward direction**
![[Obsidian-Attachments/5-Bellman-Ford Algorithm-2.png]]

**Reverse/Backward Direction of the IFF**
![[Obsidian-Attachments/5-Bellman-Ford Algorithm-3.png]]

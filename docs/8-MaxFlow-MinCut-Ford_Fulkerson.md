Network flow is represented by a directed graph, where there is a single source and a single sink. There are many edges which represent how much flow can go through that edge.

![[Obsidian-Attachments/8-MaxFlow-MinCut-1.png]]

Essentially, we want to calculate the maximum flow (units) we can send from the source to the sink through the weighted edges.

**Capacity limit:** Let $e\in E$ represent an edge, and the flow $f(e)$ over edge $e$ is bounded by $0\leq f(e)\leq c(e)$ where $c(e)$ is the capacity of the edge.

**Conservation of flow:** For each vertex $v\in V-{s,t}$, we must have:
$$
\sum_{e \text{ into } v}f(e) = \sum_{e \text{ out from }v}f(e)
$$

The value of flow $v(f) = \sum_{e \text{ out of }s}f(e)$ represents the total flow coming out of $s$ (the source). This is what we want to maximize. To do this we can solve an equivalent problem of determining which edges, if cut from the graph, disconnect s and t completely and **have the lowest weight/capacity**. This the min-cut algorithm which can be used to determine the **max-flow**.

To do this, we introduce the concept of a **residual** graph. It represents how much capacity each edge has left after each run of the following algorithm. At the beginning, it is simply a copy of the original graph.

For the edge which has been maxed out in each iteration, we introduce a *new* reverse edge of the original capacity representing how much flow we can *push back* across that edge.

The algorithm is as follows:
- Initialize a flow value variable $v$ which will represent the flow coming out of $s$.
- Take a graph and run a DFS algorithm for paths from $s\to t$, and store the path
- Find the minimum weight edge on the path (the bottleneck). 
- Remove that weight from all edges on this path **in the residual graph**.
	- For the bottleneck edge, reduce to 0 **and** 
	- Introduce a backward edge of the same weight.
- Increment $v$ by the bottleneck weight
- Repeat algorithm, which will find another, different path.
- End algorithm when there is no path from $s\to t$

At the end, we will have a residual graph where the set of vertices connected to $s$ will represent the min-cut A, and we can obtain the flow over any edge in the original graph by subtracting the capacity by the residual capacities (**ignoring backward edges**).

![[Obsidian-Attachments/8-MaxFlow-MinCut-Ford_Fulkerson.png]]

In the end we *should* have a cut where capacity of the cut is exactly equal to the flow value out of $s$.
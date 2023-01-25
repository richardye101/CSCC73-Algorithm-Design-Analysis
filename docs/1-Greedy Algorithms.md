```toc style: bullet | number | inline (default: bullet) min_depth: number (default: 2) max_depth: number (default: 6) title: string (default: undefined) allow_inconsistent_headings: boolean (default: false) delimiter: string (default: |) varied_style: boolean (default: false)
```

## Djikstra's algorithm

How to prove correctness?
	
### Exchange argument proof

Consider the edges of an optimal solution $O$ ordered by how we added them in our greedy list of edges S

$S = s_1, s_2, \dots s_n$
	Where for some $i$, the first $i-1$ items are the same as in $O$
	
Let $opt = ((x,v), d_{opt}(v))$ be the edge containing $v$ with path distance $d_{opt}(v)$ in $O$

The optimal path may choose a different edge for the ith edge on the path to $v$, or a path directly to $v$.

#### Directly to $v$

- The greedy algo will always choose the shortest path, but because it can't be shorter than the optimal solution (because its optimal) then they must be equal. This means we can do a swap and make the optimal solution more similar to the greedy one

#### Opt traverses more than a single edge to v

- Because the greedy algo will always choose the shortest path, then optimal cannot have more edges than the greedy algo

![[Pasted image 20230118132335.png]]
This example is done where the greedy algo had a direct path to v from some ith state

## Minimum lateness Problems

Need to schedule $n$ jobs to minimize how late jobs finish overall in the schedule

Ideas:

- Could do all the short jobs first?
	- Won't work because it ignores the deadlines for each job
- Sort by increasing slack time (how much time there is between deadline and time required after start of schedule)

### Optimal solution:

-   Will have no idle time (why have time in the schedule where nothing is being done?)
-   No inversion (doing a task with a later deadline before a task with a earlier deadline)
	-   If there is an inversion, there must be two consecutive jobs that are inverted.

#### Proof idea: (Exchange argument)

- Claim: All schedules with **no inversion** and **no idle time** have the same maximal lateness
- If there is an optimal soln with an inversion, you can swap the inversion and show the maximal lateness does not change. Any jobs before and after the two inverted items will not be affected as the time block will be the same.
		- By flipping the inversion, we may have reduced the lateness of these two jobs, but the maximal lateness may have been caused by a different job, which means the maximal lateness doesn’t change.

![[Pasted image 20230118132655.png]]

![[Pasted image 20230118132703.png]]

![[Pasted image 20230118132708.png]]

## Data Compression
![[Pasted image 20230118132723.png]]

![[Pasted image 20230118132805.png]]
![[Pasted image 20230118132945.png]]
![[Pasted image 20230118132950.png]]
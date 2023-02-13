Lets say we have another interval scheduling problem

## Example
A sales person needs to schedule meetings with potential clients, and each meeting has a start and end time, and also a priority. In this case, its the profit for each meeting.

We want to maximize profit in this scenario.

![[Obsidian-Attachments/Pasted image 20230125102316.png]]

We can either include the last interval 8, or we schedule the last interval that is not 8 in the optimal solution.

![[Obsidian-Attachments/Pasted image 20230125102524.png]]

### Potential inefficient algorithm
![[Obsidian-Attachments/Pasted image 20230125102856.png]]

This means we store the value of each prior call and see if we can improve it.

Uses induction on the recurrence in the memoization to prove correctness

## How to know to use DP
Greedy: 

- There is usually some sort of ordering, where the first choice (best choice) is enough to guarantee it is in the optimal solution
- Can proceed with the rest of the solution 

Divide and conquer:

- easier to do the problem when split into pieces

Dynamic programming:

- Suppose you have all the optimal solution to subproblems, and you're asked to find the optimal solution that contains with the $n^{th}$ value
- Kinda like the induction hypothesis, where you assume the $n-1$ steps all work.
- Try greedy first, and if you can't solve it then you probably need to use dynamic programming
- ![[Obsidian-Attachments/Pasted image 20230130115026.png]]

## Knapsack problem with DP

We have items with value $v_{i}$ and weight $w_{i}$, and we can select items to put in the knapsack.

Case 1:
- Does not include the $i$th item
- The optimal solution selects the best of $1,2, \dots, i-1$ items using weight limit $w$
Case 2:
- Include the $i$th item
- new weight limit is $w-w_{i}$
- Optimal solution selects best of $1,2, \dots,(i-1)$ using the new weight limit

![[Obsidian-Attachments/Pasted image 20230201102001.png]]
![[Obsidian-Attachments/Pasted image 20230201101951.png]]
![[Obsidian-Attachments/Pasted image 20230201101940.png]]
#### Logic behind the value table
The way each value in the table is selected is by taking the max value of:
- not including the new last node (look at the cell above)
- including the new last node (look at the row above, cell of max weight = current weight - weight of last node)

## Sequence alignment
![[Obsidian-Attachments/Pasted image 20230201105657.png]]
![[Obsidian-Attachments/Pasted image 20230201105830.png]]
![[Obsidian-Attachments/Pasted image 20230201105959.png]]
### Needleman-Wunsch Algorithm
![[Obsidian-Attachments/Pasted image 20230201110010.png]]
![[Obsidian-Attachments/IMG_8650 copy 2.png]]

#### Basic process
For each pair of letters in each string, we either:
- Match/mismatch (+2, -3) them to obtain a value of **the left upper diagonal** +2 or -3
	- Ex. Take (row 2, col 3) = (C,T). They do not match, so we could simply take the upper left diag value of (A,C) - 3 = 0 - 3 = **-3**
- If they don't match, we can gap the first string (-2), meaning match the bottom string letter to a gap. **This takes the value to the left and increments it.**
	- Ex. They do not match, so we can either gap the top letter T, resulting in the left value (C,C) - 2 = 2-2 = **0**.
- Gap the second string (-2), meaning match the top string letter to a gap. **This takes the value above it and increments it.**
	- Ex. They do not match, so we can gap the bottom letter C, resulting in the top value (A,T) - 2 = -4 - 2 = **-6**.
- $\max\{-3,0,-6\}=0$, so therefore **(C,T) = 0**.

#### Complexity
$O(mn)$ time complexity
$O(mn)$ space complexity

For english words/sentences, $m,n<10$ 
But for computational biology, $m,n$ could be 100,000, resulting in 10 billion operations! This needs to be improved.

### Improving the sequence alignment problem
We can represent the table $M$ as a directed acyclic graph $G_{M}$. Each node in the graph is a subproblem.

As we build the table $M$ we can think of this as a directed acyclic graph $G_M$.
Each node represents a subproblem.
For each $M[i, j]$ we have a node $(i, j)$.
Nodes $u_1, u_2, \ldots, u_k \rightarrow v$ means subproblem $v$ can only be solved once subproblems $u_1, u_2, \ldots, u_k$ have been solved.
For the sequence alignment problem, this means that $(i-1, j) \rightarrow(i, j),(i, j-1) \rightarrow(i, j)$ and $(i-1, j-1) \rightarrow(i, j)$.
Can add the weight:
- $\delta$ to $(i-1, j) \rightarrow(i, j)$ and $(i, j-1) \rightarrow(i, j)$ 
	- *coming from previous mismatch/gap*
- $\alpha$ to $(i-1, j-1) \rightarrow(i, j)$
	- *coming from prev match*

What can we say about the optimal alignment and $G_{M}$? 
A max(min) weight path from $(0, 0)$ to $(m, n)$ gives the optimal alignment.

![[Obsidian-Attachments/4-Dynamic Programming.png]]

We just need the columns $M[\cdot, j]$ and $M[\cdot, j - 1]$ to create an array $B$ with two columns and $m$ rows.

Suppose we found the optimal value, but we don't know the path that we had to take to get there. We can use the following algorithm to obtain that path.

We can use Divide and Conquer, by splitting the table in half

**First pass from (0,0) proof:**
For $i+j=0$, we have $f(i, j)=f(0,0)=0=\operatorname{Opt}(i, j)$.
For arbitrary $i, j$, suppose that $f\left(i^{\prime}, j^{\prime}\right)=\operatorname{Opt}\left(i^{\prime}, j^{\prime}\right)$ for $i^{\prime}+j^{\prime}<i+j$.
Consider the last edge on the longest path (highest weight) to $(i, j)$.
By construction of the graph, this edge comes from either $(i-1, j-1),(i-1, j)$ or $(i, j-1)$.
So
$$
\begin{aligned}
f(i, j) & =\max \left(\alpha_{x_i y_i}+f(i-1, j-1), \delta+f(i-1, j), \delta+f(i, j-1)\right) \\
\text { by I.H } & =\max \left(\alpha_{x_i y_i}+\operatorname{Opt}(i-1, j-1), \delta+\operatorname{Opt}(i-1, j), \delta+\operatorname{Opt}(i, j-1)\right) \\
& =\operatorname{Opt}(i, j)
\end{aligned}
$$

Then we want to walk through from the bottom (m,n)
![[Obsidian-Attachments/4-Dynamic Programming-3.png]]

![[Obsidian-Attachments/4-Dynamic Programming-2.png]]
#### Complexity
So the path (longest path just means path with highest weight) will just have a complexity of:
- $O(mn)$ time: computing $g(\cdot, j)$ 
- $O(m + n)$ space.

### Hirschberg’s Algorithm

Divide:
Find the index $q$ that maximizes $f(q, n/2) + g(q, n/2)$ using our **Space Efficient Algorithm** and save $(i, j) = (q, n/2)$ as part of our solution. 
Align $x_{q}$ with $y_{n/2}$ . 

Conquer:
Recursively compute optimal alignment on each piece.

![[Obsidian-Attachments/4-Dynamic Programming-1.png]]

![[Obsidian-Attachments/4-Dynamic Programming-4.png]]

#### Space complexity
![[Obsidian-Attachments/4-Dynamic Programming-5.png]]

#### Time complexity
![[Obsidian-Attachments/4-Dynamic Programming-6.png]]
![[Obsidian-Attachments/4-Dynamic Programming-7.png]]
![[Obsidian-Attachments/4-Dynamic Programming-8.png]]
^ Read over the proof a bit more, key condition is that $k\geq \frac{c}{2}$

```toc style: bullet | number | inline (default: bullet) min_depth: number (default: 2) max_depth: number (default: 6) title: string (default: undefined) allow_inconsistent_headings: boolean (default: false) delimiter: string (default: |) varied_style: boolean (default: false)
```

-   Split problem into smaller subproblems (usually equal size)
-   Solve each subproblem
-   Combine solutions (usually in linear time)
-   Often results in reduction of brute force, O(n2) algorithm to O(n log n) time

## Counting inversions
We use this method to measure how *similar* two lists are. 

### Definition
*List A: $a_1, a_2, \dots, a_n$*
There is an inversion between item $i$ and $j$ if $i < j$ but $a_i > a_j$.

There can be in total $O(n^2)$ if we look at all nodes paired with all other nodes. By counting the inversions, we can obtain a measure of "sortedness" of a list.

### How Divide and Conquer tackles inversions
By dividing a list in half, we only need to compare inversions in each smaller list. 
![[Pasted image 20230118092906.png]]
This algorithm will *still take* $O(n^2)$! To reduce it to $O(n log n)$, we must sort the list **after dividing in half.**

### Proving time complexity of MergeSort

Want to show that:
T(n) = 2(Tn/2) + O(n) + O(1) \in O(n log n)

#### Proof
$$
\begin{align*}
T(n) &= 2T(n/2) + cn \quad \text{Let } T(1) = a \\
T(n/2) &= 2T(n/4) + c n/2 \\
T(n/4) &= 2T(n/8) + c n/4 \\
\vdots & \\
T(n) &= 2(2T(n/4) +c n/2) + cn\\
T(n) &= 2(2(2T(n/8) + cn/4) + c n/2) + cn\\
&= 2^3 T(n/2^3) + cn + cn + cn\\
&=  2^3 T(n/2^3) + 3cn\\
T(n) &= 2^i T(n/2^i) + icn\\
& \quad \text{where eventually } n/2^i = 1 => i = log_2 n\\
&= n a + c n log n
\end{align*}
$$

Or you could use the [masters theorem](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)), maybe it would be helpful to memorize this.

## Finding Closest Pairs

For example, wanting to find the closest pair of points in an XY plane. 

The brute force solution is $O(n^2)$ when comparing every single point with every other point.

### D&C

- Draw a line to separate the points, and compare closest points in each half.
- Combine by comparing every point from one side with the other
- Return the closest of the three sets of points

#### Key to Divide and Conquer algorithm for Closest Pairs
![[Pasted image 20230118095931.png]]

**You only need to check the 11 points below your starting point within the white lines.**

- This means that the number of points youre checking is constant.
- There is no way you can have one point per box, because they must be $\delta$ apart and each box is size $\delta/2$ by $\delta/2$

![[Pasted image 20230118100123.png]]

### Time Complexity
![[Pasted image 20230118100844.png]]
Same kind of proof as [[#Proving time complexity of MergeSort]].

## Matrix Multiplication 

*Not really tested or need to know* 

The brute force algorithm would take $O(n^3)$

Trying to divide the matrices into n/2 blocks doesn't actually save any time.

### Strassens Algorithm
Reduce 8 multiplications to 7. Increase 4 additions to 18.
![[Pasted image 20230118102027.png]]
#### Complexity

$$
\begin{align*}
T(n) &= 7T(n/2) + 18 O(n2) = 7(log n) + O(n2 log n)\\
&\implies 2^{2.81 log\ n} + O(n^2 log n)\\
&\implies T(n) \in O(n2.81)\\
\end{align*}
$$

#### Q. How good does it get? 
A. Best known algorithms:

- O(2.521813)(12/1979) 
- O(2.521801)(01/1980) 
- O(2.376)(1987)

## Binary Multiplication
![[Pasted image 20230123111458.png]]

![[Pasted image 20230123111612.png]]
![[Pasted image 20230123111820.png]]

![[Pasted image 20230123112803.png]]
![[Pasted image 20230123112813.png]]
Where the worst case complexity is $O(n^2)$

## Complexity when only splitting and multiplying three times
![[Pasted image 20230123112841.png]]
![[Pasted image 20230123113435.png]]
![[Pasted image 20230123113424.png]]
![[Pasted image 20230123113821.png]]
![[Pasted image 20230123113826.png]]But replace the $n-1$ in the geometric sum with $k$, which is $log_2 (n-1)$

![[Pasted image 20230123114107.png]]
![[Pasted image 20230123114256.png]]

## Master theorem
Useless if it's not in the corrent format $aT(\frac{n}{b}) + n^c$

![[Pasted image 20230123114518.png]]

Can get a bound on the floor/ceil, kinda complicated how its proved, don't need to derive ourselves by hand. 
![[Pasted image 20230123114522.png]]

## In tutorial
Attempt back substitution 
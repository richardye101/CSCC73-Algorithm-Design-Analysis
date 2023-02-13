The problem states we are given a sequence of price quotes, $S = [s_{1}\dots s_{n}]$, and we are tasked with finding the longest increasing trend.

\subsection*{Solution}
The subproblem here is computing the longest increasing trend from the first element to index $i$. We will assume we are given a non-empty sequence of price quotes since in the empty case we can trivially return 0, and that all prices are non negative (stock prices cannot be negative).

Computing the solution using the subproblems:

A price can be part of any increasing trend before it as long it's larger than the largest price in that trend. 

At the first element, the longest trend ending at it trivially only includes itself, so $LIT[0]$ is set as 1. Otherwise, we can take the max length of each increasing trend ending at the price quote $j$ where $0\leq j<i$ plus the $i^{th}$ price, as long as the $j^{th}$ price quote is strictly less than the $i^{th}$ price. This recurrence relation implies that if the $i^{th}$ price is larger than the $j^{th}$ price, then price $i$ is larger than all the preceding prices in the increasing trend ending at the $j^{th}$ price.

We can solve this problem using dynamic programming, through memoizing the length of the longest increase trend ending at each index in an array $LIT$. We initialize all items in this array to 1, to indicate that each price quote consists of an increasing trend of 1 initially.

Recursive formula:
$$
\begin{aligned}
LIT(i) = \begin{cases}
1  & i = 0 \\
\max(LIT(i-j)+1)  & S[i] > S[i-j] & \text{Where } 0\leq j<i\\
\end{cases}\\
\end{aligned}
$$


```
1
1
1
max(1, 2, 2, 2) = 2
max(1, 3, 2, 2, 2) = 3
max(1, 4, 3, 2, 2, 2) = 4
max(1, 0, 0 ,0, 2, 2, 2) = 2
max(1, 3, 0, 0, 0, 2, 2, 2) = 3
max(1, 4, 3, 0, 4, 3, 2, 2, 2) = 4
max(1, 5, 4, 3, 5, 4, 3, 2, 2, 2) = 5
max(1, 0, 0, 4, 3, 0, 4, 3, 2, 2, 2) = 4
max(1, 0, 0, 0, 0, 0, 0, 0, 0, 2, 2, 0) = 2
```
In the first case, when the sequence is empty, we have no values and there is no trend. Therefore we return 0. 
In the second case, when the sequence only contains one element, we simply return 1 as the trend will only contain one value.
In the third case, when the current value is greater than the previous value, we will add 1 to the length of the longest increasing trend within $S[1],\dots,S[i-1]$, which we will have precomputed. We do this because we can create a longer increasing trend by including the current value.
In the fourth case, when the current value is smaller than the previous, we simply return the length of the longest trend within $S[1],\dots,S[i-1]$ because the current value will not increase that length as it did not increase from the previous value.
	Because we don't specify which values are in the longest increasing trend, this implies that the $S[i-1]^{th}$ value could be the largest value within  $S[1], \dots, S[i-1]$, *or* the $S[i]^{th}$ value. This covers the case where the $S[i-j] < S[i-1]$ where $1<j<i$ and $S[i] \leq S[i-1]$ but $S[i] > S[i-j]$, which is when $S[i]$ is greater than all values before it except $S[i-1]$.
	

\begin{algorithm}
\caption{COARSE\_INVERSIONS}
\begin{algorithmic}[1]
\Function{COARSE\_INVERSIONS}{L}
    \If{length(L) $\leq$ 1}
        \State \Return{0, L}
    \EndIf
    \Let{l}{L[0:(length(L) / 2)]}
    \Let{r}{L[(length(L) / 2):L.size()]}
    \Let{($x_l$, A)}{COARSE\_INVERSIONS(l)}
    \Let{($x_r$, B)}{COARSE\_INVERSIONS(r)}
    \Let{($x$, merged\_L)}{MERGE\_COUNT(A, B)}
    \State \Return{($x + x_l + x_r$, merged\_L)}
\EndFunction
\State
\Function{MERGE\_COUNT}{A, B}
    \Let{merged\_L}{[0]*(A.size() + B.size())}
    \Let{$x$, $a$, $b$}{0}
    \For{i=0; i$<$merged\_L.size(); i=i+1}
        \If{$a ==$ A.size() $||$ ($b \neq$ B.size() \&\& A[$a$] $>$ B[$b$])}
            \If{$a \neq$ A.size() \&\& A[$a$] $>$ 2 B[$b$]} 
                \Let{x}{x + (A.size() $-\;a$)} \Comment{Add inversion count to x}
            \EndIf
            \Let{merged\_L[i]}{B[$b$]}
            \Let{$b$}{$b+1$}
        \Else
            \Let{merged\_L[i]}{A[$a$]}
            \Let{$a$}{$a+1$}
        \EndIf
    \EndFor
    \State \Return{(x, merged\_L)}
\EndFunction
\end{algorithmic}
\end{algorithm}


\subsection*{Correctness Proof}
Consider the function \texttt{COARSE\_INVERSIONS}: it recursively splits the array into halfs - after which the arrays are merged using \texttt{MERGE\_COUNT}\\
\texttt{MERGE\_COUNT} is the function that counts the inversions and returns x to \texttt{COARSE\_INVERSIONS}.\\
Therefore, let's consider the correctness of the \texttt{MERGE\_COUNT}:

Let our loop invariant for \texttt{lines 14-22} be:\\
After the $k$th iteration:
\begin{enumerate}
    \item The sub-array merged\_L[0: k] consists of the sorted $k$ smallest elements of $A$ and $B$
    \item $x$ is the number of coarse inversions between $A^\prime = A\left[0: a\right]$ and $B^\prime = B\left[0:b\right]$.
\end{enumerate}

For sake of notation, let:
\begin{itemize}
    \item $\text{LIC}_k\leftarrow$``Loop-invariant condition (k)''
\end{itemize}


\textbf{Base case} On entering the loop:\\
$i = a = b = 0$\\
merged\_L is empty which satisfies $\text{LIC}_1$\\
$x = 0$ which is correct and satisfies $\text{LIC}_2$

\textbf{IH:} Suppose LI holds for $k$th iteration, WTS LI holds for $k+1$th iteration:

\textbf{Induction Step}: Consider merged\_L[0: k] after the kth loop iteration.\\
On the $k+1$th iteration, there are two cases to consider:
\begin{enumerate}
    \item The if-statement on \texttt{line 15} is true. In this case we know either:
    \begin{enumerate}
        \item There are no more values to read from A. (\texttt{a == A.size()}). In this case, $x$ should remain unchanged - which is true by if-statement  on \texttt{line 16}.
        \item There are more values to read from A. In this case, we check if A[a] $>$ 2 B[b]. If this is true, then $x$ will be incremented by A.size() $-\;a$. Which is accounts for exactly A[a] and all remaining, larger values in A that are at least as large as A[a] (recall A is sorted). Therefore the correctness of $x$'s value is persisted.
     \end{enumerate}
     In either case, B[b] will be appended to merged\_L, and since B[b] is the smallest of the remaining values, $\text{LIC}_2$ is satisfied. By (a) and (b) we conclude $\text{LIC}_2$ is also satisfied.
     \item The if-statement on \texttt{line 15} is false. In this case, A[a] will be appended to merged\_L. By definition of the if-statement on \texttt{line 15}, either B's items were already appended to merged\_L, or A[a] is the smallest remaining value. Either way $\text{LIC}_1$ will continue to hold. Also, since A[a] is the smallest remaining value, and A's items come before B, A[a] causes no coarse inversions and $\text{LIC}_2$ holds as well.
\end{enumerate}

We've shown the LI will persist for the duration of the for-loop on \texttt{line 14}. This means, after the loop:
\begin{enumerate}
    \item The array merged\_L consists of the sorted, combined elements of $A$ and $B$
    \item $x$ is the number of coarse inversions between $A$ and $B$.
\end{enumerate}
(It is clear from the logic, that at the end of the loop ($a + b =$ merged\_L.size())\\
$\therefore$ We can conclude \texttt{MERGE\_COUNT} returns the number of coarse inversions as well as a merged and sorted combination of lists A and B as wanted.\\
By induction, \texttt{COARSE\_INVERSIONS} will return the number of coarse inversions in L.\\
(Note: If a a single returned value $x$ is required, a trivial helper-function could be used).

\subsection*{Time Complexity Analysis}
\texttt{Line 4} and \texttt{line 5} are O(1)\\
\texttt{Line 6} and \texttt{line 7} are both T(n/2)\\
\texttt{Line 8} is O(n)
\\
Therefore, $T(n) = 2T(n/2) + O(n) + O(1) \in O(n \log n)$ by master theorem.\\
$\blacksquare$
$$
\begin{aligned}
f
\end{aligned}
$$
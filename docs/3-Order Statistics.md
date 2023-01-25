```toc style: bullet | number | inline (default: bullet) min_depth: number (default: 2) max_depth: number (default: 6) title: string (default: undefined) allow_inconsistent_headings: boolean (default: false) delimiter: string (default: |) varied_style: boolean (default: false)
```

GIven an array $A$ of $n$ numbers, we want to return the $k^{th}$ largest/smallest value ($k^{th}$ order statistic).

We **don't** want to sort, want better than $O(n log\ n)$ time, want $O(n)$ time

We could try Quicksort?

## Refresher on Quicksort
If its a sizeable array $A$ of size $n$, then we use a pivot to partition the array, and recursively split until you reach size 1

## Quicksort For Order Statistics
To use quicksort for this, we need to **choose a good pivot**. The pivot must:

- Reduce the input size significantly for the side we search in
- Want to pick the pivot efficiently

### How?
- Break $A$ into constant size groups.
	- Sorting these constant size groups (lets say of size 5) is constant time, as you could try a variety of methods, that are constant time.
- For each group find the median in constant time 
- Recursively find the median of the medians
	- ![[Pasted image 20230125094734.png]]
	- ![[Pasted image 20230125095016.png]]
	- $3(\frac{\lfloor n/5 \rfloor}{2}) = \lfloor 3n / 10 \rfloor$ Which is the same for the number of elements larger than $X$.
		- Where 3 is the number of values larger than the median in each group
		- n/5 thats how many groups there are in total
		- div by 2 because we're only looking at half the groups
- Until we have a pivot

### Time complexity calculation

![[Pasted image 20230125095342.png]]
![[Pasted image 20230125095705.png]]
![[Pasted image 20230125100107.png]]
Its only this bad as long as $n\ge 50$

## The Algorithm
![[Pasted image 20230125100503.png]]

How can we prove the time complexity? We can't use the masters theory, but we can use induction.

Let the $c$ if $n < 50$ be a different constant $d$ (her typo)
![[Pasted image 20230125101241.png]]


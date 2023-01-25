```toc style: bullet | number | inline (default: bullet) min_depth: number (default: 2) max_depth: number (default: 6) title: string (default: undefined) allow_inconsistent_headings: boolean (default: false) delimiter: string (default: |) varied_style: boolean (default: false)
```

Lets say we have another interval scheduling problem

## Example
A sales person needs to schedule meetings with potential clients, and each meeting has a start and end time, and also a priority. In this case, its the profit for each meeting.

We want to maximize profit in this scenario.

![[Pasted image 20230125102316.png]]

We can either include the last interval 8, or we schedule the last interval that is not 8 in the optimal solution.

![[Pasted image 20230125102524.png]]

### Potential inefficient algorithm
![[Pasted image 20230125102856.png]]
## Q1
Want to minimize the amount of stops made on a road trip to recharge. The car has a specific range of r. There is a list of $n$ stations at distance $x_i$.

Algorithm: (mine, not the one being proved)

```
stops_made(r)
	stops = 0
	range_left = r
	for i in stations-1
		if i < r
			range_left = range_left - i
		if i >= r
			# stopped at previous stop
			stops = stops + 1
			range_left = r - i
```

Proof using exchange:
	Let O be opt soln most similar to S
	Let S be the greedy soln
	Let i be the first station that differes between them


## Q2
Interval scheduling

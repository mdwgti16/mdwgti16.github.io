---
title: USACO Training - (Section 1.3) Milking Cows -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 11:27:28 +0900'
categories:
- USACO Training
---

> ## Milking Cows

### Milking Cows
#### Three farmers rise at 5 am each morning and head for the barn to milk three cows. The first farmer begins milking his cow at time 300 (measured in seconds after 5 am) and ends at time 1000. The second farmer begins at time 700 and ends at time 1200. The third farmer begins at time 1500 and ends at time 2100. The longest continuous time during which at least one farmer was milking a cow was 900 seconds (from 300 to 1200). The longest time no milking was done, between the beginning and the ending of all milking, was 300 seconds (1500 minus 1200).

#### Your job is to write a program that will examine a list of beginning and ending times for N (1 <= N <= 5000) farmers milking N cows and compute (in seconds):

	The longest time interval at least one cow was milked.
	The longest time interval (after milking starts) during which no cows were being milked.

### INPUT FORMAT

<table border="1">
<tbody><tr> <td> Line 1: </td> <td>The single integer, N</td>
</tr><tr> <td> Lines 2..N+1: </td> <td>Two non-negative integers less
than 1,000,000, respectively the starting and ending time in seconds
after 0500</td>

</tr></tbody></table>
<br>

### SAMPLE INPUT (file milk2.in)
	300 1000
	700 1200
	1500 2100
	
### OUTPUT FORMAT
#### A single line with two integers that represent the longest continuous time of milking and the longest idle time.

### SAMPLE OUTPUT (file milk2.out)
	900 300

### Answer - Python
```python
def sortKeyGen(line):
	if line:
		start,end=map(int,line.strip().split(" "))
		return start
fin = open('milk2.in','r')
nCows = int(fin.readline().strip())
times = fin.read().split('\n')
times = list(filter(None, times))
times.sort(key=sortKeyGen)
mStart,mEnd = map(int,times[0].strip().split(" "))

longestC = mEnd-mStart
longestI = 0

for time in times:
	if time:
		start, end = map(int,time.strip().split(" "))

		if start-mEnd >= longestI :
			longestI = start-mEnd

		if mStart<=start and start<=mEnd:
			if mEnd<end:
					mEnd=end
		else:
			if mEnd-mStart > longestC:
					longestC = mEnd-mStart
			mStart,mEnd = start,end

with open('milk2.out','w') as fout:
	fout.write(f"{longestC} {longestI}\n")
```

* ###### [Milking Cows]
* ###### [mdwgti16/USACO]

[Milking Cows]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=milk2
[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.3
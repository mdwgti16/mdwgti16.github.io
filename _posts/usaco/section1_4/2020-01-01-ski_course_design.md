---
title: USACO Training - (Section 1.4) Ski Course Design -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 16:37:28 +0900'
categories:
- USACO Training
---

> ## Ski Course Design

### Ski Course Design
#### Farmer John has N hills on his farm (1 <= N <= 1,000), each with an integer elevation in the range 0 .. 100. In the winter, since there is abundant snow on these hills, FJ routinely operates a ski training camp.

#### Unfortunately, FJ has just found out about a new tax that will be assessed next year on farms used as ski training camps. Upon careful reading of the law, however, he discovers that the official definition of a ski camp requires the difference between the highest and lowest hill on his property to be strictly larger than 17. Therefore, if he shortens his tallest hills and adds mass to increase the height of his shorter hills, FJ can avoid paying the tax as long as the new difference between the highest and lowest hill is at most 17.

#### If it costs x^2 units of money to change the height of a hill by x units, what is the minimum amount of money FJ will need to pay? FJ can change the height of a hill only once, so the total cost for each hill is the square of the difference between its original and final height. FJ is only willing to change the height of each hill by an integer amount.

### INPUT FORMAT

#### Line 1:	The integer N.
#### Lines 2..1+N:	Each line contains the elevation of a single hill.


### SAMPLE INPUT (file skidesign.in)
	5
	20
	4
	1
	24
	21
	
### INPUT DETAILS:
#### FJ's farm has 5 hills, with elevations 1, 4, 20, 21, and 24.


### OUTPUT FORMAT

#### The minimum amount FJ needs to pay to modify the elevations of his hills so the difference between largest and smallest is at most 17 units.
#### Line 1:


### SAMPLE OUTPUT (file skidesign.out)
	18
	
### OUTPUT DETAILS:

#### FJ keeps the hills of heights 4, 20, and 21 as they are. He adds mass to the hill of height 1, bringing it to height 4 (cost = 3^2 = 9). He shortens the hill of height 24 to height 21, also at a cost of 3^2 = 9.

		
### Answer - Python
```python
fin = open('skidesign.in','r')
N = int(fin.readline().strip())

hills= [int(l.strip()) for l in fin.readlines()]
hills.sort()

c=(hills[0]+hills[-1])//2
i=0
costMin=9999999
while i<=hills[-1]-17:    
    costs=[]
    for e in hills:
        if e<i:
            costs.append(i-e)
        elif e>i+17:
            costs.append(e-(i+17))
    
    total=0
    for c in costs:
        total+=c**2
    if total<costMin:
        costMin=total
    i+=1

with open('skidesign.out','w') as fout:
    fout.write(f"{costMin}\n")
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.4
---
title: USACO Training - (Section 1.6) Number Triangles -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 13:37:28 +0900'
categories:
- USACO Training
---

> ## Number Triangles

### Number Triangles
#### Consider the number triangle shown below. Write a program that calculates the highest sum of numbers that can be passed on a route that starts at the top and ends somewhere on the base. Each step can go either diagonally down to the left or diagonally down to the right.

            7

          3   8

        8   1   0

      2   7   4   4

    4   5   2   6   5
	
#### In the sample above, the route from 7 to 3 to 8 to 7 to 5 produces the highest sum: 30

### INPUT FORMAT

#### The first line contains R (1 <= R <= 1000), the number of rows. Each subsequent line contains the integers for that particular row of the triangle. All the supplied integers are non-negative and no larger than 100.


### SAMPLE INPUT (file numtri.in)
	5
	7
	3 8
	8 1 0
	2 7 4 4
	4 5 2 6 5


### OUTPUT FORMAT

#### A single line containing the largest sum using the traversal specified.



### SAMPLE OUTPUT (file numtri.out)
	30
	
		
### Answer - Python
```python
fin = open('numtri.in','r')
n = int(fin.readline())
nums=[ list(map(int,e.strip().split(" "))) for e in fin.readlines()]

for i, line in enumerate(nums):
    if i!=0:
        for j,e in enumerate(line):
            if j==0 :
                nums[i][j]=nums[i][j]+nums[i-1][j]
                continue
            elif e==line[-1] :
                nums[i][j]=nums[i][j]+nums[i-1][j-1]
            if i!=n-1:
                if nums[i][j]>=nums[i][j-1]:
                    nums[i+1][j]+=nums[i][j]
                else :
                    nums[i+1][j]+=nums[i][j-1]

with open('numtri.out','w') as fout:
    fout.write(f'{max(nums[-1][:])}\n')
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.6
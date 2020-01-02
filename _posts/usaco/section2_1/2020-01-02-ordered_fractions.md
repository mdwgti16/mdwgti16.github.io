---
title: USACO Training - (Section 2.1) Ordered Fractions -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-2 16:57:28 +0900'
categories:
- USACO Training
---

> ## Ordered Fractions

### Ordered Fractions
#### Consider the set of all reduced fractions between 0 and 1 inclusive with denominators less than or equal to N.

#### Here is the set when N = 5:

	0/1 1/5 1/4 1/3 2/5 1/2 3/5 2/3 3/4 4/5 1/1
	
#### Write a program that, given an integer N between 1 and 160 inclusive, prints the fractions in order of increasing magnitude.

### INPUT FORMAT
#### One line with a single integer N.

	
### SAMPLE INPUT (file frac1.in)
	5


### OUTPUT FORMAT

#### One fraction per line, sorted in order of magnitude.


### SAMPLE OUTPUT (file castle.out)
	0/1
	1/5
	1/4
	1/3
	2/5
	1/2
	3/5
	2/3
	3/4
	4/5
	1/1
	

		
### Answer - Python
```python
def getKey(s):
    num,den=map(int,s.strip().split("/"))
    return num/den

fin = open('frac1.in','r')
N = int(fin.readline().strip())
def getGcd(a,b):
    q,r=divmod(a,b)
    if r == 0:
        return b
    else :
        return getGcd(b,r)
fracList=[]
fracList.append("0/1")
for den in range(1,N+1):
    for num in range(1,den+1):
        gcd=getGcd(den,num)
        frac=f"{int(num/gcd)}/{int(den/gcd)}"
        if not frac in fracList:
            fracList.append(frac)
            
fracList.sort(key=getKey)

with open('frac1.out','w') as fout:
    for e in fracList:
        fout.write(e+'\n')
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%202/Section%201
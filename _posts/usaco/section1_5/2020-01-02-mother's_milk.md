---
title: USACO Training - (Section 1.5) Mother's Milk -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 17:57:28 +0900'
categories:
- USACO Training
---

> ## Mother's Milk

### Mother's Milk
#### Farmer John has three milking buckets of capacity A, B, and C liters. Each of the numbers A, B, and C is an integer from 1 through 20, inclusive. Initially, buckets A and B are empty while bucket C is full of milk. Sometimes, FJ pours milk from one bucket to another until the second bucket is filled or the first bucket is empty. Once begun, a pour must be completed, of course. Being thrifty, no milk may be tossed out.

#### Write a program to help FJ determine what amounts of milk he can leave in bucket C when he begins with three buckets as above, pours milk among the buckets for a while, and then notes that bucket A is empty.

### INPUT FORMAT

#### A single line with the three integers A, B, and C.


### SAMPLE INPUT (file milk3.in)
	8 9 10


### OUTPUT FORMAT

#### A single line with a sorted list of all the possible amounts of milk that can be in bucket C when bucket A is empty.



#### There will be no more than 10,000 sequences.


### SAMPLE OUTPUT (file milk3.out)
	1 2 8 9 10

### SAMPLE INPUT (file milk3.in)
	2 5 10
### SAMPLE OUTPUT (file milk3.out)
	5 6 7 8 9 10
	
		
### Answer - Python
```python
def getKey(s):
    n1,n2=map(int,s.strip().split(" "))
    return n2
fin = open('milk3.in','r')
a,b,c = map(int,fin.readline().split(" "))
result=set()
cList=[a,b,c]
mList=[0,0,c]
result=set()
temp=[]
def bucketC(mList):
    for i,p in enumerate(mList):
        for j,q in enumerate(mList):
            if i!=j:
                t=mList[:]
                if cList[j]-q>=p:
                    t[j]=q+p
                    t[i]=0
                else:
                    t[i]=p-(cList[j]-q)
                    t[j]=cList[j]
                if not t[-1] in result and not t in temp:
                    if t[0]==0:
                        result.add(t[-1])
                    temp.append(t)
                    bucketC(t)
bucketC(mList)                
result=list(result)                    
result.sort()
            
with open('milk3.out','w') as fout:
    out=''
    for e in result:
        out+=f'{e} '
    fout.write(out.strip()+"\n")
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.5
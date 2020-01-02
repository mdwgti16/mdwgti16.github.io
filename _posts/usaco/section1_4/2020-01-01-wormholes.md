---
title: USACO Training - (Section 1.4) Wormholes -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 16:17:28 +0900'
categories:
- USACO Training
---

> ## Wormholes

### Wormholes
#### Farmer John's hobby of conducting high-energy physics experiments on weekends has backfired, causing N wormholes (2 <= N <= 12, N even) to materialize on his farm, each located at a distinct point on the 2D map of his farm (the x,y coordinates are both integers).

#### According to his calculations, Farmer John knows that his wormholes will form N/2 connected pairs. For example, if wormholes A and B are connected as a pair, then any object entering wormhole A will exit wormhole B moving in the same direction, and any object entering wormhole B will similarly exit from wormhole A moving in the same direction. This can have rather unpleasant consequences.

#### For example, suppose there are two paired wormholes A at (1,1) and B at (3,1), and that Bessie the cow starts from position (2,1) moving in the +x direction. Bessie will enter wormhole B [at (3,1)], exit from A [at (1,1)], then enter B again, and so on, getting trapped in an infinite cycle!

		 | . . . .
		 | A > B .      Bessie will travel to B then
		 + . . . .      A then across to B again
		 
#### Farmer John knows the exact location of each wormhole on his farm. He knows that Bessie the cow always walks in the +x direction, although he does not remember where Bessie is currently located.

#### Please help Farmer John count the number of distinct pairings of the wormholes such that Bessie could possibly get trapped in an infinite cycle if she starts from an unlucky position. FJ doesn't know which wormhole pairs with any other wormhole, so find all the possibilities (i.e., all the different ways that N wormholes could be paired such that Bessie can, in some way, get in a cycle). Note that a loop with a smaller number of wormholes might contribute a number of different sets of pairings to the total count as those wormholes that are not in the loop are paired in many different ways.

### INPUT FORMAT

<table border="1">

<tbody><tr><td>Line 1:</td><td> The number of wormholes, N.</td></tr>

<tr><td>Lines 2..1+N:</td><td> Each line contains two space-separated
integers describing the (x,y) coordinates of a single wormhole.
Each coordinate is in the range 0..1,000,000,000.  </td></tr>

</tbody></table>

### SAMPLE INPUT (file wormhole.in)
	4
	0 0
	1 0
	1 1
	0 1
	
### INPUT DETAILS:
#### There are 4 wormholes, forming the corners of a square.


### OUTPUT FORMAT

#### Line 1:	The number of distinct pairings of wormholes such that Bessie could conceivably get stuck in a cycle walking from some starting point in the +x direction.

### SAMPLE OUTPUT (file wormhole.out)
	2
	
#### OUTPUT DETAILS

	If we number the wormholes 1..4 as we read them from the input, then if wormhole 1 pairs with wormhole 2 and wormhole 3 pairs with wormhole 4, Bessie can get stuck if she starts anywhere between (0,0) and (1,0) or between (0,1) and (1,1).

		 | . . . .
		 4 3 . . .      Bessie will travel to B then
		 1-2-.-.-.      A then across to B again
	Similarly, with the same starting points, Bessie can get stuck in a cycle if the pairings are 1-3 and 2-4 (if Bessie enters WH#3 and comes out at WH#1, she then walks to WH#2 which transports here to WH#4 which directs her towards WH#3 again for a cycle).

	Only the pairings 1-4 and 2-3 allow Bessie to walk in the +x direction from any point in the 2D plane with no danger of cycling.
		
### Answer - Python
```python
def getKey(s):
    n1,n2=map(int,s.strip().split(" "))
    return n2
fin = open('wormhole.in','r')
N = int(fin.readline().strip())

wh= [l.strip() for l in fin.readlines()]
wh.sort(key=getKey)

isLast={}
for i in range(1,N):
    p1,p2=map(int,wh[i-1].strip().split(" "))
    n1,n2=map(int,wh[i].strip().split(" "))
    if p2 != n2:
        isLast[wh[i-1]]=1
    else:
        isLast[wh[i-1]]=0
else:
    isLast[wh[-1]]=1
        
cnt=0

def isTrapped(c,i,p,pr):
    if c>N:
        return True
    if wh[i] in p:
        i = wh.index(p[wh[i]])
    else:
        i = wh.index(pr[wh[i]])
    if isLast[wh[i]]:
        return False
    return isTrapped(c+1,i+1,p,pr)

def getPair(i,c,p):
    global cnt
    if c==N//2:
        for k in range(N):
            pr={v: k for k, v in p.items()}
            if isTrapped(0,k,p,pr):
                cnt+=1
                break
        return
    for j in range(i+1,N):
        isContain=False
        for e in p.items():
            if wh[i] in e or wh[j] in e:
                isContain =True
                break
        if not isContain:
            p[wh[i]]=wh[j]
            getPair(getIndex(p),c+1,p)
        
            if len(p)>0:
                del p[wh[i]]
def getIndex(p):
    for i in range(N):
        for e in p.items():
            if wh[i] in e:
                break
        else:
            return i

p={}
getPair(0,0,p)

with open('wormhole.out','w') as fout:
    fout.write(f"{cnt}\n")
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.4
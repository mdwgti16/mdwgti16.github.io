---
title: USACO Training - (Section 1.4) Barn Repair -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 16:47:28 +0900'
categories:
- USACO Training
---

> ## Barn Repair

### Barn Repair
#### It was a dark and stormy night that ripped the roof and gates off the stalls that hold Farmer John's cows. Happily, many of the cows were on vacation, so the barn was not completely full.

#### The cows spend the night in stalls that are arranged adjacent to each other in a long line. Some stalls have cows in them; some do not. All stalls are the same width.

#### Farmer John must quickly erect new boards in front of the stalls, since the doors were lost. His new lumber supplier will supply him boards of any length he wishes, but the supplier can only deliver a small number of total boards. Farmer John wishes to minimize the total length of the boards he must purchase.

#### Given M (1 <= M <= 50), the maximum number of boards that can be purchased; S (1 <= S <= 200), the total number of stalls; C (1 <= C <= S) the number of cows in the stalls, and the C occupied stall numbers (1 <= stall_number <= S), calculate the minimum number of stalls that must be blocked in order to block all the stalls that have cows in them.

#### Print your answer as the total number of stalls blocked.

### INPUT FORMAT

<table border="1">
<tbody><tr> <td> Line 1: </td> <td>M, S, and C (space separated) </td>
</tr><tr> <td> Lines 2-C+1: </td> <td> Each line contains one integer, the
number of an occupied stall.
</td></tr></tbody></table>


### SAMPLE INPUT (file barn1.in)
	4 50 18
	3
	4
	6
	8
	14
	15
	16
	17
	21
	25
	26
	27
	30
	31
	40
	41
	42
	43
	
### OUTPUT FORMAT

#### A single line with one integer that represents the total number of stalls blocked.

### SAMPLE OUTPUT (file barn1.out)
	25
	
#### [One minimum arrangement is one board covering stalls 3-8, one covering 14-21, one covering 25-31, and one covering 40-43.]
		
### Answer - Python
```python
fin = open('barn1.in','r')
m, s, c = map(int,fin.readline().strip().split())
if m>=c:
    total=c
else:
    barnList = list(filter(None,fin.read().split('\n')))
    barnList = [int(n) for n in barnList]
    barnList.sort()
    start=end = barnList[0]
    
    bList=[]
    i=0
    for i in range(1,c-1):
        if barnList[i]-end==1:
            end=barnList[i]
        else:
            bList.append(end-start+1)

            bList.append(barnList[i]-end)
            start=end=barnList[i]
    else:
        bList.append(end-start+1)
        bList.append(barnList[i+1]-end)
        bList.append(1)

    while True:    
        min=9999
        t=0

        for i in range(2,len(bList),2):
            result=0
            for j in range(0,len(bList),2):
                if 0<i-j<=2:
                    continue
                elif i==j:
                    result+=bList[i-2]+bList[i-1]+bList[i]-1
                else:
                    result+=bList[j]

            if result<=min:
                min=result
                t=i
        
        tempList=[]
        for j in range(len(bList)):
                if 0<t-j<=2:
                    continue
                elif t==j:
                    tempList.append(bList[t-2]+bList[t-1]+bList[t]-1)
                else:
                    tempList.append(bList[j])
        bList=tempList        
        
        if int((len(bList)+1)/2)==m:
            total=min
            break
            
with open('barn1.out','w') as fout:
    fout.write(f"{total}\n")
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.4
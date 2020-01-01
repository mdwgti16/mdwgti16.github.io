---
title: USACO Training - (Section 1.4) Combination Lock -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 17:47:28 +0900'
categories:
- USACO Training
---

> ## Combination Lock

### Combination Lock
#### Farmer John's cows keep escaping from his farm and causing mischief. To try and prevent them from leaving, he purchases a fancy combination lock to keep his cows from opening the pasture gate.

#### Knowing that his cows are quite clever, Farmer John wants to make sure they cannot easily open the lock by simply trying many different combinations. The lock has three dials, each numbered 1..N (1 <= N <= 100), where 1 and N are adjacent since the dials are circular. There are two combinations that open the lock, one set by Farmer John, and also a "master" combination set by the lock maker.

#### The lock has a small tolerance for error, however, so it will open even if the numbers on the dials are each within at most 2 positions of a valid combination.

#### For example, if Farmer John's combination is (1,2,3) and the master combination is (4,5,6), the lock will open if its dials are set to (1,3,5) (since this is close enough to Farmer John's combination) or to (2,4,8) (since this is close enough to the master combination). Note that (1,5,6) would not open the lock, since it is not close enough to any one single combination.

#### Given Farmer John's combination and the master combination, please determine the number of distinct settings for the dials that will open the lock. Order matters, so the setting (1,2,3) is distinct from (3,2,1).

### INPUT FORMAT

<table border="1">

<tbody><tr><td>Line 1:</td><td>The integer N.</td></tr><tr>

</tr><tr><td>Line 2:</td><td> Three space-separated integers, specifying
	Farmer John's combination.</td></tr>

<tr><td>Line 3:</td><td> Three space-separated integers, specifying the master
        combination (possibly the same as Farmer John's combination).</td></tr>
</tbody></table>

### SAMPLE INPUT (file combo.in)
	50
	1 2 3
	5 6 7
	
### INPUT DETAILS:
#### Each dial is numbered 1..50. Farmer John's combination is (1,2,3), and the master combination is (5,6,7).

### OUTPUT FORMAT

#### Line 1:	The number of distinct dial settings that will open the lock.

### SAMPLE OUTPUT (file combo.out)
	249
	
#### [SAMPLE OUTPUT EXPLANATION](https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=combo)
		
### Answer - Python
```python
fin = open('combo.in','r')
N = int(fin.readline().strip())

total=0
f1,f2,f3= map(int, fin.readline().strip().split(" "))
m1,m2,m3 = map(int, fin.readline().strip().split(" "))

def close(a,b):
    if abs(a-b) <= 2 :return True;
    if abs(a-b) >= N-2 : return True;
    return False;
    
def close_enough(n1,n2,n3,c1,c2,c3):
    return close(n1,c1) and close(n2,c2) and close(n3,c3);

for n1 in range(1,N+1):
    for n2 in range(1,N+1):
        for n3 in range(1,N+1):
            if close_enough(n1,n2,n3,f1,f2,f3) or\
                 close_enough(n1,n2,n3,m1,m2,m3):
                total+=1
           
with open('combo.out','w') as fout:
    fout.write(f"{total}\n")
```

* ###### [Combination Lock]
* ###### [mdwgti16/USACO]

[Combination Lock]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=combo
[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.4
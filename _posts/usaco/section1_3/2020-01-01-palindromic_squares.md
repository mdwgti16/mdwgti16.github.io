---
title: USACO Training - (Section 1.3) Palindromic Squares -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 15:27:28 +0900'
categories:
- USACO Training
---

> ## Palindromic Squares

### Palindromic Squares
#### Palindromes are numbers that read the same forwards as backwards. The number 12321 is a typical palindrome.

#### Given a number base B (2 <= B <= 20 base 10), print all the integers N (1 <= N <= 300 base 10) such that the square of N is palindromic when expressed in base B; also print the value of that palindromic square. Use the letters 'A', 'B', and so on to represent the digits 10, 11, and so on.

#### Print both the number and its square in base B.

### INPUT FORMAT

#### A single line with B, the base (specified in base 10).

### SAMPLE INPUT (file palsquare.in)
	10
	
### OUTPUT FORMAT

#### Lines with two integers represented in base B. The first integer is the number whose square is palindromic; the second integer is the square itself. NOTE WELL THAT BOTH INTEGERS ARE IN BASE B!

### SAMPLE OUTPUT (file palsquare.out)
	1 1
	2 4
	3 9
	11 121
	22 484
	26 676
	101 10201
	111 12321
	121 14641
	202 40804
	212 44944
	264 69696

### Answer - Python
```python
nList=[n for n in range(10)]
nList.extend([chr(n) for n in range(ord('A'),ord('A')+10)])

def convert(num,base):
	t=[]
	while True:
		if num>=base:
			q,r=divmod(num,base)
			num=q
			t.append(nList[r])
		else:
			t.append(nList[num])
			break
	return ''.join(str(e) for e in t)[::-1]

with open('palsquare.in','r') as f:
	base = int(f.readline().strip())
    
pDict={}
num=1

while num<=300:
	square = str(convert(num**2,base))
	if square == square[::-1] :
		pDict[convert(num,base)]=square
	num+=1
    
with open('palsquare.out','w') as f:
	for k, v in pDict.items():
		f.write(f'{k} {v}\n')
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.3
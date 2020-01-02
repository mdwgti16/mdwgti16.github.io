---
title: USACO Training - (Section 1.3) Dual Palindromes -Python-
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

> ## Dual Palindromes

### Dual Palindromes
#### A number that reads the same from right to left as when read from left to right is called a palindrome. The number 12321 is a palindrome; the number 77778 is not. Of course, palindromes have neither leading nor trailing zeroes, so 0220 is not a palindrome.

#### The number 21 (base 10) is not palindrome in base 10, but the number 21 (base 10) is, in fact, a palindrome in base 2 (10101).

#### Write a program that reads two numbers (expressed in base 10):

	N (1 <= N <= 15)
	S (0 < S < 10000)
	
#### and then finds and prints (in base 10) the first N numbers strictly greater than S that are palindromic when written in two or more number bases (2 <= base <= 10).

#### Solutions to this problem do not require manipulating integers larger than the standard 32 bits.


### INPUT FORMAT

#### A single line with space separated integers N and S.


### SAMPLE INPUT (file dualpal.in)
	3 25
	
	
### OUTPUT FORMAT

#### N lines, each with a base 10 number that is palindromic when expressed in at least two of the bases 2..10. The numbers should be listed in order from smallest to largest.


### SAMPLE OUTPUT (file dualpal.out)
	26
	27
	28

### Answer - Python
```python
nList=[n for n in range(10)]
nList.extend([chr(n) for n in range(ord('A'),ord('A')+15)])

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

with open('dualpal.in','r') as f:
	n,s = map(int,f.readline().strip().split(" "))
    
l=[]
num=s+1
while len(l)<n:
	cnt=0
	for i in range(2,11):
		t = convert(num,i)
		if t == t[::-1]:
			cnt+=1
		if cnt>=2:
			l.append(num)
			break
	num+=1
    
    
with open('dualpal.out','w') as f:
	for n in l:
		f.write(f'{n}\n')
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.3
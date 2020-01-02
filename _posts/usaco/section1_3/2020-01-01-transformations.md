---
title: USACO Training - (Section 1.3) Transformations -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 14:27:28 +0900'
categories:
- USACO Training
---

> ## Transformations

### Transformations
#### A square pattern of size N x N (1 <= N <= 10) black and white square tiles is transformed into another square pattern. Write a program that will recognize the minimum transformation that has been applied to the original pattern given the following list of possible transformations:

	#1: 90 Degree Rotation: The pattern was rotated clockwise 90 degrees.
	#2: 180 Degree Rotation: The pattern was rotated clockwise 180 degrees.
	#3: 270 Degree Rotation: The pattern was rotated clockwise 270 degrees.
	#4: Reflection: The pattern was reflected horizontally 
	(turned into a mirror image of itself by reflecting around a vertical line in the middle of the image).
	#5: Combination: The pattern was reflected horizontally and then subjected to one of the rotations (#1-#3).
	#6: No Change: The original pattern was not changed.
	#7: Invalid Transformation: The new pattern was not obtained by any of the above methods.

#### In the case that more than one transform could have been used, choose the one with the minimum number above.

### INPUT FORMAT
<table>
<tbody><tr> <td> Line 1: </td> <td> A single integer, N </td> </tr>
<tr> <td> Line 2..N+1: </td> <td> N lines of N characters (each either
		`@' or `-'); this is the square before
		transformation</td> </tr>
<tr> <td> Line N+2..2*N+1: </td> <td> N lines of N characters (each either
		`@' or `-'); this is the square after transformation</td>
		</tr>
</tbody>
</table>

### SAMPLE INPUT (file transform.in)
	3
	@-@
	---
	@@-
	@-@
	@--
	--@
	
### OUTPUT FORMAT
#### A single line containing the number from 1 through 7 (described above) that categorizes the transformation required to change from the `before' representation to the `after' representation.

### SAMPLE OUTPUT (file transform.out)
	1

### Answer - Python
```python
def getPattern(f,N):
	p=[]
	for i in range(N):
		p.append(fin.readline().strip())
	return p

def rotateClockwise(p,N):
	after=[]
	for j in range(N):
		after.append(p[N-1][j])
	for i in range(N-1):
		for j in range(N):
			after[j]+=p[N-2-i][j]
	return after

def verticalReflect(p,N):
	after=[]
	for line in p:
		if N%2==1:
			after.append(line[::-1])
		else:
			after.append(line[int(N/2):N][::-1]+line[0:int(N/2)][::-1])
	return after
fin = open('transform.in','r')
N = int(fin.readline().strip())
before=getPattern(fin,N)
after=getPattern(fin,N)

isSecond=False
temp = before
isRunning=True
while isRunning:
	for rotate in range(3):
		temp=rotateClockwise(temp,N)
		if temp==after:
			if isSecond:
				result = 5
			else:
				result = rotate+1
			isRunning=False
			break
	if not isRunning:
		break
	if isSecond:
		result=7
		break

	temp=before
	temp=verticalReflect(temp,N)
	if temp==after:
		result=4
		break
	isSecond=True
with open('transform.out','w') as fout:
	fout.write(f"{result}\n")
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.3
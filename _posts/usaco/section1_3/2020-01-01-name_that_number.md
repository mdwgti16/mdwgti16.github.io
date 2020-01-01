---
title: USACO Training - (Section 1.3) Name That Number -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 14:57:28 +0900'
categories:
- USACO Training
---

> ## Name That Number

### Name That Number
#### Among the large Wisconsin cattle ranchers, it is customary to brand cows with serial numbers to please the Accounting Department. The cow hands don't appreciate the advantage of this filing system, though, and wish to call the members of their herd by a pleasing name rather than saying, "C'mon, #4734, get along."

#### Help the poor cowhands out by writing a program that will translate the brand serial number of a cow into possible names uniquely associated with that serial number. Since the cow hands all have cellular saddle phones these days, use the standard Touch-Tone(R) telephone keypad mapping to get from numbers to letters (except for "Q" and "Z"):

		2: A,B,C     5: J,K,L    8: T,U,V
		3: D,E,F     6: M,N,O    9: W,X,Y
		4: G,H,I     7: P,R,S
		
#### Acceptable names for cattle are provided to you in a file named "dict.txt", which contains a list of fewer than 5,000 acceptable cattle names (all letters capitalized). Take a cow's brand number and report which of all the possible words to which that number maps are in the given dictionary which is supplied as dict.txt in the grading environment (and is sorted into ascending order).

#### For instance, the brand number 4734 produces all the following names:

		GPDG GPDH GPDI GPEG GPEH GPEI GPFG GPFH GPFI GRDG GRDH GRDI
		GREG GREH GREI GRFG GRFH GRFI GSDG GSDH GSDI GSEG GSEH GSEI
		GSFG GSFH GSFI HPDG HPDH HPDI HPEG HPEH HPEI HPFG HPFH HPFI
		HRDG HRDH HRDI HREG HREH HREI HRFG HRFH HRFI HSDG HSDH HSDI
		HSEG HSEH HSEI HSFG HSFH HSFI IPDG IPDH IPDI IPEG IPEH IPEI
		IPFG IPFH IPFI IRDG IRDH IRDI IREG IREH IREI IRFG IRFH IRFI
		ISDG ISDH ISDI ISEG ISEH ISEI ISFG ISFH ISFI
		
#### As it happens, the only one of these 81 names that is in the list of valid names is "GREG".

#### Write a program that is given the brand number of a cow and prints all the valid names that can be generated from that brand number or ``NONE'' if there are no valid names. Serial numbers can be as many as a dozen digits long.

### INPUT FORMAT

#### A single line with a number from 1 through 12 digits in length.

### SAMPLE INPUT (file namenum.in)
	4734
	
### OUTPUT FORMAT

#### A list of valid names that can be generated from the input, one per line, in ascending alphabetical order.

### SAMPLE OUTPUT (file namenum.out)
	GREG

### Answer - Python
```python
with open('namenum.in','r') as f:
	numbers = f.readline().strip()
with open('dict.txt','r') as f:
	cDict = f.read().strip().split('\n')
                                  
keyMap = {2: 'ABC', 5: 'JKL', 8: 'TUV',
          3: 'DEF', 6: 'MNO', 9: 'WXY',
          4: 'GHI', 7: 'PRS'}
bName=[]
def keyToBrandName(name ,numbers,i):
	if i == len(str(numbers)):
		for dName in cDict:
			if name == dName:
				bName.append(name)
		return

	for k in keyMap[int(numbers[i])]:
		temp = name+k
		for dName in cDict:
			if dName.startswith(temp):
				keyToBrandName(temp, numbers, i+1)
				break

keyToBrandName("", numbers, 0)
if not bName:
	bName.append('NONE')
    
with open('namenum.out','w') as f:
	for i in range(len(bName)):
		f.write(f'{bName[i]}\n')
```

* ###### [Name That Number]
* ###### [mdwgti16/USACO]

[Name That Number]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=namenum
[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.3
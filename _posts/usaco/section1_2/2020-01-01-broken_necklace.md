---
title: USACO Training - (Section 1.2) Broken Necklace -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 11:27:28 +0900'
categories: [USACO Training]
---

> ## Broken Necklace

### Broken Necklace
#### You have a necklace of N red, white, or blue beads (3<=N<=350) some of which are red, others blue, and others white, arranged at random. Here are two examples for n=29:

                1 2                               1 2
            r b b r                           b r r b
          r         b                       b         b
         r           r                     b           r
        r             r                   w             r
       b               r                 w               w
      b                 b               r                 r
      b                 b               b                 b
      b                 b               r                 b
       r               r                 b               r
        b             r                   r             r
         b           r                     r           r
           r       r                         r       b
             r b r                             r r w
            Figure A                         Figure B
								r red bead
								b blue bead
								w white bead

#### The beads considered first and second in the text that follows have been marked in the picture.

#### The configuration in Figure A may be represented as a string of b's and r's, where b represents a blue bead and r represents a red one, as follows: brbrrrbbbrrrrrbrrbbrbbbbrrrrb .

#### Suppose you are to break the necklace at some point, lay it out straight, and then collect beads of the same color from one end until you reach a bead of a different color, and do the same for the other end (which might not be of the same color as the beads collected before this).

#### Determine the point where the necklace should be broken so that the most number of beads can be collected.

### Example

#### For example, for the necklace in Figure A, 8 beads can be collected, with the breaking point either between bead 9 and bead 10 or else between bead 24 and bead 25.

#### In some necklaces, white beads had been included as shown in Figure B above. When collecting beads, a white bead that is encountered may be treated as either red or blue and then painted with the desired color. The string that represents this configuration can include any of the three symbols r, b and w.

#### Write a program to determine the largest number of beads that can be collected from a supplied necklace.

### INPUT FORMAT
	Line 1:	N, the number of beads
	Line 2:	a string of N characters, each of which is r, b, or w

### SAMPLE INPUT (file beads.in)
	29
	wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
	
### OUTPUT FORMAT
#### A single line containing the maximum of number of beads that can be collected from the supplied necklace.

### SAMPLE OUTPUT (file beads.out)
	11

### [OUTPUT EXPLANATION](https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=beads)

### Answer - Python
```python
fin = open('beads.in','r')
bNum = int(fin.readline())
beads = fin.readline().strip()

pre = ppre = ''
i=0
cnt = wcnt = 0
bMax = left = 0
isSecond=False
while True:
	if beads[i] != pre:
		if beads[i] == 'w':
			wcnt=1
		else:
			if beads[i] == ppre:
				cnt=cnt+wcnt+1
				wcnt=0
			else:
				ppre=beads[i]
				if left+cnt+wcnt>bMax:
					bMax=left+cnt+wcnt
				if isSecond:
					break

				left=cnt

				if pre == 'w':
					cnt=wcnt+1
					wcnt=0
				else:
					cnt=1

		pre=beads[i]
	else:           
		if beads[i] == 'w':
			wcnt+=1
		else:
			cnt+=1
	i+=1   
	if i==bNum:
		if (cnt+wcnt) == (bNum*2):
			bMax=bNum
			break
		i=0
		isSecond=True
with open('beads.out','w') as fout:
	fout.write(f"{bMax}\n")
```

* ###### [Broken Necklace]
* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.2
[Broken Necklace]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=beads
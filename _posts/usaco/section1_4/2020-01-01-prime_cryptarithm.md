---
title: USACO Training - (Section 1.4) Prime Cryptarithm -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 17:17:28 +0900'
categories:
- USACO Training
---

> ## Prime Cryptarithm

### Prime Cryptarithm
#### (This poorly named task has nothing to do with prime numbers or even, really, prime digits. Sorry 'bout that.)

#### A cryptarithm is usually presented as a pencil-and-paper task in which the solver is required to substitute a digit for each of the asterisks (or, often, letters) in the manual evaluation of an arithmetic term or expression so that the consistent application of the digits results in a proper expression. A classic example is this cryptarithm, shown with its unique solution:

		  SEND            9567       S->9  E->5  N->6  D->7
		+ MORE          + 1085       M->1  O->0  R->8
		-------        -------
		 MONEY           10652       Y->2
		 
#### The following cryptarithm is a multiplication problem that can be solved by substituting digits from a specified set of N digits into the positions marked with *. Since the asterisks are generic, any digit from the input set can be used for any of the asterisks; any digit may be duplicated as many times as desired.

#### Consider using the set {2,3,5,7} for the cryptarithm below:

	    * * *
	 x    * *
	   -------
	    * * *         <-- partial product 1 -- MUST BE 3 DIGITS LONG
	  * * *           <-- partial product 2 -- MUST BE 3 DIGITS LONG
	   -------
	  * * * *
#### Digits can appear only in places marked by '*'. Of course, leading zeroes are not allowed.

#### The partial products must be three digits long, even though the general case (see below) might have four digit partial products.

#### ********** Note About Cryptarithm's Multiplication ************
#### In USA, children are taught to perform multidigit multiplication as described here. Consider multiplying a three digit number whose digits are 'a', 'b', and 'c' by a two digit number whose digits are 'd' and 'e':

#### [Note that this diagram shows far more digits in its results than the required diagram above which has three digit partial products!]

              a b c     <-- number 'abc'
            x   d e     <-- number 'de'; the 'x' means 'multiply'
         -----------
	p1      * * * *     <-- product of e * abc; first star might be 0 (absent)
	p2    * * * *       <-- product of d * abc; first star might be 0 (absent)
         -----------
          * * * * *     <-- sum of p1 and p2 (e*abc + 10*d*abc) == de*abc

#### Note that the 'partial products' are as taught in USA schools. The first partial product is the product of the final digit of the second number and the top number. The second partial product is the product of the first digit of the second number and the top number.

#### Write a program that will find all solutions to the cryptarithm above for any subset of supplied non-zero single-digits. Note that the multiplicands, partial products, and answers must all conform to the cryptarithm's framework.

### INPUT FORMAT

<table border="1">
<tbody><tr> <td> Line 1: </td> <td> N, the number of digits that will be used</td>
</tr><tr> <td> Line 2: </td> <td>N space separated non-zero digits with which to solve the cryptarithm </td>
</tr></tbody></table>

### SAMPLE INPUT (file crypt1.in)
	5
	2 3 4 6 8
	
### OUTPUT FORMAT

#### A single line with the total number of solutions. Here is the one and only solution for the sample input:

      2 2 2
    x   2 2
     ------
      4 4 4
    4 4 4
    ---------
    4 8 8 4

### SAMPLE OUTPUT (file crypt1.out)
	1
	
#### OUTPUT DETAILS
	Here's why 222x22 works: 3 digits times 2 digits yields two (equal!) partial products, each of three digits (as required). The answer has four digits, as required. Each digit used {2, 4, 8} is in the supplied set {2, 3, 4, 6, 8}.

	Why 222x23 doesn't work:

	    2 2 2   <-- OK:  three digits, all members of {2, 3, 4, 6, 8}
          2 3   <-- OK:  two digits, all members of {2, 3, 4, 6, 8}
	    ------
	    6 6 6   <-- OK:  three digits, all members of {2, 3, 4, 6, 8}
	  4 4 4     <-- OK:  three digits, all members of {2, 3, 4, 6, 8}
	 ---------
	  5 1 0 6   <-- NOT OK: four digits (good), but 5, 1, and 0 are not in
																												{2, 3, 4, 6, 8}
		
### Answer - Python
```python
fin = open('crypt1.in','r')
n = int(fin.readline().strip())
nList= [int(e) for e in fin.readline().strip().split(" ")]

def isMember(p):
    isMember=True
    for e in p:                                
        if nList.count(int(e))==0:
            isMember=False
            break
    return isMember

cnt=0
for a in nList:
    for b in nList:
        for c in nList:
            for d in nList:
                for e in nList:
                    isValid=True
                    pList=[]
                    abc=a*100+b*10+c
                    
                    for i,de in enumerate([e,d]):
                        p=str(abc*de)
                        if len(p)>3:
                            isValid=False
                            break
                        else :
                            if not isMember(p):
                                isValid=False
                                break    
                        if isValid:
                            pList.append(p)
                        else:
                            break
                    if not isValid:
                        continue
                    result=int("0"+pList[0])+int(pList[1]+"0")
                    if isMember(str(result)):
                        cnt+=1
                        
with open('crypt1.out','w') as fout:
    fout.write(f"{cnt}\n")
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.4
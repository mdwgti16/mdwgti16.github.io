---
title: USACO Training - (Section 1.4) Mixing Milk -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 15:47:28 +0900'
categories:
- USACO Training
---

> ## Mixing Milk

### Mixing Milk
#### The Merry Milk Makers company buys milk from farmers, packages it into attractive 1- and 2-Unit bottles, and then sells that milk to grocery stores so we can each start our day with delicious cereal and milk.

#### Since milk packaging is such a difficult business in which to make money, it is important to keep the costs as low as possible. Help Merry Milk Makers purchase the farmers' milk in the cheapest possible manner. The MMM company has an extraordinarily talented marketing department and knows precisely how much milk they need each day to package for their customers.

#### The company has contracts with several farmers from whom they may purchase milk, and each farmer has a (potentially) different price at which they sell milk to the packing plant. Of course, a herd of cows can only produce so much milk each day, so the farmers already know how much milk they will have available.

#### Each day, Merry Milk Makers can purchase an integer number of units of milk from each farmer, a number that is always less than or equal to the farmer's limit (and might be the entire production from that farmer, none of the production, or any integer in between).

#### Given:
	The Merry Milk Makers' daily requirement of milk
	The cost per unit for milk from each farmer
	The amount of milk available from each farmer

#### calculate the minimum amount of money that Merry Milk Makers must spend to meet their daily need for milk.

#### Note: The total milk produced per day by the farmers will always be sufficient to meet the demands of the Merry Milk Makers even if the prices are high.


### INPUT FORMAT

<table border="1">
<tbody><tr> <td> Line 1: </td> <td> Two integers, N and M.
<br>The first value, N,  (0 &lt;= N &lt;=
2,000,000) is the amount of milk that Merry Milk Makers wants per day.
<br>The second, M, (0 &lt;= M &lt;= 5,000) is the number of farmers
that they may buy from.
<br>
</td>
</tr><tr> <td> Lines 2 through M+1: </td> <td> The next M lines each contain
two integers: P<sub>i</sub> and A<sub>i</sub>. <br> P<sub>i</sub> (0 &lt;= P<sub>i</sub> &lt;= 1,000) is price in
cents that farmer i charges.<br>  A<sub>i</sub> (0 &lt;= Ai &lt;= 2,000,000) is the amount
of milk that farmer i can sell to Merry Milk Makers per day.
</td>
</tr></tbody></table>


### SAMPLE INPUT (file milk.in)
	100 5
	5 20
	9 40
	3 10
	8 80
	6 30
	
### OUTPUT FORMAT

#### A single line with a single integer that is the minimum cost that Merry Milk Makers must pay for one day's milk.

### SAMPLE OUTPUT (file milk.out)
	630
	
### [INPUT/OUTPUT EXPLANATION](https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=milk)


		
### Answer - Python
```python
def sortKeyGen(line):
	if line:
		price,amount=map(int,line.strip().split(" "))
		return price
fin = open('milk.in','r')
milk, n = map(int,fin.readline().strip().split())
unitList = fin.read().split('\n')
unitList = list(filter(None, unitList))

unitList.sort(key=sortKeyGen)

pay=0
for unit in unitList:
	if unit:
		price,amount=map(int,unit.strip().split(" "))
		if milk==0:
			break
		if milk>=amount:
			milk-=amount
			pay+=price*amount
		else:
			pay+=price*milk
			milk=0
        
with open('milk.out','w') as fout:
	fout.write(f"{pay}\n")
```

* ###### [Mixing Milk]
* ###### [mdwgti16/USACO]

[Mixing Milk]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=milk
[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.4
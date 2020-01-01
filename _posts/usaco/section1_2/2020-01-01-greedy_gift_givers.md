---
title: USACO Training - (Section 1.2) Greedy Gift Givers -Python-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 11:27:28 +0900'
categories: [USACO Training]
---

> ## Greedy Gift Givers

### Greedy Gift Givers
#### A group of NP (2 ≤ NP ≤ 10) uniquely named friends has decided to exchange gifts of money. Each of these friends might or might not give some money to some or all of the other friends (although some might be cheap and give to no one). Likewise, each friend might or might not receive money from any or all of the other friends. Your goal is to deduce how much more money each person receives than they give.

#### The rules for gift-giving are potentially different than you might expect. Each person goes to the bank (or any other source of money) to get a certain amount of money to give and divides this money evenly among all those to whom he or she is giving a gift. No fractional money is available, so dividing 7 among 2 friends would be 3 each for the friends with 1 left over – that 1 left over goes into the giver's "account". All the participants' gift accounts start at 0 and are decreased by money given and increased by money received.

#### In any group of friends, some people are more giving than others (or at least may have more acquaintances) and some people have more money than others.

		Given:
		a group of friends, no one of whom has a name longer than 14 characters,
		the money each person in the group spends on gifts, and
		a (sub)list of friends to whom each person gives gifts,
		determine how much money each person ends up with.

### INPUT FORMAT

<table border="1" style="border-collapse: collapse;">
<tbody><tr><th>Line #</th><th>Contents</th></tr>
<tr> <td align="center">1</td> <td> A single integer, NP </td></tr>
<tr> <td align="center">2..NP+1</td> <td> Line <i>i</i>+1 contains the name
of group member <i>i</i></td>
</tr>
<tr> <td valign="top" align="center">NP+2..end</td> <td>NP groups of lines organized like this:

<table>
<tbody><tr><td>The first line of each group tells the person's name who
will be giving gifts.
</td></tr><tr><td>The second line in the group contains two numbers:
	<ul>
	<li>The amount of money (in the range 0..2000) to be divided
        into gifts by the giver
        </li><li>NG<sub>i</sub> (0 ≤ NG<sub>i</sub> ≤ NP), the
        number of people to whom the giver will give gifts
	</li></ul>
</td></tr><tr><td> If NG<sub>i</sub> is nonzero, each of the next NG<sub>i</sub>
lines lists the name of a recipient of a gift; recipients are not repeated
in a single giver's list.
</td></tr></tbody></table>

</td></tr></tbody></table>
<br>
### SAMPLE INPUT (file gift1.in)
	5
	dave
	laura
	owen
	vick
	amr
	dave
	200 3
	laura
	owen
	vick
	owen
	500 1
	dave
	amr
	150 2
	vick
	owen
	laura
	0 2
	amr
	vick
	vick
	0 0

### SAMPLE OUTPUT (file gift1.out)

	dave 302
	laura 66
	owen -359
	vick 141
	amr -150

### [OUTPUT EXPLANATION](https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=gift1)
	
### Answer - Python
```python
fin = open('gift1.in','r')
np = int(fin.readline().strip())
dictOfMoney = { fin.readline().strip() : 0 for i in range(np) }

while(True):
	giver = fin.readline().strip()
	if(giver==""):
		break
	amount, divided = map(int, fin.readline().strip().split())
	receivers = [fin.readline().strip() for i in range(divided)]

	try:
		quotient, remainder = divmod(amount,divided)
	except:
		quotient = remainder = 0

	for receiver in receivers:
		dictOfMoney[receiver] += quotient
	dictOfMoney[giver] += -amount + remainder

with open('gift1.out','w') as fout:
	for name, money in dictOfMoney.items():
		fout.write(f"{name} {money}\n")
```

* ###### [Greedy Gift Givers]
* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.2
[Greedy Gift Givers]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=gift1
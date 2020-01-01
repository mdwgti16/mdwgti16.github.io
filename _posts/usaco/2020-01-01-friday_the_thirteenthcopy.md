---
title: USCAO Training - Friday the Thirteenth (Python)
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 12:27:28 +0900'
categories: USACO Training
---

> ## Friday the Thirteenth

### Friday the Thirteenth
#### Is Friday the 13th really an unusual event?

#### That is, does the 13th of the month land on a Friday less often than on any other day of the week? To answer this question, write a program that will compute the frequency that the 13th of each month lands on Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, and Saturday over a given period of N years. The time period to test will be from January 1, 1900 to December 31, 1900+N-1 for a given number of years, N. N is positive and will not exceed 400.

#### Note that the start year is NINETEEN HUNDRED, not NINETEEN NINETY.

#### There are few facts you need to know before you can solve this problem:

#### January 1, 1900 was on a Monday.
#### Thirty days has September, April, June, and November, all the rest have 31 except for February which has 28 except in leap years when it has 29.
#### Every year evenly divisible by 4 is a leap year (1992 = 4*498 so 1992 will be a leap year, but the year 1990 is not a leap year)

#### The rule above does not hold for century years. Century years divisible by 400 are leap years, all other are not. Thus, the century years 1700, 1800, 1900 and 2100 are not leap years, but 2000 is a leap year.

#### Do not use any built-in date functions in your computer language.

#### Don't just precompute the answers, either, please.

### INPUT FORMAT

	One line with the integer N.

### SAMPLE INPUT (file friday.in)
	20
	
### OUTPUT FORMAT
#### Seven space separated integers on one line. These integers represent the number of times the 13th falls on Saturday, Sunday, Monday, Tuesday, ..., Friday.

### SAMPLE OUTPUT (file friday.out)

	36 33 34 33 35 35 34

### Answer - Python
```python
fin = open('friday.in','r')
N = int(fin.readline())
dayOfWeeks = {i : 0 for i in range(7)}
months = [31,28,31,30,31,30,31,31,30,31,30,31]
day = 0;
for y in range(1900,1900+N):
	for m in months:
		dayOfWeeks[day%7] += 1
		if(m==28):
			if(y%400==0 or (y%100!=0 and y%4==0)):
				day += 29
				continue
		day += m
with open('friday.out','w') as fout:
	for day in range(6):
		fout.write("{} ".format(dayOfWeeks[day]))
	fout.write("{}\n".format(dayOfWeeks[6]))
```

* ###### [Friday the Thirteenth]

[Friday the Thirteenth]: https://train.usaco.org/usacoprob2?a=miQqOSmwjhm&S=friday
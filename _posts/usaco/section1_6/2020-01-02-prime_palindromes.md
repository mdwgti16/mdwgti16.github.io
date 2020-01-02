---
title: USACO Training - (Section 1.6) Prime Palindromes -C-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-2 13:57:28 +0900'
categories:
- USACO Training
---

> ## Prime Palindromes

### Prime Palindromes
#### The number 151 is a prime palindrome because it is both a prime number and a palindrome (it is the same number when read forward as backward). Write a program that finds all prime palindromes in the range of two supplied numbers a and b (5 <= a < b <= 100,000,000); both a and b are considered to be within the range .

### INPUT FORMAT

#### Line 1:	Two integers, a and b


### SAMPLE INPUT (file pprime.in)
	5 500


### OUTPUT FORMAT

#### The list of palindromic primes in numerical order, one per line.


### SAMPLE OUTPUT (file pprime.out)
	5
	7
	11
	101
	131
	151
	181
	191
	313
	353
	373
	383
	
		
### Answer - C
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

FILE* fout;
int a, b;

void isPrime(int n);
void getPals(int i, int n, int isMid, char* str);


int main() {
	FILE* fin = fopen("pprime.in", "r");
	fout = fopen("pprime.out", "w");
	
	int t;
	int an, bn;
	
	fscanf(fin, "%d %d", &a,&b);

	an = bn = 0;
	t = a;
	while (t)
	{
		t /= 10;
		an += 1;
	}
	t = b;
	while (t)
	{
		t /= 10;
		bn += 1;
	}

	char* str;
	str = (char*)calloc(bn+1, sizeof(char));
	
	for (int i = an; i <= bn; i++) {
		getPals(0, i, 0, str);
	}	

	return 0;
}

void isPrime(int n) {
	for (int d = 3; d*d <= n; d += 2) {
		if (n % d == 0) {
			return;
		}
	}
	if(n>=a && n<=b)
		fprintf(fout, "%d\n", n);
	
}
void getPals(int i, int n, int isMid, char* str) {
	int d = 1;
	int j = 0;
	char c[2];

	if (!isMid) {
		d = 2;
		j = 1;
	}
	
	if (n / 2 == i) {
		if (n % 2 == 1) {
			for (; j <= 9; j += d) {
				sprintf(c, "%d", j);
				str[i] = c[0];
				isPrime(atoi(str));
			}
			return;
		}

		isPrime(atoi(str));
		return;
	}
	
	for (; j <= 9; j += d) {
		sprintf(c, "%c", j);
		sprintf(c, "%d", j);
		str[i] = c[0];
		str[n - 1 - i] = c[0];

		getPals(i + 1, n, 1, str);

	}
}
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.6
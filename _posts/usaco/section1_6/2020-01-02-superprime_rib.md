---
title: USACO Training - (Section 1.6) Superprime Rib -C-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-2 14:57:28 +0900'
categories:
- USACO Training
---

> ## Superprime Rib

### Superprime Rib
#### Butchering Farmer John's cows always yields the best prime rib. You can tell prime ribs by looking at the digits lovingly stamped across them, one by one, by FJ and the USDA. Farmer John ensures that a purchaser of his prime ribs gets really prime ribs because when sliced from the right, the numbers on the ribs continue to stay prime right down to the last rib, e.g.:

     7  3  3  1
		 
#### The set of ribs denoted by 7331 is prime; the three ribs 733 are prime; the two ribs 73 are prime, and, of course, the last rib, 7, is prime. The number 7331 is called a superprime of length 4.

#### Write a program that accepts a number N 1 <=N<=8 of ribs and prints all the superprimes of that length.

#### The number 1 (by itself) is not a prime number.

### INPUT FORMAT

#### A single line with the number N.


### SAMPLE INPUT (file sprime.in)
	4


### OUTPUT FORMAT

#### The superprime ribs of length N, printed in ascending order one per line.



### SAMPLE OUTPUT (file sprime.out)
	2333
	2339
	2393
	2399
	2939
	3119
	3137
	3733
	3739
	3793
	3797
	5939
	7193
	7331
	7333
	7393

	
		
### Answer - C
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
FILE* fout;

int N;
int isPrime(int n) {
	for (int i = 3; i * i <= n; i += 2) {
		if (n % i == 0)
			return 0;
	}
	return 1;
}

void genSprime(int n, int num) {
	if (n == 0) {
		if (isPrime(num))
			//printf("%d %d\n", n, num);
			fprintf(fout, "%d\n", num);
		return;
	}
	//printf("%d %d\n", n, num);
	int i = 1;
	if (n == N) {
		genSprime(n - 1, num + 2);
		i = 3;
	}
	if (num == 0 || isPrime(num)) {
		//printf("numnum %d %d\n", num, n);
		num *= 10;
		for (i; i <= 9; i += 2) {
			//printf("num+1 num+1 %d\n", num+i);
			genSprime(n - 1, num + i);
		}
	}

}
void main() {
	FILE* fin = fopen("sprime.in", "r");
	fout = fopen("sprime.out", "w");

	fscanf(fin, "%d", &N);
	
	genSprime(N, 0);

	exit(0);
}
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.6
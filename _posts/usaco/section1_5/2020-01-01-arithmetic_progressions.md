---
title: USACO Training - (Section 1.5) Arithmetic Progressions -C-
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2020-1-1 17:57:28 +0900'
categories:
- USACO Training
---

> ## Arithmetic Progressions

### Arithmetic Progressions
#### An arithmetic progression is a sequence of the form a, a+b, a+2b, ..., a+nb where n=0, 1, 2, 3, ... . For this problem, a is a non-negative integer and b is a positive integer.

#### Write a program that finds all arithmetic progressions of length n in the set S of bisquares. The set of bisquares is defined as the set of all integers of the form p2 + q2 (where p and q are non-negative integers).

### TIME LIMIT: 5 secs

### INPUT FORMAT

<table border="1">
<tbody><tr> <td> Line 1: </td> <td>N (3 &lt;= N &lt;= 25), the length of
	  progressions for which to search </td> </tr>
<tr> <td> Line 2: </td> <td>M (1 &lt;= M &lt;= 250), an upper bound to
	  limit the search to the bisquares with 0 &lt;= p,q &lt;= M.
</td> </tr>
</tbody></table>


### SAMPLE INPUT (file ariprog.in)
	5
	7


### OUTPUT FORMAT

#### If no sequence is found, a single line reading `NONE'. Otherwise, output one or more lines, each with two integers: the first element in a found sequence and the difference between consecutive elements in the same sequence. The lines should be ordered with smallest-difference sequences first and smallest starting number within those sequences first.

#### There will be no more than 10,000 sequences.


### SAMPLE OUTPUT (file ariprog.out)
	1 4
	37 4
	2 8
	29 8
	1 12
	5 12
	13 12
	17 12
	5 20
	2 24
	
		
### Answer - C
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Result {
	int s;
	int d;
} Result;

int compare_num(const void* value1, const void* value2);

int main(void)
{
	Result* result = NULL;
	FILE* fin = fopen("ariprog.in", "r");
	FILE* fout = fopen("ariprog.out", "w");
	int N, M;
	fscanf(fin, "%d", &N);
	fscanf(fin, "%d", &M);

	int* sList = NULL;
	int size = M * M * 2;
	int rSize = 0;
	int r = 0;

	sList = (int*)malloc(sizeof(int) * size + 1);


	memset(sList, 0, sizeof(int) * (size)+1);

	for (int i = 0; i < M + 1; i++) {
		for (int j = 0; j < M + 1; j++) {
			rSize += 1;
			sList[i * i + j * j] = 1;
		}
	}
	rSize = rSize - (N - 1);
	result = (Result*)calloc(rSize, sizeof(Result));

	//int cnt = 0;
	for (int i = 0; i < M * M * 2 - (N - 1) + 1; i++) {
		if (sList[i])
			for (int j = 1; i+j*(N-1) <= M * M * 2 ; j++) {
				int isAP = 1;
				for (int k = 0; k < N; k++) {
					//cnt += 1;
					if (sList[i + j * k] != 1) {
						isAP = 0;
						break;
					}

				}

				if (isAP) {
					result[r].s = i;
					result[r].d = j;
					r += 1;
				}

			}
	}
	//printf("%d", cnt);
	if (r > 0) {
		qsort(result, r, sizeof(Result), compare_num);

		for (int i = 0; i < r; i++) {
			if (result[i].d) {
				fprintf(fout, "%d %d\n", result[i].s, result[i].d);
			}
			else {
				break;
			}
		}
	}
	else {
		fprintf(fout, "%s\n", "NONE");
	}
}


int compare_num(const void* value1, const void* value2) {
	Result* item1 = (Result*)value1;
	Result* item2 = (Result*)value2;

	if (item1->d != item2->d)
		return item1->d > item2->d ? 1 : -1;
	else
		return item1->s > item2->s ? 1 : -1;
}
```

* ###### [mdwgti16/USACO]

[mdwgti16/USACO]: https://github.com/mdwgti16/USACO/tree/master/USACO/Chapter%201/Section%201.5
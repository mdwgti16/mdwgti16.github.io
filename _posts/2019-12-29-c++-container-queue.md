---
title: C++ - Queue
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-29 16:56:28 +0900'
categories: C++
---

> ## Queue

* ### queue
	```c++
	#include <queue>
	using namespace std;
	
	queue<int> q;
	q.push(1); // 맨 뒤에 요소 추가
	q.push(2); // 맨 뒤에 요소 추가
	q.push(3); // 맨 뒤에 요소 추가

	q.pop(); // 맨 앞의 요소 제거 

	q.back(); // 맨 뒤의 요소 접근 : 3
	
	q.front(); // 맨 앞의 요소 접근 : 2

	q.size(); // q의 크기 : 2
	
	q.empty(); // false
	```
	
* ###### [C++ STL queue 기본 사용법과 예제]

[C++ STL queue 기본 사용법과 예제]: https://twpower.github.io/76-how-to-use-queue-in-cpp
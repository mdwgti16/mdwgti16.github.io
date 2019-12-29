---
title: C++ - Stack
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-29 16:26:28 +0900'
categories: C++
---

> ## Stack

* ### stack 사용법

	```c++
	stack<int> s;
	s.push(1); // 맨 뒤에 1 추가
	s.push(2); // 맨 뒤에 2 추가
	s.push(3); // 맨 뒤에 3 추가

	cout<<s.size(); // 3
	
	s.pop(); // 마지막 요소 제거

	s.top() = 10; // 마지막 요소 접근

	s.empty(); // false
	```

	
* ###### [C++ STL stack 기본 사용법과 예제]

[cplusplus std::list::insert]: https://twpower.github.io/75-how-to-use-stack-in-cpp
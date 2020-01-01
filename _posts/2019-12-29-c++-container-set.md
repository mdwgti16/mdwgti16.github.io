---
title: C++ - STL Set
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-29 17:26:28 +0900'
categories: C++
---

> ## STL Set 

* ### set 생성 및 초기화;

	```c++
	#include <set>
	using namespace std;

	set<int> s; // set 선언
	set<int> s1 = { 1,2,2,3,4,4,5 }; // {1,2,3,4,5}로 초기화, 중복은 제거 됨
	set<int> s2(s1); // s1를 복사해서 s2 초기화
	```

* ###  set 
	```c++
	set<int> s = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화

	s.insert(6); // 6 추가 {1,2,3,4,5,6}
	
	s.erase(5); // 5 제거 {1,2,3,4,6}
	
	if (s.find(n) != s.end()) // // 요소가 있는지 확인, 없을시 s.end() 반환
		cout << "find "<< n << endl;
	
	s.count(2); // 원소 개수 확인 : 1
	```

* ###  iterator
	```c++
	set<int> s = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	set<int>::iterator it;
	
	for (it = s.begin(); it != s.end(); it++)
		cout << *it << endl;
	```

* ###  for
	```c++
	set<int> s = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	
	for (int e : s)
		cout << e << endl;
	```
	
* ###### [std::set]
* ###### [C++ STL set 기본 사용법과 예제]
[std::set]: http://www.cplusplus.com/reference/set/set/
[C++ STL set 기본 사용법과 예제]: https://twpower.github.io/92-how-to-use-set-in-cpp
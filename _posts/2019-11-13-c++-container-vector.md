---
title: C++ - Vector
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-11-13 17:26:28 +0900'
categories: C++
---

> ## Vector 

* ### vector 생성 및 초기화;

	```c++
	vector<int> v; //선언
	vector<int> v(n); // n개를 0으로 초기화
	vector<int> v(n,m); // n개를 m으로 초기화
	vector<int> v = {1,2,3,4,5}; // {1,2,3,4,5}로 초기화
	vector<int> v1(v2); // v2를 복사해서 v1 초기화
	```
	
* ###  vector 접근
	```c++
	v.at(n) = 0; 
	v[n] = 0; 
	v.front() // Returns a reference to the first element in the vector.
	v/back() // Returns a reference to the last element in the vector.
	```
	
* ###  vector 정렬
	```c++
	bool desc(int a, int b) { return a > b; }
	
	vector<int> arr = { 1,9,3,6,2,1,7 };

	// 오름차순
	sort(arr.begin(), arr.end());
	// {1,1,2,3,6,7,9}

	// 내림차순
	sort(arr.begin(), arr.end());
	// {9,7,6,3,2,1,1}
	```	

* ###  vector 중복 제거
	```c++
	vector<int> arr = { 1,1,1,2,3,3,4,6,6,9,9,9 };
	arr.erase(unique(arr.begin(), arr.end()), arr.end());
	// {1, 2, 3, 4, 6, 9}
	```	
	
* ###### [STL 에서 vector 에 중복 원소 없애기]
* ###### [C++ :: STL sort() 함수 다루기]

[STL 에서 vector 에 중복 원소 없애기]: https://sgc109.tistory.com/99
[C++ :: STL sort() 함수 다루기]:https://hongku.tistory.com/153
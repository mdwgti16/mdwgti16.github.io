---
title: C++ - Vector
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-31 17:26:28 +0900'
categories: C++
---

> ## Vector 

* ### 생성 및 초기화

	```c++
	#include <vector>
	using namespace std;

	vector<int> v; //선언
	vector<int> v(n); // n개를 0으로 초기화
	vector<int> v(n,m); // n개를 m으로 초기화
	vector<int> v1 = {1,2,3,4,5}; // {1,2,3,4,5}로 초기화
	vector<int> v2(v1); // v1를 복사해서 v2 초기화
	```
	
* ###  접근
	```c++
	v.at(n) = 0; // n번 째 요소 접근
	v[n] = 0; // n번 째 요소 접근
	v.front(); // 맨 앞 요소 접근
	v.back(); // 맨 뒤 요소 접근 
	```
	
* ###  추가 및 제거
	```c++
	vector<int> v = { 1,2,3,4,5 };
	
	v.push_back(6); // 마지막에 요소 추가 {1,2,3,4,5,6}
	v.pop_back(); // 마지막 요소 제거 {1,2,3,4,5}
	
	v.insert(v.begin() + 2, 10); // 2 번째 자리에 10 추가 {1,2,10,3,4,5}
	v.erase(v.begin() + 2); // 2 번째 요소 제거 {1,2,3,4,5}
	
	v.erase(v.begin()+2, v.end()-1); // v.begin()+3 ~ v.end()-1 제거 {1,2,5}
	
	v.clear(); // 전체 요소 제거 size => 0, capacity는 그대로

	```

* ###  iterator
	```c++
	vector<int> v = { 1,2,3,4,5 };
	
	vector<int>::iterator it;
	for (it = v.begin(); it != v.end(); it++)
		cout << *it << endl;
	```

* ###  for
	```c++
	vector<int> v = { 1,2,3,4,5 };
	
	for (int e : v)
		cout << e << endl;
	
	for (int i = 0; i < v.size(); i++)
		cout << v[i] << endl;
	```
	
* ###  정렬
	```c++
	bool desc(int a, int b) { return a > b; }
	
	vector<int> v = { 1,9,3,6,2,1,7 };

	// 오름차순
	sort(v.begin(), v.end());
	// {1,1,2,3,6,7,9}

	// 내림차순
	sort(v.begin(), v.end(),desc);
	// {9,7,6,3,2,1,1}
	```	

* ###  중복 제거
	```c++
	vector<int> v = { 1,1,1,2,3,3,4,6,6,9,9,9 };
	v.erase(unique(v.begin(), v.end()), v.end());
	// {1, 2, 3, 4, 6, 9}
	```	

* ###### [std::vector]
* ###### [STL 에서 vector 에 중복 원소 없애기]
* ###### [C++ :: STL sort() 함수 다루기]

[std::vector]: https://sgc109.tistory.com/99
[STL 에서 vector 에 중복 원소 없애기]: http://www.cplusplus.com/reference/vector/vector/
[C++ :: STL sort() 함수 다루기]:https://hongku.tistory.com/153
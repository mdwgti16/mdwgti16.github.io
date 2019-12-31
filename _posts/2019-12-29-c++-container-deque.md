---
title: C++ - Deque
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-31 17:26:28 +0900'
categories: C++
---

> ## Deque

* ### 생성 및 초기화

	```c++
	#include <deque>
	using namespace std;

	deque<int> dq; //선언
	deque<int> dq(n); // n개를 0으로 초기화
	deque<int> dq(n, m); // n개를 m으로 초기화

	deque<int> dq1 = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	deque<int> dq2(dq1); // v1를 복사해서 v2 초기화

	dq.assign(n, m);// n개를 m으로 초기화
	dq.assign(dq1.begin()+1, dq1.end()-1); // dq1.begin()+1 ~ dq1.end()-1 -> {2,3,4}
	```
	
* ###  접근
	```c++
	deque<int> dq = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	dq.at(n); // n번 째 요소 접근
	dq[n];// n번 째 요소 접근
	dq.front(); // 맨 앞 요소 접근
	dq.back(); // 맨 뒤 요소 접근 
	```
	
* ###  추가 및 제거
	```c++
	deque<int> dq = { 1,2,3,4,5 };

	dq.push_back(6); // 마지막에 요소 추가 {1,2,3,4,5,6}
	dq.push_front(0); // 맨 앞에 요소 추가 {0,1,2,3,4,5,6}
	
	dq.pop_back(); // 마지막 요소 제거 {0,1,2,3,4,5}
	dq.pop_front(); // 처음 요소 제거 {1,2,3,4,5}
	
	dq.insert(dq.begin() + 2, 10); // 2 번째 자리에 10 추가 {1,2,10,3,4,5}
	
	dq.erase(dq.begin() + 2); // 2 번째 요소 제거 {1,2,3,4,5}
	dq.erase(dq.begin() + 2, dq.end() - 1); // dq.begin()+3 ~ dq.end()-1 제거 {1,2,5}

	dq.clear(); // 전체 요소 제거 size => 0, capacity는 그대로
	```

* ###  iterator
	```c++
	deque<int> dq = { 1,2,3,4,5 };

	deque<int>::iterator it;
	for (it = dq.begin(); it != dq.end(); it++)
		cout << *it << endl;
	```

* ###  for
	```c++
	deque<int> dq = { 1,2,3,4,5 };

	for (int e : dq)
		cout << e << endl;

	for (int i = 0; i < dq.size(); i++)
		cout << dq[i] << endl;
	```
	
* ###  정렬
	```c++
	bool desc(int a, int b) { return a > b; }
	
	deque<int> dq = { 1,9,3,6,2,1,7 };

	// 오름차순
	sort(dq.begin(), dq.end());
	// {1,1,2,3,6,7,9}

	// 내림차순
	sort(dq.begin(), dq.end(), desc);
	// {9,7,6,3,2,1,1}
	```	

* ###  중복 제거
	```c++
	deque<int> dq = { 1,1,1,2,3,3,4,6,6,9,9,9 };

	dq.erase(unique(dq.begin(), dq.end()), dq.end());
	// {1, 2, 3, 4, 6, 9}
	```	
	
* ###### [std::deque]	
* ###### [About STL : C++ STL 프로그래밍(5)-덱(deque) : (1)]

[std::deque]:http://www.cplusplus.com/reference/deque/deque/
[About STL : C++ STL 프로그래밍(5)-덱(deque) : (1)]: http://www.hanbit.co.kr/channel/category/category_view.html?cms_code=CMS3942847236
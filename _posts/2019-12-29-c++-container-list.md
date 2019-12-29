---
title: C++ - List
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-29 13:26:28 +0900'
categories: C++
---

> ## List

* ### list 생성 및 초기화;

	```c++
	#include <list>
	using namespace std;

	list<int> mList; //선언
	list<int> mList(n); // n개를 0으로 초기화
	list<int> mList(n, m); // n개를 m으로 초기화
	list<int> mList1 = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	list<int> mList2 (mList1); // v2를 복사해서 v1 초기화
	```

* ###  list 접근
	```c++
	list<int> mList = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화

	mList.front() = 0; // 맨 앞 요소 접근 {0,2,3,4,5}
	mList.back() = 10; // 맨 뒤 요소 접근 {0,2,3,4,10}
	```
	
* ###  list 추가 및 제거
	```c++
	list<int> mList = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	
	mList.push_back(6); // 마지막에 6 추가 {1,2,3,4,5,6}
	mList.pop_back(); // 마지막 요소 삭제 {1,2,3,4,5}

	mList.push_front(0); // 0 번째 자리에 6 추가 {0,1,2,3,4,5}
	mList.pop_front(); // 0 번째 요소 삭제 {1,2,3,4,5}
	
	list<int>::iterator it = mList.begin();
	it++; // it points now to number 2

	mList.insert(it, 10); // it 앞에 10 추가 {1,10,2,3,4,5}

	//it points now to number 2
	mList.insert(it, 2, 20); // it 앞에 20 2번 추가 {1,10,20,20,2,3,4,5}

	mList.clear(); // 전체 요소 제거

	```

* ###  list - iterator
	```c++
	list<int> mList = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	list<int>::iterator it;
	
	for (it = mList.begin(); it != mList.end(); it++)
		cout << *it << endl;
	```

* ###  list - for
	```c++
	list<int> mList = { 1,2,3,4,5 }; // {1,2,3,4,5}로 초기화
	
	for (int e : mList)
		cout << e << endl;
	```
	
* ### list 정렬
	```c++
	bool desc(int a, int b) { return a > b; }
	
	list<int> mList = { 1,9,3,6,2,1,7 };

	// 오름차순
	mList.sort();
	// {1,1,2,3,6,7,9}

	// 내림차순
	mList.sort(desc);
	// {9,7,6,3,2,1,1}
	
	// 역순
	mList.reverse();
	// {1,1,2,3,6,7,9}
	```	

* ###  list 중복 제거
	```c++
	list<int> mList = { 1,1,1,2,3,3,4,6,6,9,9,9 };
	mList.erase(unique(mList.begin(), mList.end()), mList.end());
	for (int e : mList)
		cout << e << endl;
	// {1, 2, 3, 4, 6, 9}
	```	
	
* ###### [cplusplus std::list::insert]
* ###### [cplusplus std::list]

[cplusplus std::list::insert]: http://www.cplusplus.com/reference/list/list/insert/
[cplusplus std::list]:http://www.cplusplus.com/reference/list/list/
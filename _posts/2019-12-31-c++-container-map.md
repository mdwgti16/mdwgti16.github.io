---
title: C++ - Map
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-12-29 17:26:28 +0900'
categories: C++
---

> ## Map

* ### Map 생성

	```c++
	#include <map>
	using namespace std;

	map<key, value> m;// map 선언
	
	```

* ###  map
	```c++
	map<string, int> m;// map 선언

	m.insert(pair<string, int>("apple", 0)); // "apple" : 0 추가
	m.insert(make_pair("berry", 0)); // "berry" : 0 추가
	m["lemon"] = 3; // "lemon" : 3 추가

	// at은 없는 key에 접근하면 error 발생, []는 key가 없을경우 새로 추가
	m.at("apple") = 1;
	m["berry"] = 2;
	
	if (m.find("berry") != m.end()) // key가 있을경우const  iterator 반환, 없으면 m.end() 반환
        cout << "key = " << m.find("berry")->first
        << " value = " << m.find("berry")->second << endl;
	
	m.erase("berry"); // "berry" 제거
	
	m.size(); // m의 크기 : 2
	```

* ### key_compare
	```c++
	map<string, int> m;// map 선언
	m["apple"] = 1;
	m["berry"] = 2;
	m["lemon"] = 3;

	string last = m.rbegin()->first;

	map<string, int>::key_compare comp = m.key_comp();
	map<string, int>::iterator it = m.begin();
	
	do{
		cout << "key = " << it->first << " value = " << it->second << endl;
	} while (comp(it++->first,last));
	
	```
	
* ### iterator
	```c++
	map<string, int> m;// map 선언
	m["apple"] = 1;
	m["berry"] = 2;
	m["lemon"] = 3;
	
	map<string, int>::iterator it;

	for(it=m.begin(); it!=m.end(); it++)
		cout << "key = " << it->first << " value = " << it->second << endl;
	```

* ### for
	```c++
	map<string, int> m;// map 선언
	m["apple"] = 1;
	m["berry"] = 2;
	m["lemon"] = 3;

	for (pair<string, int> p : m) // auto == pair<string, int> 
		cout << "key = " << p.first << " value = " << p.second << endl;
	```
	
* ###### [C++ STL set 기본 사용법과 예제]

[C++ STL set 기본 사용법과 예제]: https://twpower.github.io/92-how-to-use-set-in-cpp
---
title: Android DataBinding - 시작
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-14 17:26:28 +0900'
categories: Android
---

> ## DataBinding 

* ###  gradle 수정

	app수준의 gradle에 다음과 같이 추가합니다.

	```gradle
	android{
		...
		dataBinding{
			enabled =true
		}
	}
	```

* Activity
	* Java
	```java
	@Override
	protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			ActivityMainBinding binding = DataBindingUtil.setContentView(this,R.layout.activity_main);
	}
	```
	* Kotlin
	```kotlin
	override fun onCreate(savedInstanceState: Bundle?) {
			val binding = DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main)
	}
	```

* Fragment
	* Java
	```java
	@Override
	public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
			FragmentBlankBinding binding = DataBindingUtil.inflate(inflater,R.layout.fragment_blank,container,false);
			return binding.getRoot();
	}
	```
	* Kotlin
	```kotlin
	override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
			val binding = DataBindingUtil.inflate<ViewDataBinding>(inflater, R.layout.fragment_search, container, false)
	}
	```
		
		
		
		
* #### 참조
	* ##### [Android DataBinding : 기초에서 고급까지] 

[Android DataBinding : 기초에서 고급까지]: https://www.slideshare.net/NaverEngineering/11android-databinding-121395998
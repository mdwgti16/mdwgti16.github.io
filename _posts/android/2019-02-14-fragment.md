---
title: Android Java- Fragment
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-14 17:26:28 +0900'
categories: Android
---

> ## Fragment 사용하기

* Fragment 기능
	* Fragment 교체
	```java
		FragmentManager fragmentManager = getSupportFragmentManager();
		FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
		
		fragmentTransaction.replace(int container, Fragment fragment, String tag);
		fragmentTransaction.commit();
	```
	* Fragment  삭제
	```java
		FragmentManager fragmentManager = getSupportFragmentManager();
		FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

		fragmentTransaction.remove(Fragment fragmet);
		fragmentTransaction.commit();
	```

	* Fragment 찾기
	```java
		FragmentManager fragmentManager = getSupportFragmentManager();
		Fragment fragment = fragmentManager.findFragmentById(R.id.container);

		FragmentManager fragmentManager = getSupportFragmentManager();
		Fragment fragment = fragmentManager.findFragmentByTag(TAG);
	```		
	
	* Fragment 숨기기
	```java
		FragmentManager fragmentManager = getSupportFragmentManager();
		FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
		
		fragmentTransaction.hide(Fragment fragmet);
		fragmentTransaction.commit();
	```
	* Fragment 보이기
	```java
		FragmentManager fragmentManager = getSupportFragmentManager();
		FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
		
		fragmentTransaction.show(Fragment fragmet);
		fragmentTransaction.commit();
	```


Activity에 Fragment를 추가하기 위한 방법으로는 xml을 사용하는 방법과
Java 코드에서 SupportFragmentManager을 사용하는 방법이 있습니다.

* ###  xml 사용
	* activity_main.xml
	
		```xml
		<?xml version="1.0" encoding="utf-8"?>
		<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
			xmlns:app="http://schemas.android.com/apk/res-auto"
			xmlns:tools="http://schemas.android.com/tools"
			android:id="@+id/container"
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			tools:context=".MainActivity">
			<fragment
				android:layout_width="match_parent"
				android:layout_height="match_parent"
				android:name="com.ls.project.practicejava.BlankFragment"/>
		</FrameLayout>
		```
		
	* fragment_blank.xml
		
		```xml
		<?xml version="1.0" encoding="utf-8"?>
		<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
			xmlns:tools="http://schemas.android.com/tools"
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			tools:context=".BlankFragment">

			<TextView
				android:id="@+id/tv"
				android:layout_width="match_parent"
				android:layout_height="match_parent"
				android:text="@string/hello_blank_fragment"/>
		</FrameLayout>
		```
		
	* BlankFragment.java
		
		```java
		public class BlankFragment extends Fragment {
			@Override
			public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
				return inflater.inflate(R.layout.fragment_blank, container, false);
			}
		}
		```
		
	* MainActivity.java
	
		```java
		public class MainActivity extends AppCompatActivity {
			@Override
			protected void onCreate(Bundle savedInstanceState) {
				super.onCreate(savedInstanceState);
				setContentView(R.layout.activity_main);
			}
		}
		```
		
* ###  SupportFragmentManager 사용

	* activity_main.xml
	
		```xml
		<?xml version="1.0" encoding="utf-8"?>
		<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
			xmlns:app="http://schemas.android.com/apk/res-auto"
			xmlns:tools="http://schemas.android.com/tools"
			android:id="@+id/container"
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			tools:context=".MainActivity">
		</FrameLayout>
		```
		
	* fragment_blank.xml, BlankFragment.java 변경 x
	
	*	 MainActivity.java
	```java
		public class MainActivity extends AppCompatActivity {
			@Override
			protected void onCreate(Bundle savedInstanceState) {
				super.onCreate(savedInstanceState);
				setContentView(R.layout.activity_main);
				FragmentManager fragmentManager = getSupportFragmentManager();
				FragmentTransaction fragmentTransaction =fragmentManager.beginTransaction();
				fragmentTransaction.add(R.id.container,new BlankFragment());
				fragmentTransaction.commit();
			}
		}
	```
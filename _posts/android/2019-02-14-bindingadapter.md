---
title: Android DataBinding - BindingAdapter
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-14 17:26:28 +0900'
categories: Android
---

> ## BindingAdapter


* ## BindingAdapters  Class 생성
#### 예를 들어	RecyclerView의 Adapter를 BindingAdapter를 통해 xml파일에서 적용 가능
	* Java
	
	```java
	public class BindingAdapters {
		@BindingAdapter("bind:setAdapter")
		public static void setAdapter(RecyclerView recyclerView, ListRecyclerViewAdapter adapter) {
			recyclerView.setLayoutManager(new LinearLayoutManager(recyclerView.getContext()));
			recyclerView.setHasFixedSize(true);
			recyclerView.setAdapter(adapter);
		}
	}
	```
	
	* Kotlin
	 
	```kotlin
	class BindingAdapter {
		companion object {
			@JvmStatic
			@BindingAdapter("bind:setAdapter")
			fun setAdapter(
				recyclerView: RecyclerView,
				adapter: ListRecyclerViewAdapter
			) {
				recyclerView.layoutManager = LinearLayoutManager(recyclerView.context)
				recyclerView.setHasFixedSize(true)
				recyclerView.adapter = adapter
			}
		}
	}
	```
	
	* xml
	
	```xml
	<android.support.v7.widget.RecyclerView
		android:id="@+id/recyclerView"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		bind:setAdapter="@{viewModel.adapter}"
	/>
	```
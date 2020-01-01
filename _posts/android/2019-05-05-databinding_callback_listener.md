---
title: Android DataBinding - Callback Listener
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-05-05 17:26:28 +0900'
categories: Android
---

> ## ChangedCallback Listener

* ###  ObservableField

	```kotlin
	observableField.addOnPropertyChangedCallback(object : Observable.OnPropertyChangedCallback() {
		override fun onPropertyChanged(sender: Observable?, propertyId: Int) {
				...
		}
	})
	```
	
* ###  ObservableArrayList
	```kotlin
	observableArrayList.addOnListChangedCallback(object :
		ObservableList.OnListChangedCallback<ObservableList<String>>() {
		override fun onChanged(sender: ObservableList<String>?) {
			...
		}

		override fun onItemRangeRemoved(sender: ObservableList<String>?, positionStart: Int, itemCount: Int) {
			...
		}

		override fun onItemRangeMoved(
				sender: ObservableList<String>?,
				fromPosition: Int,
				toPosition: Int,
				itemCount: Int
		) {
			...
		}

		override fun onItemRangeInserted(sender: ObservableList<String>?, positionStart: Int, itemCount: Int) {
			...
		}

		override fun onItemRangeChanged(sender: ObservableList<String>?, positionStart: Int, itemCount: Int) {
			...
		}
	})
	```
	
* ###  ObservableArrayMap
	```kotlin
	observableArrayMap.addOnMapChangedCallback(object :
			ObservableMap.OnMapChangedCallback<ObservableMap<String, Boolean>, String, Boolean>() {
			override fun onMapChanged(sender: ObservableMap<String, Boolean>?, key: String?) {
				...
			}
		})
	}
	```
		
* ##### [Callback Listener Github Example]

[Callback Listener Github Example]: https://github.com/mdwgti16/Android-Kotlin-Example/tree/master/Databinding/DatabindingCallback
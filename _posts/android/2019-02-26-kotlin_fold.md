---
title: Android Kotlin - Fold
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-26 17:26:28 +0900'
categories: Android
---

> ## Fold

* #### 코틀린의 [Fold] 는 Accumulates value에 초기값을 지정하고 왼쪽부터 오른쪽으로 
#### 현재  Accumulates value과 연산을 합니다. 
```kotlin
	var list = listOf(1,2,3,4,5)

	fun sumTotal(list: List<Int>) = list.fold(0) { total, next -> total + next }
	fun subTotal(list: List<Int>) = list.fold(0) { total, next -> total - next }
	fun mulTotal(list: List<Int>) = list.fold(1) { total, next -> total * next }

	assertEquals(15, sumTotal(list)) // true
	assertEquals(-15, subTotal(list)) // true
	assertEquals(120, mulTotal(list)) // true
```

	

[Fold]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/fold.html
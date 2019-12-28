---
title: Android Kotlin - Collection Api 1
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-03-07 12:26:28 +0900'
categories: Android
---

> ## Collection Api

* ## all
```kotlin
@Test
fun all() {
	val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
	assertEquals(true, fruitList.all { it.length >= 5 })
	assertEquals(false, fruitList.all { it.contains("apple") })
}
```
#### (AND) 	주어진 조건을 모든 원소들이 만족하면 true 아니면 false 
* ## any
```kotlin
@Test
fun any() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals(true, fruitList.any { it.length >= 10 })
    assertEquals(true, fruitList.any { it.contains("apple") })
}
```
#### (OR) 	주어진 조건을 원소들이 하나라도 만족하면 true 아니면 false 
* ## none
```kotlin
@Test
fun none() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals(true, fruitList.none { it.length < 5 })
    assertEquals(true, fruitList.none { it.contains("watermelon") })
}
```
#### 	주어진 조건을 만족하는 원소가 없으면 true
* ## find
```kotlin
@Test
fun find() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals("pineapple", fruitList.find { it.contains("apple") })
    assertEquals("apple", fruitList.findLast { it.contains("apple") })
}
```
#### 	주어진 조건을 만족하는 첫번째 원소 return
* ## count
```kotlin
@Test
fun count() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals(2, fruitList.count { it.contains("apple") })
}
```
#### 	주어진 조건을 만족하는 원소의 개수 return
* ## map
```kotlin
@Test
fun map() {
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    assertEquals(110, numberList.map { it * 2 }.sum())
}
```
#### 	각 원소를 주어진 조건으로 변경
* ## flatMap
```kotlin
@Test
fun flatMap() {
    val numberList = listOf(2, 5, 11)
    assertEquals(listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
		numberList.flatMap { (it / 2 until it) })
}
```
#### 	각 원소를 주어진 조건의 List로 변경
* ## filter
```kotlin
@Test
fun filter() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals(listOf("pineapple", "apple"),
		fruitList.filter { it.contains("apple") })
}
```
#### 	주어진 조건을 만족하는 원소만 통과
* ## asSequence
```kotlin
val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    numberList.filter{ println("first no asSequence $it");it%2==0}
		.map { println("second no asSequence $it"); it*2  }.toList()
    numberList.asSequence().filter{ println("first no result $it");it%2==0}
		.map { println("second no result $it"); it*2 } // 출력 안됨
    numberList.asSequence().filter{ println("first asSequence $it");it%2==0}
		.map { println("second asSequence $it"); it*2  }.toList()

	//sequence 생성
    val numbers = generateSequence(0) { it + 1 }
    val numbersTo10 = numbers.takeWhile { print(it);it <= 10 }
    println("before sum")
    println(numbersTo10.sum())}
```
#### filter와 map는 기본적으로 수행 될때마다 list에 추가되고 그 list가 반환됩니다.
```
first no asSequence 1
first no asSequence 2
first no asSequence 3
first no asSequence 4
first no asSequence 5
second no asSequence 2
second no asSequence 4
```
####  filter를 끝나고 그 list를 map이 받아 다시 수행하고 list가 반환 됩니다.
#### 하지만 sequence일 경우
```
first asSequence 1
first asSequence 2
second asSequence 2
first asSequence 3
first asSequence 4
second asSequence 4
first asSequence 5
```
#### 이렇게 됩니다.
#### sequence를 사용할때는 마지막에 toList() 나 sum()같은 최종 연산을 해줘야됩니다.
```
before sum
0123456789101155
```
#### 아래의 생성된 sequence를 실행해보면 sum을 하기전에는 출력이 안되는 것을 볼 수 있습니다.



		
* #### 참조
	* ##### [(Kotlin) 코틀린 스트림 함수] 
	* ##### [(Kotlin) 코틀린 람다 #2 Collection API]


[(Kotlin) 코틀린 스트림 함수]: https://namget.tistory.com/entry/Kotlin-%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%95%A8%EC%88%98-map-flatMap-groupBy-filter-take-drop-first-distinct-zip-joinToString-count-any-none-max-min-average
[(Kotlin) 코틀린 람다 #2 Collection API]: https://tourspace.tistory.com/111
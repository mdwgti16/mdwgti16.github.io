---
title: Android Kotlin - Collection Api 2
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


* ## zip
```kotlin
@Test
fun zip() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    val priceList = listOf(1500,1000,2000,700,500,10000)
    assertEquals(listOf("strawberry is 1500won", "grape is 1000won", "pineapple is 2000won", "banana is 700won", "apple is 500won"),
        fruitList.zip(priceList).map {"${it.first} is ${it.second}won"})
}
```
#### 	두개의 collection을 합쳐서 처리. 해당 값은 Pair< First,Second > 로 들어가게 됩니다.
* ## groupBy
```kotlin
@Test
fun groupBy() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals(mapOf(10 to listOf("strawberry"), 5 to listOf("grape", "apple"), 9 to listOf("pineapple"),
        6 to listOf("banana")),fruitList.groupBy { it.length })
}
```
#### 	주어진 조건으로 원소를 묶음. Map< Int,List< String>>형식
* ## distinct
```kotlin
@Test
fun distinct() {
    val numberList = listOf(1, 2, 3, 4, 5,1, 2, 3, 4, 5)
    assertEquals(listOf(1, 2, 3, 4, 5,1, 2, 3, 4, 5),numberList)
    assertEquals(listOf(1, 2, 3, 4, 5),numberList.distinct())
    assertEquals(listOf(1,2,4),numberList.distinctBy { it/2 })
}
```
#### 	원소들의 중복을 제거. distinctBy는 조건을 만족하는 중복을 제거
* ## take
```kotlin
@Test
fun take() {
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    assertEquals(listOf(1, 2, 3, 4, 5),numberList.take(5))
    assertEquals(listOf(6, 7, 8, 9, 10),numberList.takeLast(5))
    assertEquals(listOf(1,2,3),numberList.takeWhile { it/4==0 })
    assertEquals(listOf(8,9,10),numberList.takeLastWhile { it/4>1 })
}
```
#### 주어진 조건 만큼 원소를 가져옴
* ## drop
```kotlin
@Test
fun drop() {
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    assertEquals(listOf(6, 7, 8, 9, 10),numberList.drop(5))
    assertEquals(listOf(1, 2, 3, 4, 5),numberList.dropLast(5))
    assertEquals(listOf(6, 7, 8, 9, 10),numberList.dropWhile { it/6==0 })
    assertEquals(listOf(1, 2, 3, 4, 5),numberList.dropLastWhile { it/3>1 })
}
```
#### 	주어진 조건만큼 원소를 제거
* ## first, last
```kotlin
@Test
fun firstAndLast() {
    val fruitList = listOf("strawberry", "grape", "pineapple", "banana", "apple")
    assertEquals("strawberry",fruitList.first())
    assertEquals("apple",fruitList.last())
}
```
#### 	처음과 마지막의 원소를 가져옴
* ## max
```kotlin
@Test
fun max() {
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    val fruitList = listOf("strawberry", "grape", "pineapple", "plum","banana", "apple")
    assertEquals(10,numberList.max())
    assertEquals("strawberry",fruitList.max())
    assertEquals("strawberry",fruitList.maxBy { it.length })
}
```
####  기본적으로	숫자일때는 최댓값, 글자일때는 a-z순으로 가장 마지막 값
* ## min
```kotlin
@Test
fun min() {
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    val fruitList = listOf("strawberry", "grape", "pineapple", "plum","banana", "apple")
    assertEquals(1,numberList.min())
    assertEquals("apple",fruitList.min())
    assertEquals("plum",fruitList.minBy { it.length })
}
```
#### 	기본적으로 숫자일때는 최솟값, 글자일때는 a-z순으로 가장 처음 값
* ## average
```kotlin
@Test
fun average() {
    val numberList = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    assertEquals(5.5,numberList.average())
}
```
#### 	원소들의 평균값 반환
* ## joinToString
```kotlin
@Test
fun joinToString() {
    val fruitList = listOf("strawberry", "grape", "pineapple","banana", "apple")
    assertEquals("strawberry, grape, pineapple, banana, apple",fruitList.joinToString())
    assertEquals("strawberry grape pineapple banana apple",fruitList.joinToString(" "))
    assertEquals("strawb!!rry, grap!!, pin!!appl!!, banana, appl!!",fruitList.joinToString {it.replace("e","!!")})
}
```
#### 	원소들을 주어진 조건으로 붙임. 기본적으로 ", "로 구분됨


		
* #### 참조
	* ##### [(Kotlin) 코틀린 스트림 함수] 
	* ##### [(Kotlin) 코틀린 람다 #2 Collection API]


[(Kotlin) 코틀린 스트림 함수]: https://namget.tistory.com/entry/Kotlin-%EC%BD%94%ED%8B%80%EB%A6%B0-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%95%A8%EC%88%98-map-flatMap-groupBy-filter-take-drop-first-distinct-zip-joinToString-count-any-none-max-min-average
[(Kotlin) 코틀린 람다 #2 Collection API]: https://tourspace.tistory.com/111
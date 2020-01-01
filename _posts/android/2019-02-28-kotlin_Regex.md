---
title: Android Kotlin - Regex
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-26 17:26:28 +0900'
categories: Android
---

> ## Regex 클래스

#### 정규식 표현(Regular Expression)의 사전적 의미는 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어입니다. 쉽게 말해 특정 조건의 문자열을 찾는것을 말합니다. 

* ###  정규식의 Null 처리
	```kotlin
	val findResult = Regex("a").find("Hello")
	val result1=findResult?.value
	val result2=findResult?.value.orEmpty()
	result1?.toUpperCase()
	result2.toUpperCase()
	```
#### 	결과 값에 orEmpty()를 붙여주면 Null을 안정적으로 처리 할 수 있습니다.
	
* ### 	정규 표현식

	![a](/assets/img/regex-table1.jpg)

	![b](/assets/img/regex-table2.jpg)

* ###  Functions

	* find
	```kotlin
	val result =Regex("""\d{3}-\d{4}-\d{4}""").find("010-1234-5678") // Kotlin String Literals
	val result =Regex("\\d{3}-\\d{4}-\\d{4}").find("010-1234-5678") // Java
	print(result?.value) // 010 1234 5678
	```
	
	* findAll
	```kotlin
	val result =Regex("""\d+""").findAll("010-1234-5678")
	val phoneNumber = StringBuilder()
	for (num in result){
		phoneNumber.append(num.value + " ")
	}
	print(phoneNumber) // 010 1234 5678
	```
	
	* matches
	```kotlin
	println(Regex("""[a-z]+""").matches("Hello Kotlin")) // false
	println(Regex("""[a-zA-Z\s]+""").matches("Hello Kotlin")) // true
	```
	
	* matchEntire
	```kotlin
	println(Regex("""[a-z]+""").matchEntire("Hello Kotlin")?.value) // null
	println(Regex("""[a-zA-Z\s]+""").matchEntire("Hello Kotlin")?.value) // Hello Kotlin
	```
	
	* containsMatchIn
	```kotlin
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	println(Regex("""[a-z]+""").containsMatchIn("Hello Kotlin")) // true
	println(Regex("""[\d]+""").containsMatchIn("Hello Kotlin")) // false
	```
	
	* split
	```kotlin
	println(Regex("""[\s]+""").split("Hello Kotlin")) // [Hello, Kotlin]
	println(Regex("""[\d]+""").split("Hello Kotlin")) // [Hello Kotlin]
	```
	
	* replace
	```kotlin
	println(Regex("""[\s]+""").replace("Hello Kotlin","! ")) // Hello! Kotlin
	```
	
* #### 참조
	* ##### [Kotlin의 정규 표현식 소개] 
	* ##### 	 [(일반) 정규식 (Regular Expression)에 대한 간단 정리]
[(일반) 정규식 (Regular Expression)에 대한 간단 정리]:http://ccambo.blogspot.com/2014/10/regular-expression.html
[Kotlin의 정규 표현식 소개]: https://riptutorial.com/ko/kotlin/example/32686/kotlin%EC%9D%98-%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D-%EC%86%8C%EA%B0%9C

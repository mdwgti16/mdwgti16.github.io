---
title: Android Kotlin - inline, noline, crossline, reified
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-03-29 5:26:28 +0900'
categories: Android
---

> ## inline , noinline , crossinline , reified

* ### Non-Inline
	```kotlin
	fun nonInlined(block: () -> Unit) {
		println("do something")
		block()
	}

	fun callNonInlined(){
        nonInlined { println("do another thing") }
	}
	```
#### 	high order function(고차함수)인 메소드를 java로 decompile하면 아래와 같습니다.

	```java
	public final void nonInlined(@NotNull Function0 block) {
		System.out.println("do something");
		block.invoke();
	}
	```
#### 	이는 kotlin에서 고차함수를 사용 할 때마다 java로 decompile시에 아래의 코드로 번역됩니다는 의미입니다.

	```java
	public void callNonInlined(){ 
		nonInlined(new Function() {
			@Override
			public void invoke() {
				System.out.println("do another thing");
			}
		});
	}
	```	
#### 	즉,  고차함수에 lambda를 전달 할 때마다 `Function` object가 생성됩니다. 만약 loop에서 사용하게 됩니다면 N개의   `Function` object가 만들어 집니다.

* ### inline
```kotlin
inline fun inlined(block: () -> Unit) {
	println("do something")
	block()
}
fun callInlined(){
	inlined { println("do another thing") }
}
```
#### 이를 방지위해 사용되는 키워드가 inline입니다. inline 메소드를 호출을 java로 decomplie하게 되면
```java
public final void callInlined() {
	System.out.println("do something");
	System.out.println("do another thing");
}
```
#### 위 처럼 `Function` object 생성 없이 바로 inlined 메소드의 내용을 호출한 곳에서 바로 사용합니다.

* ### noinline 
#### noinline 키워드는 inline 메소드에서 여러개의 람다가 있을때 inline 되지 않으려는 람다 앞에 붙여주면 inline을 막을 수 있습니다.
```kotlin
inline fun noinlined(noinline head: () -> Unit, body: () -> Unit) {
	head()
	body()
}
```

* ### crossinline
#### crossinline 키워드는 한 메소드 안에서 람다로 받은 매개변수를 다른 고차함수의 매개변수로 넘겨줄 때 non-local return이 람다 안에서 불가능 하기 때문에 crossinline으로 표시 합니다.
```kotlin
inline fun crossinlined(crossinline body: () -> Unit) {
	doSometing{
		body()
	}
}
fun doSometing(something : () -> Unit) {
	something()
}
fun callCrossinlined(){
	crossinlined{ println("do someting") }
}
```		

* ### reified
#### reified를 사용하지 않을 경우 ClassName::class.java 형식으로 클래스 타입을 넣어줘야 됩니다.
```kotlin
open class Animal
class Dog:Animal()
fun <T> nonReifiedFunc(clazz : Class<T>){
	val animal = Animal()
	if(clazz.isInstance(animal))
		print("is animal")
	else
		print("is not animal ")
}
fun callNonReifiedFunc(){
	nonReifiedFunc(Animal::class.java)
}
```
#### MethodName`<ClassName>`의 형식을 원합니다면 reified를 사용하면 됩니다.
```kotlin
inline fun <reified T> reifiedFunc(){
	val animal = Animal()
	if(animal is T)
		print("is animal")
	else
		print("is not animal ")
}
 fun callReifiedFunc(){
        reifiedFunc<Animal>()
    }
```




* #### 참조
	* ##### [inline, noinline, crossinline — What do they mean?] 
	* ##### [(Kotlin) 코틀린 람다 #2 Collection API]


[inline, noinline, crossinline — What do they mean?]: https://android.jlelse.eu/inline-noinline-crossinline-what-do-they-mean-b13f48e113c2
[(Kotlin) 코틀린 람다 #2 Collection API]: https://tourspace.tistory.com/111
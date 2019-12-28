---
title: Android Kotlin - 시간 차이 계산
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-03-31 4:00:28 +0900'
categories: Android
---

> ## 시간 차이 구하기

* ### 시간 차이를 구해서 간략화 하기
	```kotlin
	enum class TimeValue(val value: Int,val maximum : Int, val msg : String) {
			SEC(60,60,"분 전"),
			MIN(60,24,"시간 전"),
			HOUR(24,30,"일 전"),
			DAY(30,12,"달 전"),
			MONTH(12,Int.MAX_VALUE,"년 전")
	}

	fun timeDiff(time : Long):String{
		val curTime = System.currentTimeMillis()
		var diffTime = (curTime- timeStamp) / 1000
		var msg: String? = null
		if(diffTime < TimeValue.SEC.value )
			msg= "방금 전"
		else {
			for (i in TimeValue.values()) {
				diffTime /= i.value
				if (diffTime < i.maximum) {
					msg=i.msg
					break
				}
			}
		}
	}
	```

* ### System.currentTimeMillis()로 값을 받아오면 현재 시간을 밀리 단위로 값을 받아올 수 있습니다. 
	* `curTime/1000`	= 초
	*  `curTime/1000/60` = 분
	* `curTime/1000/60/60` = 시
	* `curTime/1000/60/60/24` = 일
	* `curTime/1000/60/60/24/30` = 월
	* `curTime/1000/60/60/24/30/12` = 년

* ###  오늘 날짜
```kotlin
var curTime = System.currentTimeMillis()
val dateFormat = SimpleDateFormat("yyyy-mm-dd hh:mm:ss")
val curDate = dateFormat.format(curTime)
```
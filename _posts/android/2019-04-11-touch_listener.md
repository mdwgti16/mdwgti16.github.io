---
title: Android Kotlin - OnTouchListener LongTouch
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-04-11 13:26:28 +0900'
categories: Android
---

> ## OnTouchListener에서  LongTouch 구현

* ```kotlin
	var isLongClick = false
	val LONG_TOUCH_SECOND : Long = 1500
	lateinit var thread: Thread

	private fun getLongClickThread(): Thread {
		return Thread(Runnable {
			try {
				Thread.sleep(LONG_TOUCH_SECOND)
				if (!Thread.currentThread().isInterrupted) {
					isLongClick = true
					TLog.d("thread $isLongClick")
				}
			} catch (e: InterruptedException) {
			} finally {
			}
		})
	}

	val touchListener = View.OnTouchListener { v, event ->
		when (event.action) {
			MotionEvent.ACTION_DOWN -> {
				thread = getLongClickThread()
				thread.start()
			}
			MotionEvent.ACTION_MOVE ->
				if (isLongClick) {
				}
			MotionEvent.ACTION_UP -> {
				thread.interrupt()
				isLongClick = false
			}
	}
		return@OnTouchListener true
	}
	```
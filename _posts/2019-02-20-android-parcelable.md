---
title: Android Kotlin - Parcelable을 사용한 Intent
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-20 10:50:28 +0900'
categories: Android
---

> ## Parcelable을 사용한 Intent

* ###  Library 없이 Parcelable 사용
	* activity_intent.xml
	
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout
		xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:tools="http://schemas.android.com/tools"
		xmlns:app="http://schemas.android.com/apk/res-auto"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:orientation="vertical"
		tools:context=".IntentActivity">
		<TextView android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:id="@+id/name"
			android:textSize="30sp"/>
		<TextView android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:id="@+id/age"
			android:textSize="30sp"/>
		<ListView android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:id="@+id/listView"/>
		<ListView android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:id="@+id/mapListView"/>
	</LinearLayout>
	```
	* activity_main.xml
	
	```xml
	<?xml version="1.0" encoding="utf-8"?>
	<android.support.constraint.ConstraintLayout
		xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:tools="http://schemas.android.com/tools"
		xmlns:app="http://schemas.android.com/apk/res-auto"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		tools:context=".MainActivity">
		<TextView
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="Hello World!"
			app:layout_constraintBottom_toBottomOf="parent"
			app:layout_constraintLeft_toLeftOf="parent"
			app:layout_constraintRight_toRightOf="parent"
			app:layout_constraintTop_toTopOf="parent"/>
	</android.support.constraint.ConstraintLayout>
	```
	* Person
	
	```kotlin
	class Person : Parcelable {
		var name: String? = null
		var age: Int = 0
		var favoriteFoodList: ArrayList<String> = ArrayList()
		var map: HashMap<String, Boolean> = HashMap()

		constructor(name: String, age: Int, favoriteFoodList: ArrayList<String>, map: HashMap<String, Boolean>) {
			this.name = name
			this.age = age
			this.favoriteFoodList = favoriteFoodList
			this.map = map
		}

		constructor(src: Parcel) {
			this.name = src.readString()
			this.age = src.readInt()
			src.readStringList(favoriteFoodList) // List를 read parcel
			this.map = buildTheMap(src) // Map을 read parcel 
		}

		fun buildTheMap(parcel: Parcel): HashMap<String, Boolean> {
			val size = parcel.readInt()
			val map = HashMap<String, Boolean>()

			for (i in 1..size) {
				val key = parcel.readString()
				val value = parcel.readInt() == 1
				map[key] = value
			}
			return map
		}

		override fun writeToParcel(dest: Parcel, flags: Int) {
			dest.writeString(name)
			dest.writeInt(age)
			dest.writeStringList(favoriteFoodList)  // List를 write parcel

			dest.writeInt(map.size) // Map을 write parcel 
			for ((key, value) in map) {
				dest.writeString(key)
				dest.writeInt(if (value) 1 else 0)
			}
		}

		override fun describeContents(): Int {
			return 0
		}

		companion object {
			@JvmField
			val CREATOR: Parcelable.Creator<Person> = object : Parcelable.Creator<Person> {
				override fun createFromParcel(src: Parcel): Person {
					return Person(src)
				}
				override fun newArray(size: Int): Array<Person?> {
					return arrayOfNulls(size)
				}
			}
		}
	}
	```

	* IntentActivity
	
	```kotlin
	class IntentActivity : AppCompatActivity() {
		override fun onCreate(savedInstanceState: Bundle?) {
			super.onCreate(savedInstanceState)
			setContentView(R.layout.activity_intent)

			val person = intent.getParcelableExtra<Person>("person")
			name.text=person.name
			age.text=person.age.toString()

			val adapter = ArrayAdapter(this,android.R.layout.simple_list_item_1,person.favoriteFoodList)
			listView.adapter=adapter

			var list = mutableListOf<HashMap<String,String>>()
			for(i in person.map){
				var hashMap = HashMap<String,String>()
				hashMap["id"]=i.key
				hashMap["password"]=i.value.toString()
				list.add(hashMap)
			}
			val mapAdapter = SimpleAdapter(this,list,android.R.layout.simple_list_item_2, arrayOf("id","password"),
						intArrayOf(android.R.id.text1,android.R.id.text2))
			mapListView.adapter=mapAdapter
		}
	}
	```
	
	* MainActivity
	
	```kotlin
	class MainActivity : AppCompatActivity() {
		override fun onCreate(savedInstanceState: Bundle?) {
			super.onCreate(savedInstanceState)
			setContentView(R.layout.activity_main)
			var list = arrayListOf("치킨","피자","과일")
			var hashMap = hashMapOf("A" to false,"B" to true,"C" to false,"D" to true)
			var person = Person("KKK",15,list,hashMap)
			val intent = Intent(this,IntentActivity::class.java)
			intent.putExtra("person",person)
			startActivity(intent)
		}
	}
	```
	
* ###  Library 사용
	* Library 추가
	
	```
	androidExtensions{
		experimental = true
	}
	```
	app 수준의  gradle에 추가
	* Person 수정

	```kotlin
	@Parcelize
	class Person (var name: String? = null,
		var age: Int = 0,
		var favoriteFoodList: ArrayList<String> = ArrayList(),
		var map :HashMap<String,String> = HashMap()): Parcelable
	```
## 	60 line -> 6 line
---
title: Android Kotlin - RecyclerView Event처리
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-26 08:26:28 -0400'
categories: Android
---

> ##  RecyclerView(mvvm)  

RecyclerView를 사용하기 위해서는 라이브러리가 필요하다.

app수준의 gradle에 다음의 라이브러리를 추가합니다.

	implementation 'com.android.support:cardview-v7:28.0.0'
	implementation 'com.android.support:recyclerview-v7:28.0.0'

RecyclerView의 item 이벤트를 처리하는 방법으로는 3가지 정도의 방법이 있는거 같다.

### 	ListRecyclerViewAdapter.kt

```kotlin
class ListRecyclerViewAdapter(var items: List<ListItem>) :
	RecyclerView.Adapter<ListRecyclerViewAdapter.GenericViewHolder>() {
	/** 1 */
//    lateinit var onClick: (View) -> Unit
	
	/** 2 */
//    companion object {
//        lateinit var clickListener: ClickListener
//    }

	override fun onCreateViewHolder(parent: ViewGroup, pos: Int): GenericViewHolder {
		var binding = com.ls.project.areacard.databinding.ItemListBinding.inflate(
				LayoutInflater.from(parent.context),
				parent,
				false
		)
			/** 1 */
	//        binding.linear.setOnClickListener {
	//            onClick(it)
	//        }
		return GenericViewHolder(binding)
	}

	...

	inner class GenericViewHolder(var binding: ItemListBinding) : RecyclerView.ViewHolder(binding.root)
	 /** 2 */
//        , View.OnClickListener, View.OnLongClickListener
    {
        fun bind(item: ListItem) {
            binding.setVariable(BR.item, item)
            /** 2 */
//            binding.linear.setOnClickListener(this)
//            binding.linear.setOnLongClickListener(this)

            binding.executePendingBindings()
        }
        /** 2 */

//        override fun onClick(v: View) {
//            clickListener.onItemClick(v,adapterPosition)
//        }
//        override fun onLongClick(v: View): Boolean {
//            clickListener.onItemLongClick(v,adapterPosition)
//            return true //false -> onClick 안됨
//        }
    }

    /** 2 */
//    fun setOnItemClickListener(clickListener: ClickListener) {
//        ListRecyclerViewAdapter.clickListener = clickListener
//    }
//    interface ClickListener {
//        fun onItemClick(v: View,pos: Int)
//        fun onItemLongClick(v: View,pos: Int)
//    }
}
```

### 	MainActivity.kt

```kotlin
class MainViewModel : ViewModel {
    lateinit var adapter: ListRecyclerViewAdapter
    private lateinit var items: ArrayList<ListItem>
    override fun onCreate() {
        items = arrayListOf<ListItem>()
        for (i in 0..10) {
            items.add(ListItem("구역 $i", "${i * 10}"))
        }
        adapter = ListRecyclerViewAdapter(items)
        /** 2 */
//        adapter.setOnItemClickListener(object : ListRecyclerViewAdapter.ClickListener{
//                TODO
//            }
//
//            override fun onItemLongClick(v: View, pos: Int) {
//                TODO
//            }
//        })

        /** 1 */
//        adapter.onClick = {
//            TODO
//        }
    }		
		...
}
```

### 	BindingAdapter.kt

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

            /** 3 */
//            recyclerView.addOnItemTouchListener(object :
//                RecyclerViewListener(recyclerView.context, recyclerView, object : RecyclerViewListener.OnItemClickListener {
//                    override fun onItemClick(view: View, position: Int) {
//                         TODO
//                    }
//
//                    override fun onLongItemClick(view: View?, position: Int) {
//                         TODO
//                    }
//                }){})
        }
    }
}
```

### 	RecyclerViewListener.kt
```kotlin
 /** 3 */
open class RecyclerViewListener(
    context: Context,
    recyclerView: RecyclerView,
    private val mListener: OnItemClickListener?
) : RecyclerView.OnItemTouchListener {

    var mGestureDetector: GestureDetector

    interface OnItemClickListener {
        fun onItemClick(view: View, position: Int)

        fun onLongItemClick(view: View?, position: Int)
    }

    init {
        mGestureDetector = GestureDetector(context, object : GestureDetector.SimpleOnGestureListener() {
            override fun onSingleTapUp(e: MotionEvent): Boolean {
                return true
            }

            override fun onLongPress(e: MotionEvent) {
                val child = recyclerView.findChildViewUnder(e.x, e.y)
                if (child != null && mListener != null) {
                    mListener.onLongItemClick(child, recyclerView.getChildAdapterPosition(child))
                }
            }
        })
    }

    override fun onInterceptTouchEvent(view: RecyclerView, e: MotionEvent): Boolean {
        val childView = view.findChildViewUnder(e.x, e.y)
        if (childView != null && mListener != null && mGestureDetector.onTouchEvent(e)) {
            mListener.onItemClick(childView, view.getChildAdapterPosition(childView))
            return true
        }
        return false
    }

    override fun onTouchEvent(view: RecyclerView, motionEvent: MotionEvent) {}

    override fun onRequestDisallowInterceptTouchEvent(disallowIntercept: Boolean) {}
}
```
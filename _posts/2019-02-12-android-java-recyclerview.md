---
title: Android Java- RecyclerView
layout: single
author_profile: true
read_time: true
comments: null
share: true
related: true
date: '2019-02-12 08:26:28 -0400'
categories: Android
---

> ##  RecyclerView 

RecyclerView를 사용하기 위해서는 라이브러리가 필요하다.

app수준의 gradle에 다음의 라이브러리를 추가합니다.

	implementation 'com.android.support:cardview-v7:28.0.0'
	implementation 'com.android.support:recyclerview-v7:28.0.0'


### 	activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto"
	xmlns:tools="http://schemas.android.com/tools"
	android:orientation="vertical"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	tools:context=".MainActivity">

	<android.support.v7.widget.RecyclerView
		android:id="@+id/recyclerView"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		/>
</LinearLayout>
```

### 	activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout>
	<data>
		<variable
			name="item"
			type="com.ls.project.recyclerview.model.ListItem"/>
	</data>
	<android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:card_view="http://schemas.android.com/apk/res-auto"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		xmlns:tools="http://schemas.android.com/tools"
		card_view:cardBackgroundColor="#E6E6E6"
		card_view:cardCornerRadius="8dp"
		card_view:cardElevation="8dp"
		android:layout_margin="3dp">
		<LinearLayout
			android:id="@+id/linear"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			android:orientation="horizontal"
			android:divider="@drawable/divider"
			android:showDividers="middle"
			android:padding="5dp"
			>
			<TextView
				android:id="@+id/num"
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:layout_marginLeft="6dp"
				android:layout_gravity="center"
				android:text="@{item.num}"
				tools:text="1 구역"
				android:textSize="30sp"/>
			<TextView
				android:id="@+id/name"
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:layout_marginLeft="6dp"
				android:layout_gravity="center"
				android:text="@{item.name}"
				tools:text="oo마트"
				android:textSize="23sp"/>
			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:layout_marginLeft="6dp"
				android:layout_gravity="center"
				tools:text="마지막 방문일 : "
				android:textSize="20sp"/>
			<TextView
				android:id="@+id/date"
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:layout_marginLeft="6dp"
				android:layout_gravity="center"
				android:text="@{item.date}"
				tools:text="19/5/5"
				android:textSize="26sp"/>
		</LinearLayout>
	</android.support.v7.widget.CardView>
</layout>
```

### 	ListItem.java

```java
public class ListItem {
	String num;
	String name;
	String date;

	public ListItem(String num, String name, String date) {
		this.num = num;
		this.name = name;
		this.date = date;
	}

	public String getNum() {
		return num;
	}

	public String getName() {
		return name;
	}

	public String getDate() {
		return date;
	}

	public void setNum(String num) {
		this.num = num;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setDate(String date) {
		this.date = date;
	}
}
```

### 	ListRecyclerViewAdapter.java

```java
public class ListRecyclerViewAdapter extends RecyclerView.Adapter<ListRecyclerViewAdapter.GenericViewHolder> {
    List<ListItem> items;

    public ListRecyclerViewAdapter(List<ListItem> items) {
        this.items = items;
    }

    @NonNull
    @Override
    public GenericViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int i) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_list, parent, false);
        GenericViewHolder holder = new GenericViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull GenericViewHolder holder, int i) {
        holder.num.setText(items.get(i).num);
        holder.name.setText(items.get(i).name);
        holder.date.setText(items.get(i).date);
    }

    @Override
    public int getItemCount() {
        return items.size();
    }

    public List<ListItem> getItems() {
        return items;
    }

    public void setItems(List<ListItem> items) {
        this.items = items;
    }

    public ListItem getItemPos(int pos){
        return items.get(pos);
    }

    public class GenericViewHolder extends RecyclerView.ViewHolder {
        TextView num;
        TextView name;
        TextView date;

        public GenericViewHolder(View view) {
            super(view);
            num = view.findViewById(R.id.num);
            name = view.findViewById(R.id.name);
            date = view.findViewById(R.id.date);
        }
    }
}
```

### 	MainActivity.java

```java
public class MainActivity extends AppCompatActivity {
    private RecyclerView mRcyclerView;
    private RecyclerView.Adapter mAdapter;
    private RecyclerView.LayoutManager mLayoutManager;
    private ArrayList<ListItem> items;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mRcyclerView = findViewById(R.id.recyclerView);
        mRcyclerView.setHasFixedSize(true);
        mLayoutManager = new LinearLayoutManager(this);
        mRcyclerView.setLayoutManager(mLayoutManager);

        items = new ArrayList<>();
        for (int i = 1; i < 10; i++) {
            items.add(new ListItem("구역 " + String.valueOf(i), String.valueOf(i) + " 층", String.valueOf(i * 10)));
        }
        mAdapter = new ListRecyclerViewAdapter(items);

        mRcyclerView.setAdapter(mAdapter);

    }
}
```
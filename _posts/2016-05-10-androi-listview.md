---
layout: post
title:  ListView设置条目显示四种方案（listView的优化）
date:   2016-05-10 01:08:00 +0800
categories: Android
tag: Android,ListView
---

* content
{:toc}


Listview是安卓中比不可少的一道风景，但是我用到listView的时候知道ListView<font color="red">容易造成内存的溢出</font>，如果条目很少的话 ，我们一般的是直接使用，但是对于现在大量的ListView的显示，造成内存的溢出会很常见。话不多说了，先上代码

<br>

> <font color="red">第一种很好理解，但是容易照成内存的溢出。</font>


> item的代码

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
android:paddingTop="5dp"  
android:paddingLeft="5dp"  
    android:layout_height="match_parent"
    android:orientation="horizontal" >

    <ImageView
        android:id="@+id/iv"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:src="@drawable/ic_launcher" />

    <LinearLayout
        android:gravity="center_vertical"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:orientation="vertical" >

        <TextView
            android:textColor="#000000"
            android:id="@+id/text1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="text1" />

        <TextView
            android:textColor="#000000"
            android:id="@+id/text2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="text2" />
    </LinearLayout>

</LinearLayout>
```

> mainXML的代码

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <ListView

        android:id="@+id/lv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</RelativeLayout>
```

>主函数的代码

```java
private ListView lv;
	private ArrayList<String> list = new ArrayList<String>();
	private ArrayList<Integer> iconlist=new ArrayList<Integer>();
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		setData();
		lv = (ListView) findViewById(R.id.lv);
		lv.setAdapter(new MyAdapter());

	}

	private void setData() {
		int[] icon = new int[] { R.drawable.a, R.drawable.b, R.drawable.c,
				R.drawable.d, R.drawable.e };

		for (int i = 0; i < icon.length; i++) {
			list.add("我是第" + i + "条目的数据");
			iconlist.add(icon[i]);
		}
	}
```

#### <font color="blue">第一种：</font>
     下面是Adapter的代码（今天主要就是Adapter的优化），这一种方法是本人最最不建议的一种方法，了解就行，要是开发的时候这么写，哼哼，你的组长很有可能请你喝茶。

```java
class MyAdapter extends BaseAdapter {

		@Override
		public int getCount() {
			return iconlist.size();
		}

		@Override
		public Object getItem(int position) {
			return position;
		}

		@Override
		public long getItemId(int position) {
			return iconlist.get(position);
		}

		@Override
		public View getView(int position, View convertView, ViewGroup parent) {
			View view=View.inflate(getApplicationContext(), R.layout.listviewitem,null);
			ImageView iv=(ImageView) view.findViewById(R.id.iv);
			TextView text1=(TextView) view.findViewById(R.id.text1);
			TextView text2=(TextView) view.findViewById(R.id.text2);

			iv.setImageResource(iconlist.get(position));
			text1.setText(list.get(position));
			text2.setText(list.get(position));
			return view;
		}
	}
```

#### <font color="blue">第二种:</font>
     第二种方式是对convertView的复用，这个原理就是我门创建每一个Item的时候默认的会设置给convertView，至于converView的个数是多少，就是你手机能显示的listViewItem的条目的个数加一，为什么多个一？这个就是相当于交换值时用的temp变量一样，不再解释了，当我们的手滑动的时候，前一个item消失之后被下一个即将出现的Item服用，这样就省掉了很大很大的内存空间。

> 下面是代码（只是改变了getView方法的代码）

```java
            @Override
		public View getView(int position, View convertView, ViewGroup parent) {
			View view = null;
			if (convertView != null) {
				view = convertView;
			} else {
				view = View.inflate(getApplicationContext(),
						R.layout.listviewitem, null);
			}
			ImageView iv = (ImageView) view.findViewById(R.id.iv);
			TextView text1 = (TextView) view.findViewById(R.id.text1);
			TextView text2 = (TextView) view.findViewById(R.id.text2);
			iv.setImageResource(iconlist.get(position));
			text1.setText(list.get(position));
			text2.setText(list.get(position));
			return view;
		}

```

#### <font color="blue">第三种：</font>
     第三种的方式就是使用创建ViewHolder来绑定item这样的好处的效率更加的高，而且复用行非常的强这一种方式只能称之为半面向ViewHolder，ViewHolder内部类的代码如下。

```java
static class MyViewholder {
		ImageView iv;
		TextView text1, text2;

	}
```

>相应的getView方法:

```java
@Override
		public View getView(int position, View convertView, ViewGroup parent) {
			MyViewholder viewholder = null;
			if (convertView == null) {
				viewholder = new MyViewholder();
				convertView = View.inflate(getApplicationContext(),
						R.layout.listviewitem, null);
				viewholder.iv = (ImageView) convertView.findViewById(R.id.iv);
				viewholder.text1 = (TextView) convertView.findViewById(R.id.text1);
				viewholder.text2 = (TextView) convertView.findViewById(R.id.text2);
				convertView.setTag(viewholder);
			} else {
				viewholder = (MyViewholder) convertView.getTag();
			}
			viewholder.iv.setImageResource(iconlist.get(position));
			viewholder.text1.setText(list.get(position));
			viewholder.text2.setText(list.get(position));
			return convertView;
		}
```
#### <font color="blue">第四种：</font>
     完全面向ViewHolder 效率基本上一样的，就是面向的对象完全的变成了ViewHolder，这个主要用的就是面向对象的方法。

> ViewHolder的代码

```java
static class MyViewholder {
		private ImageView iv;
		private TextView text1, text2;
		//构造函数初始化控件
		MyViewholder(View convertView) {
			iv = (ImageView) convertView.findViewById(R.id.iv);
			text1 = (TextView) convertView.findViewById(R.id.text1);
			text2 = (TextView) convertView.findViewById(R.id.text2);
		}
		/**
		 * 提供获取MyViewholder的方法
		 * @param convertView
		 * @return
		 */
		public static MyViewholder getMyViewholder(View convertView){
			//获取viewHolder
			MyViewholder viewholder=(MyViewholder) convertView.getTag();
			if (viewholder==null) {
				//iewHolder为空了重新创建
				viewholder=new MyViewholder(convertView);
				convertView.setTag(viewholder);
			}
			return viewholder;
		}
	}
```

>相应的getView的方法

```java
@Override
public View getView(int position, View convertView, ViewGroup parent) {
			if (convertView == null) {
				convertView = View.inflate(getApplicationContext(),
						R.layout.listviewitem, null);
			}
           MyViewholder myViewholder=MyViewholder.getMyViewholder(convertView);
           myViewholder.iv.setImageResource(iconlist.get(position));
           myViewholder.text1.setText(list.get(position));
           myViewholder.text2.setText(list.get(position));
			return convertView;
		}
```

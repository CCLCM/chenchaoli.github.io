---
layout: post
title:  关于ScrollView嵌套GridView和ListView不能完全显示的问题
date:   2016-05-02 00:00:00 +0800
categories: Android
tag: ScrollView嵌套[GridView,ListView]不能完全显示的问题
---

* content
{:toc}



最近为公司做的一个Demo里面用到了ScrollView嵌套了GridView和ListView，然而在嵌套的时候我发现GridView和ListView都是不能完全显示，
显示的基本上都是单行的数据.

> 显示的效果是这样的其中的Listview和GridView是可以滑动的就是显示不全

![图1]({{ '/styles/images/android-scrollview-listciew-gridview/1.jpeg' | prepend: site.baseurl  }})





>最后查找资料和翻阅文档发现
<br/>
<font color="red">原因是ListView和GridView的绘制过程中在ScrollView中无法准确的测量自身的高度
而且listVIew和GridView抢占了焦点，使得ListView和GrideView具有自身的显示的效果，这样就测量出显示
一行条目即可的距离，其他的条目根据自身的滑动显示。</font>


> <font color="red"> <strong> 下面是案例代码</strong> </font>

> 我的XMl的部分代码如下：

```xml
<ScrollView
    android:layout_height="match_parent"
    android:layout_width="fill_parent"
    android:scrollbars="none"

    >
    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:background="#23000000"
        android:orientation="vertical"
        android:paddingTop="-1dp" >

        <android.support.v4.view.ViewPager
            android:id="@+id/home_pager"
            android:layout_width="fill_parent"
            android:layout_height="100dp" >
        </android.support.v4.view.ViewPager>

        <GridView
            android:id="@+id/gv_home"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:numColumns="5"
            android:paddingBottom="10dp"
            android:paddingTop="13dp" >
        </GridView>

        <LinearLayout
            android:id="@+id/ll_clickin"
            android:layout_width="fill_parent"
            android:layout_height="30dp"
            android:layout_marginBottom="10dp"
            android:layout_marginLeft="10dp"
            android:layout_marginRight="10dp"
            android:background="@drawable/shape_home"
            android:orientation="horizontal" >

            <ImageView
                android:layout_width="85dp"
                android:layout_height="wrap_content"
                android:src="@drawable/click_in" />

            <TextView
                android:layout_width="match_parent"
                android:layout_height="fill_parent"
                android:gravity="center"
                android:singleLine="true"
                android:text="限时抢购 ,快点进入吧！"
                android:textSize="18sp" />
        </LinearLayout>

        <ListView
            android:id="@+id/list_home"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:background="#ffffff" >
        </ListView>
    </LinearLayout>
</ScrollView>
```



<br/>

<font color="red"> <strong>☺ 用自己写的方法之后才显示出来了所有的条目,如下图: </strong></font>

<br/>



![图2]({{ '/styles/images/android-scrollview-listciew-gridview/2.jpeg' | prepend: site.baseurl  }})


> 首先是关于ListView的 （注意此方法必须方到SetAdapter（）方法之后执行）

> 这是控件的查找

```java

                 list_home = (ListView) view.findViewById(R.id.list_home);
		list_home.setAdapter(new MyListViewAdapter(grideview_List));
                 list_home.setFocusable(false);//词句加不加像也没有什么影响，我是加的
		//setAdapter之后调用
		getListViewSelfHeight(list_home);
```    
> 这是 getListViewSelfHeight(ListView youListview) 方法

```java
public void getListViewSelfHeight(ListView listView) {   
           // 获取ListView对应的Adapter   
        ListAdapter listAdapter = listView.getAdapter();   
        //健壮性的判断
        if (listAdapter == null) {   
            return;   
        }  
        // 统计所有子项的总高度   
        int totalHeight = 0;   
        for (int i = 0, len = listAdapter.getCount(); i < len; i++) {   
            // listAdapter.getCount()返回数据项的数目   
            View listItem = listAdapter.getView(i, null, listView);   
            // 调用measure方法 传0是测量默认的大小
            listItem.measure(0, 0);    
            totalHeight += listItem.getMeasuredHeight();    
        }   
        //通过父控件进行高度的申请
        ViewGroup.LayoutParams params = listView.getLayoutParams();   
        //listAdapter.getCount() - 1 从零开始  listView.getDividerHeight()获取子项间分隔符占用的高度  
        params.height = totalHeight+ (listView.getDividerHeight() * (listAdapter.getCount() - 1));   
        listView.setLayoutParams(params);   
    }   
```



> 下面是GridView的方法和ListView的测量的方法基本一样  但是listView是单行条目的不用在担心列的问题问GridView则是需要进行自己分行和自己分列的 所以要注意一下




```java
    gv_home = (GridView) view.findViewById(R.id.gv_home);
 		gv_home.setSelector(new ColorDrawable(Color.TRANSPARENT));
		home_pager.setFocusable(false);
		gv_home.setAdapter(new MyGrigeViewAdapter(grideview_List));
		getGridViewSelfHeight(gv_home);
```



> 下面是getGridViewSelfHeight(GridView youGrideView)(这个方法能解决问题但是感觉不是很好灵活性太差 我用的获取的列数始终获取不到，有看神看到了 给我回复)

```java

 public void getGridViewSelfhetght(GridView gridView) {   
        // 获取GridView对应的Adapter   
        ListAdapter adapter = gridView.getAdapter();   
        if (adapter == null) {   
            return;   
        }   
        int totalHeight = 0;   
        for (int i = 0, len = adapter.getCount(); i < len; i++) {   
            // gridView.getCount()返回数据项的数目   
            View listItem = adapter.getView(i, null, gridView);   
            // 计算子项View 的宽高   
            listItem.measure(0, 0);    
            //此处方法并不好
            //5其中5是我们在Xml中的android:numColumns="5"
            //FontDisplayUtil.dip2px(MyGlobalApplication.getContext(),3)设置下边据
            totalHeight += listItem.getMeasuredHeight()/5+FontDisplayUtil.dip2px(MyGlobalApplication.getContext(),3);    
        }   

        ViewGroup.LayoutParams params = gridView.getLayoutParams();  
        params.height = totalHeight;   
        gridView.setLayoutParams(params);   
    }  
```

> 下面是FontDisplayUtil类


```java
import android.content.Context;

/**
 * dp、sp 、 px之间的相互转化的工具类
 */
public class FontDisplayUtil {
 /**
  * 将px值转换为dip或dp值，保证尺寸大小不变
  */
 public static int px2dip(Context context, float pxValue) {
  final float scale = context.getResources().getDisplayMetrics().density;
  return (int) (pxValue / scale + 0.5f);
 }

 /**
  * 将dip或dp值转换为px值，保证尺寸大小不变
  */
 public static int dip2px(Context context, float dipValue) {
  final float scale = context.getResources().getDisplayMetrics().density;
  return (int) (dipValue * scale + 0.5f);
 }

 /**
  * 将px值转换为sp值，保证文字大小不变
  */
 public static int px2sp(Context context, float pxValue) {
  final float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
  return (int) (pxValue / fontScale + 0.5f);
 }

 /**
  * 将sp值转换为px值，保证文字大小不变
  */
 public static int sp2px(Context context, float spValue) {
  final float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
  return (int) (spValue * fontScale + 0.5f);
 }
}
```

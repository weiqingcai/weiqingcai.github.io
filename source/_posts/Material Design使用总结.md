---
title: Material Design使用总结
date: 2018-11-09 16:49:37
tags: [UI支持库,Material Design]
categories: Android
---

# Material Design使用总结

Material Design是在Android5.0时新推出的一种设计规范，现在绝大部分的app都已经使用这种新的设计规范来进行界面设计。其主要是强调材质和层次感在设计中的应用，Android中也做了一些原生态的支持，但是要使用这些都必须最小兼容到Android5.0，也就是API  21，或者是添加Material Design的一个支持库。关于详细的Material Design的解释和说明，可以参考这本书：

[电子书下载地址](http://download.csdn.net/detail/wqc_csdn/9516039)
推荐两个Material Design的配色和图标网站：

一个是可以在线进行配色方案的查看：[**Materia l Palette**](http://www.materialpalette.com/)

![网站界面](http://img.blog.csdn.net/20160515213603298)

一个是提供Material Design风格的图标：[**Materia l Design Icons**](https://materialdesignicons.com/)

![网站界面](http://img.blog.csdn.net/20160515214744111)

## 主题：

![颜色分布](http://img.blog.csdn.net/20160519113211572)
最基本的主题有三个：
@android:style/Theme.Material
@android:style/Theme.Material.Light
@android:style/Theme.Material.Light.DarkActionBar
这几个是基本的主题，也是分别代表三种风格的主题，更多的需要我们自己根据自己的需要来进行定制。

```xml
<!--actionBar颜色-->
<item name="colorPrimary">@android:color/holo_green_light</item>

<!--状态栏颜色-->
<item name="colorPrimaryDark">@android:color/holo_green_light</item>

<!--控件颜色-->
<item name="colorAccent">#464545</item>
```

主要由几部分组成，colorPrimary，colorPrimaryDark，colorAccent，textColorPrimary，windowPrimary，navigationBarColor这个值可以使用系统已经提供的颜色值，也可以直接自己定制。

## 基本使用方式：

Android中为我们提供了一个便捷的提取相应主题的工具---Palette。其能根据传入的Bitmap来获取到一系列特定风格的颜色值，使得当前主题和传入的图片更为的贴切。使用该工具能很方便的创建出各种风格的主题。

- 要使用需要导入相应的包--com.android.support:palette-v7。如果是AS，则直接打开module setting，在dependencies中进行添加即可。如果是Eclipse，这需要去sdk文件夹-----extras---android--support---v7文件夹中将palette项目导入工作空间，并作为主工程的依赖项目。

- 导入完成后直接在代码中使用。

```Java

//声明一个Bitmap对象
Bitmap bitmap = BitmapFactory.decodeResource(MainActivity.this.getResources(), R.drawable.study55);
// 声明一个palette对象
Palette.Builder palette = Palette.from(bitmap);

//获取到相应的色调，并设置个相应的组件。
Palette.Swatch  darkMutedSwatch = palette.generate().getDarkMutedSwatch();
MainActivity.this.getWindow().setStatusBarColor(darkMutedSwatch.getRgb());
```

效果如下：
*点击前：*
![点击前](http://img.blog.csdn.net/20160520160704957)

*点击后：*
![点击后](http://img.blog.csdn.net/20160520160742255)

常见的一些提取的色调：

getDarkMutedColor(int defaultColor)------获取一个柔和的，深色的颜色

getLightMutedColor(int defaultColor)------获取一个柔和的，亮色的颜色

getDarkVibrantColor(int defaultColor)------获取一个有活力的，深色的颜色

getLightVibrantColor(int defaultColor)------获取一个有活力的，浅色的颜色

getMutedColor(int defaultColor)------获取一个柔和的颜色

getVibrantColor(int defaultColor)------获取最为有活力的颜色

## 视图与阴影：

在Material Design中新添加了一个布局属性，除了原本的X，Y之外，新添加了Z轴（包含静态的高度和动态的高度），使得视图在垂直与手机屏幕的方向上也有了属性，也就是产生了视图和阴影效果，而不像是之前所有的视图都在同一个平面上。

一张图来说明在Material Design中视图的推荐高度：
![图片说明](http://img.blog.csdn.net/20160518214813904)

**阴影效果：**Z轴由两部分组成，静态高度和动态高度。静态高度常用于视图的固定布局，动态高度则常用于实现动画效果。即  **Z =elevation + translationZ**

elevation=0
![elevation=0](http://img.blog.csdn.net/20160518220847552)

elevation = 100
![elevation = 100](http://img.blog.csdn.net/20160518220929704)

我们可以使用SeekBar在代码里动态的修改阴影的高度，更多的还是将其作为动画来使用。

```Java
mSeekBarElevation.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
                mCardView.setCardElevation(progress);

            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {

            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {

            }
        });
```

## 裁剪与着色：

**1.  着色：**
新添加的一种对图像的修改方式，通过修改图像中的Alpha遮罩来修改图像。
**基本使用方式：**

- 在xml文件中：直接为需要的图片设置着色颜色和着色的模式即可。

```xml
    <!--原图-->
    <ImageView
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
    <!--默认模式-->
    <ImageView
        android:tint="#3759f4"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
    <!--add-->
    <ImageView
        android:tint="#3759f4"
        android:tintMode="add"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
    <!--multiply-->
    <ImageView
        android:tint="#3759f4"
        android:tintMode="multiply"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
    <!--screen-->
    <ImageView
        android:tint="#3759f4"
        android:tintMode="screen"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
    <!--src_atop-->
    <ImageView
        android:tint="#3759f4"
        android:tintMode="src_atop"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
    <!--src_over-->
    <ImageView
        android:tint="#3759f4"
        android:tintMode="src_over"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
    <!--src_in-->
    <ImageView
        android:tint="#3759f4"
        android:tintMode="src_in"
        android:src="@mipmap/ic_launcher"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />
```

![效果图](http://img.blog.csdn.net/20160518222854717)

**2.   裁剪：** Clipping裁剪可以使我们能够为控件的外形绘制指定类型的形状，和之前的shape有相同的效果，只是这是直接修改外形，而不是作为一个背景。
**基本使用方式：**

- 在布局文件中放置一个需要用到的控件，无需有额外的设置。

```xml
 <ImageView
        android:layout_margin="10dp"
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:id="@+id/fab_add"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"/>
```

- 在Java代码中获取到这个控件并使用ViewOutlineProvider来为其设置外形。

```Java
//声明一个ViewOutlineProvider实例，并设置需要的类型，有多种图形可以选择，只需要重写其getOutline方法即可。
ViewOutlineProvider outlineProvider = new ViewOutlineProvider(){
    @Override
    public void getOutline(View view, Outline outline) {
        int fabSize = 100;
        outline.setOval(-4,-4,fabSize+2,fabSize+2);
    }
};
//调用View的setOutlineProvider（）方法为控件设置外形。
View fabView = findViewById(R.id.fab_add);
fabView.setOutlineProvider(outlineProvider);
```

- 这样就做出了一个圆形的按钮（类似于FloatingActionBar）：

![效果展示](http://img.blog.csdn.net/20160520151846222)

## 控件：

**1.RecyclerView :**
RecyclerView 是对ListView进行的一次提升和拓展，同时考虑到了布局的重用问题，通过内部的ViewHolder来提升效率，同时提供了更多新的特性和定制细节，有更大的定制功能。
**基本使用方式：**

- 首先和ListView一样，做一个Item中要用到的布局，这个根据自己额业务需要来自由定制（演示只使用一个TextView）。

**recycle_view_item.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:fontFamily="monospace"
        android:padding="10dp"
        android:textSize="20sp"
        android:id="@+id/id_show_text_view"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"/>
</LinearLayout>
```

- 然后和ListView相似，也是定制自己的Adapter，这里一般需要重新复写的方法有三个，并实现一个自己的ViewHolder（继承自RecyclerView.ViewHolder）

**RecycleViewAdapter.java**

```Java
package com.wei.designsupportlibrarystudy;

import android.support.v7.widget.RecyclerView;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.Random;

/**
 * Created by WQC
 */
public class RecycleViewAdapter extends RecyclerView.Adapter<RecycleViewAdapter.ViewHolder> {
    //保存数据的Java Bean
    private final ArrayList<DataModel> mDatas;

    public RecycleViewAdapter(ArrayList<DataModel> mDatas) {
        this.mDatas = mDatas;
    }

    /**
     * 当ViewHolder被创建时调用
     * @param parent 父控件
     * @param viewType View类型
     * @return ViewHolder的实例
     */
    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View itemView = LayoutInflater.from(parent.getContext()).inflate(R.layout.recycle_view_item, parent, false);
        return new ViewHolder(itemView);
    }
	
	/**
     * 当数据和View进行绑定时被调用
     * @param holder ViewHolder实例
     * @param position 当前View的Item位置
     */
    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
	    //根据Position来获取相应的数据
        final DataModel rowData = mDatas.get(position);
		//数据显示到视图上
	    holder.showTextView.setText(rowData.getShowText());
        holder.showTextView.setTag(rowData);

    }
	

	 /**
     * 获取列表项的长度
     * @return 列表的长度
     */
    @Override
    public int getItemCount() {
        return mDatas.size();
    }



    /**
     * 实现ViewHolder的内部类，用于布局的重用
     */
    public static class ViewHolder extends RecyclerView.ViewHolder {
	    //这里只有一个TextView，和recycle_view_item.xml布局里边是对应的，都是根据自己的业务需要进行修改。
	    
        private final TextView showTextView;
		//通过itemView 来进行重用
        public ViewHolder(View itemView) {
            super(itemView);
            showTextView = (TextView) itemView.findViewById(R.id.id_show_text_view);
        }
    }

```

- 然后就是在主布局中加入RecyclerView控件进行使用：

**activity_recycleview.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools"
android:orientation="vertical"
android:layout_width="match_parent"
android:layout_height="match_parent">

    <android.support.v7.widget.RecyclerView
        android:elevation="1dp"
        android:id="@+id/id_recycle_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
         <!--在这里进行滚动条方向的声明-->
        android:scrollbars="vertical"
        <!--在这里进行item布局的声明-->
        tools:listitem="@layout/recycle_view_item">
    </android.support.v7.widget.RecyclerView>
</RelativeLayout>
```

- 接着在Java代码中使用RecyclerView：

**RecycleViewActivity.java**

```Java
package com.wei.designsupportlibrarystudy;

import android.graphics.Outline;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.view.View;
import android.view.ViewOutlineProvider;
import android.widget.LinearLayout;

import java.util.ArrayList;

/**
 * Created by WQC.
 */
public class RecycleViewActivity extends AppCompatActivity {
    RecyclerView mRecyclerView;
    LinearLayoutManager linearLayoutManager;
    RecycleViewAdapter recycleViewAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recycleview);
        //获取到布局中声明的RecyclerView
        mRecyclerView =(RecyclerView)findViewById(R.id.id_recycle_view);
		//为RecyclerView指定一个布局管理器，可以是GridLayoutManager，LinearLayoutManager等，搭配使用有多种的表现形式。
        linearLayoutManager = new LinearLayoutManager(this);
		//设置布局管理器
        mRecyclerView.setLayoutManager(linearLayoutManager);
        //声明一个适配器实例，并传入数据。

        recycleViewAdapter = new RecycleViewAdapter(initData(20));
		//将适配器绑定到RecyclerView上
        mRecyclerView.setAdapter(recycleViewAdapter);
    }

    /**
     * 准备初始化数据
     */
    public ArrayList<DataModel> initData(int size){
        ArrayList<DataModel> datas = new ArrayList<>(size);

        for (int i = 0;i<size;i++) {
            datas.add(new DataModel("初始化数据" + "<" + i + ">"));
        }

        return datas;
    }



}

```

**ps：DataModel.java文件（JavaBean）**

```Java
package com.wei.designsupportlibrarystudy;

/**
 * Created by WQC   用于承载数据的Bean
 */
public class DataModel {
    private String showText;

    public DataModel(String showText) {
        this.showText = showText;
    }

    public String getShowText() {
        return showText;
    }

    public void setShowText(String showText) {
        this.showText = showText;
    }


}

```

至此RecyclerView的简单使用就完成了，可以尝试修改其列表方向，布局管理器的种类，不同的搭配产生不一样的效果，例如列表项，图墙，画廊效果，都可以自定义实现。

**关于RecycleView中的分隔线的问题：**

最开始Google推出的RecycleVIew并没有给出默认的分隔线实现，只是提供了一个RecyclerView.ItemDecoration类共供我们自定义实现，后来推出的版本中就给出了默认实现的分隔线。

**使用默认实现的分割线：**

```Java
mRecyclerView.addItemDecoration(new DividerItemDecoration(this, DividerItemDecoration.HORIZONTAL));
```

**使用自定义实现的分割线：**

```Java

/**
 * Created by WQC on 2016/5/12.
 * 自定义的item分隔符
 */
public class SimpleDivider extends RecyclerView.ItemDecoration {
    private static final int[] attrs = {android.R.attr.listDivider};
    private Drawable mDivider;

    public SimpleDivider(Context context) {
        TypedArray typedArray = context.obtainStyledAttributes(attrs);
        mDivider = typedArray.getDrawable(0);
        typedArray.recycle();
    }

    /**
     * 重写该方法来绘制自定义的分隔线，这里绘制的是一条直线（矩形）
     * @param canvas
     * @param parent
     * @param state
     */
    @Override
    public void onDrawOver(Canvas canvas, RecyclerView parent, RecyclerView.State state) {

        //左下角的点
        int left = parent.getPaddingLeft();

        //右下角的点
        int right = parent.getWidth() - parent.getPaddingRight();

        //获取总共的item个数
        int childCount = parent.getChildCount();


        for (int i = 0; i < childCount; i ++) {
            View child = parent.getChildAt(i);

            RecyclerView.LayoutParams layoutParams = (RecyclerView.LayoutParams) child.getLayoutParams();

            int top = child.getBottom() + layoutParams.bottomMargin;

            int bottom = top + mDivider.getIntrinsicHeight();

            mDivider.setBounds(left, top, right, bottom);

            mDivider.draw(canvas);
        }


    }

    @Override
    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
        super.onDraw(c, parent, state);
    }

    @Override
    public void getItemOffsets(Rect outRect, View view, RecyclerView parent, RecyclerView.State state) {
        super.getItemOffsets(outRect, view, parent, state);
    }
}

```

**附：鸿洋大神关于RecyclerView的文章：http://blog.csdn.net/lmj623565791/article/details/45059587**

**附：慕课网上有一个RecyclerView的使用视频教程：[视频地址](http://www.imooc.com/learn/424)**

**2. GardView**
CardView其实就是一个像其名字一样，是一个卡片布局，并且继承自Framelayout所以其也可以作为布局容器使用。

**基本使用方式：**

- 首先要导入其所在的jar包：
android.support.v7.widget.CardView。在Android Studio中直接可以找到。右键Module，选择Open Module Setting,选择Dependencies，点右边的加号，直接输入CardView搜索即可。
![指引图片](http://img.blog.csdn.net/20160516230208330)



-  在布局文件中直接使用CardView：

**content_card.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:context="com.wei.designsupportlibrarystudy.CardActivity"
    tools:showIn="@layout/activity_card">
<!--上边的都是AS自动生成的，不用理会-->
    <android.support.v7.widget.CardView
        android:id="@+id/id_card_view"
        android:layout_width="match_parent"
        android:layout_height="160dp"
        <!--控件的高度，影响阴影的效果-->
        android:elevation="200dp"
        android:layout_marginRight="@dimen/activity_horizontal_margin"
        android:layout_marginLeft="@dimen/activity_horizontal_margin"
        app:cardBackgroundColor="@color/cardview_dark_background"
        <!--控件矩形边框的角度-->
        app:cardCornerRadius="10dp">
    </android.support.v7.widget.CardView>

</LinearLayout>

```

- 然后在Java文件中通过findViewById()使用即可

**CardViewActivity.java**

```Java
 mCardView = (CardView) findViewById(R.id.id_card_view);
```

就是一个卡片的效果
![控件效果](http://img.blog.csdn.net/20160516232610723)

**3.  FloatingActionButton**
FloatingActionButton名为浮动圆形按钮，在android.support.design.widget.FloatingActionButton包下。这个兼容包主要是为了向低版本的Android设备兼容Material Design。上边图片中的邮件小图标便是FloatingActionButton的效果，其用法和ImageView相似，只是更加符合Material Design的设计规范，同时加入了阴影效果。

**基本使用方法：**

- 首先要添加上android.support.design这个库，AS中方法和之前一样，Eclipse中则需要进入sdk中，选择extra，选择android，选择support，将里边的design项目导入工作空间并作为主工程依赖的项目。

![路径图片](http://img.blog.csdn.net/20160517102104670)

- 其次就是在布局文件中直接使用FloatingActionButton

**布局文件中的使用：**

```xml
 <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="@dimen/fab_margin"
        android:src="@android:drawable/ic_dialog_info"

        <!--以下是该控件新增加的xml属性-->
        <!--设置背景颜色-->
        app:backgroundTint="#ff77"
        <!--设置大小-->
        app:fabSize="mini"
        <!--设置静态的高度-->
        app:elevation="20dp"
        <!--点击时的颜色-->
        app:rippleColor="#FFF4D6D6"/>
```

- 在Java代码中获取使用：

```Java
FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
```

该控件其实就是一个特定的ImageButton，也可以设置自己的事件监听，实现需要的业务逻辑。

该片段为FloatingActionButton设置了业务逻辑，点击时弹出一个Snackbar显示消息。

```Java
fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
            }
        });
```

**4.TextInputLayout**

TextInputLayout 其实是一个对于EditText进行拓展的新控件，原本的EditText的默认提示文本hint在点击输入时就会消失，而TextInputLayout使得点击输入后也依然显示提示信息。

**基本使用方式：**

- 首先也是导入android.support.design这个包，然后在布局文件中直接进行使用，将EditText放入TextInputLayout中。

```xml
<android.support.design.widget.TextInputLayout
        android:layout_below="@+id/tabLayout"
        android:id="@+id/textInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <EditText
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
</android.support.design.widget.TextInputLayout>
```

-  然后在Java文件中进行使用

```Java
//获取布局中的控件
        final TextInputLayout textInputLayout = (TextInputLayout) findViewById(R.id.textInput);
        //设置Hint信息
        textInputLayout.setHint("请输入用户名");
        //获取放在textInputLayout中的EditText
        EditText editText = textInputLayout.getEditText();

        if (editText != null) {
            //为EditText设置事件监听，用于处理输入事件
            editText.addTextChangedListener(new TextWatcher() {
                @Override
                public void beforeTextChanged(CharSequence s, int start, int count, int after) {
                }

                @Override
                public void onTextChanged(CharSequence s, int start, int before, int count) {

                    if (s.length() > 10) {
                        //使用textInputLayout进行消息提示
                        textInputLayout.setError("输入的用户名不能超出10位");
                        textInputLayout.setErrorEnabled(true);
                    }else {
                        textInputLayout.setErrorEnabled(false);
                    }
                }

                @Override
                public void afterTextChanged(Editable s) {

                }
            });
        }
```

效果：
![效果](http://img.blog.csdn.net/20160520161205766)

**5. Tablayout**

新出来的一个布局，可以很便捷的帮我们实现选项卡的功能。要使用需要导入android.support.design.widget.TabLayout包

**基本使用方式：**

- 首先导入包，然后直接在布局文件中添加上即可。

**布局文件**

```xml
<android.support.design.widget.TabLayout
        android:layout_alignParentTop="true"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:tabMode = "scrollable"
        android:id="@+id/tabLayout"></android.support.design.widget.TabLayout>
```

- 然后在Java代码中获取到，进行设置：

```Java
        TabLayout tabs = (TabLayout)findViewById(R.id.tabLayout);
        tabs.addTab(tabs.newTab().setText("tab1"));
        tabs.addTab(tabs.newTab().setText("tab2"));
        tabs.addTab(tabs.newTab().setText("tab3"));
        tabs.addTab(tabs.newTab().setText("tab4"));
        tabs.addTab(tabs.newTab().setText("tab4"));
        tabs.addTab(tabs.newTab().setText("tab4"));
        tabs.addTab(tabs.newTab().setText("tab4"));
        tabs.addTab(tabs.newTab().setText("tab4"));
```

这样就可以做出选项卡的效果了
![选项卡效果](http://img.blog.csdn.net/20160520165809066)

**6.  SnackBar：**

SncakBar是新出来的一款控件，他提供了一个特贴轻量级的反馈，作用类似于Toast，但是又和Toast有所区别。

官方文档的内容：
> Snackbars provide lightweight feedback about an operation. They show a brief message at the bottom of the screen on mobile and lower left on larger devices. Snackbars appear above all other elements on screen and only one can be displayed at a time. 

> Snackbars can contain an action which is set via setAction(CharSequence, android.view.View.OnClickListener).

>To be notified when a snackbar has been shown or dismissed, you can provide a Snackbar.Callback via setCallback(Callback).

翻译：
> SnackBar为一个操作提供了一个轻量级的回馈。它在手机屏幕的底部或者是在大屏幕设备的左下方显示一个简短的消息。SnackBar位于屏幕上所有元素的上方，并且只能显示一段时间

>SnackBar可以通过setAction(CharSequence, android.view.View.OnClickListener)方法来包含一个动作

>可以通过提供setCallback(Callback)接口来获取SnackBar显示和消失时的通知

**基本使用方式：**

-  类似于Toast，直接在程序中使用：

**Java代码：**

```Java
Snackbar snackbar = Snackbar.make(actionButton, "你点击了按钮", Snackbar.LENGTH_LONG);

snackbar.setAction("知道了", new View.OnClickListener() {
    @Override
    public void onClick(View v) {
      snackbar.dismiss();
    }
});

snackbar.show();
```

当然了Material Design中新出现的各种特性，新控件，新布局还有很多，这只是我自己使用的一个总结，欢迎交流和分享。

------

## 2016.11.28更新：

**7.  ToolBar：**

ToolBar已经开始取代ActionBar作为主要的标题栏的实现方式，使用toolBar也很简单，首先同样是在XML布局文件中声明好，然后在Java代码中获取使用即可。需要注意的是使用toolBar时要声明app使用的theme为android:theme="@style/AppTheme.NoActionBar",否则容易引起ActionBar和ToolBar的冲突。

//在xml布局文件中定义好

```xml
<android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:popupTheme="@style/AppTheme.PopupOverlay"/>
 //其中popupTheme是用于指定菜单选项弹出的样式的  
```

//在Java代码中获取并使用

``` Java
Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
```

这样就把ActionBar替换成了ToolBar，同时需要实现ToolBar对应的方法，包括menu菜单创建时和menu菜单项被点击时：

```Java
/**
     * 创建菜单时
     * @param menu
     * @return
     */
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }
	
	/**
     * 菜单项被点击时
     * @param item
     * @return
     */
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
```

**8.  AppBarLayout：**

参考自：http://blog.csdn.net/solo_talk/article/details/52222269

AppBarLayout可以将其布局里边所有的View都作为标题栏存在，例如可以在AppBarLayout中放一个ToolBar和一个ImageView，这样能丰富标题栏的类型。更主要的是使用AppBarLayout可以设置里边控件的滑动属性，实现滑动时AppBarLayout布局里边的控件隐藏或者是显示。

控制属性：

-  scroll：表示向下滚动的时候，设置了这个属性的View会被滚出屏幕范围，直到消失，想要实现滑动隐藏的话必须要有这个属性

- enterAlways：表示向上滚动的时候，设置了这个属性的View会随着滚动手势逐渐出现，直到恢复原来设置的位置

- enterAlwaysCollapsed：是enterAlways的附加选项，一般跟enterAlways一起使用，它是指，View在往下“出现”的时候，首先是enterAlways效果，当View的高度达到最小高度时，View就暂时不去往下滚动，直到ScrollView滑动到顶部不再滑动时，View再继续往下滑动，直到滑到View的顶部结束。

- exitUntilCollapsed：值设为exitUntilCollapsed的View，当这个View要往上逐渐“消逝”时，会一直往上滑动，直到剩下的的高度达到它的最小高度后，再响应ScrollView的内部滑动事件。

使用方式：

AppBarLayout需要有一个和它同层次的滚动布局来配合使用，同时要以CoordinatorLayout作为根布局（因为只有CoordinatorLayout布局为根布局，不同的布局之间才能相互联系起来）

伪代码表示使用方式：

```xml
<CoordinatorLayout>

	<AppBarLayout>
		
		<ToolBar
			 app:layout_scrollFlags="scroll|exitUntilCollapsed">
	    </ToolBar>
		
		<ImageView
			app:layout_scrollFlags="scroll|exitUntilCollapsed"/>
		
	</AppBarLayout>
	
	<!--和AppBarLayout同层级的滚动视图，如ScrollView等，同时设置app:layout_behavior属性-->
	<ScrollView
	app:layout_behavior="@string/appbar_scrolling_view_behavior">
	
	</ScrollView>

</CoordinatorLayout>
```

这样当下边的ScrollView向上滑动时，AppBarLayout中的控件会全部隐藏，向下滑动时会显示出来。

**9. CoordinatorLayout：**
参考自http://blog.csdn.net/u010687392/article/details/46906657
CoordinatorLayout可以组织多个子View之间相互协作，使用时同样需要引入design.support支持库。

在CoordinatorLayout布局中可以为其他的子View添加app:layout_behavior这个属性。若View添加上这个属性，那么当其产生响应的行为时CoordinatorLayout布局中的其他子View就可以获知这个消息，这时若是属性中设置了app:layout_scrollFlags这个属性的View就可以根据接受到的事件产生相应的动作。

使用方法：

```xml
<android.support.design.widget.CoordinatorLayout  
    xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    android:id="@+id/coordinator_layout"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <android.support.design.widget.AppBarLayout  
        android:id="@+id/appbar_layout"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:fitsSystemWindows="true">  
        <android.support.v7.widget.Toolbar  
            android:id="@+id/toolBar"  
            android:layout_width="match_parent"  
            android:layout_height="?attr/actionBarSize"  
            android:background="#30469b"  
            app:layout_scrollFlags="scroll|enterAlways" />  
    </android.support.design.widget.AppBarLayout>  

 <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:orientation="vertical"  
        android:scrollbars="none"    app:layout_behavior="@string/appbar_scrolling_view_behavior">    
        <!--为LinearLayout设置了behavior属性，当其滑动时，CoordinatorLayout就会通知其他设置了app:layout_scrollFlags属性的View产生变化 -->
    </LinearLayout>  
  
</android.support.design.widget.CoordinatorLayout>
```

**10. CollapsingToolbarLayout：**

这是一个可以折叠的ToolBarLayout布局，可以用它来包裹ToolBar，指定当滑动时CollapsingToolbarLayout发生折叠，而ToolBar可以保留下来，这样就可以实现当滑动时，顶部的控件都折叠了，独留下Toolbar。

使用方式：

```xml
<android.support.design.widget.AppBarLayout
        android:id="@+id/app_bar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/app_bar_height"
        android:fitsSystemWindows="true"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/toolbar_layout"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/AppTheme.PopupOverlay"/>

        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>
```

另附：

- Git-Hub上的一个UI测试库，遵循Material Design原则：[github地址](https://github.com/yhsj0919/YHUI)

- Material Design控件demo：[github地址](https://github.com/chenyangcun/MaterialDesignExample)

# AS-Memorandum
利用Android Studio制作简单的备忘录
# 项目简介

Android Studio 环境下，备忘录（简单）的实现，功能有：添加,单个查询，删除全部内容，并可获取当前时间存储在SqlLite中。所用到的显示控件为 ScrollView,EditText,TextView,ImageButton. 

## Acticity_main.xml代码展示

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/backgroundone"
    android:orientation="vertical"
    tools:context=".MainActivity">
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="15dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:id="@+id/test"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_horizontal"
            android:text="我的备忘录"
            android:textColor="#474646"
            android:textSize="45sp"
            android:textStyle="bold" />

    </LinearLayout>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="40dp"
        android:layout_gravity="center_horizontal"
        android:layout_marginLeft="10dp"
        android:orientation="horizontal">

        <EditText
            android:id="@+id/view_select"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="center_horizontal"
            android:ems="13"
            android:hint="此处可以查询数据！"
            android:inputType="textPersonName" />


        <ImageButton
            android:id="@+id/btn_select"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@android:color/transparent"
            app:srcCompat="@android:drawable/ic_menu_search" />

    </LinearLayout>


    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="490dp"
        android:layout_margin="5dp"
        android:layout_marginLeft="40dp"
        android:layout_marginRight="20dp">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical">

            <TextView
                android:id="@+id/data"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:gravity="center_horizontal"
                android:textSize="20dp"></TextView>
        </LinearLayout>

    </ScrollView>

    <ImageButton
        android:id="@+id/btn_add"
        android:layout_width="80dp"
        android:layout_height="74dp"
        android:layout_marginLeft="300dp"
        android:background="@android:color/transparent"
        app:srcCompat="@drawable/add1" />


</LinearLayout>
```

## Acativity_add.xml (添加)代码展示

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".add">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <ImageButton
            android:id="@+id/btn_back"
            android:layout_width="140dp"
            android:layout_height="60dp"
            android:background="@color/colorSilve"
            app:srcCompat="@android:drawable/ic_menu_revert" />

        <ImageButton
            android:id="@+id/btn_finish"
            android:layout_width="130dp"
            android:layout_height="60dp"
            android:visibility="visible"
            android:background="@color/colorSilve"
            app:srcCompat="@android:drawable/checkbox_on_background" />
	  <!--颜色是资源文件里设置好的，也可以自己随意改边颜色-->
        <ImageButton
            android:id="@+id/btn_clear"
            android:layout_width="140dp"
            android:layout_height="60dp"
            android:visibility="visible"
            android:background="@color/colorSilve"
            app:srcCompat="@android:drawable/ic_delete" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">
        <TextView
            android:id="@+id/now_time"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="10dp"
            android:layout_marginBottom="10dp"
            android:textStyle="bold"
            android:textSize="20dp">
        </TextView>


    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <EditText
            android:id="@+id/data_information"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#00FFFFFF"
            android:gravity="start"
            android:textSize="25dp"
            android:hint="此处写文章"
            android:textColor="#070507"
            >
        </EditText>


    </LinearLayout>




</LinearLayout>
```

## MainActivity.java代码展示

```java
package com.example.twicehomework;

import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.ImageButton;
import android.widget.TextView;
import android.widget.Toast;


public class MainActivity extends AppCompatActivity {
    private ImageButton btn_add;
    private TextView data;
    private ImageButton btn_select;
    private EditText view_select;



    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        data = findViewById(R.id.data);
        btn_select = findViewById(R.id.btn_select);
        view_select = findViewById(R.id.view_select);



        //跳转
        btn_add = (ImageButton) findViewById(R.id.btn_add);
        btn_add.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                intent.setClass(MainActivity.this, add.class);
                startActivity(intent);
            }
        });


        /*
        * 页面传递值
        * */
        Intent i = getIntent();
        String get_data = i.getStringExtra("data");
        data.setText(get_data);


        //查询监听
        btn_select.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String input = view_select.getText().toString().trim();
                String get = data.getText().toString().trim();

                //判断查询信息输入是否为空，并返回值
                if (!input.isEmpty()) {
                    int a = get.indexOf(input); //内容第一次出现的位置

                    //判断查询内容是否存在
                    if ( a != -1 ){

                        String front = get.substring(0,a+1);    //查询内容字段的位置的前一段数据截取
                        String after = get.substring(a+1,get.length()); //查询内容字段的位置的的后一段数据截取

                        int a1 = front.lastIndexOf("2020"); //前一段数据最后一次出现2020的位置
                        int b1 = after.indexOf("2020"); //后一段数据第一次出现2020的位置


                        String front2 ;//传值位
                        String after2 ;//传值位

                        //防止查询数据为第一位，出现错误
                        if (a1 == -1) {
                           front2 = front;
                        }else{
                            String front1 = front.substring(a1, front.length());
                            front2 = front1;
                        }

                        //防止查询数据为最后一位,出现错误
                        if (b1 == -1){
                            after2 = after;
                        }else {
                            String after1 = after.substring(0,b1);
                            after2 = after1;
                        }

                        showMsg(front2 + after2 );//显示查询结果信息

                    }else {
                        showMsg("无匹配数据");
                    }

                }else{
                    showMsg("请输入查询的数据");
                }
            }

        });

    }

    //显示提示消息
    private void showMsg(String msg){
        Toast.makeText(this,msg,Toast.LENGTH_SHORT).show();
    }
}
```

## add.java代码展示

```java
package com.example.twicehomework;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.content.ContentValues;
import android.content.Context;
import android.content.Intent;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.ImageButton;
import android.widget.TextView;
import android.widget.Toast;

import java.text.SimpleDateFormat;
import java.util.Date;


public class add extends AppCompatActivity {

    private ImageButton btn_back; //返回按钮
    private ImageButton btn_finish;//完成按钮
    private ImageButton btn_clear;//清楚所有数据按钮


    private TextView now_time;//当前时间
    private EditText data_information;//存储数据




    //与数据库操作相关的成员变量
    private MyDbHelper myDbHelper;//数据库帮助类
    private SQLiteDatabase db;//数据库类
    private ContentValues values;//数据表的一些参数
    private static final String mTablename = "mymemo";//数据表的名称



    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add);

        //建立联系
        btn_back = findViewById(R.id.btn_back);
        data_information = findViewById(R.id.data_information);
        now_time = findViewById(R.id.now_time);
        btn_finish = findViewById(R.id.btn_finish);


        btn_clear = findViewById(R.id.btn_clear);


        //初始化数据库工具类实例
        myDbHelper  = new MyDbHelper(this);


        //获取手机当前时间
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = new Date(System.currentTimeMillis());
        final String str_time = simpleDateFormat.format(date);
        now_time.setText(str_time);




        //返回主界面
        btn_back.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                intent.setClass(add.this,MainActivity.class);
                startActivity(intent);
                queryData();

            }
        });



        //完成并执行添加操作
        btn_finish.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String input = data_information.getText().toString().trim();
                if (input.isEmpty()){
                    showMsg("请输入要记录的内容");
                }else {
                    addData(str_time.trim(),input);
                    finish();
                    queryData();
                }
            }
        });

        btn_clear.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                clearData();
                showMsg("数据清楚完毕");
            }
        });

    }


    //添加数据方法
    private void addData(String now_time,String data_information){
        db = myDbHelper.getWritableDatabase();
        values = new ContentValues();
        values.put("now_time",now_time);
        values.put("information",data_information);
        db.insert(mTablename,null,values);
        showMsg("添加成功");

    }

    //清空所有数据的方法
    private void clearData(){

        db = myDbHelper.getWritableDatabase();
        db.delete(mTablename,null,null);
        String srtResult = "";
        Intent intent = new Intent(add.this,MainActivity.class);
        intent.putExtra("data",srtResult);
        startActivity(intent);
        showMsg("已经全部清空");
    }


    //显示消息方法
    private void showMsg(String msg){
        Toast.makeText(this,msg,Toast.LENGTH_SHORT).show();
    }


    //查询方法
    private void queryData(){
        db = myDbHelper.getReadableDatabase();
        Cursor cursor = db.query(mTablename,null,null,null,
                null,null,null);
        String srtResult = "";
        while (cursor.moveToNext()){
            srtResult += "\n" + cursor.getString(1);
            srtResult += "\n内容：" + cursor.getString(2);
            srtResult += "\n";
        }
        Intent intent = new Intent(add.this,MainActivity.class);
        intent.putExtra("data",srtResult);
        startActivity(intent);
    }





    /*
    *
    *
    * 自定义数据库帮助类，sqllite部分
    *
    * */
    class MyDbHelper extends SQLiteOpenHelper{

        public MyDbHelper(@Nullable Context context) {
            super(context, "mymemo.db", null, 2);
        }

        //只在创建数据库时，创建一次这个表
        @Override
        public void onCreate(SQLiteDatabase db) {
            db.execSQL("create table " + mTablename + "(_id integer primary key autoincrement, " +
                    "now_time varchar(50) unique,information varchar(100) )");
        }

        @Override
        public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

        }
    }
}
```

# 项目难点

## 思想

首先，整个的设计思想为一个页面显示信息，一个页面添加删除信息，因此会有两个xml文档以及对应的两个java文档，activity_main.xml 对应的MainActivity.java，activity_add.xml对应的add.java ，在activity_main.xml 中显示备忘录信息，以及查询备忘录信息。在activity_add.xml中实现把信息添加到数据库中。此项目使用的数据库为sqlLite，因为本文不涉及LIstView控件的使用，只是涉及到sqlLite通过Intent传值再现实，代码中已经体现。

## 代码

代码部分的难点有sqlLite如何创建表以及添加删除操作，页面的传值操作。

在sqlLite查询中通过页面转换的控制利用Intent进行传值，将数据传送到显示页面中在进行展示。

```java
 //查询方法
    private void queryData(){
        db = myDbHelper.getReadableDatabase();
        Cursor cursor = db.query(mTablename,null,null,null,
                null,null,null);
        String srtResult = "";
        while (cursor.moveToNext()){
            srtResult += "\n" + cursor.getString(1);
            srtResult += "\n内容：" + cursor.getString(2);
            srtResult += "\n";
        }
        Intent intent = new Intent(add.this,MainActivity.class);
        intent.putExtra("data",srtResult);
        startActivity(intent);
    }
```

除过传值，在主界面查询中会有缺陷，只能查询单次的，并且不能对查询内容进行修改。

# 声明

本文用来当做参考，为刚开始学习Android Studio的同志提供思路，如文章有缺陷，可以在下方打出，让其他同志参考。

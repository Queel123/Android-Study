# Android-Study
Some tips need to be noticed when studying Android

Android 的四大组件分别是活动，服务，广播接收器，内容提供器

### 第二章 探究活动
#### 创建活动时几个可勾选的按钮代表的意思
1. Generate Layout File会自动为活动创建一个对应的布局文件
2. Laucher Activity会自动将此活动设置为当前项目的主活动
3. Backwards Compatibility会自动为项目启用向下兼容的模式（一般需要勾选）

#### 重写Activity的onCreate()方法

```
public class 活动名 extends AppCompatActivity {

  @Override       //代表重写父类函数
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
  }

}
```

#### 创建布局
1. File name: 布局名
2. Root element: 根元素 （比如LinearLayout）
3. Design切换卡显示可视化布局编辑器，可以预览布局
4. Text通过XML的方式来编辑布局

#### 简单布局

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:orientation="vertical"
	android:layout_width="match_parent"
	android:layout_height="match_parent">
 
 <Button
	android:id="@+id/button_1"
	android:layout_width="match_parent"
	android:layout_height="wrap_content"
	android:text="Button 1"
	/>
	
</LinearLayout>
```

1. android:text指定了元素中显示的文字内容
2. match_parent代表当前元素和父元素一样宽
3. wrap_content表示当前元素的高度刚好包含里面的内容

- 调用布局
`setContentView(R.layout.布局名)`

#### 在AndroidManifest文件中注册

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="com.example.activitytest">
	<application
		android:allowBackup="true"
		android:icon="@mipmap/ic_launcher"
		android:label="@string/app_name"
		android:supportsRtl="true"
		andorid:theme="@style/AppTheme">
		<activity android:name=".FirstActivity"></activity>
	</application>
</manifest>
```

#### 配置主活动的方法
- label 指定了标题栏中的内容，也是Launcher中应用程序显示的名称

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	package="com.example.activitytest">
	<application
		android:allowBackup="true"
		android:icon="@mipmap/ic_launcher"
		android:label="@string/app_name"
		android:supportsRtl="true"
		andorid:theme="@style/AppTheme">
		<activity android:name=".FirstActivity"
			android:label="This is FirstActivity">
			<intent-filter>
				<action android:name="android.intent.action.MAIN" />
				<category android:name="android.intent.category.LAUNCHER" />
			</intent-filter>
		</activity>
	</application>
</manifest>
```

#### Toast的使用方法
- Toast 在程序中将一些短小的信息通知给用户，这些信息会在一段时间后自动消失

```
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.布局名)
	Button button1 = (Button) findViewById(R.id.button_1);
	button1.setOnClickListener(new View.OnClickLisetener() {
		@Override
		public void onClick(View v) {
			Toast.makeText(FirstActivity.this, "You clicked Button 1",
				Toast.LENGTH_SHORT).show();
		}
	});
}
```

#### 在活动中使用menu

```
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/add_item"
        android:title="Add"/>
    <item
        android:id="@+id/remove_item"
        android:title="Remove"/>
</menu>
```

- 需要重写的方法 onCreateOptionMenu 和 onOptionsItemSelected

```
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.add_item:
                Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show();
                break;
            case R.id.remove_item:
                Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show();
                break;
            default:
        }
        return true;
    }
```

#### 销毁一个活动
- 调用finish(void)函数

#### 显式Intent的使用
- 从FirstActivity跳转到SecondActivity 需要的步骤

```
 button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
               Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
	       startActivity(intent);

            }
 });
```

#### 隐式Intent的使用
- 从FirstActivity跳转到SecondActivity 需要的步骤
- 修改AndroidManifest.xml
```
<intent-filter>
	<action android:name="com.example.activitytest.ACTION_START"/>
	<category android:name="android.intent.category.DEFAULT"/>
</intent-filter>
```
```
 button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
               Intent intent = new Intent("com.example.activitytest.ACTION_START");
	       startActivity(intent);
            }
 });
```

#### 隐式Intent的导入网页用法
- Android系统内置的动作Intent.ACTION_VIEW
- Uri.parse()方法可以将网址字符串解析成Uri对象
- Intent的SetData()方法将这个Uri对象传递进去

```
 button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
               Intent intent = new Intent(Intent.ACTION_VIEW);
	       Intent.setData(Uri.parse("http://www.baidu.com"));
	       startActivity(intent);
            }
 });
```

#### 向下一个活动传递数据
- putExtra()方法接收两个参数，第一个参数是键，用于后面从Intent中取值
- 第二个参数是真正要传递的数据

- 传递数据
```
button1.setOnClickListener(new View.OnClickListener() {
      	@Override
        public void onClick(View v) {
	    String data = "xxx"; 
		Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
		Intent.putExtra("extra_data", data);
		startActivity(intent);
	}
});
```

- 传递字符串 getStringExtra()
- 传递整型 getIntExtra()
- 传递布尔型 getBooleanExtra()
- 取出数据
```
public class 活动名 extends AppCompatActivity {

  @Override       //代表重写父类函数
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
	setContentView(R.layout.second_layout);
	Intent intent = get Intent();
	String data = intent.getStringExtra("extra_data");
	Log.d("SecondActivity", data);
  }

}
```

#### 返回数据给上一个活动

```
 button1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
               Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
	       startActivityForResult(intent, 1);	//此处的1是请求码

            }
 });
```
- startActivityForResult()方法接收两个参数，第一个参数还是Intent，第二个参数是请求码，用于在回调中判断数据来源
- setResult()方法接收两个参数，第一个参数用于向上一个活动返回处理结果
- 一般只使用RESULT_OK或RESULT_CANCELED这两个值
- 第二参数将带有数据的Intent传递回去
- 使用startActivityForResult()方法启动下一个活动的时候，在下一个活动被销毁的时候
- 会回调上一个活动的onActivityResult()方法，需要在上一个活动中重写这个方法来得到返回的数据
- onActivityResult()方法有三个参数
1. 第一个参数requestCode，即启动活动的请求码
2. 第二个参数resultCode，即下一个活动返回的处理结果
3. 第三个参数data，即携带着数据的Intent

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case 1:
                if (resultCode == RESULT_OK) {
                    String returnedData = data.getStringExtra("data_return");
                    Log.d("FirstActivity", returnedData);
                }
                break;
            default:
        }
    }
```

- 需要改变Back键的返回函数效果时可以在活动中重写onBackPressed()方法

#### 活动的生命周期
- 返回栈
- 活动状态
1. 运行状态
- 活动位于栈顶
2. 暂停状态
- 活动不位于栈顶，但依然可见
3. 停止状态
- 活动不位于栈顶，且不可见
4. 销毁状态
- 活动从返回栈中移除

#### 活动的生存期
- Activity类中定义了7个回调方法，覆盖了活动生命周期的每一个环节
1. onCreate()
- 在活动第一次被创建时调用，完成活动的初始化操作，如加载布局，绑定事件等
2. onStart()
- 活动由不可见变为可见时调用
3. onResume()
- 在活动准备好与用户进行交互时调用，此时活动位于返回栈的栈顶，且处于运行状态
4. onPause()
- 在系统准备去启动或者恢复另一个活动的时候调用
5. onStop()
- 在活动完全不可见的时候调用
6. onDestroy()
- 在活动被销毁前调用，使活动变为销毁状态
7. onRestart()
- 在活动由停止状态变为运行状态之前调用，活动被重新启动

#### 3种生存期
1. 完整生存期
- onCreate和onDestroy之间
2. 可见生存期
- onStart和onStop之间
3. 前台生存期
- onResume和onPause之间
![生命周期示意图](http://images2015.cnblogs.com/blog/15207/201512/15207-20151230134402026-2097191680.jpg)




### 第九章 使用网络技术

- 访问网络时需要声明权限，需要修改AndroidManifest.xml文件，并加入权限声明
- `<uses-permission android:name="android.permission.INTERNET>`

```
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

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

### 第九章 使用网络技术

- 访问网络时需要声明权限，需要修改AndroidManifest.xml文件，并加入权限声明
- `<uses-permission android:name="android.permission.INTERNET>`

```
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

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
- 在活动完全不可见的时候调用，如果启动的新活动是一个对话框式活动
- 那么onPause()会调用，而onStop不调用
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

#### 对话框窗口的设置
- 在AndroidManifest.xml中的<activity>标签中加入
`android:theme="@android:style/Theme.Dialog"`

#### 保存临时数据
- onSaveInstanceState()回调方法
- putString类似putStringExtra
```
@Override
protected void onSaveInstanceState(Bundle outState) {
	super.onSaveInstanceState(outState);
	String tempData = "Something you just typed";
	outState.putString("data_key", tempData)
}
```

#### 日志函数log
- Log.d(tag, msg)
1. tag指当前类名，对打印信息进行过滤
2. msg指想打印的具体内容



#### 活动的启动模式
1. standard
2. singleTop
3. singleTask
4. singleInstance
- 通过在AndroidManifest.xml中给<activity>标签指定android:launchMode属性来指定启动模式

#### standard
- 默认启动模式
- 每次启动活动时会创建一个新的活动实例

#### singleTop
- 启动活动时发现返回栈栈顶已经是该活动，则认为可以直接使用它，不会再新建活动实例

#### singleTask
- 每次启动活动时系统首先在返回栈中检查是否存在该活动的实例，若已存在则直接使用该
- 实例，并且将此活动上的所有活动统统出栈

#### singleInstance
- 启动该活动时会新建一个活动栈，此活动栈与其他活动无关

#### 知晓现在运行到哪一个活动
- 加入一个BaseActivity
- 让它成为项目中所有活动的父类
- 并且在onCreate()方法中加入Log.d("BaseActivity", getClass().getSimpleName());

#### 退出所有活动
- 加入一个活动管理器ActivityCollector类
- 杀掉进程的方法
`android.os.Precess.killProcess(android.os.Process.myPid());`

#### 启动活动的最佳写法
- 尽量将一个活动里所执行的东西封装成一个函数，方便其他函数调用

```
public static void actionStart(Context context, String data1, String data2) {
	Intent intent = new Intent(context, SecondActivity.class);
	intent.putExtra("param1", data1);
	intent.putExtra("param2", data2);
	context.startActivity(intent);
}
```

### 第三章 UI开发
#### TextView
1. android:id
- 控件标识符
2. android:layout_width
- 指定宽度
3. android:layout_height
- 指定高度
4. android:text
- 指定文本内容
5. android:gravity
- 指定文字对齐方式
- 可选top, bottom, left, right, center
6. android:textSize
- 指定文字大小，以sp为单位
7. android:textColor
- 指定文字颜色
- 需要用到时查阅文档即可！！！

#### Button
1. android:textAllCaps="false"
- 可以禁用buttonID里的字母自动大写转换
2. 可以通过接口的写法实现对按钮点击事件的监听

#### EditText
1. android:hint
- 在输入框中加入一些默认的提示性文字
2. android:maxLines
- 指定最大行数，如果输入内容超过这个值时，文本会向上滚动，而EditText不会再拉伸

#### ImageView
1. android:src
- 指定图片
2. imageView.setImageResource(R.drawable.新图片的图片名)
- 更新图片

#### AlertDialog
```
AlertDialog.Builder dialog = new AlertDialog.Builder(活动名.this);
dialog.setTitle("对话框名");
dialog.setMessage("显示内容");
dialog.setCancelable(false);	//设置不可取消，代表无法通过back键返回
dialog.setPositiveButton("OK", new DialogInterface.
	OnClickListener() {
	@Override
	public void onClick(DialogInterface dialog, int which) {
	}
});
dialog.setNegativeButton("CANCEL", new DialogInterface.
	OnClickListener() {
	@Override
	public void onClick(DialogInterface dialog, int which) {
	}
});
dialog.show();
```

#### ProgressBar
- 用于显示进度条
1. android:visibility
- 三种可选值 View.GONE, View.VISIBLE, View.INVISIBLE
2. getVisibility()方法获取可视化值
3. android:max
- 给进度条设置最大值，可以再代码中动态地改变进度条的进度
4. style属性决定不同的样式

#### ProgressDialog
- 与AlertDialog对话框类似

#### 四种基本布局
1. 线性布局 LinearLayout
- 此布局会将它包含的控件在线性方向上一次排列
- android:layout_weight将所有控件指定的layout_weight值相加，得到总值再分配
- 此时需将所控制的宽度或者高度 置为 0dp

2. 相对布局 RelativeLayout
- 此布局方式以相对定位的方式使控件可以出现在布局的任何位置
- 以下四个是通过父布局定位的属性 后接true/false
- android:layout_alighParentLeft
- android:layout_alighParentRight
- android:layout_alighParentTop
- android:layout_alighParentBottom
- android:layout_centerInParent

- 以下四个是通过控件定位的属性 后接="@id/控件id"
- android:layout_below
- android:layout_above
- android:layout_toRightOf
- android:layout_toLeftOf

3. 帧布局 FrameLayout
- 所有的控件都会默认摆放在布局的左上角
- android:layout_gravity="right/left"
- 可以指定控件在布局中居左对齐或者居右对齐

4. 百分比布局 PrecentFrameLayout
- android:layout_widthPercent
- android:layout_heightPercent
- android:layout_gravity
```
<android.support.percent.PercentFrameLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto"
	>
</android.support.percent.PercentFrameLayout>
```

### 第九章 使用网络技术

- 访问网络时需要声明权限，需要修改AndroidManifest.xml文件，并加入权限声明
- `<uses-permission android:name="android.permission.INTERNET>`

```
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

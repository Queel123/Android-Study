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
'''
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
'''

1. android:text指定了元素中显示的文字内容
2. match_parent代表当前元素和父元素一样宽
3. wrap_content表示当前元素的高度刚好包含里面的内容
### 第九章 使用网络技术

- 访问网络时需要声明权限，需要修改AndroidManifest.xml文件，并加入权限声明
- `<uses-permission android:name="android.permission.INTERNET>`

```
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

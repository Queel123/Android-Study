# Android-Study
Some tips need to be noticed when studying Android

Android 的四大组件分别是活动，服务，广播接收器，内容提供器

### 第二章 探究活动
#### 创建活动时几个可勾选的按钮代表的意思
1. Generate Layout File会自动为活动创建一个对应的布局文件
2. Laucher Activity会自动将此活动设置为当前项目的主活动
3. Backwards Compatibility会自动为项目启用向下兼容的模式（一般需要勾选）




### 第九章 使用网络技术

- 访问网络时需要声明权限，需要修改AndroidManifest.xml文件，并加入权限声明
- `<uses-permission android:name="android.permission.INTERNET>`

```
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

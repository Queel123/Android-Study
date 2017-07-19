# Android-Study
Some tips need to be noticed when studying Android

第九章 使用网络技术

访问网络时需要声明权限，需要修改AndroidManifest.xml文件，并加入权限声明
`<uses-permission android:name="android.permission.INTERNET>`

```
URL url = new URL("http://www.baidu.com");
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

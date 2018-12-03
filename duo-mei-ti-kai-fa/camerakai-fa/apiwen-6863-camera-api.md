# 原文地址

[
Camera API](https://developer.android.com/guide/topics/media/camera)

#考虑

 相机需求: 如果相机设备对你的应用来说是必须的,在清单文件中声明
 
 简单拍照还是自定义相机?
 
 前台服务需求:只有运行在前台的应用才能访问相机 
 
 存储位置: 只在应用内可见还是可被其他应用(如相册和其他媒体应用)访问
 
 # 基础
 
 android.hardware.camera2 : 首选API
 
 Camera: 已废弃的API
 
 SurfaceView: 展示相机预览
 
 MediaRecorder: 相机用这个来录制视频
 
 Intent: 在不直接访问Camera对象的情况拍照或录像.
 
 # 清单文件声明
 
 相机权限:
 


```
 <uses-permission android:name="android.permission.CAMERA" />
```

相机功能:



```
<uses-feature android:name="android.hardware.camera" />
```

存储权限:


```

<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

视频录制:



```
<uses-permission android:name="android.permission.RECORD_AUDIO" />
```

定位权限(如果需要在相片上标记位置信息):


```
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
...
<!-- Needed only if your app targets Android 5.0 (API level 21) or higher. -->
<uses-feature android:name="android.hardware.location.gps" />
```

# 构建相机应用

**基本步骤:**

检测和访问相机:

创建预览类:

构造预览布局:

设置拍照监听:

捕获和保存文件:

释放相机资源:









 
# 原文地址

[Record Vedios](https://developer.android.com/training/camera/videobasics)

# 声明相机功能

<manifest ... >
    <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
    ...
</manifest>

如果requird设为false,则google play不限制设备必须具有相机功能.需要开发者自己调用hasSystemFeature(PackageManager.FEATURE_CAMERA)方法,如果没有相机则禁用app内相机相关功能.


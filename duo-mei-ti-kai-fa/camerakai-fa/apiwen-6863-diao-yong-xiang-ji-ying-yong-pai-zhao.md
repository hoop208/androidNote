# 声明需要相机功能
    
限制在Google Play只有具备相机功能的设备可见

```
<manifest ... >
    <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
    ...
</manifest>
```

# 使用相机应用拍照

在android里把动作分发给其他应用的方式是通过描述动作的intent实现:

```
static final int REQUEST_IMAGE_CAPTURE = 1;

private void dispatchTakePictureIntent() {
    Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
    if (takePictureIntent.resolveActivity(getPackageManager()) != null) {
        startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE);
    }
}
```


# 获取缩略图

# 按原图保存图片

# 添加图片到相册

# 解压压缩后的图片






















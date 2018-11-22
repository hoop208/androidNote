# 原文地址

[Record Vedios](https://developer.android.com/training/camera/videobasics)

# 声明相机功能



```
<manifest ... >
    <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
    ...
</manifest>
```



如果requird设为false,则google play不限制设备必须具有相机功能.需要开发者自己调用hasSystemFeature(PackageManager.FEATURE_CAMERA)方法,如果没有相机则禁用app内相机相关功能.

# 使用已有的相机应用录制视频




```
static final int REQUEST_VIDEO_CAPTURE = 1;

private void dispatchTakeVideoIntent() {
    Intent takeVideoIntent = new Intent(MediaStore.ACTION_VIDEO_CAPTURE);
    if (takeVideoIntent.resolveActivity(getPackageManager()) != null) {
        startActivityForResult(takeVideoIntent, REQUEST_VIDEO_CAPTURE);
    }
}
```

# 播放录制的视频



```
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent intent) {
    if (requestCode == REQUEST_VIDEO_CAPTURE && resultCode == RESULT_OK) {
        Uri videoUri = intent.getData();
        mVideoView.setVideoURI(videoUri);
    }
}
```





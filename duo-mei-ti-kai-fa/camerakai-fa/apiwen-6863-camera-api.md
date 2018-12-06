# 原文地址

[Camera API](https://developer.android.com/guide/topics/media/camera)

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

### 检测和访问相机:

如果没有在清单文件中声明相机需求,那么在应用中用到的地方需要做运行时检查:


```

/** Check if this device has a camera */
private boolean checkCameraHardware(Context context) {
    if (context.getPackageManager().hasSystemFeature(PackageManager.FEATURE_CAMERA)){
        // this device has a camera
        return true;
    } else {
        // no camera on this device
        return false;
    }
}
```

访问相机对象:



```
/** A safe way to get an instance of the Camera object. */
public static Camera getCameraInstance(){
    Camera c = null;
    try {
        c = Camera.open(); // attempt to get a Camera instance
    }
    catch (Exception e){
        // Camera is not available (in use or does not exist)
    }
    return c; // returns null if camera is unavailable
}
```


创建预览类:



```
/** A basic Camera preview class */
public class CameraPreview extends SurfaceView implements SurfaceHolder.Callback {
    private SurfaceHolder mHolder;
    private Camera mCamera;

    public CameraPreview(Context context, Camera camera) {
        super(context);
        mCamera = camera;

        // Install a SurfaceHolder.Callback so we get notified when the
        // underlying surface is created and destroyed.
        mHolder = getHolder();
        mHolder.addCallback(this);
        // deprecated setting, but required on Android versions prior to 3.0
        mHolder.setType(SurfaceHolder.SURFACE_TYPE_PUSH_BUFFERS);
    }

    public void surfaceCreated(SurfaceHolder holder) {
        // The Surface has been created, now tell the camera where to draw the preview.
        try {
            mCamera.setPreviewDisplay(holder);
            mCamera.startPreview();
        } catch (IOException e) {
            Log.d(TAG, "Error setting camera preview: " + e.getMessage());
        }
    }

    public void surfaceDestroyed(SurfaceHolder holder) {
        // empty. Take care of releasing the Camera preview in your activity.
    }

    public void surfaceChanged(SurfaceHolder holder, int format, int w, int h) {
        // If your preview can change or rotate, take care of those events here.
        // Make sure to stop the preview before resizing or reformatting it.

        if (mHolder.getSurface() == null){
          // preview surface does not exist
          return;
        }

        // stop preview before making changes
        try {
            mCamera.stopPreview();
        } catch (Exception e){
          // ignore: tried to stop a non-existent preview
        }

        // set preview size and make any resize, rotate or
        // reformatting changes here

        // start preview with new settings
        try {
            mCamera.setPreviewDisplay(mHolder);
            mCamera.startPreview();

        } catch (Exception e){
            Log.d(TAG, "Error starting camera preview: " + e.getMessage());
        }
    }
}
```


构造预览布局:

示例布局文件:


```

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    >
  <FrameLayout
    android:id="@+id/camera_preview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:layout_weight="1"
    />

  <Button
    android:id="@+id/button_capture"
    android:text="Capture"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="center"
    />
</LinearLayout>
```

activity中的代码:



```
public class CameraActivity extends Activity {

    private Camera mCamera;
    private CameraPreview mPreview;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        // Create an instance of Camera
        mCamera = getCameraInstance();

        // Create our Preview view and set it as the content of our activity.
        mPreview = new CameraPreview(this, mCamera);
        FrameLayout preview = (FrameLayout) findViewById(R.id.camera_preview);
        preview.addView(mPreview);
    }
}
```


设置拍照监听:

**拍照:**

在Camera.PictureCallback回调中接受拍照返回的数据并写入文件:

```
private PictureCallback mPicture = new PictureCallback() {

    @Override
    public void onPictureTaken(byte[] data, Camera camera) {

        File pictureFile = getOutputMediaFile(MEDIA_TYPE_IMAGE);
        if (pictureFile == null){
            Log.d(TAG, "Error creating media file, check storage permissions");
            return;
        }

        try {
            FileOutputStream fos = new FileOutputStream(pictureFile);
            fos.write(data);
            fos.close();
        } catch (FileNotFoundException e) {
            Log.d(TAG, "File not found: " + e.getMessage());
        } catch (IOException e) {
            Log.d(TAG, "Error accessing file: " + e.getMessage());
        }
    }
};
```

点击按钮调用拍照方法:


```
// Add a listener to the Capture button
Button captureButton = (Button) findViewById(R.id.button_capture);
captureButton.setOnClickListener(
    new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            // get an image from the camera
            mCamera.takePicture(null, null, mPicture);
        }
    }
);
```

**录制视频:**

和拍照不同,录制视频必须按照指定顺序调用方法.

1.打开相机     Camera.open()

2.连接预览    Camera.setPreviewDisplay().

3.开始预览    Camera.startPreview()

4.开始视频录制
    
    a.解锁相机    Camera.unlock()
    b.配置MediaRecorde
        b1.setCamera()
        b2.setAudioSource()
        b3.setVideoSource()
        b4.设置输出和编码格式
            b41.setOutputFormat()
            b42.setAudioEncoder()
            b43.setVideoEncoder()
        b5.setOutputFile()
        b6.setPreviewDisplay()
    c.准备媒体录制器    MediaRecorder.prepare().
    d.开始录制    Start MediaRecorder

5.停止视频录制

    a.停止录制    MediaRecorder.stop().
    b.充值媒体录制器    MediaRecorder.reset().
    c.释放媒体录制器    MediaRecorder.release().
    d.锁住相机    Camera.lock()(android4.0以后不再需要调用,除非MediaRecorder.prepare()方法调用失败)
    
6.停止预览    Camera.stopPreview().

7.释放相机    Camera.release().

捕获和保存文件:

释放相机资源:









 
# 打开相机对象

获取相机示例是直接操作相机的第一步.如果相机被别的应用占用,调用Camera.open()方法会抛出异常.所以我们把整个代码块包裹在try catch中.



```
private boolean safeCameraOpen(int id) {
    boolean qOpened = false;

    try {
        releaseCameraAndPreview();
        mCamera = Camera.open(id);
        qOpened = (mCamera != null);
    } catch (Exception e) {
        Log.e(getString(R.string.app_name), "failed to open Camera");
        e.printStackTrace();
    }

    return qOpened;
}

private void releaseCameraAndPreview() {
    mPreview.setCamera(null);
    if (mCamera != null) {
        mCamera.release();
        mCamera = null;
    }
}
```

# 创建相机预览

Preview类:



```
class Preview extends ViewGroup implements SurfaceHolder.Callback {

    SurfaceView mSurfaceView;
    SurfaceHolder mHolder;

    Preview(Context context) {
        super(context);

        mSurfaceView = new SurfaceView(context);
        addView(mSurfaceView);

        // Install a SurfaceHolder.Callback so we get notified when the
        // underlying surface is created and destroyed.
        mHolder = mSurfaceView.getHolder();
        mHolder.addCallback(this);
        mHolder.setType(SurfaceHolder.SURFACE_TYPE_PUSH_BUFFERS);
    }
...
}
```

开启预览:

相机示例和它的相关预览必须按顺序创建.在用户对相机做了修改后,预览必须在surfaceChanged()回调中重启.


```
public void setCamera(Camera camera) {
    if (mCamera == camera) { return; }

    stopPreviewAndFreeCamera();

    mCamera = camera;

    if (mCamera != null) {
        List<Size> localSizes = mCamera.getParameters().getSupportedPreviewSizes();
        mSupportedPreviewSizes = localSizes;
        requestLayout();

        try {
            mCamera.setPreviewDisplay(mHolder);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Important: Call startPreview() to start updating the preview
        // surface. Preview must be started before you can take a picture.
        mCamera.startPreview();
    }
}
```

#修改相机设置 

相机设置可以修改拍照的方式,从缩放级别到曝光补偿等.

```
@Override
public void surfaceChanged(SurfaceHolder holder, int format, int w, int h) {
    // Now that the size is known, set up the camera parameters and begin
    // the preview.
    Camera.Parameters parameters = mCamera.getParameters();
    parameters.setPreviewSize(mPreviewSize.width, mPreviewSize.height);
    requestLayout();
    mCamera.setParameters(parameters);

    // Important: Call startPreview() to start updating the preview surface.
    // Preview must be started before you can take a picture.
    mCamera.startPreview();
}
```

# 设置相机方向

setCameraDisplayOrientation()

#拍照 

在预览启动后使用Camera.takePicture()方法拍照.可以创建Camera.PictureCallback和Camera.ShutterCallback对象作为参数传入这个方法.

#重启预览

每次拍照之后必须重启预览才能再次拍照.

```
 @Override
public void onClick(View v) {
    switch(mPreviewState) {
    case K_STATE_FROZEN:
        mCamera.startPreview();
        mPreviewState = K_STATE_PREVIEW;
        break;

    default:
        mCamera.takePicture( null, rawCallback, null);
        mPreviewState = K_STATE_BUSY;
    } // switch
    shutterBtnConfig();
} 
```

# 停止预览并释放相机资源

一旦你的应用使用完相机,就应该马上释放资源.尤其要释放Camera对象,否则可能导致其它应用崩溃,甚至是你自己应用的新的实例.


```

@Override
public void surfaceDestroyed(SurfaceHolder holder) {
    // Surface will be destroyed when we return, so stop the preview.
    if (mCamera != null) {
        // Call stopPreview() to stop updating the preview surface.
        mCamera.stopPreview();
    }
}

/**
 * When this function returns, mCamera will be null.
 */
private void stopPreviewAndFreeCamera() {

    if (mCamera != null) {
        // Call stopPreview() to stop updating the preview surface.
        mCamera.stopPreview();

        // Important: Call release() to release the camera for use by other
        // applications. Applications should release the camera immediately
        // during onPause() and re-open() it during onResume()).
        mCamera.release();

        mCamera = null;
    }
}

```

初始化相机的时候都需要先停止预览.







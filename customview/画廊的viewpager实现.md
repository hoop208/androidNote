# 目标效果 #

![](imgs/1.gif)

# 实现思路 #

1.一页现实多个

    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:clipChildren="false"
    android:background="#CC000000"
    tools:context=".ViewpagerActivity">

    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="200dp"
        android:layout_height="480dp"
        android:layout_centerInParent="true"
        android:clipChildren="false"
        tools:background="#ff0000" />

	</RelativeLayout>

让viewpager不要充满屏幕,在viewpager和它的父容器设置属性android:clipChildren="false"

2.页面效果

    /**
     * 切换效果
     */
    private class GalleryTransformer implements ViewPager.PageTransformer {

        private float MIN_SCALE = 0.68f;

        @Override
        public void transformPage(@NonNull View page, float position) {
            Log.d(TAG, "transformPage=" + position);
            int maxWitdh = page.getWidth();
            Log.d(TAG, "pageWidth=" + maxWitdh);
            //阈值限制
            position = Math.min(position, 1);
            position = Math.max(position, -1);
            float scaleFactor = MIN_SCALE + (1 - MIN_SCALE) * (1 - Math.abs(position));
            //缩放
            page.setScaleX(scaleFactor);
            page.setScaleY(scaleFactor);
            //旋转
            page.setRotationY(-20 * position);
            //位移补充(抵消缩放造成的间隔)
            float translateFactor = (1 - scaleFactor) / 2;
            translateFactor *= position < 0 ? 1 : -1;
            page.setTranslationX(maxWitdh * translateFactor);
        }
    }

3.图片倒影

    /**
     * 生成带背影的图片
     * <p>思路<br/>
     * 第一：利用Matrix矩阵来实现图片的旋转。<br>
     * 第二：利用旋转后的图片创建一个位图reflectionImage，宽度不变，高度是原始图片的一般（自己可以随意设置），就是效果图中倒影的大小<br>
     * 第三：创建一个能包含原始图片和倒影图片的位图finalReflection（宽度一样，高度是原始图片的高度加上倒影图片的高度）<br>
     * 第四：用刚创建的位图finalReflection创建一个画布<br>
     * 第五：把原始图片和倒影图片添加到画布上去<br/>
     * 第六：创建线性渐变LinearGradient对象，实现倒影图片所在的区域是渐变效果</p>
     *
     * @param originalImage 原图
     * @param ratio         背影占原图的比例
     * @return
     */
    public static Bitmap createReflectedImage(Bitmap originalImage, @FloatRange(from = 0, to = 1) float ratio) {
        int width = originalImage.getWidth();
        int height = originalImage.getHeight();
        Matrix matrix = new Matrix();
        // 实现图片翻转90度
        matrix.preScale(1, -1);
        // 创建倒影图片（是原始图片的一半大小）
        Bitmap reflectionImage = Bitmap.createBitmap(originalImage, 0, (int) (height * (1 - ratio)), width, (int) (height * ratio), matrix, false);
        // 创建总图片（原图片 + 倒影图片）
        Bitmap finalReflection = Bitmap.createBitmap(width, (int) (height + height * ratio), Bitmap.Config.ARGB_8888);
        // 创建画布
        Canvas canvas = new Canvas(finalReflection);
        canvas.drawBitmap(originalImage, 0, 0, null);
        //把倒影图片画到画布上
        canvas.drawBitmap(reflectionImage, 0, height + 1, null);
        Paint shaderPaint = new Paint();
        //创建线性渐变LinearGradient对象
        LinearGradient shader = new LinearGradient(0, originalImage.getHeight(), 0, finalReflection.getHeight() + 1, 0x70ffffff,
                0x00ffffff, Shader.TileMode.MIRROR);
        shaderPaint.setShader(shader);
        shaderPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_IN));
        //画布画出反转图片大小区域，然后把渐变效果加到其中，就出现了图片的倒影效果。
        canvas.drawRect(0, height + 1, width, finalReflection.getHeight(), shaderPaint);
        return finalReflection;
    }
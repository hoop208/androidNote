# 绘制经过多点的曲线 #
目标效果:
![](/imgs/chart/cubic.png)

实现思路:
原理:

第一段和最后一段用二阶贝塞尔曲线,中间的n段用三阶贝塞尔曲线

如何取控制点:

计算线段的中点-->计算中点的中点-->两个中点连一条线段,平移到中点的中点已原始点重合,此时两端的坐标就是控制点

1.绘制点

2.绘制中点

3.绘制中点的中点

4.绘制控制点

5.绘制贝塞尔曲线

添加了辅助点的效果图:
黑色:原始点
中点:蓝色
中点的中点:黄色
控制点:绿色

![](/imgs/chart/cubic2.png)

    /**
	 * 自定义控件:绘制经过多点的曲线
	 * <p>
	 * 实现思路:    第一段和最后一段曲线为二阶贝塞尔，中间N段都为三阶贝塞尔曲线。
	 * </p>
	 * Created by 曾丽 on 2017/11/14.
	 */
	
	public class CurveView extends View {

    public static final String TAG = "CurveView";

    public static final int LINE_WIDTH = 3;
    public static final int POINT_WIDTH = 10;

    private Context mContext;

    /**
     * 要穿越过的点的集合
     */
    private List<Point> mPoints = new ArrayList<>();
    /**
     * 中点的集合
     */
    private List<Point> mMidPoints = new ArrayList<>();
    /**
     * 中点的中点的集合
     */
    private List<Point> mMidMidPoints = new ArrayList<>();
    /**
     * 控制点的集合
     */
    private List<Point> mControlPoints = new ArrayList<>();

    private int mWidth;
    private int mHeight;

    private Paint mPaint;
    private Path mPath;

    public CurveView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init(context);

    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        mWidth = w;
        mHeight = h;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        //原点移动到左下角
        canvas.translate(0, mHeight);
        //反转y轴
        canvas.scale(1, -1);
        drawPoints(canvas);
        drawPoliLine(canvas);
        drawMidPoints(canvas);
        drawMidMidPoints(canvas);
        drawControlPoints(canvas);
        drawBezier(canvas);
    }

    private void init(Context context) {
        mPaint = new Paint();
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setAntiAlias(true);
        mPaint.setDither(true);
        mPath = new Path();
        mContext = context;
    }

    /**
     * 绘制要经过的点
     *
     * @param canvas
     */
    private void drawPoints(Canvas canvas) {
        mPaint.setStrokeWidth(POINT_WIDTH);
        mPaint.setColor(Color.BLACK);
        for (int i = 0; i < mPoints.size(); i++) {
            Point point = mPoints.get(i);
            Log.d(TAG, "x=" + point.x + ";y=" + point.y);
            canvas.drawPoint(point.x, point.y, mPaint);
        }
    }

    /**
     * 绘制经过原始点的折线
     *
     * @param canvas
     */
    private void drawPoliLine(Canvas canvas) {
        mPaint.setStrokeWidth(LINE_WIDTH);
        mPaint.setColor(Color.RED);
        mPath.reset();
        for (int i = 0; i < mPoints.size(); i++) {
            Point point = mPoints.get(i);
            if (i == 0) {
                mPath.moveTo(point.x, point.y);
            } else {
                mPath.lineTo(point.x, point.y);
            }
        }
        canvas.drawPath(mPath, mPaint);
    }

    /**
     * 绘制中点
     *
     * @param canvas
     */
    private void drawMidPoints(Canvas canvas) {
        mPaint.setStrokeWidth(POINT_WIDTH);
        mPaint.setColor(Color.BLUE);
        for (int i = 0; i < mMidPoints.size(); i++) {
            Point point = mMidPoints.get(i);
            canvas.drawPoint(point.x, point.y, mPaint);
        }
    }

    /**
     * 绘制中点的中点
     *
     * @param canvas
     */
    private void drawMidMidPoints(Canvas canvas) {
        mPaint.setStrokeWidth(POINT_WIDTH);
        mPaint.setColor(Color.YELLOW);
        for (int i = 0; i < mMidMidPoints.size(); i++) {
            Point point = mMidMidPoints.get(i);
            canvas.drawPoint(point.x, point.y, mPaint);
        }
    }

    /**
     * 绘制控制点
     *
     * @param canvas
     */
    private void drawControlPoints(Canvas canvas) {
        mPaint.setStrokeWidth(POINT_WIDTH);
        mPaint.setColor(Color.GREEN);
        for (int i = 0; i < mControlPoints.size(); i++) {
            Point point = mControlPoints.get(i);
            canvas.drawPoint(point.x, point.y, mPaint);
        }
    }

    /**
     * 绘制贝塞尔曲线
     *
     * @param canvas
     */
    private void drawBezier(Canvas canvas) {
        mPath.reset();
        mPaint.setStrokeWidth(LINE_WIDTH);
        mPaint.setColor(Color.BLACK);
        for (int i = 0; i < mPoints.size(); i++) {
            Point point = mPoints.get(i);
            if (i == 0) {
                //第一个点
                mPath.moveTo(point.x, point.y);
            } else if (i == 1 || i == mPoints.size() - 1) {
                //第一段和最后一段曲线用二阶贝塞尔
                Point control = i == 1 ? mControlPoints.get(0) : mControlPoints.get(mControlPoints.size() - 1);
                mPath.quadTo(control.x, control.y, point.x, point.y);
            } else {
                //中间的n端曲线用三阶贝塞尔
                Point control1 = mControlPoints.get(1+2*(i-2));
                Point control2 = mControlPoints.get(2*(i-1));
                mPath.cubicTo(control1.x,control1.y,control2.x,control2.y,point.x,point.y);
            }
        }
        canvas.drawPath(mPath,mPaint);
    }

    /**
     * 设置要绘制的点
     *
     * @param points
     */
    public void setPoints(List<Point> points) {
        this.mPoints = points;
        initMidPoints();
        initMidMidPoints();
        initControlPoints();
        invalidate();
    }

    /**
     * 初始化中点的集合
     */
    private void initMidPoints() {
        for (int i = 0; i < mPoints.size() - 1; i++) {
            Point pointPre = mPoints.get(i);
            Point pointPost = mPoints.get(i + 1);
            int x = (pointPre.x + pointPost.x) / 2;
            int y = (pointPre.y + pointPost.y) / 2;
            mMidPoints.add(new Point(x, y));
        }
    }

    /**
     * 初始化中点的中点集合
     */
    private void initMidMidPoints() {
        for (int i = 0; i < mMidPoints.size() - 1; i++) {
            Point pointPre = mMidPoints.get(i);
            Point pointPost = mMidPoints.get(i + 1);
            int x = (pointPre.x + pointPost.x) / 2;
            int y = (pointPre.y + pointPost.y) / 2;
            mMidMidPoints.add(new Point(x, y));
        }
    }

    /**
     * 初始化控制点的集合
     * <p>
     * 两个中点和中点的中点连一条直线,平移到对应原始点使中点的中点和原始点重合,这时候两边的中点坐标就是控制点
     * </p>
     */
    private void initControlPoints() {
        for (int i = 0; i < mMidMidPoints.size(); i++) {
            //中点的中点
            Point midmid = mMidMidPoints.get(i);
            //前一个中点
            Point midPre = mMidPoints.get(i);
            //后一个中点
            Point midPost = mMidPoints.get(i + 1);
            //平移的目标原始点
            Point point = mPoints.get(i + 1);
            //一个中点的中点-->对应两个控制点
            mControlPoints.add(new Point(midPre.x + point.x - midmid.x, midPre.y + point.y - midmid.y));
            mControlPoints.add(new Point(midPost.x + point.x - midmid.x, midPost.y + point.y - midmid.y));
        }
    }

	}
# 目标效果 #

![](imgs/drawer.gif)

# 实现思路 #
1.view的绘制:量测菜单控件和内容控件大小-->菜单布局在父容器左侧,内容部分布局在父容器位置
    
	@Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        //设置父容器的大小
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);

        setMeasuredDimension(widthSize,heightSize);

        //设置菜单的大小
        mLeftMenuView = getChildAt(1);
        LayoutParams menuLayoutParams = mLeftMenuView.getLayoutParams();
        int menuWidthSpec = getChildMeasureSpec(widthMeasureSpec,mMinDrawerMargin,menuLayoutParams.width);
        int menuHeightSpec = getChildMeasureSpec(heightMeasureSpec,0,menuLayoutParams.height);
        mLeftMenuView.measure(menuWidthSpec,menuHeightSpec);

        //设置内容部分的大小
        mContentView = getChildAt(0);
        LayoutParams contentLayoutParams = mContentView.getLayoutParams();
        int contentWidthSpec = MeasureSpec.makeMeasureSpec(widthSize,MeasureSpec.EXACTLY);
        int contentHeightSpec = MeasureSpec.makeMeasureSpec(heightSize,MeasureSpec.EXACTLY);
        mContentView.measure(contentWidthSpec,contentHeightSpec);
    }

    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        //布局内容
        mContentView.layout(left,top,right,bottom);

        //布局菜单
        int menuLeft = left-mLeftMenuView.getMeasuredWidth();
        int menuTop = top;
        int menuRight = left;
        int menuBottom = bottom;
        mLeftMenuView.layout(menuLeft,menuTop,menuRight,menuBottom);
    }

2.用ItemTouchHelper实现边界拖动(菜单默认不可见)

	 mHelper = ViewDragHelper.create(this, new ViewDragHelper.Callback() {
            @Override
            public boolean tryCaptureView(View child, int pointerId) {
                return child == mLeftMenuView;
            }

            @Override
            public int clampViewPositionHorizontal(View child, int left, int dx) {
                int newLeft = Math.min(Math.max(-child.getWidth(), left), 0);
                return newLeft;
            }

            @Override
            public void onEdgeDragStarted(int edgeFlags, int pointerId) {
                mHelper.captureChildView(mLeftMenuView, pointerId);
            }

            @Override
            public int getViewHorizontalDragRange(View child) {
                return child == mLeftMenuView ? child.getWidth() : 0;
            }

            @Override
            public void onViewReleased(View releasedChild, float xvel, float yvel) {
                int left = Math.abs(releasedChild.getLeft())>releasedChild.getWidth()/2?-releasedChild.getWidth():0;
                mHelper.settleCapturedViewAt(left,0);
                invalidate();
            }
        });
        //设置边界拖动
        mHelper.setEdgeTrackingEnabled(ViewDragHelper.EDGE_LEFT);

	@Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        return mHelper.shouldInterceptTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        mHelper.processTouchEvent(event);
        return true;
    }

3.允许拖动菜单
	
	mHelper = ViewDragHelper.create(this, new ViewDragHelper.Callback() {
            @Override
            public boolean tryCaptureView(View child, int pointerId) {
                return child == mLeftMenuView;
            }

            @Override
            public int clampViewPositionHorizontal(View child, int left, int dx) {
                int newLeft = Math.min(Math.max(-child.getWidth(), left), 0);
                return newLeft;
            }

            @Override
            public int getViewHorizontalDragRange(View child) {
                return child == mLeftMenuView ? child.getWidth() : 0;
            }

            
        });

4.释放的复位处理:

	mHelper = ViewDragHelper.create(this, new ViewDragHelper.Callback() {
            @Override
            public void onViewReleased(View releasedChild, float xvel, float yvel) {
                int left = Math.abs(releasedChild.getLeft())>releasedChild.getWidth()/2?-releasedChild.getWidth():0;
                mHelper.settleCapturedViewAt(left,0);
                invalidate();
            }
        });

	@Override
    public void computeScroll() {
        if(mHelper.continueSettling(true)){
            invalidate();
        }
    }
# 使用

# 源码
[SnapHelper源码深度解析](http://www.jcodecraeer.com/a/anzhuokaifa/2018/1109/12472.html)

# 自定义靠左对齐的StartSnapHelper

目标效果:

![](/imgs/startsnap.gif)

实现:

1.定义StartSnapHelper继承自LinearSnapHelper

2.复写calculateDistanceToFinalSnap关于滚动距离的计算



```
@Override
    public int[] calculateDistanceToFinalSnap(@NonNull RecyclerView.LayoutManager layoutManager, @NonNull View targetView) {
        //计算要滚动的距离
        int[] out = new int[2];
        if (layoutManager.canScrollHorizontally()) {
            out[0] = this.distanceToStart(layoutManager, targetView, this.getHorizontalHelper(layoutManager));
        } else {
            out[0] = 0;
        }

        if (layoutManager.canScrollVertically()) {
            out[1] = this.distanceToStart(layoutManager, targetView, this.getVerticalHelper(layoutManager));
        } else {
            out[1] = 0;
        }

        return out;
    }
    
    private int distanceToStart(@NonNull RecyclerView.LayoutManager layoutManager, @NonNull View targetView, OrientationHelper helper) {
        //滚动距离=目标view到父容器左边的距离
        return helper.getDecoratedStart(targetView) - helper.getStartAfterPadding();
    }
```

3.复写findSnapView方法,关于目标view的捕获



```
 @Override
    public View findSnapView(RecyclerView.LayoutManager layoutManager) {
        //捕获目标view
        if (layoutManager instanceof LinearLayoutManager) {
            if (layoutManager.canScrollHorizontally()) {
                return findStartView(layoutManager, getHorizontalHelper(layoutManager));
            } else {
                return findStartView(layoutManager, getVerticalHelper(layoutManager));
            }
        }
        return super.findSnapView(layoutManager);
    }

    private View findStartView(RecyclerView.LayoutManager layoutManager, OrientationHelper helper) {
        //遍历返回距离父容器左边最近的item
        int childCount = layoutManager.getChildCount();
        if (childCount == 0) {
            return null;
        } else {
            View closestChild = null;
            int start = helper.getStartAfterPadding();
            int absClosest = 2147483647;

            for (int i = 0; i < childCount; ++i) {
                View child = layoutManager.getChildAt(i);
                int childStart = helper.getDecoratedStart(child);
                int absDistance = Math.abs(childStart - start);
                if (absDistance < absClosest) {
                    absClosest = absDistance;
                    closestChild = child;
                }
            }

            return closestChild;
        }
    }
```








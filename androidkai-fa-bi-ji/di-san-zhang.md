### 第三章 View 的事件体系

#### 一、**基础知识**

**坐标系**

> getX\(\),getY\(\)返回的是相对当前View 左上角的X和Y的坐标。getRawX\(\)，getRawY\(\)返回的是相对于手机屏幕左上角的X和Y的坐标。

**TouchSlop**

> 系统能够识别的被认为是滑动的最小距离。获取方式：ViewConfiguration.get\(context\).getScaledTouchSlop\(\)

**VelocityTracker**

> 用于追踪手指在滑动过程中的速度

```
VelocityTracker velocityTracker=VelocityTracker.obtain();
velocityTracker.addMovement(event);
velocityTracker.computeCurrentVelocity(1000);
float xVelocity = velocityTracker.getXVelocity();
float yVelocity = velocityTracker.getYVelocity();
velocityTracker.clear();
velocityTracker.recycle();
```

** GestureDetector**

> 手势检测，用于辅助用户的单击，长按，双击等行为。

```
//this中实现 GestureDetector.OnGestureListener 接口
GestureDetector gestureDetector=new GestureDetector(context, this);

//在 onTouchEvent中 
gestureDetector.onTouchEvent(event);
```

**Scroller **

> 弹性滑动对象 ，用于View 对象的弹性滑动。

```
 Scroller mScroller = new Scroller(context);

 //平滑滚动
 private void smoothScrollTo(int destX, int destY) {
        int scrollX = getScrollX();
        int delta = destX - scrollX;
        mScroller.startScroll(scrollX, 0, delta, 0, 1000);
        invalidate();
 }

 @Override
 public void computeScroll() {
    if (mScroller.computeScrollOffset()) {
         scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
         postInvalidate();
     }
}
```

#### **二、View 的滑动**

**1、使用scrollTo / scrollBy**

**2、使用动画**

```
ObjectAnimator.ofFloat(tragetView, "translationX", 0, 100).setDuration(1000).start();
```

**3、改变布局参数**

```
     ViewGroup.MarginLayoutParams params=(ViewGroup.MarginLayoutParams) tragetView.getLayoutParams();
        params.width+=100;
        params.leftMargin+=100;
        tragetView.requestLayout();//tragetView.setLayoutParams(params);
```

比较

> **scrollTo /scrollBy** ，适合对View 内容的滑动。
>
> **动画**，适合于没有交互的View和实现复杂的动画效果。
>
> **改变布局参数，**适合于有交互的动画

#### 三**、View 的事件分发机制**

**1、事件分发流程**

> Activity --&gt; Window --&gt;DecorView\(顶级ViewGroup\)

**2、ViewGroup 事件传递**

```
 //事件分发
 public boolean dispatchTouchEvent(MotionEvent ev)
 //事件拦截
 public boolean onInterceptTouchEvent(MotionEvent ev) 
 //事件消费
 public boolean onTouchEvent(MotionEvent event)
```

[事件传递规则参考](http://www.jianshu.com/p/e75dd6fba1b7)![](/assets/import.png)

#### 四**、View 的滑动冲突**

**1、滑动冲突场景**

> &lt;1&gt;、外部滑动与内部滑动的方向不一致
>
> &lt;2&gt;、外部滑动与内部滑动的方向一致
>
> &lt;3&gt;、上面两种场景的同时嵌套

**1、滑动冲突解决**

> 对于第一种情形可以根据 滑动方向解决，对第二，第三种情形解决方案有
>
> **1，外部拦截发 **
>
> 即在父容器\(ViewGroup\)中根据逻辑需求重写 ** onInterceptTouchEvent **方法
>
> **2，内部拦截法**
>
> 即在父容器不拦截任何事件，所有事件都传递给子元素，如果子控件需要此事件就直接消耗掉，否则就交由父容器处理。此时如要配合 **requestDisallowInterceptTouchEvent **方法。
>
>  




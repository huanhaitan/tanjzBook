### 第一章 Activity 生命周期和启动模式

#### 一，Activity** 生命周期**

![](/assets/2012050219053256.jpg)**知识要点**

> 1，onStart ,onStop 可见不可交互,
>
> 2，onResume,onPause 可见可交互（位于 前台）
>
> 3，当打开的新Activity 采用透明主题时，当前的Activity 不会回调 onStop 方法
>
> 4，当前Activity打开新Activity时，旧Activity 会先执行onPause方法然后再打开新的Activity的onCreate,onStart,onResume后再执行旧Activity的onStop。

**异常生命周期**

**what **什么是异常生命周期？

> Activity 被意外的杀死并重新创建。

**Why **为什么发生异常生命周期？

> 1，资源相关的_**系统配置**_发生改变导致Activty被杀死并重建。
>
> 如：屏幕方向，键盘类型发生改变等。
>
> 2，资源内存不足导致的_**低优先级**_的Activity被杀死并重建。
>
> Activity优先级（从高到低）：
>
> （1），前台Activity（可交互的Activity）。
>
> （2），可见不可交互的Activity（如弹出Dialog的Activity）。
>
> （3），后台Activity（如执行了onStop的Activty）。

**How** 如何解决Activity 异常情况下的 Activity状态？

> 1， 当Activity 异常杀死时，系统会调用** onSaveInstanceState\(Bundle bunlde\)**方法，可以通过 **Bundle**保存数据。在Activity 重新创建时系统会调用 **onRestoreInstanceState\(Bundle bundle\)**方法，可以通过 **Bundle** 恢复数据。（延伸：View类型 系统会自动调用同样的方法保存恢复数据）
>
> 2，系统配置发生改变的情况，可以在 **AndroidMenifest.xml** 中 对应的 **activity标签** 的中配置             **android:configChanges="orientation\|keyboardHidden\|screenSize"**， 在**旋转屏幕**或**键盘改变**时不让系统 重建Activity 并调用 onConfigurationChanged\(Configuration newConfig\) 方法，可以通过onConfigurationChanged 方法 处理 配置改变。（[系统配置参考](https://git.oschina.net/aleung/Dev_Android/issues/1)）

#### 二，Activity的启动模式（LunchMode）

**知识要点**

> 1，任务栈

1，**standard  标准模式**

2，**singleTop 栈顶复用模式**

3，**singleTask  栈内复用模式**

4，**singleInstance 单例模式**






### 第一章 Activity 生命周期和启动模式

#### 一，Activity** 生命周期**

![](/assets/2012050219053256.jpg)

**知识要点**

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

> 1，任务栈\([参考](http://blog.csdn.net/liuhe688/article/details/6761337)\)

1，standard  标准模式

> 每启动一次Activity都会创建一个新的实例。

2，singleTop 栈顶复用模式

> 如果新的Activity的实例已经位于**任务栈**的的**栈顶**，该Activity不会创建新的实例，但是会回调**onNewIntent**方法。

3，singleTask  栈内复用模式

> 如果新的Activity的实例已经存在于**任务栈**的**栈内**，该Activity不会创建新的实例，同时回调**onNewIntent**方法。并且会使**该Activity实例以上的**其他实例出栈（**clearTop**）。

4，singleInstance 单例模式

> Activity实例单独存在于一个任务栈中，Android 系统中有且只有一个该实例。

**IntentFilter 的匹配规则**

> Activity启动方式分为显示启动和隐式启动，**显示启动**需要明确的指定被启动对象的组件信息（包名，类名）。**隐式启动**需要根据IntentFilter 的过滤信息来启动。一个IntentFilter中的过滤信息有 **action**，**category**，**data。**一个Activity可以有多个intentfilter,只要Intent 能够其中一个intentfilter就可以启动Activity。

action 

> 一个Intentfilter中可以有多个action 。Intent中的action只要有一个与inentfilter中的一个action完全相同（区分大小写）就可以启动Activity。

category

> 一个Intentfilter中可以有多个category。如果被启动的Activity的intentfilter中的有category，那么Intent中的category必须与intentfilter中的每一个category相同。

data

> 一个Intentfilter中可以有多个data 。Intent中的data只要有一个与inentfilter中的一个data相同。






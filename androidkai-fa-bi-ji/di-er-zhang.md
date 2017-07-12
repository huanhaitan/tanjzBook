### 第二章 IPC 机制

> IPC 即 Inter Process Communication 进程间通信，在多个进程间进行数据交换。Android 中IPC 主要指 Binder机制，如四大组件的通信，AIDL，Messenger，ContentProvider就是采用Binder机制实现的。（Binder机制 [参考](http://blog.csdn.net/universus/article/details/6211589)）

**Android 中多进程开始方式**

> 在AndroidMenifest中给四大组件（Activity ，Service，Receiver，ContentProvider）的标签添加android:process属性。




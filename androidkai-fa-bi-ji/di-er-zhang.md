### 第二章 IPC 机制

> IPC 即 Inter Process Communication 进程间通信，在多个进程间进行数据交换。Android 中IPC 主要指 Binder机制，如四大组件的通信，AIDL，Messenger，ContentProvider就是采用Binder机制实现的。（Binder机制 [参考](http://blog.csdn.net/universus/article/details/6211589)）

**Android 中多进程开启方式**

> 在AndroidMenifest中给四大组件（Activity ，Service，Receiver，ContentProvider）的标签添加android:process属性，该属性的命名有：

`1，android:process=":remote"  2，android:process="com.android.test2"`

> 1方式，以“  **:  **” 开头表示当前应用（com.android.test）的私有进程，其**完整的进程名**为：**com.android.test:remote。**其他应用的组件不能和它跑在一个进程中。
>
> 2方式，表示一个完整命名（com.android.test2）的全局进程，其他应用可以通过**相同的签名**和**相同的ShareUID**跑在同一个进程中并可以相互访问对方的私有数据，组件信息和共享内容数据等。

**Android 中开启多进程产生的问题**

> 1，静态成员和单例模式完全失效
>
> 2，线程同步机制完全失效
>
> 3，SharePreferences 可靠性下降
>
> 4，Application 多次创建
>
> **原因：**
>
> 由于 系统在创建新的进程（应用）的同时分配独立的虚拟机，虚拟机有自己的内存地址。所以多进程（应用间）的内存共享失败。

**Android IPC 方式的优缺点和适用场景**

| 名称 | 优点 | 缺点 | 适用场景 |
| :--- | :--- | :--- | :--- |
| Bundle | 简单易用 | 只能传输Bundle支持的数据类型 | 四大组件间的进程间通信 |
| 文件共享 | 简单易用 | 不适合高并发场景，并且无法做到进程间的即时通信 | 无并发访问情形，交换简单的数据实时性不高的场景 |
| AIDL | 功能强大，支持一对多并发通信，支持实时通信 | 使用稍复杂，需要处理好线程间同步 | 一对多通信且有RPC\(远程过程调用\)的需求 |
| Messenger | 功能一般，支持一对多串行通信，支持实时通信 | 不能很好的处理高并发情形，不支持RPC，数据通过Message进行传输，只能传输Bundle支持的数据类型 | 地并发的一对多及时通信，无RPC需求，或者无须要返回结果的RPC需求。 |
| ContentProvider | 在数据源访问方面功能强大，支持一对多并发数据共享，可通过CAll方法扩展其他操作。 | 可以理解为受约束的AIDL,主要提供数据源的CRUD操作 | 一对多的进程间的数据共享 |
| Socket | 功能强大，可以通过网络传输字节流，支持一对多并发实时通信 | 实现细节稍微有点繁琐，不支持直接的RPC | 网络数据交换 |




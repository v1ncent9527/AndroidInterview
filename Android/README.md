# Android 面试题

## Activity 生命周期

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/activity_fragment_lifecycle.webp)

### 横竖屏切换时候 Activity 的生命周期

不设置 Activity 的 android:configChanges 时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。

设置 Activity 的 android:configChanges=”orientation”时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次 设置 Activity 的 android:configChanges=”orientation |keyboardHidden”时，切屏不会重新调用各个生命周期，只会执行 onConfigurationChanged 方法

## ANR

在 Android 上，如果你的应用程序有一段时间响应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应 用程序无响应（ANR：Application NotResponding）对话框。 用户可以选择让程序继续运行，但是，他们在使用你的 应用程序时，并不希望每次都要处理这个对话框。因此 ，在程序里对响应性能的设计很重要这样，这样系统就不会显 示 ANR 给用户。

不同的组件发生 ANR 的时间不一样，Activity 是 5 秒，BroadCastReceiver 是 10 秒，Service 是 20 秒（均为前台）。

如果开发机器上出现问题，我们可以通过查看/data/anr/traces.txt 即可，最新的 ANR 信息在最开始部分。

- 主线程被 IO 操作（从 4.0 之后网络 IO 不允许在主线程中）阻塞。
- 主线程中存在耗时的计算
- 主线程中错误的操作，比如 Thread.wait 或者 Thread.sleep 等 Android 系统会监控程序的响应状况，一旦出现下面两种情况，则弹出 ANR 对话框
  应用在 5 秒内未响应用户的输入事件（如按键或者触摸）
- BroadcastReceiver 未在 10 秒内完成相关的处理
- Service 在特定的时间内无法处理完成 20 秒

## 内存泄露

内存泄漏(Memory Leak)是指程序中已动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。

### 内存泄露的危害

- 用户对单次的内存泄漏并没有什么感知，但是当泄漏积累到内存都被消耗完，就会导致卡顿，甚至崩溃
- gc 回收频繁 造成应用卡顿 ANR
- 当内存不足的时候,gc 会主动回收没用的内存.但是,内存回收也是需要时间的
- 内存回收和 gc 回收垃圾资源之间高频率交替的执行.就会产生内存抖动
- 很多数据就会污染内存堆，马上就会有许多 GCs 启动，由于这一额外的内存压力，也会产生突然增加的运算造成卡顿现象
- 任何线程的任何操作都会需要暂停，等待 GC 操作完成之后，其他操作才能够继续运行,所以垃圾回收运行的次数越少，对性能的影响就越少;

### 内存泄漏的原因

- 1、内存空间使用完毕后没有被回收，就会导致内存泄漏。虽然 Java 有垃圾回收机制，但是 Java 中任然存在很多造成内存泄漏的代码逻辑，垃圾回收器会回收掉大部分的内存空间，但是有一些内存空间还保持着引用，但是在逻辑上已经不会再用到的对象，这时候垃圾回收器就很无能为力，不能回收它们，比如：

  - 忘记释放分配的内存;
  - 应用不需要这个对象了，但是却没有释放这个对象的引用;
  - 强引用持有的对象，垃圾回收器是无法回收这个对象;
  - 持有对象生命周期过长，导致无法回收;

- 2、Android(Java)平台的内存泄漏是指没用的对象资源与 GC Roots 之间保持可达路径，导致系统无法进行回收;
- 3、那么从栈中弹出的对象将不会被当作垃圾回收，即使程序不再使用栈中的这些队象，他们也不会回收，因为栈中仍然保存这对象的引用，俗称过期引用，这个内存泄露很隐蔽;

### 常见的内存泄露场景详解

#### 1.单例导致内存泄露

单例模式在 Android 开发中会经常用到，但是如果使用不当就会导致内存泄露。因为单例的静态特性使得它的生命周期同应用的生命周期一样长，如果一个对象已经没有用处了，但是单例还持有它的引用，那么在整个应用程序的生命周期它都不能正常被回收，从而导致内存泄露。

```java
public class AppSettings {
    private static volatile AppSettings singleton;
    private Context mContext;
    private AppSettings(Context context) {
        this.mContext = context;
    }
    public static AppSettings getInstance(Context context) {
        if (singleton == null) {
            synchronized (AppSettings.class) {
                if (singleton == null) {
                    singleton = new AppSettings(context);
                }
            }
        }
        return singleton;
    }
}
```

像上面代码中这样的单例，如果我们在调用 getInstance(Context context)方法的时候传入的 context 参数是 Activity、Service 等上下文，就会导致内存泄露。以 Activity 为例，当我们启动一个 Activity，并调用 getInstance(Context context)方法去获取 AppSettings 的单例，传入 Activity.this 作为 context，这样 AppSettings 类的单例 sInstance 就持有了 Activity 的引用，当我们退出 Activity 时，该 Activity 就没有用了，但是因为 sIntance 作为静态单例(在应用程序的整个生命周期中存在)会继续持有这个 Activity 的引用，导致这个 Activity 对象无法被回收释放，这就造成了内存泄露。

为了避免这样单例导致内存泄露，我们可以将 context 参数改为全局的上下文：

```java
private AppSettings(Context context) {
        this.mContext = context.getApplicationContext();
}
```

#### 2.静态变量导致内存泄漏

静态变量存储在方法区，它的生命周期从类加载开始，到整个进程结束。一旦静态变量初始化后，它所持有的引用只有等到进程结束才会释放。比如下面这样的情况，在 Activity 中为了避免重复的创建 info，将 sInfo 作为静态变量：

```java
public class MainActivity2 extends AppCompatActivity {
    public static Info sInfo;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        sInfo = new Info(this);
    }
    class Info {
        private Context mContext;
        public Info(Context context) {
            this.mContext = context;
        }
    }
}
```

Info 作为 Activity 的静态成员，并且持有 Activity 的引用，但是 sInfo 作为静态变量，生命周期肯定比 Activity 长。所以当 Activity 退出后，sInfo 仍然引用了 Activity，Activity 不能被回收，这就导致了内存泄露。

在 Android 开发中，静态持有很多时候都有可能因为其使用的生命周期不一致而导致内存泄露，所以我们在新建静态持有的变量的时候需要多考虑一下各个成员之间的引用关系，并且尽量少地使用静态持有的变量，以避免发生内存泄露。当然，我们也可以在适当的时候讲静态量重置为 null，使其不再持有引用，这样也可以避免内存泄露。

#### 3.非静态内部类导致内存泄露

非静态内部类(包括匿名内部类)默认就会持有外部类的引用，当非静态内部类对象的生命周期比外部类对象的生命周期长时，就会导致内存泄露。非静态内部类导致的内存泄露在 Android 开发中有一种典型的场景就是使用 Handler，很多开发者在使用 Handler 是这样写的：

```java
public class MainActivity2 extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        start();
    }
    private void start() {
        Message message = Message.obtain();
        message.what = 1;
        mHandler.sendMessage(message);
    }
    private Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            if (msg.what == 1) {
                //doNothing
            }
        }
    };
}
```

也许有人会说，mHandler 并未作为静态变量持有 Activity 引用，生命周期可能不会比 Activity 长，应该不一定会导致内存泄露呢，显然不是这样的!熟悉 Handler 消息机制的都知道，mHandler 会作为成员变量保存在发送的消息 msg 中，即 msg 持有 mHandler 的引用，而 mHandler 是 Activity 的非静态内部类实例，即 mHandler 持有 Activity 的引用，那么我们就可以理解为 msg 间接持有 Activity 的引用。msg 被发送后先放到消息队列 MessageQueue 中，然后等待 Looper 的轮询处理(MessageQueue 和 Looper 都是与线程相关联的，MessageQueue 是 Looper 引用的成员变量，而 Looper 是保存在 ThreadLocal 中的)。那么当 Activity 退出后，msg 可能仍然存在于消息对列 MessageQueue 中未处理或者正在处理，那么这样就会导致 Activity 无法被回收，以致发生 Activity 的内存泄露。

通常在 Android 开发中如果要使用内部类，但又要规避内存泄露，一般都会采用静态内部类+弱引用的方式。

```java
MyHandler mHandler;
public static class MyHandler extends Handler {
        private WeakReference<Activity> mActivityWeakReference;
        public MyHandler(Activity activity) {
            mActivityWeakReference = new WeakReference<>(activity);
        }
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
        }
}
```

mHandler 通过弱引用的方式持有 Activity，当 GC 执行垃圾回收时，遇到 Activity 就会回收并释放所占据的内存单元。这样就不会发生内存泄露了。上面的做法确实避免了 Activity 导致的内存泄露，发送的 msg 不再已经没有持有 Activity 的引用了，但是 msg 还是有可能存在消息队列 MessageQueue 中，所以更好的是在 Activity 销毁时就将 mHandler 的回调和发送的消息给移除掉。

```java
@Override
    protected void onDestroy() {
        super.onDestroy();
        mHandler.removeCallbacksAndMessages(null);
 }
```

非静态内部类造成内存泄露还有一种情况就是使用 Thread 或者 AsyncTask。要避免内存泄露的话还是需要像上面 Handler 一样使用静态内部类+弱应用的方式(代码就不列了，参考上面 Hanlder 的正确写法)。

#### 4.未取消注册或回调导致内存泄露

比如我们在 Activity 中注册广播，如果在 Activity 销毁后不取消注册，那么这个刚播会一直存在系统中，同上面所说的非静态内部类一样持有 Activity 引用，导致内存泄露。因此注册广播后在 Activity 销毁后一定要取消注册。在注册观察则模式的时候，如果不及时取消也会造成内存泄露。比如使用 Retrofit+RxJava 注册网络请求的观察者回调，同样作为匿名内部类持有外部引用，所以需要记得在不用或者销毁的时候取消注册。

#### 5.资源未关闭或释放导致内存泄露

在使用 IO、File 流或者 Sqlite、Cursor 等资源时要及时关闭。这些资源在进行读写操作时通常都使用了缓冲，如果不及时关闭，这些缓冲对象就会一直被占用而得不到释放，以致发生内存泄露。因此我们在不需要使用它们的时候就及时关闭，以便缓冲能及时得到释放，从而避免内存泄露。

### 总结

- 对于生命周期比 Activity 长的对象(单例)，要避免直接引用 Activity 的 context，可以考虑使用 ApplicationContext，静态变量不使用时及时置空;
- Handler 持有的引用最好使用弱引用，在 Activity 被释放的时候要记得清空 Message，取消 Handler 对象的 Runnable;
- 非静态内部类、非静态匿名内部类会自动持有外部类的引用，为避免内存泄露，可以考虑把内部类声明为静态的;
- 广播接收器、EventBus 等的使用过程中，注册/反注册应该成对使用，但凡有注册的都应该有反注册;
- 不再使用的资源对象 Cursor、File、Bitmap 等要记住正确关闭;

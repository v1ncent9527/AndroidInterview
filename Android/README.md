# Android 面试题

## Activity/Fragment 生命周期

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

## AsyncTask

AsyncTask 是一种轻量级的异步任务类，它可以在线程池中执行后台任务，然后把执行的进度和最终结果传递给主线程并在主线程中更新 UI。

AsyncTask 是一个抽象的泛型类，它提供了 **Params**、**Progress** 和 **Result** 这三个泛型参数，其中 Params 表示参数的类型，Progress 表示后台任务的执行进度和类型，而 Result 则表示后台任务的返回结果的类型，如果 AsyncTask 不需要传递具体的参数，那么这三个泛型参数可以用 Void 来代替。

```java
class ReverseTask extends AsyncTask<String,Integer,String> {
    //主线程执行,一般般可以做一些简单的UI初始化操作，如进度条的显示等。
    @Override
    protected void onPreExecute() {
        progressDialog.show();
    }

    //子线程执行
    @Override
    protected String doInBackground(String... params) {
        int progress = 0;
        try {
            for(int i = 0;i <= 10;i ++){
                progress += 10;
                Thread.sleep(200);
                publishProgress(progress);
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return Reverse(params[0]);
    }

    //主线程执行
    @Override
    protected void onProgressUpdate(Integer... values) {
        progressDialog.setMessage(values[0]+"% Finished");
    }

    //主线程执行
    @Override
    protected void onPostExecute(String s) {
        textView.setText(s);
        progressDialog.dismiss();
    }

    void String Reverse(String string){
        StringBuilder builder = newStringBuilder(string);
        return new String(builder.reverse());
    }

    ReverseTask reverseTask = new ReverseTask();
    reverseTask.execute(text);
}
```

### 原理

- AsyncTask 中有两个线程池（SerialExecutor 和 THREAD_POOL_EXECUTOR）和一个 Handler（InternalHandler），其中线程池 SerialExecutor 用于任务的排队，而线程池 THREAD_POOL_EXECUTOR 用于真正地执行任务，InternalHandler 用于将执行环境从线程池切换到主线程。
- sHandler 是一个静态的 Handler 对象，为了能够将执行环境切换到主线程，这就要求 sHandler 这个对象必须在主线程创建。由于静态成员会在加载类的时候进行初始化，因此这就变相要求 AsyncTask 的类必须在主线程中加载，否则同一个进程中的 AsyncTask 都将无法正常工作。

### 注意事项

为了防止内存泄漏，要在 Activity 的 onDestroy()中终止 AsyncTask 的任务，通过代码逻辑或者强制关闭任务，例如　：
cancel(boolean mayInterruptIfRunning)

```java
reverseTask.cancel(true);
```

使用 AsyncTask 建议不要使用内部类，建议在外部定义一个 task 类，task 中使用弱引用来指向 Activity，通过这种方法来更新 UI 就能很大程度降低泄漏的危险

## onSaveInstanceState 和 onRestoreInstanceState

Activity 的 onSaveInstanceState() 和 onRestoreInstanceState()并不是生命周期方法，它们不同于 onCreate()、onPause()等生命周期方法，它们并不一定会被触发。当应用遇到意外情况（如：内存不足、用户直接按 Home 键）由系统销毁 一个 Activity 时，onSaveInstanceState() 会被调用。但是当用户主动去销毁一个 Activity 时，例如在应用中按返回键，onSaveInstanceState()就不会被调用。因为在这种情 况下，用户的行为决定了不需要保存 Activity 的状态。通常 onSaveInstanceState()只适合用于保存一些临时性的状态，而 onPause()适合用于数据的持久化保存。
在 activity 被杀掉之前调用保存每个实例的状态,以保证该状态可以在 onCreate(Bundle)或者 onRestoreInstanceState(Bundle) (传入的 Bundle 参数是由 onSaveInstanceState 封装好的)中恢复。这个方法在一个 activity 被杀死前调用，当该 activity 在将来某个时刻回来时可以恢复其先前状态。

例如，如果 activity B 启用后位于 activity A 的前端，在某个时刻 activity A 因为系统回收资源的问题要被杀掉，A 通过 onSaveInstanceState 将有机会保存其用户界面状态，使得将来用户返回到 activity A 时能通过 onCreate(Bundle)或者 onRestoreInstanceState(Bundle)恢复界面的状态。

这就是 onSaveInstanceState() 和 onRestoreInstanceState() 两个函数的基本作用和用法。(ps:关于原理实现请追寻源码，就是 view 的保存与绘制)

### onRestoreInstanceState()调用时机

当某个 activity 变得“容易”被系统销毁时，该 activity 的 onSaveInstanceState 就会被执行，除非该 activity 是被用户主动销毁的，例如当用户按 BACK 键的时候。

**_注意上面的双引号，何为"容易"？意思就是说该 activity 还没有被销毁，而仅仅是一种可能性_**。这 种可能性有哪些？通过重写一个 activity 的所有生命周期的 onXXX 方法，包括 onSaveInstanceState()和 onRestoreInstanceState() 方法，我们可以清楚地知道当某个 activity（假定为 activity A）显示在当前 task 的最上层时，其 onSaveInstanceState()方法会在什么时候被执行，有这么几种情况：

- (1)、当用户按下 HOME 键时。
  　　这是显而易见的，系统不知道你按下 HOME 后要运行多少其他的程序，自然也不知道 activity A 是否会被销毁，因此系统会调用 onSaveInstanceState()，让用户有机会保存某些非永久性的数据。以下几种情况的分析都遵循该原则

- (2)、长按 HOME 键，选择运行其他的程序时。

- (3)、按下电源按键（关闭屏幕显示）时。

- (4)、从 activity A 中启动一个新的 activity 时。

- (5)、屏幕方向切换时，例如从竖屏切换到横屏时。

在屏幕切换之前，系统会销毁 activity A，在屏幕切换之后系统又会自动地创建 activity A，所以 onSaveInstanceState()一定会被执行，且也一定会执行 onRestoreInstanceState()。

总而言之，onSaveInstanceState()的调用遵循一个重要原则，即当系统存在“未经你许可”时销毁了我们的 activity 的 可能时，则 onSaveInstanceState()会被系统调用，这是系统的责任，因为它必须要提供一个机会让你保存你的数据（当然你不保存那就随便 你了）。如果调用，调用将发生在 onPause()或 onStop()方法之前。（虽然测试时发现多数在 onPause()前）

### onRestoreInstanceState()调用时机

onRestoreInstanceState() 被调用的前提是，activity A“确实”被系统销毁了，而如果仅仅是停留在有这种可能性的情况下，则该方法不会被调用，例如，当正在显示 activity A 的时候，用户按下 HOME 键回到主界面，然后用户紧接着又返回到 activity A，这种情况下 activity A 一般不会因为内存的原因被系统销毁，故 activity A 的 onRestoreInstanceState 方法不会被执行 此也说明上二者，大多数情况下不成对被使用。
onRestoreInstanceState()在 onStart() 和 onPostCreate(Bundle)之间调用。

### Serialzable 和 Parcelable 的区别

因为 bundle 传递数据时只支持基本数据类型，所以在传递对象时需要序列化转换成可存储或可传输的本质状态（字节流）。序列化后的对象可以在网络、IPC（比如启动另一个进程的 Activity、Service 和 Reciver）之间进行传输，也可以存储到本地。

**Serializable（Java 自带）**：
Serializable 是序列化的意思，表示将一个对象转换成存储或可传输的状态。序列化后的对象可以在网络上进传输，也可以存储到本地。

**Parcelable（android 专用）**：
除了 Serializable 之外，使用 Parcelable 也可以实现相同的效果，不过不同于将对象进行序列化，Parcelable 方式的实现原理是将一个完整的对象进行分解，而分解后的每一部分都是 Intent 所支持的数据类型，这也就实现传递对象的功能了。

两者区别在于**存储媒介**的不同。

Serializable 使用 IO 读写存储在硬盘上。序列化过程使用了反射技术，并且期间产生临时对象。优点代码少。

Parcelable 是直接在内存中读写，我们知道内存的读写速度肯定优于硬盘读写速度，所以 Parcelable 序列化方式性能上要优于 Serializable 方式很多。但是代码写起来相比 Serializable 方式麻烦一些。(Kotlin 除外，只需加一个@Parcelable 注解即可)

## Android 更新 UI 方式

- Activity.runOnUiThread(Runnable)
- View.post(Runnable)，View.postDelay(Runnable, long)（可以理解为在当前操作视图 UI 线程添加队列）
- Handler
- AsyncTask
- Rxjava(各种 BUS)
- LiveData

## Asset 目录与 res 目录的区别

- **assets**：不会在 R 文件中生成相应标记，存放到这里的资源在打包时会打包到程序安装包中。（通过 AssetManager 类访问这些文件）

- **res**：会在 R 文件中生成 id 标记，资源在打包时如果使用到则打包到安装包中，未用到不会打入安装包中。

- **res/anim**：存放动画资源。

- **res/raw**：和 asset 下文件一样，打包时直接打入程序安装包中（会映射到 R 文件中）。

# Java 面试题

## 1、面向对象

### 多态

多态是指父类的某个方法被子类重写时，可以产生自己的功能行为，同一个操作作用于不同对象，可以有不同的解释，产生不同的执行结果。

#### 多态存在的三个必要条件

- 继承
- 重写
- 父类引用指向子类对象：Parent p = new Child();

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/polymorphic.webp)

#### 多态的好处

- **1.可替换性（substitutability）**：多态对已存在代码具有可替换性。例如，多态对圆 Circle 类工作，对其他任何圆形几何体，如圆环，也同样工作。
- **2.可扩充性（extensibility）**：多态对代码具有可扩充性。增加新的子类不影响已存在类的多态性、继承性，以及其他特性的运行和操作。实际上新加子类更容易获得多态功能。例如，在实现了圆锥、半圆锥以及半球体的多态基础上，很容易增添球体类的多态性。
- **3.接口性（interface-ability）**：多态是超类通过方法签名，向子类提供了一个共同接口，由子类来完善或者覆盖它而实现的。
- **4.灵活性（flexibility）**：它在应用中体现了灵活多样的操作，提高了使用效率。
- **5.简化性（simplicity）**：多态简化对应用软件的代码编写和修改过程，尤其在处理大量对象的运算和操作时，这个特点尤为突出和重要。

#### Java 中多态的实现方式

- 接口实现
- 继承父类进行方法重写
- 同一个类中进行方法重载。

#### final

继承可以允许子类覆写父类的方法。如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为 final。用 final 修饰的方法不能被 Override：

```java
class Person {
    protected String name;
    public final String hello() {
        return "Hello, " + name;
    }
}

class Student extends Person {
    // compile error: 不允许覆写
    @Override
    public String hello() {
    }
}
```

如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为 final。用 final 修饰的类不能被继承：

```java
final class Person {
    protected String name;
}

// compile error: 不允许继承自Person
class Student extends Person {
}
```

对于一个类的实例字段，同样可以用 final 修饰。用 final 修饰的字段在初始化后不能被修改。例如：

```java
class Person {
    public final String name = "Unamed";
}
```

对 final 字段重新赋值会报错：

```java
Person p = new Person();
p.name = "New Name"; // compile error!
```

可以在构造方法中初始化 final 字段：

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

final 修饰符有多种作用：

- final 修饰的方法可以阻止被覆写；
- final 修饰的 class 可以阻止被继承；
- final 修饰的 field 必须在创建对象时初始化，随后不可修改。

#### 重写(Override)与重载(Overload)

##### 重写(Override)

重写是子类对父类的允许访问的方法的实现过程进行重新编写, **返回值**和**形参**都不能改变。**即外壳不变，核心重写！**

##### 重载(Overload)

重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

最常用的地方就是构造器的重载。

重写与重载之间的区别
| 区别点 | 重载方法 |重写方法 |
| ---- | ---- |---- |
| 参数列表 | 必须修改 |一定不能修改 |
| 返回类型 | 可以修改 |一定不能修改 |
| 异常 | 可以修改 |可以减少或删除，一定不能抛出新的或者更广的异常 |
| 访问 | 可以修改 |一定不能做更严格的限制（可以降低限制） |

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/overloading-vs-overriding.webp)

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/overloading-vs-overriding2.webp)

##### 总结

方法的重写(Overriding)和重载(Overloading)是 java 多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。

- (1)方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)。
- (2)方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)。
- (3)方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。

### 设计模式

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/design-pattern.webp)

Java 中一般认为有 23 种设计模式，我们不需要所有的都会，但是其中常用的种设计模式应该去掌握。下面列出了所有的设计模式。要掌握的设计模式我单独列出来了，当然能掌握的越多越好。

**创建型（5）：描述怎么创建对象**

- 1.单例模式
- 2.原型模式：对象的拷贝
- 3.建造者模式
- 4.工厂模式：建立一个工厂方法来制造新的对象
- 5.抽象工厂模式：

**结构型（7）：描述如何将类或对象按某种规则组成更大的结构**

- 1.桥接模式：对于两个或以上纬度独立变化的场景，将抽象与具体实现分离，实例：用不同颜色画不同形状
- 2.外观模式：对外有一个统一接口，外部不用关心内部子系统的具体实现，这是"迪米特原则"的典型应用
- 3.适配器模式：改变类的接口，使原本由于接口不匹配而无法一起工作的两个类能够在一工作，实例：RecycleView 的 Adapter 不管什么类型的 View 都返回 ViewHolder
- 4.代理模式：由代理对象控制对原对象的引用，包括静态代理和动态代理
- 5.组合模式：将对象组成树形结构，用于对单个对象和组合对象的使用具有一致性，实例：ViewGroup
- 6.装饰模式：对对象包装一层，动态的增加一些额外功能，实例：ContextWrapper 包装 Context
- 7.享元模式：复用对象，实例：java 的常量池（比如 String），线程池，Message.obtain 等

**行为型（11）：描述类或对象之间怎么相互协作，怎样分配指责**

- 1.观察者模式：一对多依赖关系，多个观察者可以同时监听某一个对象，实例：jetpack 的 lifeCycle 添加生命周期观察者
- 2.中介者模式：定义一个中介对象封装一系列对象的交互，解耦这些对象，实例：MVP 的 P
- 3.访问者模式：将作用于某数据结构中各元素的操作分离出来封装成独立的类，对这些元素添加新的操作，但不改变原数据结构，实例：asm 中的 classVisitor 中再分别对类注解、变量、方法等进行处理
- 4.状态模式：行为由状态决定，不同状态下由不同行为，与策略模式类似，实例：不同状态下有同一种操作的不同行为的子类实现
- 5.命令模式：将一个请求封装为一个对象发出，交给别的对象去处理请求，实例：Handler 发送定义好的消息事件
- 6.策略模式：将一系列的算法封装起来，方便替换，实例：动画的时间插值器
- 7.责任链模式：让多个对象都有机会处理一个事件，实例：View 事件传递机制
- 8.备忘录模式：保存对象之前的状态，方便后面恢复
- 9.迭代器模式：提供一种方法遍历容器中的元素，而不需要暴露该对象的内部表示，实例：集合的迭代器
- 10.解释器模式：多次出现的问题有一定规律，就可以归纳成一种简单的语言来解释，实例：AndroidManifest 文件、GLES 着色器语言
- 11.模版方法模式：定义一套固定步骤，方便直接执行，实例：AsyncTask

#### 创建型 - 单例模式(Singleton pattern)

使用一个私有构造函数、一个私有静态变量以及一个公有静态函数来实现。

私有构造函数保证了不能通过构造函数来创建对象实例，只能通过公有静态函数返回唯一的私有静态变量。

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/singleton_pattern.webp)

**懒汉式-线程不安全**

以下实现中，私有静态变量 uniqueInstance 被延迟实例化，这样做的好处是，如果没有用到该类，那么就不会实例化 uniqueInstance，从而节约资源。

这个实现在多线程环境下是不安全的，如果多个线程能够同时进入 if (uniqueInstance == null) ，并且此时 uniqueInstance 为 null，那么会有多个线程执行 uniqueInstance = new Singleton(); 语句，这将导致多次实例化 uniqueInstance。

```java
public class Singleton {

    private static Singleton uniqueInstance;

    private Singleton() {
    }

    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```

**懒汉式-线程安全**
只需要对 getUniqueInstance() 方法加锁，那么在一个时间点只能有一个线程能够进入该方法，从而避免了多次实例化 uniqueInstance 的问题。

但是当一个线程进入该方法之后，其它试图进入该方法的线程都必须等待，因此性能上有一定的损耗。

```java
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton (){}
    public static synchronized Singleton getInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```

**饿汉式-线程安全**
线程不安全问题主要是由于 uniqueInstance 被多次实例化，采取直接实例化 uniqueInstance 的方式就不会产生线程不安全问题。

但是直接实例化的方式也丢失了延迟实例化带来的节约资源的好处。

```java
public class Singleton {
    private static Singleton uniqueInstance = new Singleton();
    private Singleton (){}
    public static Singleton getInstance() {
    return uniqueInstance;
    }
}
```

**双重校验锁-线程安全**
uniqueInstance 只需要被实例化一次，之后就可以直接使用了。加锁操作只需要对实例化那部分的代码进行，只有当 uniqueInstance 没有被实例化时，才需要进行加锁。

双重校验锁先判断 uniqueInstance 是否已经被实例化，如果没有被实例化，那么才对实例化语句进行加锁。

```java
public class Singleton {
    private volatile static Singleton singleton;
    private Singleton (){}
    public static Singleton getSingleton() {
    if (singleton == null) {
        synchronized (Singleton.class) {
            if (singleton == null) {
                singleton = new Singleton();
            }
        }
    }
    return singleton;
    }
}
```

#### 创建型 - 简单工厂(Simple Factory)

在创建一个对象时不向客户暴露内部细节，并提供一个创建对象的通用接口。

简单工厂不是设计模式，更像是一种编程习惯。它把实例化的操作单独放到一个类中，这个类就成为简单工厂类，让简单工厂类来决定应该用哪个具体子类来实例化。

这样做能把客户类和具体子类的实现解耦，客户类不再需要知道有哪些子类以及应当实例化哪个子类。因为客户类往往有多个，如果不使用简单工厂，所有的客户类都要知道所有子类的细节。而且一旦子类发生改变，例如增加子类，那么所有的客户类都要进行修改。

**实现**

```java
public interface Product {
}
```

```java
public class ConcreteProduct implements Product {
}
```

```java
public class ConcreteProduct1 implements Product {
}
```

```java
public class ConcreteProduct2 implements Product {
}
```

以下的 Client 类中包含了实例化的代码，这是一种错误的实现，如果在客户类中存在实例化代码，就需要将代码放到简单工厂中。

```java
public class Client {
    public static void main(String[] args) {
        int type = 1;
        Product product;
        if (type == 1) {
            product = new ConcreteProduct1();
        } else if (type == 2) {
            product = new ConcreteProduct2();
        } else {
            product = new ConcreteProduct();
        }
        // do something with the product
    }
}
```

以下的 SimpleFactory 是简单工厂实现，它被所有需要进行实例化的客户类调用。

```java
public class SimpleFactory {
    public Product createProduct(int type) {
        if (type == 1) {
            return new ConcreteProduct1();
        } else if (type == 2) {
            return new ConcreteProduct2();
        }
        return new ConcreteProduct();
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        SimpleFactory simpleFactory = new SimpleFactory();
        Product product = simpleFactory.createProduct(1);
        // do something with the product
    }
}
```

#### 创建型 - 工厂方法(Factory Method)

定义了一个创建对象的接口，但由子类决定要实例化哪个类。工厂方法把实例化操作推迟到子类。

在简单工厂中，创建对象的是另一个类，而在工厂方法中，是由子类来创建对象。

**实现**

```java
public abstract class Factory {
    abstract public Product factoryMethod();
    public void doSomething() {
        Product product = factoryMethod();
        // do something with the product
    }
}
```

```java
public class ConcreteFactory extends Factory {
    public Product factoryMethod() {
        return new ConcreteProduct();
    }
}
```

```java
public class ConcreteFactory1 extends Factory {
    public Product factoryMethod() {
        return new ConcreteProduct1();
    }
}
```

```java
public class ConcreteFactory2 extends Factory {
    public Product factoryMethod() {
        return new ConcreteProduct2();
    }
}
```

#### 创建型 - 抽象工厂(Abstract Factory)

抽象工厂模式创建的是对象家族，也就是很多对象而不是一个对象，并且这些对象是相关的，也就是说必须一起创建出来。而工厂方法模式只是用于创建一个对象，这和抽象工厂模式有很大不同。

抽象工厂模式用到了工厂方法模式来创建单一对象，AbstractFactory 中的 createProductA() 和 createProductB() 方法都是让子类来实现，这两个方法单独来看就是在创建一个对象，这符合工厂方法模式的定义。

至于创建对象的家族这一概念是在 Client 体现，Client 要通过 AbstractFactory 同时调用两个方法来创建出两个对象，在这里这两个对象就有很大的相关性，Client 需要同时创建出这两个对象。

从高层次来看，抽象工厂使用了组合，即 Cilent 组合了 AbstractFactory，而工厂方法模式使用了继承。

**实现**

```java
public class AbstractProductA {
}
```

```java
public class AbstractProductB {
}
```

```java
public class ProductA1 extends AbstractProductA {
}
```

```java
public class ProductA2 extends AbstractProductA {
}
```

```java
public class ProductB1 extends AbstractProductB {
}
```

```java
public class ProductB2 extends AbstractProductB {
}
```

```java
public abstract class AbstractFactory {
    abstract AbstractProductA createProductA();
    abstract AbstractProductB createProductB();
}
```

```java
public class ConcreteFactory1 extends AbstractFactory {
    AbstractProductA createProductA() {
        return new ProductA1();
    }

    AbstractProductB createProductB() {
        return new ProductB1();
    }
}
```

```java
public class ConcreteFactory2 extends AbstractFactory {
    AbstractProductA createProductA() {
        return new ProductA2();
    }

    AbstractProductB createProductB() {
        return new ProductB2();
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        AbstractFactory abstractFactory = new ConcreteFactory1();
        AbstractProductA productA = abstractFactory.createProductA();
        AbstractProductB productB = abstractFactory.createProductB();
        // do something with productA and productB
    }
}
```

#### 行为型 - 责任链(Chain Of Responsibility)

为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。

在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。

**实现**

```java
public abstract class Handler {
    protected Handler successor;

    public Handler(Handler successor) {
        this.successor = successor;
    }

    protected abstract void handleRequest(Request request);
}
```

```java
public class ConcreteHandler1 extends Handler {
    public ConcreteHandler1(Handler successor) {
        super(successor);
    }

    @Override
    protected void handleRequest(Request request) {
        if (request.getType() == RequestType.type1) {
            System.out.println(request.getName() + " is handle by ConcreteHandler1");
            return;
        }
        if (successor != null) {
            successor.handleRequest(request);
        }
    }
}
```

```java
public class ConcreteHandler2 extends Handler{
    public ConcreteHandler2(Handler successor) {
        super(successor);
    }

    @Override
    protected void handleRequest(Request request) {
        if (request.getType() == RequestType.type2) {
            System.out.println(request.getName() + " is handle by ConcreteHandler2");
            return;
        }
        if (successor != null) {
            successor.handleRequest(request);
        }
    }
}
```

```java
public class Request {
    private RequestType type;
    private String name;

    public Request(RequestType type, String name) {
        this.type = type;
        this.name = name;
    }

    public RequestType getType() {
        return type;
    }

    public String getName() {
        return name;
    }
}
```

```java
public enum RequestType {
    type1, type2
}
```

```java
public class Client {
    public static void main(String[] args) {
        Handler handler1 = new ConcreteHandler1(null);
        Handler handler2 = new ConcreteHandler2(handler1);
        Request request1 = new Request(RequestType.type1, "request1");
        handler2.handleRequest(request1);
        Request request2 = new Request(RequestType.type2, "request2");
        handler2.handleRequest(request2);
    }
}
```

```
request1 is handle by ConcreteHandler1
request2 is handle by ConcreteHandler2
```

#### 行为型 - 观察者(Observer)

定义对象之间的一对多依赖，当一个对象状态改变时，它的所有依赖都会收到通知并且自动更新状态。

主题(Subject)是被观察的对象，而其所有依赖者(Observer)称为观察者。

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/observer.webp)

**实现**

天气数据布告板会在天气信息发生改变时更新其内容，布告板有多个，并且在将来会继续增加。

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/observer_weather.webp)

```java
public interface Subject {
    void resisterObserver(Observer o);

    void removeObserver(Observer o);

    void notifyObserver();
}
```

```java
public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<>();
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObserver();
    }

    @Override
    public void resisterObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i >= 0) {
            observers.remove(i);
        }
    }

    @Override
    public void notifyObserver() {
        for (Observer o : observers) {
            o.update(temperature, humidity, pressure);
        }
    }
}
```

```java
public interface Observer {
    void update(float temp, float humidity, float pressure);
}
```

```java
public class StatisticsDisplay implements Observer {

    public StatisticsDisplay(Subject weatherData) {
        weatherData.resisterObserver(this);
    }

    @Override
    public void update(float temp, float humidity, float pressure) {
        System.out.println("StatisticsDisplay.update: " + temp + " " + humidity + " " + pressure);
    }
}
```

```java
public class CurrentConditionsDisplay implements Observer {

    public CurrentConditionsDisplay(Subject weatherData) {
        weatherData.resisterObserver(this);
    }

    @Override
    public void update(float temp, float humidity, float pressure) {
        System.out.println("CurrentConditionsDisplay.update: " + temp + " " + humidity + " " + pressure);
    }
}
```

```java
public class WeatherStation {
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();
        CurrentConditionsDisplay currentConditionsDisplay = new CurrentConditionsDisplay(weatherData);
        StatisticsDisplay statisticsDisplay = new StatisticsDisplay(weatherData);

        weatherData.setMeasurements(0, 0, 0);
        weatherData.setMeasurements(1, 1, 1);
    }
}
```

```
CurrentConditionsDisplay.update: 0.0 0.0 0.0
StatisticsDisplay.update: 0.0 0.0 0.0
CurrentConditionsDisplay.update: 1.0 1.0 1.0
StatisticsDisplay.update: 1.0 1.0 1.0
```

#### Android 中常用的设计模式

- AlertDialog、Notification 源码中使用了 Bulider（建造者）模式完成参数的初始化
- 日常开发的 BaseActivity 抽象工厂模式
- Okhttp 内部使用了责任链模式来完成每个 Interceptor 拦截器的调用
- RxJava 的观察者模式（各种 BUS）
- AIDL 代理模式

## 2、集合框架

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/java_collection_structure.webp)

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/java_collection_structure2.webp)

| 名称 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| List | 有序、可重复；索引查询速度快；插入、删除伴随数据移动，速度慢 |
| Set  | 无序，不可重复                                               |
| Map  | 键值对，键唯一，值多个                                       |

### Set 和 List 对比

Set：检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。

List：和数组类似，List 可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变。

### 线程安全集合类与非线程安全集合类

LinkedList、ArrayList、HashSet 是非线程安全的，Vector 是线程安全的;

HashMap 是非线程安全的，HashTable 是线程安全的;

StringBuilder 是非线程安全的，StringBuffer 是线程安的。

### ArrayList 与 LinkedList 的区别和适用场景

**_Arraylist_**：

优点：ArrayList 是实现了基于动态数组的数据结构,因地址连续，一旦数据存储好了，查询操作效率会比较高（在内存里是连着放的）。

缺点：因为地址连续，ArrayList 要移动数据,所以插入和删除操作效率比较低。

**_LinkedList_**：

优点：LinkedList 基于链表的数据结构，地址是任意的，其在开辟内存空间的时候不需要等一个连续的地址，对新增和删除操作 add 和 remove，LinedList 比较占优势。LikedList 适用于要头尾操作或插入指定位置的场景。

缺点：因为 LinkedList 要移动指针,所以查询操作性能比较低。

适用场景分析：

当需要对数据进行对此访问的情况下选用 ArrayList，当要对数据进行多次增加删除修改时采用 LinkedList。

### ArrayList 和 LinkedList 怎么动态扩容的吗？

**_ArrayList_**:

ArrayList 初始化大小是 10 （如果你知道你的 arrayList 会达到多少容量，可以在初始化的时候就指定，能节省扩容的性能开支） 扩容点规则是，新增的时候发现容量不够用了，就去扩容 扩容大小规则是，扩容后的大小= 原始大小+原始大小/2 + 1。（例如：原始大小是 10 ，扩容后的大小就是 10 + 5+1 = 16）

**_LinkedList_**:

linkedList 是一个双向链表，没有初始化大小，也没有扩容的机制，就是一直在前面或者后面新增就好。

### HashMap

数据结构：哈希表结构（链表散列：数组+链表）实现，结合数组和链表的优点。当链表长度超过 8 时，链表转换为红黑树

#### HashMap 的 key 可以为 null 吗？

HashMap 最多只允许一条记录的键为 null(多个会覆盖)，允许多条记录的值为 null

#### HashMap 是线程安全的吗？

HashMap 是线程不安全的。

#### 如何保证 HashMap 线程安全？

- 使用 Collections.synchronizedMap 方法，使 HashMap 具有线程安全的能力
  Map m = Collections.synchronizedMap(new HashMap(...))

```java
Map m = Collections.synchronizedMap(new HashMap(...))

```

- 或者使用 ConcurrentHashMap，ConcurrentHashMap 是一个 Segment 数组， Segment 通过继承 ReentrantLock 来进行加锁，所以每次需要加锁的操作锁住的是一个 segment，这样只要保证每个 Segment 是线程安全的，也就实现了全局的线程安全

#### 有几种方式遍历 HashMap？

- Map.Entry

```java
Map<Integer , Integer> map = new HashMap<>();
map.put(1, 3);
map.put(2,4);
Set<Map.Entry<Integer, Integer>> entries = map.entrySet();
for (Map.Entry<Integer, Integer> entry : entries) {
    System.out.println("key="+entry.getKey()+",value="+entry.getValue());
}
```

- keySet

```java
Set<Integer> keySet = map.keySet();
for (Integer key : keySet) {
    System.out.println("key="+key+",value="+map.get(key));
}
```

- values

```java
Collection<Integer> values = map.values();
for (Integer value : values) {
    System.out.println("value="+value);
}
```

- Lambda 表达式

```java
map.forEach((key , value)->{
 System.out.println("key="+key+",value="+value);
});
```

#### 负载因子是多少？它有什么用

一般来说，默认的负载系数(0.75)在时间和空间成本之间提供了一个很好的权衡。较高的值减少了空间开销，但增加了查找成本(反映在 HashMap 类的大多数操作中，包括 get 和 put)。在设置 map 的初始容量时，应考虑其期望的表项数及其负载因子，以尽量减少重哈希操作的次数。如果初始容量大于最大条目数除以负载因子，则不会发生重新哈希操作。

#### jdk 1.7 和 jdk 1.8 HashMap 的区别？

- jdk 1.7 由数组+链表组成，查找的时候，根据 hash 值我们能够快速定位到数组的具体下标，但是之后的话， 需要顺着链表一个个比较下去才能找到我们需要的，时间复杂度取决于链表的长度，为 O(n)。
- jdk 1.8 数组+链表+红黑树 组成，为了降低这部分的开销，在 Java8 中， 当满足一定条件时，会将链表转换为红黑树，在这些位置进行查找的时候可以降低时间复杂度为 O(logN)。

| 左对齐                   | JDK1.7                                                    | JDK1.8                               | JDK1.8 的优势                                                      |
| ------------------------ | --------------------------------------------------------- | ------------------------------------ | ------------------------------------------------------------------ |
| 底层结构                 | 数组+链表                                                 | 数组+链表/红黑树(链表大于 8)         | 避免单条链表过长而影响查询效率，提高查询效率                       |
| hash 值计算方式          | 9 次扰动 = 4 次位运算 + 5 次异或运算                      | 2 次扰动 = 1 次位运算 + 1 次异或运算 | 可以均匀地把之前的冲突的节点分散到新的桶（具体细节见下面扩容部分） |
| 插入数据方式             | 头插法（先讲原位置的数据移到后 1 位，再插入数据到该位置） | 尾插法（直接插入到链表尾部/红黑树）  | 解决多线程造成死循环地问题                                         |
| 扩容后存储位置的计算方式 | 重新进行 hash 计算                                        | 原位置或原位置+旧容量                | 省去了重新计算 hash 值的时间                                       |

#### 链表转红黑树要满足什么条件？

- 链表中的元素的个数为 8 个或超过 8 个
- 当前数组的长度大于或等于 64 才会把链表转变为红黑树。

#### HashMap 是如何扩容的？

1.HashMap 的扩容指的就是数组的扩容， 因为数组占用的是连续内存空间，所以数组的扩容其实只能新开一个新的数组，然后把老数组上的元素转移到新数组上来，这样才是数组的扩容

2.先新建一个 2 被数组大小的数组

3.然后遍历老数组上的每一个位置，如果这个位置上是一个链表，就把这个链表上的元素转移到新数组上去

4.在 jdk8 中，因为涉及到红黑树，这个其实比较复杂，jdk8 中其实还会用到一个双向链表来维护红黑树中的元素，所以 jdk8 中在转移某个位置上的元素时，会去判断如果这个位置是一个红黑树，那么会遍历该位置的双向链表，遍历双向链表统计哪些元素在扩容完之后还是原位置，哪些元素在扩容之后在新位置，这样遍历完双向链表后，就会得到两个子链表，一个放在原下标位置，一个放在新下标位置，如果原下标位置或新下标位置没有元素，则红黑树不用拆分，否则判断这两个子链表的长度，如果超过 8，则转成红黑树放到对应的位置，否则把单向链表放到对应的位置。

5.元素转移完了之后，在把新数组对象赋值给 HashMap 的 table 属性，老数组会被回收到

#### HashMap & TreeMap & LinkedHashMap 使用场景？

一般情况下，使用最多的是 HashMap。

HashMap：在 Map 中插入、删除和定位元素时；

TreeMap：在需要按自然顺序或自定义顺序遍历键的情况下；

LinkedHashMap：在需要输出的顺序和输入的顺序相同的情况下。

#### HashMap 和 HashTable 有什么区别？

- HashMap 是线程不安全的，HashTable 是线程安全的；

- 由于线程安全，所以 HashTable 的效率比不上 HashMap；

- HashMap 最多只允许一条记录的键为 null，允许多条记录的值为 null，而 HashTable 不允许；

- HashMap 默认初始化数组的大小为 16，HashTable 为 11，前者扩容时，扩大两倍，后者扩大两倍+1；

- HashMap 需要重新计算 hash 值，而 HashTable 直接使用对象的 hashCode

## 3、反射

一般情况下，我们使用某个类时必定知道它是什么类，是用来做什么的。于是我们直接对这个类进行实例化，之后使用这个类对象进行操作。

```java
Apple apple = new Apple(); //直接初始化，「正射」
apple.setPrice(4);
```

```java
Class clz = Class.forName("com.v1ncent.reflect.Apple");
Method method = clz.getMethod("setPrice", int.class);
Constructor constructor = clz.getConstructor();
Object object = constructor.newInstance();
method.invoke(object, 4);
```

#### 获取反射中的 Class 对象

在反射中，要获取一个类或调用一个类的方法，我们首先需要获取到该类的 Class 对象。

在 Java API 中，获取 Class 类对象有三种方法：

**第一种，使用 Class.forName 静态方法**。当你知道该类的全路径名时，你可以使用该方法获取 Class 类对象。

```java
Class clz = Class.forName("java.lang.String");

```

**第二种，使用 .class 方法。**

```java
Class clz = String.class;
```

**第三种，使用类对象的 getClass() 方法。**

```java
String str = new String("Hello");
Class clz = str.getClass();
```

## 4、泛型

泛型是 Java SE1.5 的新特性，泛型的本质是**参数化类型**，也就是说所操的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法。 Java 语言引入泛型的好处是安全简单。

泛型有三种使用方式，分别为：**泛型类**、**泛型接口**、**泛型方法**

### 泛型类

泛型类型用于类的定义中，被称为泛型类。通过泛型可以完成对一组类的操作对外开放相同的接口。最典型的就是各种容器类，如：List、Set、Map。

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{
    //key这个成员变量的类型为T,T的类型由外部指定
    private T key;

    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }

    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}
```

```java
复制代码
//泛型的类型参数只能是类类型（包括自定义类），不能是简单类型
//传入的实参类型需与泛型的类型参数类型相同，即为Integer.
Generic<Integer> genericInteger = new Generic<Integer>(123456);

//传入的实参类型需与泛型的类型参数类型相同，即为String.
Generic<String> genericString = new Generic<String>("key_vlaue");
Log.d("泛型测试","key is " + genericInteger.getKey());  //泛型测试: key is 123456
Log.d("泛型测试","key is " + genericString.getKey());  //泛型测试: key is key_vlaue
```

定义的泛型类，就一定要传入泛型类型实参么？并不是这样，在使用泛型的时候如果传入泛型实参，则会根据传入的泛型实参做相应的限制，此时泛型才会起到本应起到的限制作用。如果不传入泛型类型实参的话，在泛型类中使用泛型的方法或成员变量定义的类型可以为任何的类型。

```java
Generic generic = new Generic("111111");
Generic generic1 = new Generic(4444);
Generic generic2 = new Generic(55.55);
Generic generic3 = new Generic(false);

Log.d("泛型测试","key is " + generic.getKey()); //key is 111111
Log.d("泛型测试","key is " + generic1.getKey()); //key is 4444
Log.d("泛型测试","key is " + generic2.getKey()); //key is 55.55
Log.d("泛型测试","key is " + generic3.getKey()); //key is false

```

#### 注意

- 泛型的类型参数只能是类类型，不能是简单类型。
- 不能对确切的泛型类型使用 instanceof 操作。如下面的操作是非法的，编译时会出错。
  > if(ex_num instanceof Generic<Number>){ }

### 泛型接口

泛型接口与泛型类的定义及使用基本相同。泛型接口常被用在各种类的生产器中，可以看一个例子：

```java
//定义一个泛型接口
public interface Generator<T> {
    public T next();
}
```

当实现泛型接口的类，未传入泛型实参时：

```java
/**
 * 未传入泛型实参时，与泛型类的定义相同，在声明类的时候，需将泛型的声明也一起加到类中
 * 即：class FruitGenerator<T> implements Generator<T>{
 * 如果不声明泛型，如：class FruitGenerator implements Generator<T>，编译器会报错："Unknown class"
 */
class FruitGenerator<T> implements Generator<T>{
    @Override
    public T next() {
        return null;
    }
}
```

当实现泛型接口的类，传入泛型实参时：

```java
/**
 * 传入泛型实参时：
 * 定义一个生产器实现这个接口,虽然我们只创建了一个泛型接口Generator<T>
 * 但是我们可以为T传入无数个实参，形成无数种类型的Generator接口。
 * 在实现类实现泛型接口时，如已将泛型类型传入实参类型，则所有使用泛型的地方都要替换成传入的实参类型
 * 即：Generator<T>，public T next();中的的T都要替换成传入的String类型。
 */
public class FruitGenerator implements Generator<String> {

    private String[] fruits = new String[]{"Apple", "Banana", "Pear"};

    @Override
    public String next() {
        Random rand = new Random();
        return fruits[rand.nextInt(3)];
    }
}
```

### 泛型通配符

```java
public void showKeyValue1(Generic<Number> obj){
    Log.d("泛型测试","key value is " + obj.getKey());
}
```

```java
Generic<Integer> gInteger = new Generic<Integer>(123);
Generic<Number> gNumber = new Generic<Number>(456);

showKeyValue(gNumber);

// showKeyValue这个方法编译器会为我们报错：Generic<java.lang.Integer>
// cannot be applied to Generic<java.lang.Number>
// showKeyValue(gInteger);
```

我们可以将上面的方法改一下：

```java
public void showKeyValue1(Generic<?> obj){
    Log.d("泛型测试","key value is " + obj.getKey());
}
```

类型通配符一般是使用？代替具体的类型实参，注意了，此处’？’是类型实参，而不是类型形参 。重要说三遍！此处’？’是类型实参，而不是类型形参 ！ 此处’？’是类型实参，而不是类型形参 ！再直白点的意思就是，此处的？和 Number、String、Integer 一样都是一种实际的类型，可以把？看成所有类型的父类。是一种真实的类型。

可以解决当具体类型不确定的时候，这个通配符就是 ? ；当操作类型时，不需要使用类型的具体功能时，只使用 Object 类中的功能。那么可以用 ? 通配符来表未知类型。

### 泛型方法

#### 泛型方法的基本用法

```java
public class GenericTest {
   //这个类是个泛型类，在上面已经介绍过
   public class Generic<T>{
        private T key;

        public Generic(T key) {
            this.key = key;
        }

        //我想说的其实是这个，虽然在方法中使用了泛型，但是这并不是一个泛型方法。
        //这只是类中一个普通的成员方法，只不过他的返回值是在声明泛型类已经声明过的泛型。
        //所以在这个方法中才可以继续使用 T 这个泛型。
        public T getKey(){
            return key;
        }

        /**
         * 这个方法显然是有问题的，在编译器会给我们提示这样的错误信息"cannot reslove symbol E"
         * 因为在类的声明中并未声明泛型E，所以在使用E做形参和返回值类型时，编译器会无法识别。
        public E setKey(E key){
             this.key = keu
        }
        */
    }

    /**
     * 这才是一个真正的泛型方法。
     * 首先在public与返回值之间的<T>必不可少，这表明这是一个泛型方法，并且声明了一个泛型T
     * 这个T可以出现在这个泛型方法的任意位置.
     * 泛型的数量也可以为任意多个
     *    如：public <T,K> K showKeyName(Generic<T> container){
     *        ...
     *        }
     */
    public <T> T showKeyName(Generic<T> container){
        System.out.println("container key :" + container.getKey());
        //当然这个例子举的不太合适，只是为了说明泛型方法的特性。
        T test = container.getKey();
        return test;
    }

    //这也不是一个泛型方法，这就是一个普通的方法，只是使用了Generic<Number>这个泛型类做形参而已。
    public void showKeyValue1(Generic<Number> obj){
        Log.d("泛型测试","key value is " + obj.getKey());
    }

    //这也不是一个泛型方法，这也是一个普通的方法，只不过使用了泛型通配符?
    //同时这也印证了泛型通配符章节所描述的，?是一种类型实参，可以看做为Number等所有类的父类
    public void showKeyValue2(Generic<?> obj){
        Log.d("泛型测试","key value is " + obj.getKey());
    }

     /**
     * 这个方法是有问题的，编译器会为我们提示错误信息："UnKnown class 'E' "
     * 虽然我们声明了<T>,也表明了这是一个可以处理泛型的类型的泛型方法。
     * 但是只声明了泛型类型T，并未声明泛型类型E，因此编译器并不知道该如何处理E这个类型。
    public <T> T showKeyName(Generic<E> container){
        ...
    }
    */

    /**
     * 这个方法也是有问题的，编译器会为我们提示错误信息："UnKnown class 'T' "
     * 对于编译器来说T这个类型并未项目中声明过，因此编译也不知道该如何编译这个类。
     * 所以这也不是一个正确的泛型方法声明。
    public void showkey(T genericObj){

    }
    */

    public static void main(String[] args) {


    }
}
```

#### 泛型方法与可变参数

```java
public <T> void printMsg( T... args){
    for(T t : args){
        Log.d("泛型测试","t is " + t);
    }
}

printMsg("111",222,"aaaa","2323.4",55.55);
```

### 泛型上下边界

在使用泛型的时候，我们还可以为传入的泛型类型实参进行上下边界的限制，如：类型实参只准传入某种类型的父类或某种类型的子类。

为泛型添加上边界，即传入的类型实参必须是指定类型的子类型

```java
public void showKeyValue1(Generic<? extends Number> obj){
    Log.d("泛型测试","key value is " + obj.getKey());
}

Generic<String> generic1 = new Generic<String>("11111");
Generic<Integer> generic2 = new Generic<Integer>(2222);
Generic<Float> generic3 = new Generic<Float>(2.4f);
Generic<Double> generic4 = new Generic<Double>(2.56);

//这一行代码编译器会提示错误，因为String类型并不是Number类型的子类
//showKeyValue1(generic1);

showKeyValue1(generic2);
showKeyValue1(generic3);
showKeyValue1(generic4);
```

如果我们把泛型类的定义也改一下:

```java
public class Generic<T extends Number>{
    private T key;

    public Generic(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
}

//这一行代码也会报错，因为String不是Number的子类
Generic<String> generic1 = new Generic<String>("11111");
```

再来一个泛型方法的例子：

```java
//在泛型方法中添加上下边界限制的时候，必须在权限声明与返回值之间的<T>上添加上下边界，即在泛型声明的时候添加
//public <T> T showKeyName(Generic<T extends Number> container)，编译器会报错："Unexpected bound"
public <T extends Number> T showKeyName(Generic<T> container){
    System.out.println("container key :" + container.getKey());
    T test = container.getKey();
    return test;
}
```

通过上面的两个例子可以看出：泛型的上下边界添加，必须与泛型的声明在一起 。

## 5.注解

注解相当于一种标记，在程序中加了注解就等于为程序打上了某种标记。程序可以利用 ava 的**反射机制**来了解你的类及各种元素上有无何种标记，针对不同的标记，就去做相应的事件。标记可以加在包，类，字段，方法，方法的参数以及局部变量上。

## 6、IO

![image](https://raw.githubusercontent.com/v1ncent9527/AndroidInterview/main/Snapshot/iostream.webp)

### 分类

虽然 java IO 类库庞大，但总体来说其框架还是很清楚的。从是读媒介还是写媒介的维度看，Java IO 可以分为：

输入流：InputStream 和 Reader
输出流：OutputStream 和 Writer
而从其处理流的类型的维度上看，Java IO 又可以分为：

字节流：InputStream 和 OutputStream
字符流：Reader 和 Writer
下面这幅图就清晰的描述了 JavaIO 的分类：

|              | 字节流       | 字符流 |
| ------------ | ------------ | ------ |
| 输入流（读） | InputStream  | Reader |
| 输出流（写） | OutputStream | Writer |

我们的程序需要通过 InputStream 或 Reader 从数据源读取数据，然后用 OutputStream 或者 Writer 将数据写入到目标媒介中。其中，InputStream 和 Reader 与数据源相关联，OutputStream 和 writer 与目标媒介相关联。

### 字节流

用字节流写文件

```java
public static void writeByteToFile() throws IOException{
    String hello= new String( "hello word!");
     byte[] byteArray= hello.getBytes();
    File file= new File( "d:/test.txt");
     //因为是用字节流来写媒介，所以对应的是OutputStream
     //又因为媒介对象是文件，所以用到子类是FileOutputStream
    OutputStream os= new FileOutputStream( file);
     os.write( byteArray);
     os.close();
}
```

用字节流读文件

```java
public static void readByteFromFile() throws IOException{
    File file= new File( "d:/test.txt");
     byte[] byteArray= new byte[( int) file.length()];
     //因为是用字节流来读媒介，所以对应的是InputStream
     //又因为媒介对象是文件，所以用到子类是FileInputStream
    InputStream is= new FileInputStream( file);
     int size= is.read( byteArray);
    System. out.println( "大小:"+size +";内容:" +new String(byteArray));
     is.close();
}
```

### 字符流

字符流读文件

```java
public static void writeCharToFile() throws IOException{
    String hello= new String( "hello word!");
    File file= new File( "d:/test.txt");
    //因为是用字符流来读媒介，所以对应的是Writer，又因为媒介对象是文件，所以用到子类是FileWriter
    Writer os= new FileWriter( file);
    os.write( hello);
    os.close();
}
```

用字符流写文件

```java
public static void readCharFromFile() throws IOException{
    File file= new File( "d:/test.txt");
    //因为是用字符流来读媒介，所以对应的是Reader
    //又因为媒介对象是文件，所以用到子类是FileReader
    Reader reader= new FileReader( file);
    char [] byteArray= new char[( int) file.length()];
    int size= reader.read( byteArray);
    System. out.println( "大小:"+size +";内容:" +new String(byteArray));
    reader.close();
}
```

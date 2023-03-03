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

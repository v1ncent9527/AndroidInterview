# Java 面试题

## 面向对象

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

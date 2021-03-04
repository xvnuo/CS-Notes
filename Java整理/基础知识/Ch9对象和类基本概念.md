# Ch9 Object and Class

## 面向对象编程的概念object-oriented

对象表示现实世界中可以清楚识别的实体。 例如，一个学生，一个桌子，一个圆圈，一个按钮，甚至一个贷款。对象具有独特的身份、状态和行为。对象的状态由一组数据字段（也称为属性）及其当前值组成。对象的行为由一组方法定义。

对象既有状态又有行为。状态定义对象，行为定义对象做什么。

##### 1.类Class的概念

类定义了相同类型的对象。Java类使用变量variables定义数据阈data fields,使用方法定义行为。构造函数--用于从类创建对象。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216224952803.png" alt="image-20201216224952803" style="zoom:67%;" />

##### 2.UML图的绘制要素

UML图中的+表示public限定词修饰

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216225038153.png" alt="image-20201216225038153" style="zoom:80%;" />

![image-20201216225134892](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216225134892.png)

##### 3.构造函数constructors

构造函数是用来构造对象的一种特殊方法。①没有参数的构造函数称为无参构造函数no_arg constructor。②构造函数必须和类名同名；③构造函数没有返回类型——甚至不是void类型；④在创建对象时使用new运算符调用构造函数。构造函数起到初始化对象的作用：new ClassName();

默认构造函数default constructors--类可以在没有自定义构造函数的情况下定义。在这种情况下，类中隐式定义一个空体的无参构造函数。当且仅当在类中没有显式定义构造函数时，才会自动提供称为默认构造函数的构造函数。

##### 4.声明对象引用变量Object Reference Variable

若要引用对象，需要将对象赋值assign给引用变量reference variable。

声明方式：ClassName objectRefVar;

声明/创建一步到位：ClassName objectRefVar=new ClassName();//创建并赋值

##### 5.对象内的成员变量/函数

--访问数据 objectRefVar.data

--调用成员函数 objectRefVar.methodName(arguements)

实例化类--一个类可以有很多实例/对象，不能通过类名直接调用函数，只能通过引用对象的变量名。静态类，如Math, Character等用static定义，必须通过类名来访问或调用成员变量/函数。例：myCircle.getArea();

引用数据域reference date fields:类中的成员变量的类型可以是引用类型，如：

成员变量name的类型是String，String就是一个引用类

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216230728226.png" alt="image-20201216230728226" style="zoom: 80%;" />

##### 6.默认值default value

成员变量可通过默认构造函数初始化，所以都有各自类型的默认值：①数据类型--0；②字符串类型/引用类型null；③boolean类型false；④char类型：‘\u0000'.

以下变量都是成员变量，分别输出各自的默认值<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216230943003.png" alt="image-20201216230943003" style="zoom:80%;" />

注意区分：Java不给方法内的局部变量local variable inside a method分配默认值。如果一个局部变量没有初始化就使用，会发生编译错误报错。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216231332368.png" alt="image-20201216231332368" style="zoom: 67%;" />

Java VS. c++

编译器会为这些数据成员进行**默认初始化**，实际上是把刚分配的对象内存都置零.在对象里定义一个引用，且不将其初始化时，默认初始化为null。这种默认初始化的实现是，在创建（new）一个对象时，在堆上对对象分配足够的空间之后，这块存储空间会被清零，这样就自动把基本类型的数据成员都设置成了默认值。默认初始化动作之后，才执行指定初始化。也就是说下面的i经历过被初始化为0后，再赋值为999的过程。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217101505830.png" alt="image-20201217101505830" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217101525036.png" alt="image-20201217101525036" style="zoom:80%;" />

java也可以使用构造函数来进行初始化，但构造函数的初始化无法阻止指定初始化和默认初始化的进行，而且总是在它们之后，才会执行构造函数初始化。总结起来说，java中数据成员的初始化过程是： 

– ① 先默认初始化 ② 进行定义处的初始化（指定初始化）③ 构造函数初始化

C++禁止在定义数据成员时就进行指定初始化，而且C++也没有默认初始化：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217101803317.png" alt="image-20201217101803317" style="zoom:80%;" />

上面实际上是C++的默认构造函数进行的构造函数初始化。当类没有构造函数时，编译器会为类声明并实现一个默认构造函数，默认构造函数将数据成员初始化为默认值。所以C++数据成员的初始值，只能依赖：①成员初始化列表②构造函数

##### 7.null类型

若一个引用类型reference type不引用reference任何对象，则指向null

##### 8.原始数据类型primitive data types和对象类型object types

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217101951848.png" alt="image-20201217101951848" style="zoom:80%;" />

两种类型的复制过程也不同：引用类型--改变引用的指向

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217102110540.png" alt="image-20201217102110540" style="zoom:80%;" />

##### 9.垃圾回收机制Garbage Collection

垃圾garbage--不再被任何引用变量reference variable引用的对象object称为垃圾。这种垃圾被JVM自动回收。

如果不再需要这个对象，可以显式地explicitly将null分配给这个对象的引用变量reference variable。那么该对象就不再被任何变量引用，JVM将自动收集空间。

一般而言，只要类自己管理内存，程序员就应该警惕内存泄漏问题。如前面Stack类自己管理内存: elements数组：这里elements是存储池，size大小的内存则是活动区域，而剩余部分则是free的。但，垃圾回收器并不知道这个。所以，对于垃圾回收器来说，elements中的所有对象引用都是同等有效的。而只有程序员才知道非活动区域是不重要的。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217102803331.png" alt="image-20201217102803331" style="zoom:80%;" />

内存泄漏另一个常见的来源是缓存。当把对象引用放到缓存中，就容易被遗忘。可以用软引用(Soft Reference)来实现缓存。（Java中提供了4个级别的引用：强应用、软引用、弱引用和虚引用）

平时我们用的是强引用（**Final Reference**），如Object obj = new Object()这类的引用，只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象。 JVM宁愿抛出OOM异常也不回收强引用所指向的对象。

**软引用**：是用来描述一些还有用但并非必须的对象。对于软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中进行第二次回收。如果这次回收还没有足够的内存，才会抛出内存溢出异常。软引用主要应用于内存敏感的高速缓存，在android系统中经常使用到。一般情况下，Android应用会用到大量的默认图片，这些图片很多地方会用到。如果每次都去读取图片，由于读取文件需要硬件操作，速度较慢，会导致性能较低。所以我们考虑将图片缓存起来，需要的时候直接从内存中读取。但是，由于图片占用内存空间比较大，缓存很多图片需要很多的内存，就可能比较容易发生OutOfMemory异常。这时，我们可以考虑使用软引用技术来避免这个问题发生。

## Date类

Java实现了独立于system-independent系统的时间和日期的封装类java.util.Date。可使用Date类创建当前日期和时间的实例instance，使用toString方法返回当前时间和日期的字符串形式。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217103454931.png" alt="image-20201217103454931" style="zoom:80%;" />

```java
java.util.Date date = new java.util.Date();
System.out.println(date.toString());
//输出：Sun Mar 09 13:50:19 EST 2003
```

## Random类

java.util.Random类提供产生随机数的方法

![image-20201217103735318](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217103735318.png)

如果两个随机对象Random object具有相同的种子seed，它们将生成相同的数字序列identical sequences of numbers。例如，下面的代码创建两个具有相同种子3的随机对象:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217104015745.png" alt="image-20201217104015745" style="zoom:80%;" />

## 实例、变量和方法instance，variable and methods

##### 1.实例

实例变量属于一个特定的实例。实例方法需要通过一个特定类的特定实例调用。

##### 2.静态变量、常量和方法

用static声明。静态变量被类的所有实例共享。静态方法不和某一个特定的对象捆绑在一起，而是属于这个类。静态常量即final 类的变量，被这个类的所有实例所共享。

Static Methods只能访问Static Variables，不能访问Instance variables; 可以调用其他的Static methods，但不能调用instance methods. Instance methods既能访问instance variables，也能访问static variables; 既能调用instance methods，也能调用static methods。UML中用下划线表示静态方法或变量。

![image-20201217104540038](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217104540038.png)

## Visibility Modifiers and Accessor/Mutator Methods

默认情况下(没有限制符)，类\变量或方法可以由同一包package中的任何类访问。

##### 1.public

public修饰的类、数据或方法可以被任意包里的任意类class可见

##### 2.private

private修饰的类数据或方法只能被该声明类declaring内的数据或方法访问。可以通过get\set方法来获取private修饰限定的方法或数据

![image-20201217105136988](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217105136988.png)

private私有修饰符限制类内within a class的访问，默认default修饰符限制包within a package内的访问，公共修饰符public允许不受限制unrestricted access的访问。

对象无法在它的类外访问其私有成员private member，如(b)所示。但是，如果对象是在自己的类中声明并访问的，它是可以的，如(A)所示

![image-20201217105403356](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217105403356.png)

private--保护数据，使得易于维护maintain。数据封装例子：-表示private限制符

![image-20201217105622152](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217105622152.png)

##### 3.private限定的构造函数constructor

该构造函数不能创建类的实例，类只能被静态访问：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217105820539.png" alt="image-20201217105820539" style="zoom:80%;" />

能创建类的实例，但是只能被类的静态方法调用。常见场景：单例模式。

![image-20201217105933598](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217105933598.png)

![image-20201217110010970](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217110010970.png)

只能被其他构造函数调用，用于减少重复代码：

![image-20201217110043878](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217110043878.png)

## 类和对象的生命周期

当程序运行的时候，第一次new创建一个类的对象，或通过类名访问静态变量或静态方法时，java会将类加载进内存，为这个类分配一块空间（其实是方法区），这块空间会包括类的定义、它的变量和方法信息，还有类的静态变量，并对静态变量赋值。类加载进内存后，一般不会释放，直到程序结束。一般情况，类只会加载一次，所以静态变量在内存中只存在一份。通过new创建一个对象时，对象产生，在内存中会存储这个对象的实例变量值。每new一次，都会产生一个变对象，会有一份独立的实例变量。

## 将对象作为函数的形参传递

①传递原始类型值primitive types的值（该值传递给参数）；②传递引用类型的值(这个值是该对象的引用)

![image-20201217110416390](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217110416390.png)

很多程序设计语言（特别是C++, Pascal)提供了两种参数传递的方式：值传递和引用传递。有些程序员会认为JAVA采用的是引用传递，实际上，这是不对的。其实本质上，JAVA对象引用还是按值传递的。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217130518581.png" alt="image-20201217130518581" style="zoom: 67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217130542987.png" alt="image-20201217130542987" style="zoom: 50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217130623528.png" alt="image-20201217130623528" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217130812800.png" alt="image-20201217130812800" style="zoom: 50%;" />

## 对象数组array of objects

Circle[] circleArray = new Circle[10];

对象数组实际上是对象的引用变量的数组。调用诸如circleArray[1].getArea()实际上是两级引用，首先是circleArray引用了整个数组，然后是circleArray[1]引用了这个类的一个对象。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217131156714.png" alt="image-20201217131156714" style="zoom:80%;" />

## 不可变对象和类immutable

##### 1.不可变类和对象概念

如果一个对象的内容一旦被创建就不能改变once the object is created，那么这个对象就被称为不可变对象immutable object，它的类被称为不可变类immutable class。

所有数据字段data fields都为private限制的，且没有mutator(叛变者？)的类不一定是不可变的immutable。例如，下面的类Student有所有私有数据字段private data field，没有mutator，但是它是mutable可变的。

![image-20201217131811529](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217131811529.png)

##### 2.不可变类的特性和实现

要使类是不可变的，它必须把所有数据字段data field标记为私有private，并且不提供mutator方法methods和“可以返回对可变数据字段对象引用的访问方法accessor methods”

Java平台类库包含许多不可变的类，如String, 基本类型的包装类(如Integer, Long等），BigInteger和BigDecimal。Java 8中提供了LocalDate, LocalTime, Local Date Time也是不可变类。

不可变类的优点：更易于设计、实现和使用，更安全。使类变为不可变类，要遵循下面五条规则：

–①不要提供任何会修改对象状态的方法（mutator）–②保证类不会被扩展。防止粗心或恶意的子类假装对象的状态已改变。一般做法是使这个类为final。–③使所有域都是final的 –④使所有域都是private的–⑤确保对于任何可变组件的互斥访问⑥如果类有指向可变对象的域，则必须确保该类的客户端无法获得指向这些对象的引用。

为了确保不可变类，类不允许自身被子类化。除了“使类成为final”之外，还有一种更加灵活的方式：让类的所有构造器都变为私有的或包级私有的，并添加公共的静态工厂来代替公有的构造器。如：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217134445974.png" alt="image-20201217134445974" style="zoom: 50%;" />

Constant Pool(Java常量池技术)：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217134530983.png" alt="image-20201217134530983" style="zoom:67%;" />

##### 3.必要时进行保护性拷贝

![image-20201217132536405](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217132536405.png)

*类具有公有的静态**final**数组域，或者返回这种域的访问方法，这是安全漏洞的一个常见根源*

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217133236906.png" alt="image-20201217133236906" style="zoom: 50%;" />

##### 4.不可变类的优点

不可变对象比较简单，可以只有一种状态，即被创建时的状态。不可变对象本质上是线程安全的。不可变的类可以提供静态工厂，把频繁被请求的实例缓存起来。基本类型的包装类和BigInteger都有这样的静态工厂。不需要进行保护性拷贝，因为拷贝始终都是原始的对象。如：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217133803228.png" alt="image-20201217133803228" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217133822611.png" alt="image-20201217133822611" style="zoom: 50%;" />

##### 5.不可变类的缺点

对于每一个不同的值都需要一个单独的对象。如：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217134104222.png" alt="image-20201217134104222" style="zoom:50%;" />

虽然只是修改了一个bit，但是flipBit需要创建一个新的BigInteger，消耗时间和空间。如果执行一个多步骤的操作，则每个步骤都会产生一个新的对象，除了最后的结果之外其他对象都会被丢弃。与BigInteger类似，BitSet代表一个任意长度的位序列，但其是可变的。

String类也是不可变类，String的可变配套类为StringBuilder

## Java反编译工具JAD， Java Decompile

![image-20201217134301592](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217134301592.png)



## 变量作用域、作用范围scope

##### 1.对象实例和静态变量

对象实例和静态变量的作用域是整个类，他们可以在类内的任意地方被声明。

##### 2.局部变量

局部变量的作用范围是从它开始被声明的地方到包含这个变量的块结束。局部变量不会被赋默认值，在使用前一定要显式初始化initialized explicitly。

##### 3.this关键字

是代表该对象本身的引用。可以用来调用该类中的隐藏数据阈。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217140911801.png" alt="image-20201217140911801" style="zoom:80%;" />

还可使构造函数能够调用同一类的另一个构造函数。调用重载的构造函数：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217141010691.png" alt="image-20201217141010691" style="zoom: 50%;" />

## 对象构建的建议

##### 1.遇到多个构造器参数时可以考虑用构建器Builder

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217141227703.png" alt="image-20201217141227703" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217141247553.png" alt="image-20201217141247553" style="zoom:80%;" />

这种方式通常需要许多你本不想设置的参数，但还是不得不为它们传递值，这个例子中fat传递了一个为0的初始值。如果参数仅此6个，情况还不算太糟糕，但是随着成员域的增多，这种情况很快就会失去控制。首先更多的参数构造器会让代码非常难以阅读，使用这个类需要非常仔细的探究每个参数具体是什么意思；其次一长串类型相同的参数会导致一些人为的微妙错误，比如客户端不小心颠倒了其中连个参数的位置，此时编译器不会报错，但是程序运行会有错误的显示

另一种方案是：用一个无参的构造器来创建对象，然后调用setter方法来设置每个必要的参数，以及每个相关的可选参数。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217141415752.png" alt="image-20201217141415752" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217141518527.png" alt="image-20201217141518527" style="zoom:80%;" />

这种方式将构造过程分到了几个调用中，在构造过程中对象可能处于不一致的状态。类无法仅仅通过校验构造器参数的有效性来保证一致性。阻止了把类做成不可变类的可能，这时需要程序员付出额外的努力来确保它线程安全。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217141606310.png" alt="image-20201217141606310" style="zoom:80%;" />

这里 NutritionFacts 是不可变的，所有默认参数值都单独放在一个地方。builder 的具名的 setter 方法返回 builder 本身，以便可以进行类似链式操作的方式进行可选参数的赋值，客户端代码可以像这样：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217142638244.png" alt="image-20201217142638244" style="zoom:80%;" />

##### 2.用静态工厂方法代替构造器

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217142812968.png" alt="image-20201217142812968" style="zoom:80%;" />

静态工厂有名字，更易于阅读：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217142951323.png" alt="image-20201217142951323" style="zoom:80%;" />

静态工厂不必在每次调用它们的时候都创建一个新的对象，这使得不可变类可以使用预先构建好的实例，或者将构建好的实例缓存起来，进行重复利用。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217143149103.png" alt="image-20201217143149103" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217143231782.png" alt="image-20201217143231782" style="zoom:80%;" />

静态工厂可以返回原返回类型的任何子类型的对象。可以选择返回对象的类有更大的灵活性。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217143323693.png" alt="image-20201217143323693" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217143357872.png" alt="image-20201217143357872" style="zoom:80%;" />

##### 3.单例对象构建

Singleton通常用于那些本质上唯一的对象，如文件系统、窗口管理器等。

实现方法：

有特权的客户端可以借助AccessibleObject.setAccessible方法，通过反射机制，调用私有构造器。要抵御这种攻击，可以修改构造器，要求创建第二个实例的时候抛出异常。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217143528210.png" alt="image-20201217143528210" style="zoom: 67%;" />

实现方法--编写一个包含单个元素的枚举。Enum提供了序列化的方式，而且绝对防止多次实例化。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217143638181.png" alt="image-20201217143638181" style="zoom: 50%;" />

## 包Package

##### 1.类的导入

①可以在每个类前添加完整的包名：

java.time.LocalDate today = java.time.LocalDate.now();

②可以使用import语句：

import java.time.*;    LocalDate today = LocalDate.now();

③也可以只导入包中的特定类：import java.time.LocalDate;

import java.time.*的语法比较简单，对代码的大小也没有任何负面的影响。但如果能明确指出导入的类，可以让读者明确知道加载了哪个类。

当发生命名冲突时，需要注意包的名字，如java.util和java.sql都有Date类，如果同时有：

```java
import java.util.*
import java.sql.*
Date today;//就会发生编译错误

需要进行命名的明确：
import java.util.Date;
java.util.Date today = new java.util.Date();
```

import不会递归，只会引入当前package下的直接类。import.java.util.*只会引入java.util下的直接类，不会引入它的嵌套包的类。试图嵌套引入的形式也是无效的，如

> import.java.util.*.* 

##### 2.import和include

C++程序员会将import与#include弄混。其实，两者并没有共同之处。C++中需要用#include将外部声明加载进来，这是因为C++编译器只能查看正在编译的文件和include的文件；而java编译器可以查看其他文件，只要告诉它到哪里去查看就可以了。在Java中，显式给出包名时（如java.util.Date），则不需要import；而C++中无法避免用#include。Java中，package与import类似于C++中的namespace和using

##### 3.默认import java.lang.*

我们在程序中经常使用System.out这个类，为什么没有import System.out呢，因为java.lang 这个套件实在是太常用到了，几乎没有程序不用它的，所以不管你有没有写 import java.lang;，编译器都会自动帮你补上，也就是说编译器只要看到没有package的类别，它就会自动去 java.lang 里面找找看，看这个类别是不是属于这个套件的。所以我们就不用特别去import java.lang 了.

##### 4.静态导入

import不仅可以导入类，还能导入静态方法和静态域。如：

> import static Java.lang.System.*;

就可以是哦那个System类的静态方法和静态域了：

> out.println("Hello World!");//相当于System.out
>
> exit(0);//相当于System.exit

也可以静态导入特定的方法或域：

> import static java.lang.System.out

##### 5将类放到包中

要将类放到包中，必须将包的名字放在源文件的开始，例如：

> package Com.CN.test;
>
> public class TestMain
>
> {
>
> ......
>
> }

如果没有放置package语句，则这个类被放置在默认包（default package)中。需要将类文件切实安置到其所归属之Package所对应的相对路径下。如上述例子，需要将TestMain.java文件放到Com/CN/test目录下，编译器也会将class文件放到相同的目录结构中。

##### 6.编译器在编译源文件时不检查目录结构：

若放错目录，编译不会出错，在执行时反而会出错。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217145617742.png" alt="image-20201217145617742" style="zoom:80%;" />

## JAR

Java ARchive。是 一组Java类classes和支持的文件supported files组合成一个用ZIP格式压缩的文件，并给出.JAR扩展extension。

优点：①compressed;quicker download; ②just one file; less mess;③can be executable

##### 1.创建JAR archive

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217150029481.png" alt="image-20201217150029481" style="zoom: 67%;" />

##### 2.运行JAR

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217150058695.png" alt="image-20201217150058695" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217150144579.png" alt="image-20201217150144579" style="zoom:67%;" />

##### 3.JAR内部的资源

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217150217545.png" alt="image-20201217150217545" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201217150232277.png" alt="image-20201217150232277" style="zoom:67%;" />


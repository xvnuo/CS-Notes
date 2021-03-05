# Ch13 Exception Handling and IO

## 例子引入

##### 1.没有异常处理的除法，number2=0时必定发生异常

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222115253345.png" alt="image-20201222115253345" style="zoom:50%;" />

##### 2.带有if语句，排除异常情况

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222115406093.png" alt="image-20201222115406093" style="zoom: 50%;" />

##### 3.用函数方法处理异常情况

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222115448741.png" alt="image-20201222115448741" style="zoom:50%;" />

## 异常处理机制

##### 1.异常机制的优势

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222115710073.png" alt="image-20201222115710073" style="zoom:50%;" />

异常处理exception handling使得方法可以向调用者抛出异常throw an exception to its caller.没有这个机制，方法必须解决异常情况或终止程序。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222115807374.png" alt="image-20201222115807374" style="zoom:50%;" />

通过异常处理InputMismatchException，程序可以持续读入直到读入正确。

##### 2.异常的类型exception types

①系统错误system error:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222120033757.png" alt="image-20201222120033757" style="zoom:80%;" />

所有系统错误system error都是可抛出的throwable。系统错误由JVM抛出，用Error类表示。Error类描述内部系统操作internal system error，这些错误很少发生，如果发生了，除了通知用户和尝试终止程序，没有其他解决方式。

②异常Exceptions

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222120626229.png" alt="image-20201222120626229" style="zoom:80%;" />

异常描述了由程序program或外部环境external造成的错误。这些错误可以被捕获caught并由程序program解决--通过try...catch

运行时异常runtime exception：是由程序program error错误造成的，如错误的类型转换bad casting, 数组越界accessing an out-of-bounds array、数组错误numeric errors。

##### 3.Checked Exception VS Unchecked Exceptions

Java把所有非正常情况分为两种：异常和错误，他们都继承throwable分类

RuntimeException、Error和他们的子类被称为非检查异常，其他异常为称为检查性异常。

检查异常就是编译器要求你必须处理的异常。比如我们在编程某个文件的读写时，编译器要求你必须要对某段代码try...catch...或者抛出异常，这就是检查异常。简单的来说，你代码还没有运行，编译器就会检查你的代码，对可能出现的异常必须做相对的处理。

非检查异常，编译器不要求强制处置的异常，虽然有可能出现错误，但是不会再编译的时候检查，主要为runtime异常和error，反应了程序的逻辑错误。程序中可以选择捕获处理也可以不处理。这种异常通常是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生，运行时异常的特点是Java编译器不会检查它，当程序中可能出现这类异常，即使没有用try...catch捕获它，也没有用throws它也会编译通过。

异常处理的主要过程：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222175154326.png" alt="image-20201222175154326" style="zoom:80%;" />

##### 4.异常的声明declaring exceptions

每个方法method都必须声明它里面可能存在的checked exceptions，即异常的声明。

> public void myMethod() throws IPException;
>
> public void myMethod() throws IOException, OtherException

##### 5.抛出异常throwing exceptions

当程序检测到错误，会创建一个对应的异常类型appropriate exception type的实例instance并抛出它，这个过程就是抛出异常。

> throw new TheException();//throw关键字+异常实例对象
>
> TheException ex = new TheException();
>
> throw ex;

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222175819412.png" alt="image-20201222175819412" style="zoom: 50%;" />

##### 6.捕获异常catching exceptions

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222180015426.png" alt="image-20201222180015426" style="zoom:50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222180030930.png" alt="image-20201222180030930" style="zoom:80%;" />

try内的语句是真正要做的东西，里面可能会调用函数，函数里面内用if...throws检测是否存在异常，若存在则抛出该异常，try运行到该语句的时候若捕获到异常，则执行catch后的语句。

the stack trace：追踪栈，记录正在执行的所有方法的内部列表internal list。是程序运行时调用的一连串的方法的记录追踪。它记录了当异常发生时正在执行的方法以及所有为了这个方法执行而被调用的方法。如StackTrace.java:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222181141812.png" alt="image-20201222181141812" style="zoom:67%;" />

java强迫你去处理checked exception，如果一个方法声明了一个checked exception，你必须去在try-catch中调用它，或者在调用方法中声明throw the exception.例如：方法p1中调用来方法p2, p2可能会抛出checked exception，那么就必须在p1中用try-catch去调用这个checked exception或者在p1的开头声明throws exception。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222181558491.png" alt="image-20201222181558491" style="zoom:67%;" />

##### 7.throws和throw要区分清楚

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222181708961.png" alt="image-20201222181708961" style="zoom: 50%;" />

函数开头声明，用throws；在函数内部真正抛出异常的时候，用throw

##### 8.多重throw，rethrowing和finally

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222182702787.png" alt="image-20201222182702787" style="zoom:50%;" />

①若statements中无异常，执行该语句，然后跳到finally中执行finalStatement, 然后跳出来执行next statement.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222183613450.png" alt="image-20201222183613450" style="zoom:50%;" />

②若statement2中抛出了异常，跳过statement3，跳到catch中，解决异常handling ex.然后跳到finally中执行finalStatements，最后执行nextStatement.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222183816904.png" alt="image-20201222183816904" style="zoom:50%;" />

③如果statement2抛出了类型2的异常Exception2，先跳到第二个catch语句，解决异常，解决好之后跳到finally语句，完结。④如果在解决异常2的过程中又抛出了异常throw ex，即rethrow, 则控制权被转移给了调用该段代码的单元，该段catch代码会到该语句戛然而止，然后跳出执行finally语句，后面的next statement也会终止，由掌握控制权的另一端代码来解决异常。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222184333916.png" alt="image-20201222184333916" style="zoom:80%;" />

上述代码在发生2/0的异常后，直接跳出try块，执行完finally部分，然后结束。

##### 9.Java Exception使用规范

①不要忽略checked exception

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222184502230.png" alt="image-20201222184502230" style="zoom: 67%;" />

这段代码捕获了异常却不作任何处理，可以算得上Java编程中的杀手。调用一下printStackTrace算不上“处理异常”。调用printStackTrace对调试程序有帮助，但程序调试阶段结束之后，printStackTrace就不应再在异常处理模块中担负主要责任了。

②不要捕获unchecked exception

有两种unchecked Exception：①Error：这种情况属于JVM发生了不可恢复的故障， 例如内存溢出，无法处理。②RuntimeException：这种情况属于错误的编码导致的，出现异常后需要修改代码才能修复，一般来说 catch后没有恰当的处理方式，因此不应该捕获。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222185935687.png" alt="image-20201222185935687" style="zoom: 50%;" />

③不要一次捕获所有的异常

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222190015148.png" alt="image-20201222190015148" style="zoom:67%;" />

这里有两个潜在的缺陷: 一是针对try块中抛出的每种Exception，很可能需要不同的处理和恢复措施，而由于这里只有一个catch块，分别处理就不能实现 。 二是try块中还可能抛出RuntimeException,代码中捕获了所有可 能抛出的Runtime Exception而没有作任何处理，掩盖了编程的错误 ，会导致程序难以调试。

④使用finally块释放资源

资源：程序中使用的数量有限的对象，或者只能独占式访问的对象 。例如：文件、线程、线程池、数据库连接、ftp连接。因为资源是 “有限的”，因此资源使用后必须释放，以避免程序中的资源被耗 尽，影响程序运行。某些资源，使用完毕后会自动释放，如线程。 某些资源则需要显示释放，如数据库连接。

finally关键字保证无论程序使用任何方式离开try块，finally中的语句都会被执行。在以下情况下，finally块的代码都会执行：①try块中的代码正常执行完毕。②在try块中抛出异常。③在try块中执行return、break、continue。④catch块中代码执行完毕。⑤在catch块中抛出异常。⑥在catch块中执行return、break、continue。

⑤finally块不能抛出异常

JAVA异常处理机制保证无论在任何情况下都先执行finally块的代码， 然后再离开整个try，catch，finally块。 – 在try，catch块中向外抛出异常的时候，JAVA虚拟机先转到finally块执行finally块中的代码，finally块执行完毕后，再将异常抛出。 – 但如果在finally块中抛出异常，try，catch块的异常就不能抛出，外部捕捉到的异常就是finally块中的异常信息，而try，catch块中发生 的真正的异常堆栈信息则丢失了。

⑥抛出自定义异常时带上原始异常信息

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222190327878.png" alt="image-20201222190327878" style="zoom:80%;" />

⑦打印异常信息时带上异常堆栈

不是打印e,而是调用e.printStack()

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222190439321.png" alt="image-20201222190439321" style="zoom:80%;" />

⑧不要使用同时使用异常机制和返回值来进行异常处理

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222190553403.png" alt="image-20201222190553403" style="zoom:80%;" />

如果有多种不同的异常情况，就定义多种不同的异常，而不 要像上面代码那样综合使用Exception和返回值。

⑨不要让try块过于庞大

出于省事的目的，很多人习惯于用一个庞大的try块 包含所有可能产生异常的代码，这样有两个坏处： 阅读代码的时候，在try块冗长的代码中，不容易知道到底 是哪些代码会抛出哪些异常，不利于代码维护。使用try捕获异常是以程序执行效率为代价的，将不需要捕获异常的代码包含在try块中，影响了代码执行的效率。

⑩守护线程中需要catch runtime exception

守护线程是指在需要长时间运行的线程，其生命周期一般和整个程序的时间一样长，用于提供某种持续的服务。例如服务器的告警定时同步线程，客户端的告警分发线程。由于守护线程需要长时间提供服务，因此需要对runtime exception进行保护，避免因为某一次偶发的异常而导致线程被终止。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222190804943.png" alt="image-20201222190804943" style="zoom:80%;" />



try只与catch语句块使用时，可以使用多个catch语句来捕获 try语句块中可能发生的多种异常。异常发生后，Java虚拟 机会由上而下来检测当前catch语句块所捕获的异常是否与 try语句块中某个发生的异常匹配，若匹配，则不执行其他 的catch语句块。如果多个catch语句块捕获的是同种类型的异常，则捕获子类异常的catch语句块要放在捕获父类异常的catch语句块前面。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222190927900.png" alt="image-20201222190927900" style="zoom: 50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222191020229.png" alt="image-20201222191020229" style="zoom:50%;" />

B是A的子类，B异常也是A类异常，这样的过程要把子异常放在父亲异常的前面，否则会发生错误。

##### 11.异常处理机制的优缺点

异常处理将错误处理error-handling和正常的代码分开处理，让整个程序更易于理解。但是它通常也需要更多的时间和资源因为它需要实例化异常对象、回滚rolling back the call stack 以及propagating the errors to the calling methods将错误返回给调用它的方法。

##### 10.when to throw/use  exceptions

异常发生在方法中occurs in a method.如果想要异常被它的调用者加工processed，需要创建异常对象并抛出它。如果可以在该方法中解决异常，那么就不需要抛出。

什么时候应该将try-catch用在代码中？应该用它来解决unexpected错误，不要用来处理简单，意想得到的场景，如

```java
try {
    System.out.println(refVar.toString());
}
catch (NullPointerException ex) {
    System.out.println("refVar is null");
}

//可以代替为：
if (refVar != null)
    System.out.println(refVar.toString());
else System.out.println("refVar is null");
```

##### 11.Defining Custom Exception Classes定义常规的异常类

通过扩展extending异常类或异常类的子类来实现Exception or a subclass of Exception.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222193050880.png" alt="image-20201222193050880" style="zoom: 50%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222193112529.png" alt="image-20201222193112529" style="zoom:67%;" />

通过扩展Exception类定义了自定义异常类InvalidRadiusException，CircleWithRadiusException在方法setRadius中进行情景的模拟，若radisu小于0，则抛出该自定义异常。主函数main中进行了测试，调用了CircleWith...方法，并用catch来捕获是否存在抛出的异常。

## 断言Assertions

##### 1.概念

是一个Java Statements陈述，运行在程序开始插入一个设想assert an assumption. 包含了一个Boolean表达式，该表达式应该在程序执行时保持true.断言可以用来确保程序执行时的正确性，避免逻辑错误logic errors。

##### 2.声明declaring

assertion是一个Boolean表达式，detailMessage是原始数据类型或一个Object值。用关键字assert或以下语句声明：

> assert assertion
>
> assert assertion：detailMessage

##### 3.执行executing

当执行断言语句时，Java计算evaluates断言。如果为false，将抛出断言错误AssertionError。断言错误类有一个无参构造函数和七个重载的int、long、float、double、boole、char和Object类型的单参数构造函数。

对于没有详细消息的第一个assert语句，使用无参构造函数。 对于带有详细消息的第二个断言语句，使用适当的断言错误构造函数来匹配消息的数据类型。由于断言错误是错误类Error的子类，当断言变为false时，程序将在控制台上显示消息并退出。

```java
public class AssertionDemo {
    public static void main(String[] args) {
        int i, sum = 0;
        for (i = 0; i < 10; i++) {
            sum += i; 
		}
		assert i == 10;
		assert sum > 10 && sum < 5 * 10 : "sum is " + sum;
	}
}
```

由于assert是JDK1.4中引入的一个新的Java关键字，所以必须使用JDK1.4编译器来编译程序。 此外，还需在编译器命令中包含switch-source1.4，如下所示：javac-source1.4 AssertionDemo.java 注意：如果使用JDK1.5，则不需要在命令中使用-source1.4选项。

默认情况下，断言在运行时被禁用disabled at running。要启用它，请使用switch-enableassertions，或者-ea，如下所示：

> java -ea AssertionDemo

可以在类级别或包级别选择性地启用或禁用断言。 禁用开关是-disableassertions或-da。 例如，下面的命令启用包package1中的断言，并禁用类Class1中的断言。 

> java-ea：package1-da：Class1 AssertionDemo

##### 4.断言和异常处理机制的区别

断言不应用于替换异常处理。异常处理处理程序执行过程中的异常情况。保证程序的正确性。异常处理解决健壮性问题，断言解决正确性问题。与异常处理一样，断言不用于正常测试，而是用于内部一致性和有效性检查。断言在运行时被检查，并且可以在启动时打开或关闭。

不要在公共方法中使用断言进行参数检查。可以传递给公共方法的有效参数被认为是该方法合同的一部分。无论是否启用或禁用断言，都必须始终遵守合同。 

用断言来重申假设reaffirm assumptions。这给了你更多的信心来确保程序的正确性。断言的一个常见用途是用代码中的断言替换假设。

断言的另一个很好的用法是将断言放置在没有默认情况的switch语句中。 例如

```java
switch (month) {
    case 1: ... ; break;
    case 2: ... ; break;
        ...
    case 12: ... ; break;
	default: assert false : "Invalid month: " + month
}
```

## File类

##### 1.概念

File Class文件类的目的是提供一个抽象abstraction，以一种与机器无关的方式处理文件和路径名称的大多数与机器相关的复杂性。文件名是一个字符串。文件类是文件名及其目录路径directory path的包装类wrapper class。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222195403768.png" alt="image-20201222195403768" style="zoom:80%;" />

## Text I/O

文件对象File object封装encapsulates文件或路径的属性，但不包含from/to文件read/write数据的方法。为了执行I/O，需要使用适当的Java I/O 类创建对象。对象包含读取/写入文件数据的方法。本节介绍如何使用Scanner和Print Writer类from/to文本文件read/write字符串和数值。

##### 1.Readers, Writers and Streams

存储数据的两种形式text和binary format

①文本形式：人类可读的形式，由一系列字符组成。更方便人类：更容易产生输入和检查输出.Readers和Writers以文本形式处理数据。

②二进制格式：数据项以字节byte表示。更紧凑更高效。Stream流来处理。

##### 2.处理输入输出的相关类

Java Stream相关类是用来处理字节流的,但不适合用来字符流。因为一个字节是8bit，而一个字符是16bit。字符串是由字符组成，字符串类型天然处理的是字符而不是字节。更重要的是，字节流无法知道字符集及其字符编码。Java中可以用Reader / Writer相关类来处理字符。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222200416296.png" alt="image-20201222200416296" style="zoom:80%;" />

##### 3.用PrintWriter类写数据

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222200530790.png" alt="image-20201222200530790" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222200621566.png" alt="image-20201222200621566" style="zoom: 50%;" />

创建文件类对象，创建一个文件，保存写入的数据。用PrintWriter创建输出流output,通过output将输出的字符都写入该文件。

##### 4.Scanner处理数据的读入

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222200849691.png" alt="image-20201222200849691" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222200908452.png" alt="image-20201222200908452" style="zoom:67%;" />

可以指定字符集，否则会使用运行这个程序的机器的默认编码，可能在不同的机器上有不同的表现。如：

> PrintWriter out = new PrintWriter("myfile.txt", "UTF-8");//UTF字符集编码

Scanner也是一样，可以设定字符编码

> Scanner in = new  Scanner(Paths.get(“myfile.txt”),”UTF-8”);
>
> Scanner in = new Scanner(“myfile.txt”); //ERROR?

构造了一个带字符串参数的Scanner，将字符串解释为数据，而不是文件名。

> Scanner in = new Scanner(new File(“myfile.txt”),”UTF-8”);



##### 5.从Web上读取数据

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222204635412.png" alt="image-20201222204635412" style="zoom: 50%;" />

> URL url = **new** URL(**"www.google.com/index.html"**);

创建URL对象后，可以使用URL类中定义的openStream()方法打开输入流，使用此流创建Scanner对象如下：

> Scanner input = **new** Scanner(url.openStream());
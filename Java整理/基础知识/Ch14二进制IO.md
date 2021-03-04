# Ch14: Binary I/O

## Java如何处理 I/O 

##### 1.如何向文本文件中输入输出

文件对象File object封装文件或路径的属性，但不包含从/到文件读取/写入数据的方法。 为了执行I/O，需要使用适当的JavaI/O类创建对象。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222211737973.png" alt="image-20201222211737973" style="zoom:80%;" />

分别创建input\output，可以直接向该文件输入输出流。

##### 2.文本文件和二进制文件Text File vs Binary File

存储在文本文件text file中的数据以可读human-readable的形式表示。存储在二进制文件中的数据以二进制形式表示。无法读取二进制文件。二进制文件被设计成由程序读取。 例如，Java源程序存储在文本文件中，可以由文本编辑器读取，但Java类存储在二进制文件中，由JVM读取。 二进制文件的优点是它们比文本文件更有效地处理.虽然它在技术上是不精确和正确的，但可以想象文本文件由字符序列组成，二进制文件由位序列组成。 例如，十进制整数199作为三个字符的序列存储在文本文件中：‘1’、‘9’、‘9’，同样的整数存储在二进制文件中，因为十进制199等于十六进制C7.

##### 3.二进制文件Binary I/O

①文本I/O需要编码encoding和解码decoding。JVM在编写字符时将Unicode转换为文件特定编码specific encoding，在读取字符时将文件特定编码转换成Unicode。②二进制I/O不需要转换。将字节写入文件时，将原始字节复制到文件中。从文件中读取字节时，将返回文件中的确切字节。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222212409359.png" alt="image-20201222212409359" style="zoom:67%;" />

## 二进制I/O 的类classes

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222212433083.png" alt="image-20201222212433083" style="zoom:67%;" />

##### 1.InputStream类

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222212541698.png" alt="image-20201222212541698" style="zoom:80%;" />

read():int函数返回的是一个字节，a byte as an int type

##### 2.输出流OutputStream

write(int b): void中的b也是is a byte as an int type

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222212712016.png" alt="image-20201222212712016" style="zoom:80%;" />

![image-20201222215055732](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222215055732.png)

##### 3.FileInputStream\ FileOutputStream

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222212828786.png" alt="image-20201222212828786" style="zoom:80%;" />

FileInputStream/FileOutputStream将二进制binary input/output输入/输出流与外部文件external file关联。文件输入流/文件输出流中的所有方法都是从其超类super class继承的。

①FileInputStream: 如果用一个不存在的文件创建一个文件输入流，就会发FileNotFoundException异常。要构造文件输入流，请使用以下构造函数

> public FileInputStream(String filename);//根据文件名构造
>
> public FileInputStream(File file)；//根据文件对象构造

②FileOutputStream：主要构造函数为：

> public FileOutputStream(String filename);//文件名构造
>
> public FileOutputStream(File file);//用文件对象构造
>
> public FileOutputStream(String filename, boolean append);//加上append
>
> public FileOutputStream(File file, boolean append)

如果文件不存在，将创建一个新文件。如果文件已经存在，前两个构造函数将删除文件中的当前内容，然后再将二进制流保存到文件中。若要保留当前内容并将新数据追加到文件中，要通过传递true到append参数来使用最后两个构造函数。应用:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222213358366.png" alt="image-20201222213358366" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222213437069.png" alt="image-20201222213437069" style="zoom:50%;" />

##### 3.FilterInputStream和FilterOutputStream

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222213658189.png" alt="image-20201222213658189" style="zoom:80%;" />

过滤流Filter stream是为了某种目的过滤字节的流。基本字节输入流提供了一种只能用于读取字节的读取方法。如果要读取整数、浮点数或字符串，则需要一个筛选类来包装字节输入流。使用筛选类可以读取整数、浮点数和字符串，而不是字节和字符。过滤输入流FilterInput和过滤输出流FilterOutput是过滤数据的基类。当需要处理原始数字类型时，使用数据输入流DataInputStream和数据输出流DataOutputStream来过滤字节。

##### 4.数据输入流DataInputStream和数据输出流DataOutputStream

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222214037330.png" alt="image-20201222214037330" style="zoom:80%;" />

数据输入流从流中读取字节并将它们转换为适当的原始类型值或字符串。数据输出流将原始类型值或字符串转换为字节，并将字节输出到流。

①数据输入流：扩展了过滤器输入流FilterInputStream，实现了数据输入接口DataInput interface。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222214154310.png" alt="image-20201222214154310" style="zoom:80%;" />

②数据输出流：扩展了过滤器输出流FilterOutputStream，实现了数据输出接口DataOutput interface。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222214216565.png" alt="image-20201222214216565" style="zoom:80%;" />

数据流Data streams被用作现有输入和输出流的包装器，以过滤原始流中的数据。 它们是使用以下构造函数创建的：

> public DataInputStream(InputStream instream)
>
> public DataOutputStream(OutputStream outstream)

下面给出的语句创建数据流。第一个语句为文件in.dat创建一个输入流；第二个语句为文件out.dat创建一个输出流。

> DataInputStream infile = new DataInputStream(new FileInputStream("in.dat")); //创建输入流infile
>
> DataOutputStream outfile = new DataOutputStream(new FileOutputStream("out.dat")) //创建输出流outfile

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222215512489.png" alt="image-20201222215512489" style="zoom:80%;" />

①必须以相同的顺序和相同的格式读取数据。 例如，由于名称是使用写UTF在UTF-8中必须使用读UTF读取名称。②如果继续在流的末尾读取数据，就会出现EOF Exception。那么如何检查文件的末尾呢？ 可以使用input.available()来检查.可用input.available()==0表示它是文件的末尾。

##### 5.BufferedInputStream 和BufferedOutputStream

![image-20201222215833985](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222215833985.png)

使用缓存加速I/O读写。缓冲输入流/缓冲输出流不包含新方法。 所有方法缓冲输入流/缓冲输出流都是从输入流/输出流类继承的。

管道的pipe line概念:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222215947903.png" alt="image-20201222215947903" style="zoom:80%;" />

构建缓冲输入流/缓冲输出流:

```java
// 构建缓冲输入流
public BufferedInputStream(InputStream in);
public BufferedInputStream(InputStream in, int bufferSize)
    
// 构建缓冲输出流
public BufferedOutputStream(OutputStream out)
public BufferedOutputStream(OutputStream out, int bufferSize)
```

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222220149478.png" alt="image-20201222220149478" style="zoom:80%;" />

##### 6.FilterInputStream和装饰着模式

一般有两种方式可以实现给一个类或对象增加行为：①继承机制，使用继承机制是给现有类添加功能的一种有效途径，通过继承一个现有类可以使得子类在拥有自身方法的同时还拥有父类的方法。但是这种方法是静态的，用户不能控制增加行为的方式和时机。②关联机制，即将一个类的对象嵌入另一个对象中，由另一个对象来决定是否调用嵌入对象的行为以便扩展自己的行为，我们称这个嵌入的对象为装饰器(Decorator)

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222220459433.png" alt="image-20201222220459433" style="zoom:80%;" />

FilterInpustStream子类可以分成两类：①DataInputStream能以一种与机器无关的方式，直接从地从字节输入流读取JAVA基本类型和String类型的数据。②其它的子类使得能够对InputStream进行改进，即在原有的InputStream基础上可以提供了新的功能特性。日常中用的最多的就是BufferedInputStream，使得inputStream具有缓冲的功能。

**DataInputStream**：之前的InputStream我们只能读取byte，这个类使得我们可以直接从stream中读取int，String等类型。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222220733465.png" alt="image-20201222220733465" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222220908181.png" alt="image-20201222220908181" style="zoom:80%;" />

**BufferedInputStream**：在read时，干脆多读一部分数据进来，放在内存里，等你每次操作流的时候，读取的数据直接从内存中就可以拿到，这就减少了io次数

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222220955530.png" alt="image-20201222220955530" style="zoom:80%;" />

每次调用read读取数据时，先查看要读取的数据是否在缓存中，如果在缓存中，直接从缓存中读取；如果不在缓存中，则调用fill方法，从InputStream中读取一定的存储到buf中。

##### 7.Object I/O

对象输入流ObjectInputStream/对象输出流ObjectOutputStream除了可以对原始类型值和字符串执行I/O之外，还可以对对象object执行I/O。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222221212638.png" alt="image-20201222221212638" style="zoom:80%;" />

对象输入流扩展了输入流InputStream，实现了对象输入ObjectInput和对象流常量ObjectStreamConstancts接口。对象输出流同理。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222221320711.png" alt="image-20201222221320711" style="zoom:80%;" />

可以使用以下方法将对象输入流/对象输出流包装在任何输入流/输出流上：

> public ObjectInputStream(InputStream in); // Create an ObjectInputStream
>
> public ObjectOutputStream(OutputStream out) // ObjectOutputStream

使用方式如下：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222221520540.png" alt="image-20201222221520540" style="zoom:80%;" />

##### 8.Serializable接口

并非所有对象都可以写入输出流。可以写入对象流的对象是可序列化serializable的。可序列化对象a serializable object是java.io.Serializable接口的实例。所以可序列化对象的类必须实现Serializable接口。可序列化接口是标记接口marker，它没有方法，所以不需要在实现Serializable的类中添加其他代码。实现此接口使Java序列化机制能够自动化存储对象和数组的过程。

如果一个对象是Serializable的实例，但它包含不可序列化的实例数据字段，那么该对象是否可以序列化？ 答案是否定的。为了使对象能够序列化，可以使用transient 关键字标记这些不可序列化的数据字段，告诉JVM在将对象写入对象流时忽略这些字段。

```java
public class Foo implements java.io.Serializable { 
    private int v1;
    private static double v2;
    private transient A v3 = new A(); 
}
class A { } // A is not serializable
```

当一个Foo类的对象被序列化时，只有变量v1被序列化。变量v2不可序列化是因为它是静态变量，变量v3不序列化是因为它被标记为瞬态transient。如果v3没有标记为transient，就会发生java.io.NotSerializableException。

如果数组的所有元素都是可序列化的，则数组是可序列化的。 因此，可以使用write Object将整个数组保存到文件中，然后使用read Object恢复。下面是一个例子，它存储一个由五个int值和一个由三个字符串组成的数组，并将它们读回来显示在控制台上。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222222022174.png" alt="image-20201222222022174" style="zoom:80%;" />

## 二进制I/O 中的字符和字符串

##### 1.Unicode编码

Unicode由两个字节组成--two bytes=8 bits。writeChar(charc)方法将字符c的Unicode编码写入输出。writeChars(String s)方法将字符串s中每个字符的Unicode编码写入输出。

##### 2.UTF-8编码

UTF8：是一种编码方案，允许系统同时使用ASCII和Unicode进行有效的操作。大多数操作系统使用ASCII。Java使用Unicode。ASCII字符集是Unicode字符集的子集。由于大多数应用程序只需要ASCII字符集，将8位ASCII字符表示为16位Unicode 字符是一种浪费。UTF-8是一种替代方案，它使用变长1、2或3字节存储字符。ASCII值(小于0x7F)编码为一个字节。小于0x7FF的Unicode值以两个字节编码。其他Unicode值以三个字节编码。

##### 3.编码比较

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222214620161.png" alt="image-20201222214620161" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222214651645.png" alt="image-20201222214651645" style="zoom:67%;" />

##### 4.字符和代码Characters and Code

String底层使用char数组来存储数据，每个char占用两个字节，即16位，每个char表示一个Unicode，String实际存储的是文字的Unicode序列。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222214843299.png" alt="image-20201222214843299" style="zoom:67%;" />

每个字符可用一个4位16进制的数表示。加上"\u"前缀。

## 序列化的一些问题

##### 1.反序列化后的对象，需要调用构造函数重新构造吗？

不需要。对于可序列化的Serializable对象，对象完全以它存储的二进制位作为基础来构造，而不调用构造器。

##### 2.序列前的对象与序列化后的对象是什么关系？是("=="浅复制，还是equal？深复制)

深复制，反序列化还原后的对象地址与原来的的地址不同。通过序列化操作，我们可以实现对任何可Serializable对象的”深度复制"——这意味着我们复制的是整个对象网，而不仅仅是基本对象及其引用。对于同一流的对象，他们的地址是相同，说明他们是同一个对象，但是与其他流的对象地址却不相同。

##### 3.serialVersionUID 的作用？

Serializable接口的代价一：若一个类实现Serializable接口，会大大降低了“改变这个类的实现”的灵活性。

如果不设计一种自定义的序列化形式，而是采用默认的序列化形式，则会被束缚在该类最初的内部表示法上。就是说，若之后改变该类的内部表示法，结果会导致不兼容。用旧版本来序列化一个类，而用新版本来反序列化，会导致程序失效。

凡是实现Serializable接口的类都有一个表示序列化版本标识符的静态long类型变量：private static final long serialVersionUID;如果没有设置这个值，在序列化一个对象之后，改动了该类的字段或者方法名之类的，再反序列化想取出之前的那个对象时就可能会抛出异常，因为serialVersionUID是根据类名、接口名、成员方法及属性等来生成一个64位的哈希字段，当修改后的类去反序列化时，该类的serialVersionUID值和之前保存在文件中的serialVersionUID值不一致，所以就会抛出异常。

##### 4.writeObject和readObject

当 ObjectOutputStream对一个Customer对象进行序列化时，如果该对象具有writeObject()方法，那么就会执行这一方法， 否则就按默认方式序列化。在该对象的writeObject()方法中，可以先调用ObjectOutputStream的 defaultWriteObject()方法，使得对象输出流先执行默认的序列化操作。同理可得出反序列化的情况，不过这次是 defaultReadObject()方法。

##### 5.Externalizable

Java默认的序列化机制非常简单，而且序列化后的对象不需要再次调用构造器重新生成，但是在实际中，我们会希望对象的某一部分不需要被序列化，或者说一个对象被还原之后，其内部的某些子对象需要重新创建，从而不必将该子对象序列化。 在这些情况下，我们可以考虑实现Externalizable接口从而代替Serializable接口来对序列化过程进行控制（通过transient的方式更简单）。

相当于完全自己控制序列化和反序列化，之前基于Serializable接口的序列化机制就将失效。Externalizable接口extends Serializable接口，而且在其基础上增加了两个方法：writeExternal()和readExternal()。这两个方法会在序列化和反序列化还原的过程中被自动调用，以便执行一些特殊的操作。

##### 6.对单例模式的影响

破坏了单例性。无论是实现Serializable接口，或是Externalizable接口，当从I/O流中读取对象时，readResolve()方法都会被调用到。实际上就是用readResolve()中返回的对象直接替换在反序列化过程中创建的对象。

## 随机访问文件类RandomAccessFiles

##### 1.概念

到目前为止，使用的所有流都称为只读或只写流。这些流的外部文件是顺序文件sequential files，不创建新文件就无法更新。通常需要修改文件或将新记录插入文件中。Java提供随机访问文件类RandomAccessFile，允许在随机位置读取和写入文件。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222223342899.png" alt="image-20201222223342899" style="zoom:80%;" />

随机访问文件由字节序列sequence of bytes组成。有一个特殊的标记称为文件指针file pointer，它位于其中一个字节。读或写操作发生在文件指针的位置。打开文件时，文件指针设置在文件的开头。读取或写入数据到文件时.文件指针向前移动到下一个数据。 例如，如果使用readInt()读取int值，则JVM从文件指针读取四个字节，现在文件指针比前一个位置提前四个字节。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222224227940.png" alt="image-20201222224227940" style="zoom:80%;" />

随机存取文件中的许多方法与数据输入流和数据输出流中的方法相同。 例如，readInt()、readLong()、writeDouble()、可以用于数据输入流或数据输出流以及随机访问文件流。

将从随机访问文件流开始的偏移量设置为下一个读或写发生的地方:

> void seek(long pos) throws IOException;

返回从文件开始到下一次读或写发生的地方的当前偏移量(以字节为单位:

> long getFilePointer() IOException;

##### 2.Fixed Length String I/O

随机访问文件通常用于处理记录文件。为了方便起见，在随机访问文件中使用固定长度的记录，这样记录就可以很容易地定位。记录由固定数量的字段组成。字段可以是字符串或原始数据类型。固定长度记录中的字符串具有最大大小。 如果字符串小于最大大小，则字符串的其余部分用空格填充。

## Aadvanced Text IO--字符流

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222224904834.png" alt="image-20201222224904834" style="zoom:80%;" />

##### 1.字符流概述

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222224933575.png" alt="image-20201222224933575" style="zoom:80%;" />

##### 2.字符流基类

Reader类--读取字符流的抽象类，常用方法：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225054057.png" alt="image-20201222225054057" style="zoom:67%;" />

Writer类--写入字符流的抽象类，常用方法：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225135682.png" alt="image-20201222225135682" style="zoom:67%;" />

3.字符转换流

InputStreamReader：字节流转字符流，它使用的字符集可以由名称指定或显式给定，否则将接受平台默认的字符集。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225232407.png" alt="image-20201222225232407" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225300098.png" alt="image-20201222225300098" style="zoom:67%;" />

OutputStreamWriter--字节流转字符流：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225349553.png" alt="image-20201222225349553" style="zoom:67%;" />

##### 4.字符缓冲流

BufferedReader：字符缓冲流，从字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225433469.png" alt="image-20201222225433469" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225450678.png" alt="image-20201222225450678" style="zoom:67%;" />

BufferedWriter：字符缓冲流，将文本写入字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222225523719.png" alt="image-20201222225523719" style="zoom:67%;" />

##### 5.FileReader和FileWriter

FileReader：InputStreamReader类的直接子类，用来读取字符文件的便捷类，使用默认字符编码。用来方便的从文件中读出字符的类，默认编码和默认缓冲区大小假设是可以接受的。如果要改变默认编码和默认缓冲区大小可以用FileInputStream来构造InputStreamReader来实现。FileReader意味着是用来读字符的流，要实现读取字节流，请考虑使用FileInputStream。构造函数有：

①FileReader(File file) --用File对象来构造FileReader②FileReader(FileDescriptor fd) --用文件描述符构造FileReader③FileReader(String fileName) --用文件的路径名来构造FileReader；

主要的函数大多集成于Reader类，其他主要的函数有：①public String getEncoding() --返回这个流使用的编码方式



FileWriter：OutputStreamWriter类的直接子类，用来写入字符文件的便捷类，使用默认字符编码。FileWriter是用来写字符流的，主要的构造函数有：

①FileWriter(File file) --用File对象来构造FileWriter，写数据时，从文件开头开始写起，会覆盖以前的数据②FileWriter(File file, boolean append) --还是用File对象构造，如果第二个参数为true的话，表示以追加的方式写数据，从文件尾部开始写起；③FileWriter(FileDescriptor fd)--用文件描述符来构造FileWriter ④FileWriter(FileDescriptor fd, boolean append) --用文件描述符来构造，第二个参数为true的话，表示以追加的形式写入数据；⑤ FileWriter(String fileName) 用文件的路径名来构造FileWriter⑥ FileWriter(String fileName,boolean append) 用文件路径名来构造FileWriter，第二个参数为true的话，表示以追加的形式写入。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222230001098.png" alt="image-20201222230001098" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222230052207.png" alt="image-20201222230052207" style="zoom:67%;" />

## PipedOutputStream和PipedInputStream

##### 1.概念

**PipedOutputStream**和**PipedInputStream**分别是管道输出流和管道输入流。

它们的作用是让多线程可以通过管道进行线程间的通讯。在使用管道通信时，必须将PipedOutputStream和PipedInputStream配套使用。

使用管道通信时，大致的流程是：我们在线程A中向PipedOutputStream中写入数据，这些数据会自动的发送到与PipedOutputStream对应的PipedInputStream中，进而存储在PipedInputStream的缓冲中；此时，线程B通过读取PipedInputStream中的数据。就可以实现，线程A和线程B的通信。

##### 2.PipedReader和PipedWriter

PipedReader和PipedWriter即管道输入流和输出流，可用于线程间管道通信。它们和PipedInputStream/PipedOutputStream区别是前者操作的是字符后者是字节。

## PrintWriter和PrintStream

PrintStream主要操作byte流，而PrintWriter用来操作字符流。读取文本文件时一般用后者。PrintStream和PrintWriter的API几乎相同，都能输出各种形式的数据，构造方法也几乎相同

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222230444699.png" alt="image-20201222230444699" style="zoom: 67%;" />

PrintWriter多了个接受Writer参数的构造函数

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201222230503059.png" alt="image-20201222230503059" style="zoom:67%;" />

两个类的功能基本相同 ， PrintStream 能做的PrintWriter也都能实现，并且PrintWriter的功能更为强大。但是由于PrintWriter出现的比较晚，较早的System.out使用的是PrintStream来实现的，所以为了兼容就没有废弃PrintStream。 

两个类最大的差别是，PrintStream在输出字符，将字符转换为字节时采用的是系统默认的编码格式，这样当数据传输另一个平台，而另一个平台使用另外一个编码格式解码时就会出现问题，存在不可控因素。而PrintWriter可以在传入Writer时,可由程序员指定字符转换为字节时的编码格式，这样兼容性和可控性会更好。


















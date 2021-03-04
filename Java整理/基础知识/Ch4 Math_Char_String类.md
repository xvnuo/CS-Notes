# Ch4 Mathematical Functions, Characters and Strings

## Java的Math类

##### 1.类内定义的常量constants

--PI：圆周率，引用时：Math.PI

--E：自然常数,引用时：Math.E

##### 2.三角函数方法Trigonometric Methods

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215223214339.png" alt="image-20201215223214339" style="zoom: 50%;" />

##### 3.指数运算方法Exponent Methods

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215223242296.png" alt="image-20201215223242296" style="zoom:50%;" />

##### 4.Rounding Method

--double ceil(double x)：x向上取整，结果以double类型返回

--double floor(double x)：x向下取整，结果以double类型返回

--double rint(double x)：取离x最近的整数，若一样近则取偶数，返回其double值

--int round(float x)：返回(int)Math.floor(x+0.5)

--long round(double x)：返回(long)Math.floor(x+0.5)

例：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215223748194.png" alt="image-20201215223748194" style="zoom:80%;" />

A：-4.0；B：-4;  C:-3.0;  D:函数使用错误

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215223809466.png" alt="image-20201215223809466" style="zoom: 50%;" />

##### 5.floorMod

floorMod(position+adjustment)%12总是返回0~11的数，除非是负除数，floorMod会得到负数结果。

##### 6.min, max, abs, random()

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215224210656.png" alt="image-20201215224210656" style="zoom: 67%;" />

--Math.random()返回[0,1.0)范围内的随机一个小数。可以扩展成生成特定范围内的随机数：a+Math.random()*b =>返回[a,a+b)内随机的数字

选B，强制类型转换符的优先级高于乘法运算符，(int)Math.random()*980==0.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215224458102.png" alt="image-20201215224458102" style="zoom:67%;" />

## Characters类型

```java
char letter='A'; //ACSII
char numChar='4';//ASCII
char letter='/u0041'; //Unicode
char numChar='/u0034';//unicode
```

##### 1.自增/自减运算符

也可以用在char类型变量上来找到该字符下一个或前一个Unicode编码的字符。

```java
char ch='a';
System.out.println(++ch);//输出b
```

##### 2.Unicode格式

Java使用Unicode编码，这是Unicode Consortium创建的16位编码模式，它支持世界各种语言中书面文本written texts的转换interchange、处理processing和显示display. Unicode工16位，刚好占据了2个字节bytes，用4个16进制数字和加上前缀\u在一起表示。范围从\u0000到\uFFFF, 可以表示2^16=65535+1个字符.

ASCII码表是Unicode的子集，从\u0000到\u007f。常见字符的ASCII码和Unicode码对照表：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215225743986.png" alt="image-20201215225743986" style="zoom:80%;" />

转义字符：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215225834832.png" alt="image-20201215225834832" style="zoom:80%;" />

需要注意注释中的“\u”，例// \u000A，\u000A会被认作下一行的单独一个字符，

例： // Look inside c:\users，会出现语法错误，因为\u后并不是4个十六进制数

##### 3.char和Numeric类型的转换

```java
int i='a';//自动转换，等价于int i=(int)'a'
char c=97;//等价于char c=(char)97
```

##### 4.字符之间的比较方式

```java
if (ch >= 'A' && ch <= 'Z') 
System.out.println(ch + " is an uppercase letter"); 
else if (ch >= 'a' && ch <= 'z') 
System.out.println(ch + " is a lowercase letter"); 
else if (ch >= '0' && ch <= '9') 
System.out.println(ch + " is a numeric character");
```

##### 5.Character字符类的方法

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215230547468.png" alt="image-20201215230547468" style="zoom:80%;" />

--isDigit(ch)判断ch是否是数字字符

--isLetter(ch)判断ch是否是字母字符

--isLetterOfDigit(ch)判断ch是否是字母或数字字符

--isLowerCase(ch)判断ch是否是一个小写的字母

--isUpperCase(ch)判断ch是否是一个大写的字母

--toLowerCase(ch)将ch转换成一个小写字符

--toUpperCase(ch)将ch转换成一个大写字符

## Strings类型

String实际上是Java库中的预定义类predifined class，就像System类和Scanner类一样。字符串类型不是原始primitive类型而是参考类型reference type。任何Java类都可以用作变量的引用类型。参考数据类型将在第9章“Objects and Classes”中进行详细讨论。

##### 1.String 对象的简单方法

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215231059092.png" alt="image-20201215231059092" style="zoom:80%;" />

--concat(s1)返回一个和s1连接起来的新字符串

--trim()返回一个将本字符串两边空格都删去的新字符串(细品"新"字)

```java
String message = "Welcome to Java";
System.out.println("The length of " + message + " is " 
+ message.length());//获取字符串的长度length()

System.out.println(message.charAt(0));//获取字符串某下标字符

```

##### 2.实例方法instance method和静态方法static method

Strings是Java中的对象。上表中的方法只能通过特定的字符串实例调用。所以，这些方法也称做实例方法instance methods。非实例方法称为静态方法static methods。可以在不使用对象的情况下调用静态方法。在Math类中定义的所有方法都是静态方法。它们不绑定not tied to到特定的对象实例。调用实例方法的方式是:

**referenceVariable.methodName(arguments)**. 

##### 3.字符串拼接concatenation

```java
String s3 = s1.concat(s2);//调用concat方法将s2放到s1后面
String s3 = s1 + s2;//直接用➕连接

String message = "Welcome " + "to " + "Java";
String s = "Chapter" + 2; // s becomes Chapter2
String s1 = "Supplement" + 'B'; // s1 becomes SupplementB
```

##### 4.读取方式：next()和nextLine()

```java
Scanner input = new Scanner(System.in);
String s1 = input.next();//next()以换行符为标志结束
String s2 = input.next();
String s3 = input.next();
System.out.println("s1 is " + s1);
System.out.println("s2 is " + s2);
System.out.println("s3 is " + s3);
```

从控制台读取一个字符：

```java
Scanner input = new Scanner(System.in);
String s = input.nextLine();
char ch = s.charAt(0);
System.out.println("The character entered is " + ch);
```

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215232432847.png" alt="image-20201215232432847" style="zoom:80%;" />

nextInt()方法会读取下一个int型标志的token.但是焦点不会移动到下一行，仍然处在这一行上.当使用nextLine()方法时会读取该行剩余的所有的内容，包括换行符，然后把焦点移动到下一行的开头。所以这样就无法接收到下一行输入的String类型的变量。

next()从遇到第一个有效字符（非 空格、换行符）开始扫描，遇到第一个分隔符或结束符（空格’ ‘或者换行符 ‘\n’）时结束。nextLine()则是扫描剩下的所有字符串知道遇到回车为止。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215232759541.png" alt="image-20201215232759541" style="zoom:67%;" />

##### 5.字符串之间的比较

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215232912522.png" alt="image-20201215232912522" style="zoom:80%;" />

--equals(s1)比较该字符的内容是否和s1相同

--equalsIgnoreCase(s1)忽略大小写，比较两个字符串的内容是否一样

--compareTo(s1)按字典序比较，若大于返回正数，等于返回0，小于返回负数

--startsWith(prefix)判断该字符串是否已prefix开头

--endsWith(suffix)判断是否以suffix结尾

##### 6.获取字符串的子串

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215233352819.png" alt="image-20201215233352819" style="zoom:80%;" />

message.substring(0,11)表示从下标0开始的11个字符；message.substring(11)表示下标为11开始到结尾的子串。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215233527488.png" alt="image-20201215233527488" style="zoom:80%;" />

##### 7.字符串和数字之间的转换

```java
int intValue = Integer.parseInt(intString);//Integer的方法
double doubleValue=Double.parseDouble(doubleString);//Double
String s = number + "";//数字转换成字符串的形式，＋""
```

## 格式化输出

##### 1.使用printf语句--占位符

```java
System.out.printf(format,iems);//format--格式说明符
System.out.printf("count is %d and amount is %f",a,b);
```

常见的占位符：b--boolean

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216192954560.png" alt="image-20201216192954560" style="zoom:67%;" />

占位符的完整格式为：%index 标识*最小宽度 . 精度 转换符

①%：占位符的起始字符，如果在占位符内部使用%要表示成%%②index：位置索引，从1开始算，用于指定索引响应的实参进行格式化并替换掉该占位符③标识：用于增强格式化能力，④最小宽度：当字符串长度小于最小宽度时，左边补空格凑齐⑤转换符：指定格式化的样式，限制对应入参的数据类型

![image-20201216193708294](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216193708294.png)

##### 2.字符、字符串格式化

宽度限制默认是在左边补空格，当数值设置为负数时，是在右边补空格。

![image-20201216193914391](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216193914391.png)

##### 3.整数格式化

![image-20201216193933826](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216193933826.png)

4.浮点数格式化

![image-20201216194308977](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216194308977.png)

##### 5.日期和时间格式化

![image-20201216194420816](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216194420816.png)

![image-20201216194436124](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216194436124.png)

6.对于参数索引index的说明

索引必须紧跟在%后面，以$终止，如

System.out.printf(“%1$s %2$tB %2$te,%2$tY”, “Due date:”, new Date());

输出为：

Due date: February 9, 2015 

还可以用<标志，表明前面格式说明中的参数被再次使用

System.out.printf(“%s %tB %<te, %<tY”, “Due date:”, new Date());




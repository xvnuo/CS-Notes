# Ch2: Elementary Programming

## 零零散散的一些知识

##### 1.如何从控制台console读入和输出数据数据

```Java
Scanner input=new Scanner(System.in);//首先new一个Scanner对象
double d=input.nextDouble();		//用生成的Scanner对象直接读取响应类型的数据

System.out.println("Enter a double value: ");//从控制台输出字符串的方式
```

##### 2.标识符Identifiers

合法的标识符①只能由字母、数字、_和$组成，②必须以字母下划线或$开头。不能以数字开头，③不能是保留字reserved word，④不可以是true, false或null，⑤可以是任意长度⑥区分大小写，⑦可以是汉字。如下：显然是B

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215201732311.png" alt="image-20201215201732311" style="zoom:80%;" />

##### 3.声明变量/ 赋值/ 声明+赋值语句

```Java
int x; //声明
x=1;   //赋值
int x=1;//声明和赋值
```

C/ C++区分变量的声明和定义：int i=10是个定义，而external int i;是一个声明。java中不区分变量的声明和定义。

```Java
public class Test {
	public static void main(String[] args) {
		int x;    //局部变量，无默认值default value 
		String y; //局部变量，无默认值
		System.out.println("x is " + x);//编译错误，变量未初始化 
		System.out.println("y is " + y); //编译错误
	} 
}
```

C/C++中：①对于函数内的局部变量，编译器不会赋默认初始值，使用未初始化的指针访问内存会导致程序崩溃；②对于类对象，编译器使用类的默认构造函数对对象进行初始化。

Java不为方法method中的局部变量local variable分配默认值。对于方法的局部变量，java以编译时错误来保证变量在使用前都能得到恰当的初始化。

选择C：上面讲的是方法中的局部变量不会默认初始化，类内的成员变量可以用该方法的默认构造函数初始化，int型默认初始化为0.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215202821669.png" alt="image-20201215202821669" style="zoom:80%;" />

##### 4.final的用法

①修饰方法method：标识一个java函数不可更改，“不可更改”指的是该函数不能被重载了。

②修饰类class：表示整个类不能被继承了，类里面的所有方法也相当于被加了final

③修饰变量variable：final变量不可更改，但是它的值可以在运行时初始化，也可以在编译时初始化，甚至可以放在构造函数中初始化，不必在声明的时候初始化。如果修饰的是类对象，并不是表示这个对象不可更改，而是表示这个变量不可再被其他对象赋值，类似C++中的Class const *p。

```Java
final datatype CONSTANTNAME = VALUE;//常量声明方式
final int i=1;//✔，在编译时初始化
final int i=(int)(Math.Random()*10);//✔，运行时初始化
final int i;//✔，构造函数里初始化

final Value v=new Value();//✔，修饰类对象变量
v=new Value();//❌，该变量不能再赋成其他对象
```

选D：const修饰成员变量，表示成员变量不能被修改，只能在初始化列表中赋值

![image-20201215203534904](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215203534904.png)

##### 5.命名惯例naming conventions

要使用有内涵，贴近功能meaningful、descriptive的名字

①变量名和方法method名：单个单词--小写；如果名字由几个单词组成，第一个单词小写，后面的每个单词的首字母大写。如radius, computeArea.

②类名：首字母大写，若多个单词组成，将每个单词的第一个字母大写。

③常量constants：将常量中的所有字母大写，若由多个单词，用下划线隔开。如MAX_VALUE

## 数据类型Numerical Data Types

##### 1.byte--单个字节，8bits, short--16bits, int--32bits; long--64bits

C/C++中需要针对不同的处理器选择最高效的整型，造成了一个在32位处理器上运行很好的程序在16位系统上运行可能会发生整数溢出。Java中，整型的范围与运行JAVA代码的机器无关，解决了平台移植的问题。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215205241101.png" alt="image-20201215205241101" style="zoom:80%;" />

##### 2.如何从键盘读取数字--Scanner类

```java
Scanner input=new Scanner(System.in);//先new一个Scanner对象
int value=input.nextInt();
```

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215205629760.png" alt="image-20201215205629760" style="zoom: 67%;" />

##### 3.数据运算：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215205659067.png" alt="image-20201215205659067" style="zoom:67%;" />

涉及浮点数的计算都是近似的，因为这些数字都美元完全准确的存储。整数可以准确存储。但是对于整数除法--整数类型统统舍去小数：5/2=2，5.0/2=2.5。

```java
System.out.println(1.0 - 0.1 - 0.1 - 0.1 - 0.1 - 0.1);
//displays 0.5000000000000001, not 0.5
System.out.println(1.0 - 0.9);
//displays 0.09999999999999998, not 0.1
```

指数运算：返回值都是浮点类型

```java
System.out.println(Math.pow(2, 3));  //Displays 8.0 
System.out.println(Math.pow(4, 0.5));//Displays 2.0
System.out.println(Math.pow(2.5, 2));//Displays 6.25
System.out.println(Math.pow(2.5, -2));//Displays 0.16
```

##### 4.Interger Literals

Number Literals指的是那些可以在程序中直接用数字表示的数。一个Interger Literals可以赋值给一个整数变量，只要它能fit into该变量。如果数字太大，变量无法容纳，则会出现编译错误compilation error。

```java
byte b=1000;//❌，byte类型范围为-127~128
```

一个小数的类型默认为double类型而不是float类型。可以通过加上字符f/F声明为float.如100.2f. 两者精度不同：

```java
System.out.println("1.0 / 3.0 is " + 1.0 / 3.0);
//double类型，输出小数点后16位0.3333333333333333
System.out.println("1.0F / 3.0F is " + 1.0F / 3.0F);
//float类型，输出小数点后7位0.33333334
```

##### 5.特殊的浮点数值

```java
Double.POSITIVE_INFINITY  //正无穷大
Double.NEGATIVE_INFINITY  //负无穷大
Double.NaN				  //NaN不是一个数字，可用来判断是否是数字
```

##### 6.科学计数法scientific notation

浮点数可被表示成科学计数法的形式：

1.23456e+2== 1.23456e2 == 123.456;  1.23456E-2 == 0.0123456

##### 7.计算当前的时间：currentTimeMillis方法

该方法返回1970GMT午夜12：00至今的时间长度，按millisecond计算。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215211740985.png" alt="image-20201215211740985" style="zoom:80%;" />

使用方式：1s=1000ms

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215211813877.png" alt="image-20201215211813877" style="zoom:80%;" />

##### 8.自运算augmented assignment operators

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215211956187.png" alt="image-20201215211956187" style="zoom:80%;" />

##### 9.Increment and Decrement operators

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215212054346.png" alt="image-20201215212054346" style="zoom:80%;" />

##### 10.类型转换type conversion

当涉及到两个不同类型的操作数时，java按以下规则自动转换automatically：①如果其中一个操作数是double，另一个自动给转换为double类型；②无double时，若一个是float,自动转换成float;③若无浮点型，且有一个操作数是long，则自动转成long；④否则，都转换成int类型的，自动转换规则如下图：实现表示无信息丢失的转换，虚线表示可能有精度损失的转换。

(int-->float和long-->double, long-->float的损失是因为float32位的整数部分表示的范围<int可以表示的范围；double64位的整数部分比int范围大，但不如long大)

自动转换=>隐式转换；不能自动转换=>显式转换，需要加上强制类型转换符'()'.

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215212523710.png" alt="image-20201215212523710" style="zoom:80%;" />

```java
double d=3;//✔，隐式转换implicit casting
int i=(int)3.9；//✔，显示转换explicit casting，小数部分被截掉。
    
int x=5/2.0;//❌，double类型不能隐式的转换成int类型，要加上强制转换符：int x=(int)(5/2.0);
int nx=(int)Math.round(x)；//✔，round返回的是long类型，需要强制转换
```

x op = y等价于x = (T)(x op y), T是x的类型。

```java
int sum=0;
sum+=4.5;//等价于sum=(int)(sum+4.5),所以sum=4
```

在c和c++中有时出现数据类型的隐含转换，这就涉及了自动强制类型转换问题。例如，在c++中可将一浮点值赋予整型变量，并去掉其尾数。Java不支持c++中的自动强制类型转换，如果需要，必须由程序显式进行强制类型转换。Java是一个严格进行类型检查的程序语言，对于下面这样的程序，在C++的编译器上编译时最多只会出现警告的信息，但是在Java里则不予通过。Java中必须要程序设计强制转型(type casting)，Java的编译器才会接受.

```java
int aInt;
Double aDouble=2.71828;
aInt = aDouble;//❌，java中不支持自动强制类型转换
aInt = (int)aDouble;//✔，java可显式进行强制类型转换
```

## 常见错误类型

##### 1.Undeclared/Uninitialized Variables and Unused Variables 

##### 2.Interger Overflow

##### 3.Round-off Errors

##### 4.Unintended Integer Division

##### 5.Redundant Input Objects

同一个块中定义了多个Scanner对象--没必要








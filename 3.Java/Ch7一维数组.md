# Ch7 Signle-Dimensional Arrays

## 数组--代表数据的集合

##### 1.数组的声明和创建方式

```java
datatype[] arrayRefVar;//数组的正常声明方式
double[] myList;
datatype arrayRefVar[];//正确但是不可取的声明方式

//创建数组：
arrayRefVar=new datatype[arraySize];

//声明和创建数组
datatype[] arrayRefVar = new datatype[arraySize]
int[] m=new int[10];
```

##### 2.数组的长度

数组一旦被创建，他的大小就固定了。长度length为其固有的属性，不是函数。

arrayRefVar.length返回数组的元素个数、长度、大小，不用带括号！

3.数组的默认值default values

当数组创建时，数组内每个元素都有默认值：

①数据类型--0 ②字符类型'\u0000' ③Boolean类型false

##### 4.索引变量index variable

从0开始到length-1

##### 5.数组初始化initializers

```java
double[] myLits={1.9, 2.9, 3.9, 3.5};//声明、创建、初始化一气呵成

使用这种方式，三个步骤：声明创建初始化必须一体，否则会发生语法错误
double[] myList;
myList = {1.9, 2.9, 3.4, 3.5};//错误,myList这时候只是一个头
```

##### 5.Java和C++中数组的区别

①Java数组分配在堆上。C++int a[100]分配在栈上，int* a=new int[100]分配在堆上。

②Java中的[]运算符被预定义为检查数组边界，而且没有指针运算，就不能通过a+1得到下一个元素。Java中 int a[100]：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216212435443.png" alt="image-20201216212435443" style="zoom:67%;" />

③命令行参数，带有 String[] args。在java中，args[0]是第一个参数，程序名没有存储在args中。

## 数组的处理processing Arrrays

##### 1.用输入值初始化数组

```Java
for (int i = 0; i < myList.length; i++) 
	myList[i] = input.nextDouble();
```

##### 2.用随机值初始化数组

```java
for (int i = 0; i < myList.length; i++) {
	myList[i] = Math.random() * 100;
}
```

##### 3.打印数组

##### 4.求和

##### 5.寻找数组中的最大值

##### 6.寻找数组中最大值的下标

##### 7.random shuffling随机交换两个元素的位置

##### 8.shifting elements

##### 9.Enhanced for loop

```java
for (double value: myList) 
	System.out.println(value);
```

##### 10.arraycopy函数

将一个源数组的值赋给另一个数组(目标数组)中

```java
arraycopy(sourceArray, src_pos, targetArray, tar_pos, length);//源数据，初始位置；目标数组，初始位置和长度
System.arraycopy(sourceArray, 0, targetArray, 0, sourceArray.length);//将源数组的值赋给目标数组
```

或者用Arrays的copyOf函数

int[] copiedLuckyNumbers = Arrays.copyOf(luckNumbers,luckyNumbers.length);

第2个参数是新数组的长度，这个方法通常用来增加数组的大小：

int[]copiedLuckyNumbers=Arrays.copyOf(luckNumbers,2*luckyNumbers.length);

如果数组元素是数值型，则多余的元素被赋值为0；若是布尔型，则赋值为false。如果长度小于原始数组长度，则只拷贝前面的元素。

##### 11.将数组作为函数的形参

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216214133997.png" alt="image-20201216214133997" style="zoom:67%;" />

匿名数组anonymous array，没有名字的数组。如

printArray(new int[]{3,1,2,6})通过new datatype[]{n1,n2,n3}的语法创建了一个匿名数组并直接进行打印。匿名数组没有显示引用它的变量。

##### 12.pass by value

Java使用传递值将参数传递给方法。传递原始数据类型变量的值与传递数组之间存在重要差异。①对于原始类型值的参数，传递实际值。改变方法内部局部变量的值不影响方法外部变量的值。②对于数组类型的参数，参数的值包含对数组的引用；该引用也传递给了方法。方法主体内部对数组的任何更改都将影响作为参数传递的原数组，即形参的改变会影响到实参。例：

```java
public class Test {
	public static void main(String[] args) {
		int x = 1; //原始数据
		int[] y = new int[10]; //数组类型
		m(x, y); //调用函数，传入实参
		System.out.println("x is " + x);
		System.out.println("y[0] is " + y[0]);
	}
	public static void m(int number, int[] numbers) {
		number = 1001; //不会影响到x
		numbers[0] = 5555;//会影响到y，y[0]=5555
	}
}
```

## 数组元素的查找

##### 1.线性查找linear search

##### 2.二叉搜索binary search--已经有序的数组

```java
/** Use binary search to find the key in the list */
public static int binarySearch(int[] list, int key) {
	int low = 0;
	int high = list.length - 1;
	while (high >= low) {
		int mid = (low + high) / 2;
		if (key < list[mid])
			high = mid - 1;
		else if (key == list[mid])
			return mid;
		else
			low = mid + 1;
	}
	return -1 - low;
}
```

##### 3.数组排序sorting

选择排序selection sorting--每次从剩余数组中选择最小的放到最前面

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216221628272.png" alt="image-20201216221628272" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216221731434.png" alt="image-20201216221731434" style="zoom:67%;" />

Array.sort()方法--提供对int\double\short\char\long\float等数据类型可重载的数据排序方法：使用如下：是在原有数据的基础上进行排序的

```java
double[] numbers = {6.0, 4.4, 1.9, 2.9, 3.4, 3.5};
java.util.Arrays.sort(numbers);
char[] chars = {'a', 'A', '4', 'F', 'D', 'P'};
java.util.Arrays.sort(chars);
```

##### 4.Arrays.toString(list)方法

返回该数组的字符串表示

## Main方法中的命令行参数

B中的main方法被A中的方法调用了，A中的数组strings充当了B中main方法的命令行参数。

![image-20201216222242090](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216222242090.png)

java编译时的命令为：javac +文件名

java运行时的命令为：java+文件名+参数0+参数1+参数2...参数n

其中的文件名并不算进命令行参数中，真正传入main方法中的args数组的元素分别为参数0、参数1、参数2...参数n等


























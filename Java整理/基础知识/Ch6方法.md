# Ch6 Methods

## 一.基本概念

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216200431392.png" alt="image-20201216200431392" style="zoom: 80%;" />

1.方法签名method signature--是函数名和参数表的组合parameter list

2.形参formal parameter--方法头中定义的变量称为形参。

3.实参actual parameters--调用方法时传递给形参的实际的值称为实参

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216200605219.png" alt="image-20201216200605219" style="zoom:80%;" />

4.返回值类型return value type--返回值类型即函数的类型，若无返回值，则函数为void类型。

值返回方法需要返回语句。下面(a)中所示的方法在逻辑上是正确的，但它有编译错误compilation error，因为Java编译器认为这种方法可能不返回任何值。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216200827760.png" alt="image-20201216200827760" style="zoom:80%;" />

n有可能不是数，所以为了更全面化，用最后的else概括其余情况。

函数的一大好处是复用reuse。

## 二.另一些概念

##### 1.值传递pass by value

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216201135924.png" alt="image-20201216201135924" style="zoom:67%;" />

值传递情景中，形参和实参或不相干，函数调用并不影响实参的取值。

##### 2.模块化代码modularizing code

方法可以用来减少冗余编码redundant code，并启用代码重用reuse。方法也可以用来模块化代码，提高程序的质量。

##### 3.函数重载overloading methods

函数名完全一样，参数表和返回的值类型可能不同。

有时调用方法可能有两个或两个以上的匹配，但是编译器无法确定最佳的匹配。 这被称为模棱两可的调用ambiguous invocation。模糊调用会产生编译错误compile error。例：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216201635577.png" alt="image-20201216201635577" style="zoom:67%;" />

##### 4.函数内局部变量local variable的作用范围

局部变量--方法内定义的变量；作用范围scope--函数可以被引用有效果的范围。①局部变量的范围从其声明declaration开始，并继续到包含该变量的块的末尾。局部必须声明后才能使用。②可以在方法中的不同非嵌套块中多次声明同名的局部变量，但不能在嵌套块中两次声明本地变量。③在for循环头的初始操作部分中声明的变量在整个循环中具有其作用域。在for循环体中声明的变量在循环体中的范围从声明到包含变量的块的末尾都有限制。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216203051758.png" alt="image-20201216203051758" style="zoom:80%;" />

同名局部变量的作用域不能有重合，否则会发生编译错误。

##### 5.方法的抽象method abstraction

可以将函数体抽象成一个封装了具体实现的黑匣子：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216204158295.png" alt="image-20201216204158295" style="zoom: 67%;" />

方法的好处：

重用reuse；信息隐藏information hiding③降低复杂性reduce complexity
























# Ch3 Selections

## 琐琐碎碎的知识点

##### 1.boolean类型和关系运算符relational operators

boolean b=(1>2); b=>false;

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215215554984.png" alt="image-20201215215554984" style="zoom:80%;" />

##### 2.if-else 语句

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215215646878.png" alt="image-20201215215646878" style="zoom:80%;" />



代码的精简tips:

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215215746595.png" alt="image-20201215215746595" style="zoom:67%;" />

if(even!=0)是错误的，布尔值不能和整型数据相互转换：

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215215811630.png" alt="image-20201215215811630" style="zoom:80%;" />

##### 2.逻辑运算符logical operator

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215220030825.png" alt="image-20201215220030825" style="zoom:67%;" />

&&和|| 运算符按照“短路”方式来求值。如果第一个操作数已经能够确定表达式的值，第二个操作数就不必计算了。&和|是二进制位运算符，&和|不采用短路的方式求值。

##### 3.switch语句

java中switch后的表达式类型①只能是byte\short\char\int等类型。②在Java1.7后支持了对String的判断。③long类型不能作为switch的参数。④支持枚举类型。case后需要是常数表达式，不能是包含变量的表达式。所以选D

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215221659731.png" alt="image-20201215221659731" style="zoom:80%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215221943247.png" alt="image-20201215221943247" style="zoom:80%;" />

##### 4.条件语句conditional expressions  eg.  a=(a>b?)1:2;

##### 5.运算发优先级和结合性

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215222302207.png" alt="image-20201215222302207" style="zoom:80%;" />

除了赋值运算外，所有二进制运算符都是左结合的。所以有a-b+c-d等价于((a-b)+c)-d. 赋值运算符都是右结合的。所以有a=b+=c=5等价于a=(b+=(c=5)).

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201215220030825.png" alt="image-20201215220030825" style="zoom:67%;" />
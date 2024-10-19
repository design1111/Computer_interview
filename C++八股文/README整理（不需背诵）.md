## 1、define、ifdef、ifndef

```
#define            定义一个预处理宏
#undef            取消宏的定义

#if                   编译预处理中的条件命令，相当于C语法中的if语句
#ifdef              判断某个宏是否被定义，若已定义，执行随后的语句
#ifndef            与#ifdef相反，判断某个宏是否未被定义
#elif                若#if, #ifdef, #ifndef或前面的#elif条件不满足，则执行#elif之后的语句，相当于C语法中的else-if
#else              与#if, #ifdef, #ifndef对应, 若这些条件不满足，则执行#else之后的语句，相当于C语法中的else
#endif             #if, #ifdef, #ifndef这些条件命令的结束标志.
defined         　与#if, #elif配合使用，判断某个宏是否被定义
```



## 2、pragma使用方法

```
 1、#pragma用于指示编译器完成一些特定的动作
```

       2、#pragma所定义的很多指示字是编译器特有的(每种编译可能都不一样)
    
              （1）#pragma message 用于自定义编译信息
    
              （2）#pragma once 用于保证头文件只被编译一次
    
              （3）#pragama pack用于指定内存对齐(一般用在结构体)
    
                        struct占用内存大小
    
                            1）第一个成员起始于0偏移处
    
                            2）每个成员按其类型大小和pack参数中较小的一个进行对齐
    
                                  ——偏移地址必须能被对齐参数整除
                                  ——结构体成员的对齐参数(注意是对齐参数，而不是结构体长度)取其内部长度最大的数据成员作为其大小
    
                            3）结构体总长度必须为所有对齐参数的整数倍
                            编译器在默认情况下按照4字节对齐
    
       3、#pragma在不同的编译器间是不可移植的
    
              （1）预处理器将忽略它不认识#pragma指令
    
              （2）不同的编译器可能以不同的方式解释同一条#pragma指令
详细参考：[C语言#pragma使用方法_c pragma-CSDN博客](https://blog.csdn.net/liuchunjie11/article/details/80502529)



## 3、new、new()和new[]区别

```
new A 来创建一个不确定值的对象或实例,new() 创建一个值为零的对象或实例.而new(X),用于创建一个被初始化为X的对象或实例.
只有当A是POD类型的时候,new A和new A()才会有上面的区别
何为POD?POD是plain old data的缩写,它是一个struct或者类,且不包含析构函数以及虚函数
当不是POD时,有构造函数时,两个都被初始化为零,属于默认构造.
没有构造函数是,两个都初始化为一个随机值,且两个值相同.
```

new和new[]区别

```
A * ptr = new A[10];//分配10个A对象

使用new[]分配的内存必须使用delete[]进行释放：

delete [] ptr;  //释放

new对数组的支持体现在它会分别调用构造函数函数初始化每一个数组元素，释放对象时为每个对象调用析构函数。注意delete[]要与new[]配套使用，不然会找出数组对象部分释放的现象，造成内存泄漏。

至于malloc，它并知道你在这块内存上要放的数组还是啥别的东西，反正它就给你一块原始的内存，在给你个内存的地址就完事。所以如果要动态分配一个数组的内存，还需要我们手动自定数组的大小：
int * ptr = (int *) malloc( sizeof(int)* 10 );//分配一个10个int元素的数组
```



## 4、C++类实例化的两种方式：new和不new的区别

```
A a;  // a存在栈上
A* a = new a();  // a存在堆中
```

1 前者在栈中分配内存，后者在堆中分配内存

2 动态内存分配会使对象的可控性增强

3 大程序用new，小程序不加new，直接申请

4 **new必须delete删除，不用new系统会自动回收内存**

- ```
  new创建类对象特点：
  
  - new创建类对象需要指针接收，一处初始化，多处使用
  - new创建类对象使用完需delete销毁
  - new创建对象直接使用堆空间，而局部不用new定义类对象则使用栈空间
  - new对象指针用途广泛，比如作为函数返回值、函数参数等
  - 频繁调用场合并不适合new，就像new申请和释放内存一样
  ```

  

## 5、new 动态数组

| int* pi = new int(1);     | 表示动态分配一个int ，初始化为 1  |
| ------------------------- | --------------------------------- |
| char* pc = new char('c'); | 和上方同理，初始化为字符c         |
| int* pa = new int[1];     | 表示动态分配一个数组，数组大小为1 |

### 删除方法（尤其注意）

```
char *str1=new char[5];
delete [] str1;//创建用new []，释放用delete []
char *str2=new char;
delete str2;//创建用new,释放用delete
```

疑问： Box* myBoxArray = new Box[4]; 在菜鸟教程里面有这么一段代码，对一个对象开辟4个数组，这到底是干嘛的，要怎么用？

### 一维数组

```
// 动态分配,数组长度为 m
int *array=new int [m];
 
//释放内存
delete [] array;
```



### 二维数组

```
int **array;
// 假定数组第一维长度为 m， 第二维长度为 n
// 动态分配空间
array = new int *[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int [n];
}
//释放
for( int i=0; i<m; i++ )
{
    delete [] array[i];
}
delete [] array;
```

### 三维数组

```
int ***array;
// 假定数组第一维为 m， 第二维为 n， 第三维为h
// 动态分配空间
array = new int **[m];
for( int i=0; i<m; i++ )
{
    array[i] = new int *[n];
    for( int j=0; j<n; j++ )
    {
        array[i][j] = new int [h];
    }
}
//释放
for( int i=0; i<m; i++ )
{
    for( int j=0; j<n; j++ )
    {
        delete[] array[i][j];
    }
    delete[] array[i];
}
delete[] array;
```

### vector动态数组

首先介绍一下容器的方法：a.resize()方法使用：
a.resize(n):如果n>a.size()，则在a.end()之前插入n-a.size()个元素；否则，删除第n个元素之后的所有元素。
a.resize(n,t): 如果n>a.size(), 则在a.end()之前插入 t 的n-a.size()个拷贝；否则，删除第n个元素之后的所有元素。

```
方法一：
vector<vector<int> > a;
a.resize(row, vector<int>(column));

方法二：
vector<vector<int> > a(row, vector<int>(column)); //默认用0初始化
//vector<vector<int> > a(row, vector<int>(column,-1));//用-1初始化二维数组
注：如果row > a.size()，则在a.end()之前插入vector<int>(column)的row次拷贝，相当于row行column列

方法三：
vector<vector<int> > a;
a.resize(row);
for (int i = 0; i < row; ++i)
	a[i].resize(column);

```



## 6、new类的用法

**类(class)的基本概念和用法**

①对象：通过类，我们可以自定义变量(与结构体相似)。由类定义出来的变量又称为类的实例，也就是我们所说的对象。

②**内存分配**：对象所占用的内存空间的大小，等于所有成员变量的大小之和。

③对象间的运算：和结构变量一样，对象之间可以用”=”进行赋值(**复制构造函数**),但在未经重载的情况下不可以用”==”,”>”,”<”等运算符进行比较。(重载就是重新定义这些运算符,利用operaor操作)

④使用类的成员变量和成员函数:

```
#include <iostream>
using namespace std;
 
class Box
{
   public:
      double length;   // 长度
      double breadth;  // 宽度
      double height;   // 高度
      // 成员函数声明
      double get(void);
      void set( double len, double bre, double hei );
};
// 成员函数定义
double Box::get(void)
{
    return length * breadth * height;
}
 
void Box::set( double len, double bre, double hei)
{
    length = len;
    breadth = bre;
    height = hei;
}
int main( )
{
   Box Box1;        // 声明 Box1，类型为 Box
   Box Box2;        // 声明 Box2，类型为 Box
   Box Box3;        // 声明 Box3，类型为 Box
   double volume = 0.0;     // 用于存储体积
 
   // box 1 详述
   Box1.height = 5.0; 
   Box1.length = 6.0; 
   Box1.breadth = 7.0;
 
   // box 2 详述
   Box2.height = 10.0;
   Box2.length = 12.0;
   Box2.breadth = 13.0;
 
   // box 1 的体积
   volume = Box1.height * Box1.length * Box1.breadth;
   cout << "Box1 的体积：" << volume <<endl;
 
   // box 2 的体积
   volume = Box2.height * Box2.length * Box2.breadth;
   cout << "Box2 的体积：" << volume <<endl;
 
 
   // box 3 详述
   Box3.set(16.0, 8.0, 12.0); 
   volume = Box3.get(); 
   cout << "Box3 的体积：" << volume <<endl;
   return 0;
}
```

### 调用方式

方式一：**对象名.成员名**

```
#int main()
{
	Box b1,b2;
	b1.w=5;
	b2.w=3;
}
```

方式二：**指针->成员名**

```
#int main()
{
	Box b1,b2;
	Box *p1 = &b1;
	Box *p2 = &b2;
	p1->w=5;
	p2->w=3;
}
```

方式三：**引用名.成员名**

```
#int main()
{
	Box b1;
	Box & p = b1;
	p.w=5;
}
```

方式四：重载+构造函数

```
class a
{
	private:
	int b;
	int c;
public:
	a(int d,int e)
	{
		b=d;
		c=e;
	}
	a(int d)
	{
		b=d;
	}
}
#int main()
{
	a aa={1,2};
	a aa(1,2);
}
```

方式五：

### 派生类

一个派生类继承了所有的基类方法，但下列情况除外：

- 基类的构造函数、析构函数和拷贝构造函数。
- 基类的重载运算符。
- 基类的友元函数。

```
#include <iostream>
using namespace std;
// 基类
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 派生类
class Rectangle: public Shape
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   return 0;
}
```

### 多继承



```
#include <iostream>
using namespace std;
// 基类 Shape
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 基类 PaintCost
class PaintCost 
{
   public:
      int getCost(int area)
      {
         return area * 70;
      }
};
 
// 派生类
class Rectangle: public Shape, public PaintCost
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
   int area;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   area = Rect.getArea();
   
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   // 输出总花费
   cout << "Total paint cost: $" << Rect.getCost(area) << endl;
 
   return 0;
}
//结果
//Total area: 35
//Total paint cost: $2450
```







## 8、关键字operator

```
Box operator+(const Box&);
```

```
#include <iostream>
using namespace std;
class Box
{
   public:
 
      double getVolume(void)
      {
         return length * breadth * height;
      }
      void setLength( double len )
      {
          length = len;
      }
 
      void setBreadth( double bre )
      {
          breadth = bre;
      }
 
      void setHeight( double hei )
      {
          height = hei;
      }
      // 重载 + 运算符，用于把两个 Box 对象相加
      Box operator+(const Box& b)
      {
         Box box;
         box.length = this->length + b.length;
         box.breadth = this->breadth + b.breadth;
         box.height = this->height + b.height;
         return box;
      }
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   Box Box3;                // 声明 Box3，类型为 Box
   double volume = 0.0;     // 把体积存储在该变量中
 
   // Box1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // Box2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // Box1 的体积
   volume = Box1.getVolume();
   cout << "Volume of Box1 : " << volume <<endl;
 
   // Box2 的体积
   volume = Box2.getVolume();
   cout << "Volume of Box2 : " << volume <<endl;
 
   // 把两个对象相加，得到 Box3
   Box3 = Box1 + Box2;
 
   // Box3 的体积
   volume = Box3.getVolume();
   cout << "Volume of Box3 : " << volume <<endl;
 
   return 0;
}
结果如下
Volume of Box1 : 210
Volume of Box2 : 1560
Volume of Box3 : 5400
```



## 多态

**多态**按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。

C++ 多态意味着调用成员函数时，会根据调用函数的对象的类型来执行不同的函数。

```
#include <iostream> 
using namespace std;
 
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      virtual int area()  //需要设置为虚函数
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};
class Rectangle: public Shape{
   public:
      Rectangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Rectangle class area :" <<endl;
         return (width * height); 
      }
};
class Triangle: public Shape{
   public:
      Triangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Triangle class area :" <<endl;
         return (width * height / 2); 
      }
};
// 程序的主函数
int main( )
{
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);
 
   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();
 
   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();
   
   return 0;
}
```

结果：

```
Rectangle class area :
Triangle class area :
```

### 虚函数

**虚函数** 是在基类中使用关键字 **virtual** 声明的函数。在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。

我们想要的是在程序中任意点可以根据所调用的对象类型来选择调用的函数，这种操作被称为**动态链接**，或**后期绑定**。

### 纯虚函数

## 纯虚函数

您可能想要在基类中定义虚函数，以便在派生类中重新定义该函数更好地适用于对象，但是您在基类中又不能对虚函数给出有意义的实现，这个时候就会用到纯虚函数。

我们可以把基类中的虚函数 area() 改写如下：

```
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      // pure virtual function
      virtual int area() = 0;
};
```

= 0 告诉编译器，函数没有主体，上面的虚函数是**纯虚函数**。



## 内存隔离

使用了 **虚拟地址空间**

用户态进程每次访问内存，都要通过页表，**操作系统只要控制页表**，把不同进程的虚拟地址映射到不同的物理地址上，**就可以实现进程间的内存隔离了**。同时，用户代码不能访问页表，所以用户进程也就不能修改内存映射了。





## 静态库与动态库

**静态库（.a、.lib）**

这类库在编译的时候会直接整合到目标程序中，所以利用静态函数库编译成的文件会比较大。

**优点：**

①静态库被打包到应用程序中加载速度快
②发布程序无需提供静态库，移植方便

**缺点：**

①相同的库文件数据可能在内存中被加载多份，消耗系统资源，浪费内存
②库文件更新需要重新编译项目文件，生成新的可执行程序，浪费时间。



**动态库（.so、.dll）**

与静态函数库被整个捕捉到程序中不同，动态函数库在编译的时候，在程序里只有一个“指向”的位置而已，也就是说当可执行文件需要使用到函数库的机制时，程序才会去读取函数库来使用

①可实现不同进程间的资源共享
②动态库升级简单，只需要替换库文件，无需重新编译应用程序
③可以控制何时加载动态库，不调用库函数动态库不会被加载                                                       

缺点：

①加载速度比静态库慢
②发布程序需要提供依赖的动态库



## **C++** **中内存分配情况**

**栈**：由编译器管理分配和回收，存放局部变量和函数参数。

**堆**：由程序员管理，需要⼿动 new malloc delete free 进⾏分配和回收，空间较⼤，但可能会出现内存泄漏和空闲碎⽚的情况。

**全局/静态存储区**：分为初始化和未初始化两个相邻区域，存储初始化和未初始化的全局变量和静态变量。

**常量存储区**：存储常量，⼀般不允许修改。

**代码区**：存放程序的⼆进制代码。



## 内联函数

在C语言中，如果一些函数被频繁调用，不断地有函数入栈，即函数栈，会造成栈空间或栈内存的大量消耗。为了解决这个问题，特别的引入了inline修饰符，表示为内联函数。

（1）内联是以代码膨胀（复制）为代价，仅仅省去了函数调用的开销，从而提高函数的执行效率。



## strcpy , memset函数实现

```
char* strcpy(char *dest,const char *src) {
     char *ret = dest;
     assert(dest!=NULL);//优化点1：检查输⼊参数
     assert(src!=NULL);
     while(*src!='\0')
     	*(dest++)=*(src++);
     *dest='\0';//优化点2：⼿动地将最后的'\0'补上
     return ret;
}
//考
```

```

void *my_memset(void *dest, int set, unsigned len)
{
	if (dest == NULL || len < 0)
	{
		return NULL;
	}
	char *pdest = (char *)dest;
	while (len-->0)
	{
		*pdest++ = set;
	}
	return dest;
```



```
//把 str1 所指向的字符串和 str2 所指向的字符串进⾏⽐较。
//该函数返回值如下：
//如果返回值 < 0，则表示 str1 ⼩于 str2。
//如果返回值 > 0，则表示 str1 ⼤于 str2。
//如果返回值 = 0，则表示 str1 等于 str2。
int strcmp(const char *s1,const char *s2) {
     assert(s1!=NULL);
     assert(s2!=NULL);
     while(*s1!='\0' && *s2!='\0') {
         if(*s1>*s2)
         	return 1;
         else if(*s1<*s2)
         	return -1;
         else {
         	s1++,s2++;
     }
     }
     //当有⼀个字符串已经⾛到结尾
     if(*s1>*s2)
     	return 1;
     else if(*s1<*s2)
     	return -1;
     else
     	return 0;
}
```



```
//把 src 所指向的字符串追加到 dest 所指向的字符串的结尾。
char* strcat(char *dest,const char *src) {
     //1. 将⽬的字符串的起始位置先保存，最后要返回它的头指针
     //2. 先找到dest的结束位置,再把src拷⻉到dest中，记得在最后要加上'\0'
     char *ret = dest;
     assert(dest!=NULL);
     assert(src!=NULL);
     while(*dest!='\0')
     	dest++;
     while(*src!='\0')
      	*(dest++)=*(src++);
     *dest='\0';
     return ret;
}
```



assert函数

用于在运行时检查一个条件是否为真，如果条件不满足，则运行时将终止程序的执行并输出一条错误信息。



## 位域

C 语言的位域（bit-field）是一种特殊的结构体成员，允许我们按位对成员进行定义，指定其占用的位数。



## signed和unsigned

**signed**关键字只能用于修饰整数类型，不能用于修饰浮点类型或其他类型。

- **signed**关键字不能和**const**，**volatile**或**static**等其他修饰符混用，这会造成语义错误。如果想要表示一个常量，易变量或静态变量，只需要在**signed**关键字之前或之后使用相应的修饰符即可。
- 

**有符号整数**和**无符号整数**

C语言中的有符号整数类型有四种：signed char，signed short，signed int和signed long。它们的取值范围和精度取决于编译器和平台的实现，但一般来说，它们遵循以下规则：

signed char的取值范围是-128到127，占用1个字节（8位）的存储空间。
signed short的取值范围是-32768到32767，占用2个字节（16位）的存储空间。
signed int的取值范围是-2147483648到2147483647，占用4个字节（32位）的存储空间。
signed long的取值范围是-9223372036854775808到9223372036854775807，占用8个字节（64位）的存储空间。
————————————————————————————————————————————————————————————

有符号整数的表示方法是采用二进制补码，也就是说，最高位（最左边的一位）是符号位，用于表示正负，0表示正，1表示负。其余的位是数值位，用于表示数值的大小。例如，以下是一些有符号整数的二进制补码表示：

42的二进制补码是00000000 00000000 00000000 00101010，符号位是0，表示正数，数值位是101010，表示42。
-42的二进制补码是11111111 11111111 11111111 11010110，符号位是1，表示负数，数值位是101010的按位取反加一，也就是010101的取反是101010，再加一是101011，表示-42。
0的二进制补码是00000000 00000000 00000000 00000000，符号位是0，表示正数，数值位是全0，表示0。
-128的二进制补码是10000000，符号位是1，表示负数，数值位是全0，表示-128。
————————————————



## typedef

### 1、传统的用法

C 语言提供了 **typedef** 关键字，您可以使用它来为类型取一个新的名字。下面的实例为单字节数字定义了一个术语 **BYTE**：

```
typedef unsigned char BYTE;
```

在这个类型定义之后，标识符 BYTE 可作为类型 **unsigned char** 的缩写，例如：

```
BYTE  b1, b2;
```

### 2、结构体中的使用

在旧的版本中，形式为：<font color='blue'> struct 结构名 对象名 </font>

```
struct Student
{
	int a;
};
struct Student Stu1;   
```

在新的用法中，形式为：<font color='blue'>结构名 对象名 </font>

```
typedef struct Student
{
	int a;
}Stu;
Stu stu1;  //这里的Stu就相当于struct student的别名
```

#### 需要注意的地方

在C++当中使用，要注意区别

```
struct Student
{
	int a;
}stu1;//stu1是一个变量
stu1.a;  //调用方法
```

```
struct 结构名
{
类型 变量名;
类型 变量名;
...
} 结构变量;

```

<font color='red'>结构名是结构的标识符不是变量名。</font>

```
typedef struct 结构名
{
类型 变量名;
类型 变量名;
...
} 结构别名;
```

<font color='red'>在C中，struct不能包含函数。在C++中，对struct进行了扩展，可以包含函数。</font>

```
typedef struct Student2
{
	int a;
}stu2;//stu2是一个结构体类型
stu2 s2;
s2.a=10;  //调用方法
```

使用时可以直接访问<font color='red'>stu1.a </font>
但是stu2则必须先 stu2 s2;
然后 s2.a=10;



### 3、typedef和define的区别

- **typedef** 仅限于为类型定义符号名称，**#define** 不仅可以为类型定义别名，也能为数值定义别名，比如您可以定义 1 为 ONE。
- **typedef** 是由编译器执行解释的，**#define** 语句是由预编译器进行处理的。



## using的使用方法

命名空间

1、导入命名空间

```
// 导入整个命名空间到当前作用域
using namespace std;
// 只导入某个变量到当前作用域 
using std::cout; 
```

2、指定别名

C++ 11 通过 using 指定别名，作用等同于 typedef，但相比 typedef，逻辑更直观，可读性更好。

```
typedef int T; // 用 T 代替 int
using T = int; // 用 T 代替 int
```

3、在派生类中引用基类成员

<img src="README整理（不需背诵）.assets/v2-c8efe8b7be239e2a01c2089b7bb64880_1440w.webp" alt="img" style="zoom:50%;" />

## template 函数模板

1. **为什么有函数模板**
   当某几个函数算法相同但是参数的类型不同时，使用函数模板能够简化代码量，使程序更简洁

2. **函数模板是什么**
   函数模板就是将具体函数中的数据类型参数化—即用通用的参数取代函数中具体的数据类型，从而形成一个通用模板来代表数据类型不同的一组函数。

3. 函数模板怎么用
   定义函数模板的语法如下：

```
template <class T1,class T2,…,class Tn>
返回值类型 函数名(用模板参数取代具体类型的形参列表)
{
    用模板参数取代具体数据类型的函数体；
}
```

实例用法：

```
#include<stdio.h>
template<class T>
T max(T num1, T num2)  //函数模板
{
    T themax;
    themax = (num1 >= num2) ? num1 : num2;
    return themax;
}
int main() {
    int x1;
    double x2;
    x1 = max(1, 2);
    x2 = max(1.2, 2.2);
    printf("%d %.3f", x1, x2);
    return 0;
}
```



## char/unsigned char  char*/unsigned char *区别

vc编译器、x86上的 gcc 都把 char 定义为 signed char；而 arm-linux-gcc 却把 char 定义为 unsigned char 

（1）char和unsigned char

相同点：在内存中都是一个字节，8位（2^8=256），都能表示256个数字
不同点：char的最高位为符号位，因此char能表示的数据范围是-128~127，unsigned char没有符号位，因此能表示的数据范围是0~255

```
char a = -1;					//结果 -1
signed char b = -1;				//结果 -1
unsigned char c = -1;			//结果 255
```

（2）char*和unsigned char *

char*是有符号的，如果大于127即0x7F的数就是负数了，使用%x格式化输出，系统自动进行了符号扩展，就会产生变化。

在C语言中，unsigned char* 和 char*之间可以进行隐式转换：

	char buffer[2048] = {"hello world!"};
	unsigned char* p;
	p = buffer;		//编译器不报错
但在C++中，unsigned char* 和 char*之间不可以进行隐式转换：

```
char buffer[2048] = {"hello world!"};
unsigned char* address_mac = nullptr;
address_mac = buffer;	//不能将 "char *" 类型的值分配到 "unsigned char *" 类型的实体*
```

```
unsigned char buffer[2048] = { "hello world!" };
char* address_mac = nullptr;
address_mac = buffer;    //不能将 "unsigned char *" 类型的值分配到 "char *" 类型的实体
```



## printf格式

1. %d 有符号10进制整数

2. %i 有符号10进制整数
3. %o 无符号8进制整数
4. %u 无符号10进制整数
5. %x 无符号的16进制数字，并以小写abcdef表示
6. %X 无符号的16进制数字，并以大写ABCDEF表示
7. %F/f 浮点数
8. %E/e 用科学表示格式的浮点数
9. %g 使用%f和%e表示中的总的位数表示最短的来表示浮点数 G 同g格式，但表示为指数
10. %c 单个字符
11. %s 字符串
12. %p 将所给存储单元以十六进制输出指针变量对应的地址值。

**（1）整型数**

%d，十进制整型；------->有符号的十进制整型

%ld, 十进制长整型；

%3d, 位数为3，不足在左边补空格；

%-3d, 位数为3，不足在右边补空格；（-可以理解为非，默认是在左边加0和空格的，-表示不是在左边，那就是在右边了？）

%05d, 位数为5，不足的在左边补0 //不可能在右边补0
<font color=purple>%u, 无符号十进制整型；</font> 

%lu, 无符号十进制长整型；

%o, 无符号八进制整型；//形如012

%lo, 无符号八进制长整型；

%x, 无符号十六进制整型；//形如0x12

%X, 无符号十六进制整型大写；//形如0xAA

<font color=purple>%04x, 位数为4，不足的在左边补0-------------------->经常使用</font>  

%lx,无符号十六进制长整型；

**（2）实型**

%f   float

%lf  double实型；

m.n：<font color=purple>m指域宽</font>，即实型数所占的总的位数，包含小数点(并不是整数部分的位数！！）。<font color=purple>n指精度</font>,即实型数的小数位数。

未指定n时，隐含的精度为n=6位。即%f的话，输出的是6位小数。

%f：不指定宽度，整数部分全部输出并输出6位小数。
%m.nf：输出共占m列，其中有n位小数，如数值宽度小于m左端补空格。 
%-m.nf：输出共占n列，其中有n位小数，如数值宽度小于m右端补空格。

m的存在，与整型变量的位数类似，主要是为了保持数据的整齐。
**（3）字符**

%c,字符

**（4）字符串**

%s，字符串

%7s，字符串,不足7位的在左边补空格//形式跟int一样

<font color=purple>%07s，字符串，不足7位的在左边补0//形式跟int一样</font>

**（5）指针**

%p,指针



## 大小端

**所谓的大端模式**，就是高位字节排放在内存的低地址端，低位字节排放在内存的高地址端。

**所谓的小端模式**，就是低位字节排放在内存的低地址端，高位字节排放在内存的高地址端。

```
union{
        short i;
        char a[2];
    }u;
    u.a[0] = 0x11;
    u.a[1] = 0x02;
    printf("0x%02x\n",u.i);
```



## 常见函数

1、**strstr**  在一个字符串中是否存在一个子字符串

```
char *strstr(const char *haystack, const char *needle)
```

2、**memcpy()**  函数用于：复制内存块（将 *num* 字节值从源指向的位置直接复制到目标内存块）

```
void * memcpy ( void * destination, const void * source, size_t num );
```

3、**memcmp()**  把存储区 str1 和存储区 str2 的前 n 个字节进行比较，主要用来比较字符串的。

```
int memcmp(const void *str1, const void *str2, size_t n));
```

4、**atoi**   将字符串里的数字字符转化为整形数。返回整形值。

```
int atoi(const char *nptr);
```

5、**atol**  把参数 **str** 所指向的字符串转换为一个长整数（类型为 long int 型）

```
long int atol(const char *str)
```

6、

## 全局变量

<font color=blue>extern声明的全局变量的作用范围是整个工程</font>，我们通常在“.h”文件中声明extern变量之后，在其他的“.c”或者“.cpp”中都可以使用。extern扩大了全局变量的作用域范围，拓展到整个工程文件中。
我们注意的问题是如何使用extern修饰全局变量，可能在使用过程中出现了这样那样的问题，尤其是链接问题。
推荐的用法：

1.h文件中

```
extern int e_i;//全局变量的声明
```

1.cpp或者1.c文件中

```
#include "1.h"
int e_i = 0;//全局变量的定义
```


其他.cpp或者.c文件中可以直接使用e_i变量。

```
#include "1.h"
int i = e_i;
```

## enum枚举



```
#include <stdio.h>
 
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
 
int main()
{
    enum DAY day;
    day = WED;
    printf("%d",day);
    return 0;
}
```



## union共用体



几个不同的变量共同占用一段内存的结构，被称为**共用体类型结构**，简称**共用体**。一般定义形式为：

```
union 共用体名 
{ 
    数据类型 成员名 1; 
    数据类型 成员名 2; 
    ...... 
    数据类型 成员名 n; 
}变量名表列;
```



共用体类型数据具有以下**特点**：
（1）同一个内存段可以用来存放几种不同类型的成员，但是在每一瞬间只能存放其中的一种，而不是同时存放几种。换句话说，**每一瞬间只有一个成员起作用，其他的成员不起作用，即不是同时都存在和起作用的**。
（2）**共用体变量中起作用的成员是最后一次存放的成员**，在存入一个新成员后，原有成员就失去作用。**共用体变量的地址和它的各成员的地址都是同一地址**。

不能对共用体变量名赋值，也不能企图引用变量名来得到一个值，并且，不能在定义共用体变量时对它进行初始化。
不能把共用体变量作为函数参数，也不能是函数返回共用体变量，但可以使用指向共用体变量的指针。共用体类型可以出现在结构体类型的定义中，也可以定义**共用体数组**。反之，结构体也可以出现在共用体类型的定义中，数组也可以作为共用体的成员。

例子：

```
#include<stdio.h>
union INFO
{
    int a;
    int b;
    int c;
};
int main()
{
    union INFO A;
    A.a=1;
    A.b=2;
    A.c=3;
    printf("a:%d\n",A.a);
    printf("b:%d\n",A.b);
    printf("c:%d\n",A.c);
    return 0;
}
运行结果如下：
a:3
b:3
c:3
```

## 位域



## 常见函数

### IO函数

#### getchar

**getchar**函数的功能是接收用户从键盘上输入的一个字符

**getchar**会以返回值的形式返回接收到的字符。即该字符的**ASC码**，通常的用法如下：

```
char c;  /*定义字符变量c*/
c=getchar();  /*将读取的字符赋值给字符变量c*/
```

#### putchar

**putchar函数**是**字符输出函数**，其功能是在终端（显示器）输出单个字符

函数原型为：

```
int putchar(int ch);
```

ch表示要输出的字符内容，返回值作用为：如果输出成功返回一个字符的ASC码，失败则返回EOF即-1，如代码：

```
putchar(‘A’); /*输出大写字母A */
putchar(x); /*输出字符变量x的值*/
putchar(‘\n’); /*换行*/
```

#### gets

格式：

```
char *gets(char *str);
```

此过程与scanf函数类似，最主要的不同在于，scanf接收时的结束标志有空格和回车，而gets不包括空格。也就意味着<font color=blue>**gets可以接收空格**</font>本身作为内容的一部分。

```
# include <stdio.h>
int main(void)
{
    char str[100] = "\0";  
    printf("请输入字符串：\n");
    gets(str);
    printf("刚才输入的字符串是：\n");
    printf("%s\n", str);
    return 0;
}
```

需要注意的是gets不会检查输入的字符串长度，即可能超出字符串数组的长度造成内存溢出，这其实也是gets函数不安全的原因。

#### puts

格式：

```
int puts(const char *s);
```

```
#include <stdio.h>
int main(void)
{
    char str[100] = "www.dotcpp.com";
    printf("%s\n", str);  
    puts(str);  
    return 0;
}
```

可以看到puts比printf函数方便得多，不需要指定字符串类型，而且末尾不用加换行符会自动换行，对于单独字符串的使用，确实方便很多。

### 字符串操作函数

#### strsub()

#### strcmp

#### strcpy

strcpy() 函数用于将源字符串（包括 null 终止符）复制到目标字符串数组中。它的原型如下：

```
char *strcpy(char *dest, const char *src);
```

- **dest**：指向用于存储复制内容的目标数组。
- **src**：指向要复制的源字符串。

**返回值**：返回指向目标字符串的指针。

> **注意**：`strcpy()` 不会检查目标数组 `dest` 的大小是否足以容纳源字符串 `src`。如果目标数组太小，将会导致缓冲区溢出，这是一个常见的安全漏洞。

#### strncpy

`strncpy()` 函数类似于 `strcpy()`，但它允许指定最大复制的字符数（包括 null 终止符，如果源字符串足够短的话）。它的原型如下：

```
char *strncpy(char *dest, const char *src, size_t n);
```

- **dest**：指向用于存储复制内容的目标数组。
- **src**：指向要复制的源字符串。
- **n**：指定要复制的最大字符数。

**返回值**：返回指向目标字符串的指针。

> **注意**：
>
> 如果源字符串的长度小于 n，则 strncpy() 会在目标字符串的末尾添加一个 null 终止符。
> 如果源字符串的长度大于或等于 n，则目标字符串不会被 null 终止，除非源字符串的前 n-1 个字符中恰好包含了一个 null 字符。
> 由于 strncpy() 可能不会为目标字符串添加 null 终止符，因此在使用 strncpy() 后，通常需要手动检查并添加 null 终止符（如果尚未添加的话）。



#### strcat



#### 





#### strlen

其中strlen(str)和str.length()和str.size()都可以用来求字符串的长度
str.length()和str.size()是用于求[string类](https://so.csdn.net/so/search?q=string类&spm=1001.2101.3001.7020)对象的成员函数
strlen(str) 是用于求字符串数组的长度，其参数是char*

```
char str[20]="0123456789"; 
int   a=strlen(str); /*a=10;strlen 计算字符串的长度，以\0'为字符串结束标记。 
int   b=sizeof(str); /*b=20;sizeof 计算的则是分配的数组str[20] 所占的内存空间的大小，不受里面存储的内容影响
```



#### sprintf

sprintf函数打印到字符串中（要注意字符串的长度要足够容纳打印的内容，否则会出现内存溢出）

```
int sprintf( char *buffer, const char *format [, argument,…] );
```



#### length

```
string s = "hello world";
int len = s.lenght();
cout<<len<<endl;   //长度为11
```

由于string变量的末尾没有“\0”字符，所以length()返回的是字符串的真实长度，而不是长度+1

#### sizeof

```
char *str1="absde";
char str2[]="absde";
char str3[8]={'a',};
char ss[] = "0123456789";

输出：

sizeof(str1)=4;
sizeof(str2)=6;
sizeof(str3)=8;
sizeof(ss)=11
```



### 文件操作函数

#### open

#### write



#### mkdir

```
#include <sys/stat.h>
#include <sys/types.h>
int mkdir(const char* pathname,mode_t mode);
参数1：创建的目录路径
参数2：定义新目录的权限(可以省略)
```

mode方式：可多个权限相或，如0755表示S_IRWXU | S_IRGRP | S_IXGRP | S_IROTH | S_IXOTH

<img src="README整理（不需背诵）.assets/image-20241012091907567.png" alt="image-20241012091907567" style="zoom:33%;" />

示例代码：

```
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
int main() {
    const char *path = "/home/user/new_directory";
    if (mkdir(path, 0777) == 0) {
        printf("Directory created successfullyn");
    } else {
        perror("Error creating directory");
    }
    return 0;
}
```

跨平台代码

```
#include <stdio.h>

#if defined(_WIN32)     	//window系统
    #include <direct.h>
    #define MKDIR(path) _mkdir(path)
#else								//linux系统
    #include <sys/stat.h>
    #include <sys/types.h>
    #define MKDIR(path) mkdir(path, 0777)
#endif

int main() {
    const char *path = "new_directory";
    if (MKDIR(path) == 0) {
        printf("Directory created successfullyn");
    } else {
        perror("Error creating directory");
    }
    return 0;
}
```



#### rmdir

```
#include <unistd.h>  //头文件
int rmdir(const char* pathname);
```

需要删除目录的对应路径名，并且该目录必须是一个空目录，也就是该目录下只有.和..这两个目录。

返回值：**成功返回0**；**失败返回-1**；并会设置errno

#### system

`system`函数可以调用系统命令，如`mkdir`。其典型用法如下：

```
#include <stdlib.h>
int system(const char *command);
```



示例代码：

```
#include <stdlib.h>
#include <stdio.h>
int main() {
    const char *command = "mkdir -p /home/user/new_directory";
    int ret = system(command);
    if (ret == 0) {
        printf("Directory created successfullyn");
    } else {
        printf("Error creating directoryn");
    }
    return 0;
}
```

在这个例子中，`system`函数调用了系统的`mkdir`命令，尝试创建一个新目录。**这种方法的好处是简单易用，但依赖于系统命令，不够跨平台**。

#### stat

判断该文件或目录是否否存在；得到st_mode，然后判断是不是目录文件。 
  stat()系统调用看是否成功，不成功就不存在，成功判断返回的st_mode是否是一个文件夹。

#### access

access()函数用来判断用户是否具有访问某个文件的权限(或判断某个文件是否存在).

```
#include<unistd.h>
int access(const char *pathname,int mode)
 参数:
         pathname:表示要测试的文件的路径
         mode:表示测试的模式可能的值有:
         R_OK:是否具有读权限
         W_OK:是否具有可写权限
         X_OK:是否具有可执行权限
         F_OK:文件是否存在
返回值:若测试成功则返回0,否则返回-1
```

### 线程、进程操作

#### exit

```
#include <stdlib.h>
void exit(int status);
status：退出状态码。
```

例程：

```
#include <stdio.h>
#include <stdlib.h>
int main() {
    printf("程序开始n");
    // 进行一些操作
    if (/* 某些条件 */) {
        printf("发生错误，退出程序n");
        exit(EXIT_FAILURE); // 退出程序，返回失败状态码
    }
    printf("程序结束n");
    return 0;
}
```



### linux中open类型

**open是linux下的底层系统调用函数，fopen与freopen c/c++下的标准I/O库函数，带输入/输出缓冲。**

open和fopen的区别：

- fread是带缓冲的,read不带缓冲.
- fopen是标准c里定义的,open是POSIX中定义的.
- fread可以读一个结构.read在linux/unix中读二进制与普通文件没有区别.
- fopen不能指定要创建文件的权限.open可以指定权限.
- fopen返回文件指针,open返回文件描述符(整数).
- linux/unix中任何设备都是文件,都可以用open,read.



### 内存操作

\#include <sys/mman.h>

>  int mlock(const void *addr, size_t len);
>
> int munlock(const void *addr, size_t len);
>
> int mlockall(int flags);
>
> int munlockall(void);

系统调用 mlock 家族允许程序在物理内存上锁住它的部分或全部地址空间。这将阻止Linux 将这个内存页调度到交换空间（swap space），即使该程序已有一段时间没有访问这段空间。

### 网络函数

#### sendto

函数说明：sendto() 用来将数据由指定的socket 传给对方主机. 参数s 为已建好连线的socket, 如果利用UDP协议则不需经过连线操作. 参数msg 指向欲连线的数据内容, 参数flags 一般设0, 详细描述请参考send(). 参数to 用来指定欲传送的网络地址, 结构sockaddr 请参考bind(). 参数tolen 为sockaddr 的结果长度.

```
头文件：#include <sys/types.h>   #include <sys/socket.h>
定义函数：int sendto(int s, const void * msg, int len, unsigned int flags, const struct sockaddr * to, int tolen);

返回值：成功则返回实际传送出去的字符数, 失败返回－1, 错误原因存于errno 中.
```

#### send

```
int send( SOCKET s , const char FAR *buf , int len , int flags );  
```

不论是客户还是服务器应用程序都用send函数来向TCP连接的另一端发送数据。客户程序一般用send函数向服务器发送请求，而服务器则通常用send函数来向客户程序发送应答。
第一个参数指定发送端套接字描述符；
第二个参数指明一个存放应用程序要发送数据的缓冲区；
第三个参数指明实际要发送的数据的字节数；
第四个参数一般设置为0。

#### sendmsg



#### recv

```
int recv( SOCKET s , char FAR * buf , int len , int flags );   

第一个参数指定接收端套接字描述符；
第二个参数指明一个缓冲区，该缓冲区用来存放recv函数接收到的数据；
第三个参数**指明buf的长度**；
第四个参数一般置0。
```

不论是客户还是服务器应用程序都用recv函数从TCP连接的另一端接收数据。


这里只描述同步socket的recv函数的执行流程。当应用程序调用recv函数时，==recv先等待s的发送缓冲中的数据被协议传送完毕==，如果协议在传送s的发送缓冲中的数据时出现网络错误，那么recv函数返回SOCKET_ERROR，如果s的发送缓冲中没有数据或者数据被协议成功发送完毕后，==recv先检查套接字s的接收缓冲区，如果s接收缓冲区中没有数据或者协议正在接收数据，那么recv就一直等待，只到协议把数据接收完毕==。当协议把数据接收完毕，recv函数就把s的接收缓冲中的数据copy到buf中（注意协议接收到的数据可能大于buf的长度，所以在这种情况下要调用几次recv函数才能把s的接收缓冲中的数据copy完。recv函数仅仅是copy数据，真正的接收数据是协议来完成的），recv函数返回其实际copy的字节数。如果recv在copy时出错，那么它返回SOCKET_ERROR；如果recv函数在等待协议接收数据时网络中断了，那么它返回0。


#### recvfrom

```
// 接收数据, 如果没有数据,该函数阻塞
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
                 struct sockaddr *src_addr, socklen_t *addrlen);

sockfd: 基于 udp 的通信的文件描述符
buf: 这个指针指向的内存中存储了要发送的数据
len: 要发送的数据的实际长度
flags: 设置套接字属性，一般使用默认属性，指定为 0 即可
dest_addr: 接收数据的一端对应的地址信息，大端的 IP 和端口
addrlen: 参数 dest_addr 指向的内存大小
```

 from是一个struct sockaddr类型的变量，该变量保存源机的IP地址及端口号。fromlen常置为sizeof （struct sockaddr）。当recvfrom() 返回时，fromlen 包含实际存入from中的数据字节数。recvfrom() 函数返回接收到的字节数或当出现错误时返回－1，并置相应的errno。 


#### socket

基于 UDP 进行套接字通信，创建套接字的函数还是 socket() 但是第二个参数的值需要指定为 SOCK_DGRAM，通过该参数指定要创建一个基于报式传输协议的套接字，最后一个参数指定为 0 表示使用报式协议中的 UDP 协议。

```
int socket(int domain, int type, int protocol);

domain：地址族协议，AF_INET -> IPv4，AF_INET6-> IPv6
type：使用的传输协议类型，报式传输协议需要指定为 SOCK_DGRAM
protocol：指定为 0，表示使用的默认报式传输协议为 UDP

返回值：函数调用成功返回一个可用的文件描述符（大于 0），调用失败返回 - 1
```



#### bind





## 内存对齐与强制不对齐

结构体内存对齐规则

1.结构体的第⼀个成员对⻬到和结构体变量起始位置偏移量为0的地址处。

2.其他成员变量要对⻬到某个数字（对⻬数）的整数倍（0，1，2...）的地址处。

**对⻬数=编译器默认的⼀个对⻬数与该成员变量⼤⼩的较⼩值。**

**- VS 中默认的值为 8**

**- Linux中 gcc 没有默认对⻬数，对⻬数就是成员⾃⾝的⼤⼩**

3.**结构体总⼤⼩为最⼤对⻬数（结构体中每个成员变量都有⼀个对⻬数，所有对⻬数中最⼤的）的整数倍。**

**4. 如果嵌套了结构体的情况，嵌套的结构体成员对⻬到⾃⼰的成员中最⼤对⻬数的整数倍处，结构体的整体⼤⼩就是所有最⼤对⻬数（含嵌套结构体中成员的对⻬数）的整数倍。** 



### 内存对齐

（1）普通内存对齐

```
struct S{
    double a;
    char b;
    int c;
    char d;
}S1;  //大小为24字节
```

不要理解为这里是16字节，实际为24字节；首先按照规则一，double a存储在第0-7字节，char b存储在第8字节，int c存储在第12-15字节，char d存储在第16字节；再按照规则二，此时总的存储大小是17个字节，不是最大对齐数8的整数倍，所以进行补齐，最后总的存储大小为24个字节。

![img](README整理（不需背诵）.assets/8ee8d5d933f06e27cfda74097de5c2d7.png)



（2）含有数组结构体

```
struct S5 {
    char a;
    int b[2];
    char c;
    char d;
} s5;//占16字节
```

![img](README整理（不需背诵）.assets/6c4737c235000d2314c09bd56df1a9b4.png)

含指针情况下，不论什么类型的指针在64位操作系统下面都为8字节，以此来计算。

（3）嵌套结构体

```
struct S1 {
    char a;
    int b;
    double c;
};  //占16个字节
 
struct S2 {
    char a;
    S1 b;
}; //占24字节
```

S2中char a存储在第0字节，然后因为结构体变量S1 b的最大对齐数是8，因此从第8个字节开始存储，存储在第8-23字节，然后按照规则二检查存储大小是否是最大对齐数的整数倍，满足，则结束（最大对齐数是8（double类型变量的对齐数），总的存储大小是24）。



案例二：

```
typedef struct ss4
{
    char a;
    int b[2];
    char c;
    char d;
}s4; //占16字节
typedef struct ss5
{
    char a;
    s3 ss;
}s5;  //占20字节
```

### 内存不对齐

#### 修改内存对齐规则

（1）修改内存对齐大小一：

**修改内存对齐**：大小为16

```
// 修改内存对齐方法一
typedef struct S2
{
    int a2;
    char a;
    int b;
    char c;
}__attribute__((aligned(8)))s2;
```



**修改内存对齐**：大小为12

```
// 修改内存对齐方法二
#pragma pack(4)  //设置以4个字节为对齐长度
typedef struct S3
{
	char a;
	double b;
}s3;   //占12字节，原本为16字节
#pragma pack() //设置默认值为对齐长度
```



#### 取消内存对齐

**取消内存对齐方法**：结构体大小为10

```
// 取消内存对齐方法一
typedef struct S4
{
    int a2;
    char a;
    int b;
    char c;
}__attribute__((packed)) s4;  //占10字节
```



```
#pragma pack(1)  //设置以4个字节为对齐长度
typedef struct S6
{
    char c;
    double d;
}s6;   //占9字节
```

\#pragma pack(n)
n的取值可以为1、2、4、8、16，在编译过程中按照n个字结对齐
\#pragma pack()
取消对齐，按照编译器的优化对齐方式对齐
__attribute__ ((packed));
是说取消结构在编译过程中的优化对齐。
__attribute__ ((aligned (n)));
让所作用的成员对齐在n字节自然边界上,如果结构中有成员的长度大于n，则按照最大成员的长度来对齐

visual studio中编译器默认的数字是8（windows），gcc默认是4（Linux）。

#### _ _attribute__

可以设置**函数属性**（Function Attribute）、**变量属性**（Variable Attribute）和**类型属性**（Type Attribute）。

__attribute__与结构体

```
__attribute__((aligned(n)))   //采用n字节对齐
__attribute__((packed))       //采用1字节对齐
```

- `__attribute__((aligned(n)))`中，n的有效参数为2的幂值，32位最大为`2 ^ 32`,64位为`2 ^ 64`，这个时候编译器会将**让n与默认的对齐字节数进行比较，取较大值为对齐字节数**，**与#pragma pack(n)恰好相反。**
- `__attribute__((packed))`则为**取消结构在编译过程中的优化对齐,按照实际占用字节数进行对齐，也就是采用1字节对齐。**

注意：

在64位操作系统64位编译器的环境下，当n ≥ 8时，内存对齐的字节数是n，不然为8
在32位操作系统32位编译器的环境下，当n ≥ 4时，内存对齐的字节数是n，不然为4



## 链表



### 双向链表



## 栈





## 结构体

### 1、结构体定义和使用

结构体创建变量的三种方式

1、struct 结构体名  变量名

2、struct 结构体名  变量名={成员1值, 成员2值}



```
#include <iostream>
#include<string>
using namespace std;
 
struct Student{
	string name;
	int age;
	int score;
}; 
int main()
{
	
	//创建方式1
	struct Student s1; //这里的struct可以省略
	//给S1属性赋值
	s1.name="张三";
	s1.age=18;
	s1.score=30;
	cout<<"姓名: "<<s1.name<<"  年龄: "<<s1.age<<" 分数： "<<s1. score<<endl;
	
	//创建方式2
	 struct Student s2={"李四",12,90};
	 cout<<"姓名: "<<s2.name<<"  年龄: "<<s2.age<<" 分数： "<<s2. score<<endl;
	return 0; 
}
```

3、定义结构体时顺便创建变量

```
#include <iostream>
#include<string>
using namespace std;
 
struct Student{
	string name;
	int age;
	int score;
}s3; 
int main()
{
	//创建方式3
	s3.name="王麻子";
	s3.age=20;
	s3.score=22;
	cout<<"姓名: "<<s3.name<<"  年龄: "<<s3.age<<" 分数： "<<s3. score<<endl;
	return 0; 
}
```

### 2、结构体数组

```
#include <iostream>
#include<string>
using namespace std;
 
struct Student{
	string name;
	int age;
	int score;
}; 
int main()
{
	
	struct Student stuArray[8]=
	{
		{"张三",18,20},
		{"李四",10,30},
		{"王麻子",28,20}
	};
	for(int i=0;i<3;i++)
	{
		cout<<"姓名: "<<stuArray[i].name<<"  年龄: "<<stuArray[i].age<<" 分数： "<<stuArray[i]. score<<endl;
	 } 
	return 0; 
}
```

### 3、结构体指针

使用->访问

```
#include <iostream>
#include<string>
using namespace std;
 
struct Student{
	string name;
	int age;
	int score;
}; 
int main()
{
	struct Student s={"张三",19,20};
	//2、通过指针指向结构体变量
	Student *p = &s;
	cout<<"姓名: "<<p->name<<"  年龄: "<<p->age<<" 分数： "<<p->score<<endl;
	return 0; 
}
```

### 4、结构体嵌套结构体

```
#include <iostream>
#include<string>
using namespace std;
 
struct Student{
	string name;
	int age;
	int score;
}; 
struct Teacher{
	string name;
	int age;
	struct Student stu;  //嵌套的结构体 
}; 
int main()
{
	//创建老师
	Teacher t;
	t.name="老王";
	t.age=90;
	t.stu.name="小王";
	t.stu.age=18;
	t.stu.score=60;
	 
	cout<<"姓名: "<<t.name<<"  年龄: "<<t.age<<" 学生："<<t.stu.name<<" 学生年龄："<<t.stu.age<<
	" 学生的成绩："<<t.stu.score<<endl;
	return 0; 
}
```

### 5、结构体做函数参数

值传递和地址传递的区别：在函数中值传递，不改变整个变量的值，只改变函数内部的值；但是函数中地址传递，函数中改变参数的值，整个值就会发生改变，

```
#include <iostream>
#include<string>
using namespace std;
 
struct Student{
	string name;
	int age;
	int score;
}; 
//第一种传参方式，值传递 
void printStudenInfo1(struct Student s)
{
	s.name="李四";
	cout<<"方式1 姓名: "<<s.name<<"  年龄: "<<s.age<<" 分数： "<<s. score<<endl;
}
//第2种传参方式，地址传递 
void printStudenInfo2(struct Student *s)
{
	s->name="王麻子";
	cout<<"方式2 姓名: "<<s->name<<"  年龄: "<<s->age<<" 分数： "<<s->score<<endl;
}
int main()
{
	
	struct Student s={"张三",18,20};
	
	//第一种传参方式，值传递 
	printStudenInfo1(s);
	cout<<"主函数中显示 姓名: "<<s.name<<"  年龄: "<<s.age<<" 分数： "<<s. score<<endl;
	//第2种传参方式，地址传递 
	printStudenInfo2(&s);
	cout<<"主函数中显示 姓名: "<<s.name<<"  年龄: "<<s.age<<" 分数： "<<s. score<<endl;
	return 0; 
}
```





# 指针

```
%d 将所给存储单元以十进制有符号型形式输出。
%p 将所给存储单元以十六进制输出指针变量对应的地址值。
%x 将所给存储单元以十六进制形式输出。
```

 

1、指针是可存储地址的变量，存储在指针中的地址可以是变量或其他数据的地址。

## 简单指针使用

```
	int var_runoob = 10;
    int *p;              // 定义指针变量
    p = &var_runoob;
    cout<<"指针p指向的值： "<<*p<<endl;              //指针p指向的值： 10
    cout<<"var_runoob 变量的地址： "<<p<<endl;      //var_runoob 变量的地址： 0x7ffc77b6285c
    cout<<"C语法输出\n"<<endl;
    printf("指针p指向的值：%d\n",*p);               //指针p指向的值：10
    printf("var_runoob 变量的地址：%p\n",p);        //var_runoob 变量的地址：0x7ffd84d510ac
    printf("var_runoob 变量的地址：%d\n",p);        //var_runoob 变量的地址：-2066411348
```

（1）空指针

```
   int  *ptr = NULL;
   printf("ptr 的地址是 %p\n", ptr  );   //ptr 的地址是 0x0
```



## 指针和数组的区别

​	　int array[10] = {0};

　　int *p = array;

　　<font color=red>array是数组名，数组第一个元素的地址，是一个常量，不能用于算数运算，array++, array+=1这些运算都是不允许的</font>。指针和它的区别是，<font color=red>指针是一个变量</font>，也就是说，指针的那块内存中你想存啥就存啥

​		void func(int a[]);

　　void func(int *a);

　　<font color=red>以上两种声明方式编译器都会把a理解为一个指针，所以这两个声明是一样的</font>。C语言传参方式是按值传递，当把数组名作为参数传进去的时候，因为把数组名直接作为右值会得到第一个元素的地址，所以相当于把数组第一个元素的地址存储到了a对应的内存中。这也是常说的数组名转换为指针的本质。其实和上面的代码： int *p = array 本质上是一样的，都是把数组名代表的地址赋值给了指针。



## 指针的算数符运算

（1）假设 **ptr** *是一个指向地址 1000 的整型指针，是一个 32 位的整数，执行了`ptr++`之后，**ptr** 将指向位置 1004，因为 ptr 每增加一次，它都将指向下一个整数位置，即当前位置往后移 4 字节。

> - 指针的每一次递增，它其实会指向下一个元素的存储单元。
> - 指针的每一次递减，它都会指向前一个元素的存储单元。
> - 指针在递增和递减时跳跃的字节数取决于指针所指向变量数据类型长度，比如 int 就是 4 个字节。

（2）指针 p++

```
int var[] = {10, 100, 200};
int *p=var;
for(int i=0;i<(sizeof(var)/sizeof(var[0]));i++)
{
    cout<<*p<<endl;
    p++;
}
//结果为数组内容依次输出
```

（3）结构体递增

```
#include <stdio.h>
struct Point {
    int x;
    int y;
};
int main() {
    struct Point points[] = {{1, 2}, {3, 4}, {5, 6}};
    struct Point *ptr = points;  // 指针指向结构体数组的第一个元素

    printf("初始点: (%d, %d)\n", ptr->x, ptr->y);  // 输出 (1, 2)
    ptr++;  // 递增指针，使其指向下一个结构体
    printf("递增后点: (%d, %d)\n", ptr->x, ptr->y);  // 输出 (3, 4)
}
//输出结果
//初始点: (1, 2)
//递增后点: (3, 4)
```

（4）指针位置比较

```
    int a = 5;
    int b = 10;
    int *ptr1 = &a;
    int *ptr2 = &a;
    int *ptr3 = &b;
 
    if (ptr1 == ptr2) {
        printf("ptr1 和 ptr2 指向相同的内存地址\n");  // 这行会被输出
    } else {
        printf("ptr1 和 ptr2 指向不同的内存地址\n");
    }
    //ptr1 和 ptr2 指向相同的内存地址
```



```
	int arr[] = {10, 20, 30, 40, 50};
    int *ptr1 = &arr[1];  // 指向 arr[1]，值为 20
    int *ptr2 = &arr[3];  // 指向 arr[3]，值为 40

    if (ptr1 < ptr2) {
        printf("ptr1 在 ptr2 之前\n");  // 这行会被输出
    } else {
        printf("ptr1 在 ptr2 之后或相同位置\n");
   //ptr1 在 ptr2 之前
   //ptr1 在 ptr2 之前或相同位置
```

（5）遍历数组并比较指针

```
#include <stdio.h>
int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *start = arr;           // 指向数组的第一个元素
    int *end = &arr[4];         // 指向数组的最后一个元素
    int *ptr;
    for (ptr = start; ptr <= end; ptr++) {
        printf("当前指针指向的值: %d\n", *ptr);
    }
    return 0;
}
//此时依次打印数组的值
```



## 指针数组

指针数组是一个数组，其中的每个元素都是指向某种数据类型的指针。

指针数组存储了一组指针，每个指针可以指向不同的数据对象。

C++编译器将 <font color=red>数组名[下标] </font> 解释为 <font color=red>*(数组首地址+下标)  </font>

数组第n个元素的地址是： <font color=red>数组首地址+n</font>

```
   int  var[] = {10, 100, 200};
   int i, *ptr[3];
 
   for ( i = 0; i < 3; i++)
   {
      ptr[i] = &var[i]; /* 赋值为整数的地址 */
   }
   for ( i = 0; i < 3; i++)
   {
      printf("Value of var[%d] = %d\n", i, *ptr[i] );  //打印结果值
      printf("Value of var[%d] = %d\n", i, ptr[i] );	//打印地址
   }
   //第二种方法
   for ( i = 0; i < 3; i++)
   {
      printf("Value of var[%d] = %d\n", i, *(ptr+i) );  //打印结果值
      printf("Value of var[%d] = %d\n", i, ptr+i );	//打印地址
   }
 //依次打印指针的值
```

对于参数传递

```

//传参方式正确
//*arr[20]是指针数组，传过去的是数组名
void test2(int *arr[20])
{}

//传参方式正确
//传过去是指针数组的数组名，代表首元素地址，首元素是个指针向数组的指针，再取地址，就表示二级指针，用二级指针接收
void test2(int **arr)
{}
int main()
{
	int *arr2[20] = {0};
	test2(arr2);
}
```

参考例子

```
void test(int **nums)  //定义方式二
{
    for(int i=0;i<3;i++)
    {
        for(int j=0;j<3;j++)
        {
            cout<<nums[i][j]<<" ";
        }
        cout<<endl;
    }
        
}
void test1(int* nums[])  //定义方式一
{
    for(int i=0;i<3;i++)
    {
        for(int j=0;j<3;j++)
        {
            cout<<nums[i][j]<<" ";
        }
        cout<<endl;
    }
}
int main()
{
    int num1[] = {1,2,3};
    int num2[] = {2,3,4};
    int num3[] = {2,4,5};
    
    //定义方式一
    int *p1 = num1;
    int *p2 = num2;
    int *p3 = num3;
    int* nums[]={p1,p2,p3};
    test(nums);
    test1(nums);
    
    //定义方式二
    int* nums1[3];
    nums1[0] = p1;
    nums1[1] = p3;
    nums1[2] = p2;
    test(nums1);
}
```

对于字符串的操作

注意：<font color=red>这里char指针没有加const关键字，会有警告。</font>

```
	const char *names[] = {
                   "Zara Ali",
                   "Hina Ali",
                   "Nuha Ali",
                   "Sara Ali",
   };
   int i = 0;
 
   for ( i = 0; i < 4; i++)
   {
      printf("Value of names[%d] = %s\n", i, names[i] );
      //printf("Value of names[%d] = %s\n", i, *names[i] );  //这种写法是错误的
   }
```

<font color=red>注意，int指针数组打印数据时需要加星号，char类型打印时不需要加星号。</font>

### 指向指针数组的指针

> char *a[3]={0};
>
> char **p=a;
>
> 解析：p是指向指针型数据的指针变量，初始时为指针数组a的首元素a[0]，a[0]为指针型数据，指向一个char型数组的首元素，而p初值为该元素的地址



## 数组指针（行指针）

语法：<font color=red>数据类型 (*行指针名)[行的大小] </font> ；  行的大小即数组长度。

int (*p1)[3] ;      p1是行指针，用于指向数组长度为3的int型数组。

主要用来存放多维数组，例如

```
int nums[2][3] = {{1,2,3},{2,3,4}};
int (*p)[3] = nums;   //这样是正确的
int* p = nums;    //这样是错误的 
```

（2）把二维数组传递给函数

 void fun(int (*p)[3], int len); 

void fun(int p[] [3], int len);

### 指向数组指针的指针





## 指针数组和数组指针区别

**指针数组**：它实际上是一个数组，数组的每个元素存放的是一个指针类型的元素。

![这里写图片描述](README整理（不需背诵）.assets/09491dfb18d19e113f84c4ed61b7aa96.png)

> int* arr[8];
> //优先级问题：[]的优先级比* 高
> //说明arr是一个数组，而int* 是数组里面的内容
> //这句话的意思就是：arr是一个含有8个int*的数组



**数组指针**：它实际上是一个指针，该指针指向一个数组。

> int (* arr)[8];
> //由于[]的优先级比* 高，因此在写数组指针的时候必须将* arr用括号括起来
> //arr先和*结合，说明p是一个指针变量
> //这句话的意思就是：指针arr指向一个大小为8个整型的数组。





## 二维指针



```
int a = 5;  
int *p = &a;  // p 是一个指向 int 的指针  
int **pp = &p; // pp 是一个指向 p（也就是指向 int* 的指针）的指针  

printf("原始值: %d\n", *p); // 通过 p 访问 a 的值  
p = &a + 1; // 假设这样做只是为了演示（实际上这样做是危险的，因为可能越界）  
printf("修改 p 后的值（但尚未通过 pp 修改）: %d\n", *p); // 此时 p 指向未知的内存  

*pp = &a; // 通过 pp 修改 p 的值，使其重新指向 a 
//pp存放的是p的地址。*pp即对pp值=p的地址解引用，得到p的值=a的地址
printf("通过 pp 修改 p 后，p 指向的值: %d\n", *p);  
```



当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置。

```
   int  V;
   int  *Pt1;
   int  **Pt2;
   V = 100;
   /* 获取 V 的地址 */
   Pt1 = &V;
   /* 使用运算符 & 获取 Pt1 的地址 */
   Pt2 = &Pt1;
 
   /* 使用 pptr 获取值 */
   printf("var = %d\n", V );
   printf("Pt1 = %p\n", Pt1 );
   printf("*Pt1 = %d\n", *Pt1 );
   printf("Pt2 = %p\n", Pt2 );
   printf("**Pt2 = %d\n", **Pt2);
//var = 100
//Pt1 = 0x7ffee2d5e8d8
//*Pt1 = 100
//Pt2 = 0x7ffee2d5e8d0
//**Pt2 = 100
```

### 指针和数组的区别

1. **存储方式：**

数组在内存中是连续存放的，开辟一块连续的内存空间。数组是根据数组的下进行访问的，<font color=red>多维数组在内存中是按照一维数组存储的，只是在逻辑上是多维的。</font>

> **数组的存储空间，不是在静态区就是在栈上。**



> **指针：由于指针本身就是一个变量，再加上它所存放的也是变量，所以指针的存储空间不能确定。**

2. **初始化**

```
（1）char a[]={"Hello"};//按字符串初始化，大小为6.
（2）char b[]={'H','e','l','l'};//按字符初始化（错误，输出时将会乱码，没有结束符）
（3）char c[]={'H','e','l','l','o','\0'};//按字符初始化
```

3. 

​	　int array[10] = {0};

　　int *p = array;

　　<font color=red>array是数组名，数组第一个元素的地址，是一个常量，不能用于算数运算，array++, array+=1这些运算都是不允许的</font>。指针和它的区别是，<font color=red>指针是一个变量</font>，也就是说，指针的那块内存中你想存啥就存啥

​		void func(int a[]);

　　void func(int *a);

　　<font color=red>以上两种声明方式编译器都会把a理解为一个指针，所以这两个声明是一样的</font>。C语言传参方式是按值传递，当把数组名作为参数传进去的时候，因为把数组名直接作为右值会得到第一个元素的地址，所以相当于把数组第一个元素的地址存储到了a对应的内存中。这也是常说的数组名转换为指针的本质。其实和上面的代码： int *p = array 本质上是一样的，都是把数组名代表的地址赋值给了指针。



### 二维指针和指针数组的区别

本质上是没有区别的。







## 函数返回指针

C 语言不支持在调用函数时返回局部变量的地址，除非定义局部变量为 **static** 变量。

```
#include <stdio.h>
#include <time.h>
#include <stdlib.h> 
/* 要生成和返回随机数的函数 */
int * getRandom( )
{
   static int  r[10];
   int i;
   /* 设置种子 */
   srand( (unsigned)time( NULL ) );
   for ( i = 0; i < 10; ++i)
   {
      r[i] = rand();
      printf("%d\n", r[i] );
   }
   return r;
}
/* 要调用上面定义函数的主函数 */
int main ()
{
   /* 一个指向整数的指针 */
   int *p;
   int i;
   p = getRandom();
   for ( i = 0; i < 10; i++ )
   {
       printf("*(p + [%d]) : %d\n", i, *(p + i) );
   }
   return 0;
}
```

```
//输出结果
1523198053
1187214107
1108300978
430494959
1421301276
930971084
123250484
106932140
1604461820
149169022
*(p + [0]) : 1523198053
*(p + [1]) : 1187214107
*(p + [2]) : 1108300978
*(p + [3]) : 430494959
*(p + [4]) : 1421301276
*(p + [5]) : 930971084
*(p + [6]) : 123250484
*(p + [7]) : 106932140
*(p + [8]) : 1604461820
*(p + [9]) : 149169022

```



## 函数指针

声明格式：类型说明符 （*函数名）（参数）

```
int (*fun)(int x,int y);
```

本质是一个指针，指向函数的指针变量，其包含了函数的地址，通过它来调用函数。

对函数的调用可以使用：方法一：<font color= Lime>fun()</font>；方法二：<font color= Lime>(*fun)()</font>

```
int add(int x,int y){
    return x+y;
}
int sub(int x,int y){
    return x-y;
}
//函数指针
int (*fun)(int x,int y);

int main(int argc, char *argv[])
{
    //第一种写法
    fun = add;
    cout<< "(*fun)(1,2) = " << (*fun)(1,2) <<endl;
	//第二种写法
    fun = &sub;
    cout << "(*fun)(5,3) = " << (*fun)(5,3)  <<endl<<"fun(5,3)= "<< fun(5,3) <<endl;
    
    //第三种写法
    fun = add;
    cout<< "fun(1,2) = " << fun(1,2) <<endl;
    return 0;
}
```

## 回调函数



**函数指针作为某个函数的参数**

函数指针变量可以作为某个函数的参数来使用的，回调函数就是一个通过函数指针调用的函数。

```
#include <stdlib.h>  
#include <stdio.h>
 
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}
 
// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}
 
int main(void)
{
    int myarray[10];
    /* getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针*/
    populate_array(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}
```



## 指针函数

声明格式：*类型标识符  函数名（参数表）

```
int *fun(int x,int y);
```

本质是一个函数，此函数返回某一类型的指针。

```
typedef struct _Data{
    int a;
    int b;
}Data;
//指针函数
Data* f(int a,int b){
    Data * data = new Data;
    data->a = a;
    data->b = b;
    return data;
}

int main(int argc, char *argv[])
{
    //调用指针函数
    Data * myData = f(4,5);
    cout<< "f(4,5) = " << myData->a << myData->b<<endl;
    return 0;
}
```



## 指针作为函数参数

如果把函数的形参声明为指针，调用的时候把实参的地址传进去，形参中存放的是实参的地址，在函数中通过解引用的方法直接操作内存中的数据，可以修改实数的值，这种方法被通俗的称为**地址传递**或**传地址**。

**值传递**：函数的形参是普通变量。

传地址的意义：

（1）可以在函数中修改实参的值。

（2）减少内存拷贝，提升性能。



## const修饰指针

（1）常量指针

语法   <font color=blue>const 数据类型  *变量名</font>

<font color=red>不能通过解引用的方法修改内存地址中的值（用原始的变量名是可以修改的）</font>。

注意：

- 指向的变量（对象）可以改变（之前是指向变量a的，后来可以改为指向变量b）。
- 一般用于修饰函数的形参，表示不希望在函数里修改内存地址中的值。
- 如果用于形参，虽然指向的对象可以改变，但这样做没有任何意义。
- 如果形参的值不需要改变，建议加上const修饰，程序可读性更好。

```
int a=3;
const int *p = &a;
cout<<"*p= "<<*p<<endl;

但是下方的操作就是错误的
*p=10;  //这句就会报错
a=10;   //这句是正确的
cout<<"*p= "<<*p<<endl;
```

在函数中作为形参的表示。

```
fun(const int *f1,const string *str)
{
	以下两句就会报错
	// *f1 = 10;
	// *str = "这句话会报错";
	cout<<"f1："<<*f1<<", str="<<*str<<endl;
}
```

（2）指针常量

语法：<font color=blue>数据类型 * const 变量名</font>；

指向的变量（对象）不可改变。

注意：

- 在定义的同时必须初始化，否则没有意义。
- 可以通过解引用的方式修改内存中的值。
- C++编译器把指针常量做了一些特别的处理，改头换面之后，有一个新名字，叫引用。

```
int a=1, b=2;
//int *const p;  //如果不赋初值，就会报错。
int *const p = &a;
*p=10;  //可以修改指针的值
p = &b; //此时是不可以的，会报错。
```

（3）常指针常量

语法： <font color=blue>const 数据类型 * const 变量名 </font>;

指向的变量（对象）不可改变，不能通过解引用的方法修改内存地址中的值。



**总结：**

常量指针：指针指向可以修改，指针指向的值不可以修改。

指针常量：指针指向不可以改，指针指向的值可以更改。

常指针常量：指针指向不可以改，指针指向的值不可以更改。

理解这些声明的技巧在于，查看关键字const右边来确定什么被声明为常量 ，如果该关键字的右边是类型，则值是

常量；如果关键字的右边是指针变量，则指针本身是常量。

## 函数形参 void*

<font color=red>函数形参用void*，表示接受任意数据类型的指针。</font>

注意：

- 不能用void声明变量，它不能代表一个真实的变量。
- 不能对void*指针直接解引用（需要转换成其他类型的指针）。
- 把其他类型的指针赋值给void*指针不需要转换。
- 把void* 指针赋值给其它类型的指针需要转换。

## 动态内存分配new

（1）声明一个指针；

（2）用new运算符向系统申请一块内存，让指针指向这块内存。

（3）通过对指针解引用的方法，像使用变量一样使用这块内存；

（4）如果这块内存不用了，使用delete运算符释放它。

```
int *p = new int(5);  //类似于int *p = 5
```



## 二级指针



## 空指针

在C或C++中，用0或NULL都可以表示空指针。声明指针后，在赋值之前，让它指向空，表示没有指向任何地址。

<font color=red>C++11建议使用nullptr表示空指针，也就是 (void*)0 </font>；

（1）使用空指针的后果

1. 如果对空指针解引用，程序会崩溃。
2. 如果对空指针使用delete运算符，系统将忽略该操作，不会出现异常。所以，内存被释放后，也应该把指针指向空。



## 野指针

野指针就是指针指向的不是一个有效（合法）的地址。在程序中，如果访问野指针，可能会造成程序的崩溃。

（1）出现野指针的情况主要有三种：

1. 指针在定义的时候，如果没有初始化，它的值是不确定的。
2. 如果用指针指向了动态分配的内存，内存被释放后，指针不会置空，但是指向的地址已失效。【可理解为：在释放了指针后没有将其指向NULL】
3. 指针指向的变量已超越变量作用域（变量的内存空间已被系统回收），让指针指向了函数的局部变量，或者把函数的局部变量的地址作为返回值赋给了指针。

**危害**
  当一个指针称为了野指针，它的指向就是随机的，当你使用了随机地址的指针时，危害程度也是随机的，一般会造成内存泄露。

### 空指针和野指针的区别

没有存储任何的内存地址的指针就称为空指针（NULL指针）。

空指针就是被赋值为0的指针，在没有初始化之前，它的值为0。



## 悬空指针

空悬指针是指向已经被释放（如删除、回收）的内存的指针。

这种指针仍然具有以前分配的内存地址，但是这块内存可能已经被其他对象或数据占用。

访问空悬指针同样会导致未定义行为。



## 引用传递

注意函数中，function(int a);function(int *a);function(int &a);三者的区别 

```
#include <iostream>
#include<string>
using namespace std;

void Swap1(int a,int b)
{
	int temp=a;
	a=b;
	b=temp;
	cout<<"Swap1中 a:"<<a<<" b: "<<b<<endl;
}
void Swap2(int *a,int *b)
{
	int temp= *a;
	*a=*b;
	*b=temp;
 } 
 void Swap3(int &a,int &b)
 {
 	int temp=a;
 	a=b;
 	b=temp;
 }
 int main()
 {
 	int a=10;
 	int b=20;
 	Swap1(10,20);  //值传递，形参不会修饰实参 
 	
 	Swap2(&a,&b);	//地址传递，形参会修饰实参的 
 	
 	Swap3(a,b);		//引用传递，形参会修饰实参的
	 cout<<"Swap3中 a:"<<a<<" b: "<<b<<endl;
	 return 0; 
 }
```





## 额外补充

指针在数组中的表示：

```
 int nums[] = {1,2,3,4,5};
    int *p = nums;

    for(int i=0;i<5;i++)
    {
        cout<<"*p="<<*(p+i)<<endl;  //表示方法1
        cout<<"p[]="<<p[i]<<endl;  //表示方法2

    }
    cout<<(&nums[1])[1] <<endl;   //解释为 *(第二个元素的地址 + 1)  ，结果为4
```

2、

int *p;          整型指针

int *p[3] ;       一维整型指针数组，元素是3个整型指针（p[0]、p[1]、p[2]）

int* p()；       函数p的返回值类型是整型的地址。

int (*p)(int , int)            p是函数指针，函数的返回值是整型。


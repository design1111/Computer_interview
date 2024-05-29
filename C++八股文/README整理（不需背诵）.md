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

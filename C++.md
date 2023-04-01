#  变量

无符号数：unsigned

1字节：char，bool

2字节：short int

4字节：int，long，float（浮点）

8字节：long long ，double（浮点）

获取字节大小：sizeof（）

# 头文件

#：预编译

<>:编译器标准库

“ ”：可以引用所有文件

include：复制和粘贴文件放入cpp中

#pragma once:头文件监督，预防重新定义

```c++
#ifndef _log_h
#define _log_h

......

#endif
```



### log.h

```c++
#pragma once
void log(const char * message);
void Initlog();
```

### log.cpp

```c++
#include<iostream>
void log(const char* message)
{
	std::cout << message << std::endl;
}

void Initlog()
{
	log("Initializing log");
}
```

### main.cpp

```c++
#include<iostream>
#include"log.h"

int Multiply(int a, int b)
{
	log("Multiply");
	return a * b;
}

int main()
{
	log("Hello word!");
	Initlog();

	std::cout << Multiply(5,8) << std::endl;
}
```



# 代码调试   Debug

设置断点

step into：进入当前代码语句

step over：从当前函数跳到代码下一行

step out：跳出当前代码语句

内存视图：Debug-----windows-----Memory

# 控制流语句

### 条件与分支if()

```C++
int a = 8;
bool num = a == 9;
if (num)
{
	log("Hello word!");
	Initlog();
}
else if (num == NULL)
{
	log("NULL");
}
```

### 循环语句（for and while）

for(声明变量；条件；下一次迭代动作)      *********//循环结束后自动释放内存，即可在内部声明变量*********

{

循环体；

}

------

while（条件）  ***//循环结束无法自动释放内存，即无法在内部声明变量***

{

循环体；

}

------

do

{

循环体；

}while（条件）

### 跳转语句（continue,break,return)

 **break**：用于终止并退出循环，执行循环语句块之后的代码。

**continue**： 用于终止当前正在执行的循环，程序流回到循环条件判断，进行下一次循环。

**return**：退出函数



# 指针

保存地址的**变量**

```C++
char* buffer = new char[8];
memset(buffer, 0, 8);
char** ptr = &buffer;

delete[] buffer;
```

# 引用（指针常量）

伪装的指针,是给已定义的变量起别名,引用不改变指向

```c++
int a = 5;
int& ref = a;
ref = 2;
std::cout << a << std::endl;
```
![image-20230217003217331](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230217003217331.png)

**和类型在一起的是引用，和变量在一起的是取址**

int a=3；
int &b=a；    \*//引用\*
int \*p=&a;     \*//取地址\*

**不能改变引用的东西**（不能修改指向地址，只能修改地址内容）

```C++
int a = 5;
int b = 8;
int& ref = a;
ref = b;
//a = 8, b = 8,意为b赋值给a，

int* ref = &a;
*ref = 2;
ref = &b;
*ref = 3;


```

# 类

默认情况下，类中所有东西均为私有成员（private），仅类中的函数才能访问变量，在类外可访问的为公有成员（public）部分内容。

```c++
#include<iostream>
#define log(x) std::cout << x << std::endl

class Player
{
public:
	int x, y;
	int speed;

	void Move(int xa, int ya)
	{
		x += xa * speed;
		y += ya * speed;
	}
};

int main()
{
	Player Player;
	Player.x = 5;
	Player.y = 5;
	Player.speed = 2;
	Player.Move(1, -1);
	log(Player.x);
	log(Player.y);

	std::cin.get();
}
```

![image-20230217232946472](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230217232946472.png)

------

##### “class与struct的区别”

在C++中，class与struct的区别仅为代码的可见性，即私有成员部分，在未标注声明的情况下，class默认为私有成员，而struct默认为共有成员。struct在C++中存在的主要目的是向后兼容C，因为C中没有class的定义。

## 一个简单C++类

```c++
//LOG()
#include<iostream>

class Log
{
public:
	const int LogLevelError = 0;
	const int LogLevelwarning = 1;
	const int LogLevelInfo = 2;
private:
	int m_LogLevel = LogLevelInfo;
public:
	void SetLevel(int level)
	{
		m_LogLevel = level;
	}
	void Warn(const char* message)
	{
		if(m_LogLevel >= LogLevelwarning)
		std::cout << "[WARNING]:" << message << std::endl;
	}
	void Error(const char* message)
	{
		if (m_LogLevel >= LogLevelError)
		std::cout << "[ERROR]:" << message << std::endl;
	}
	void Info(const char* message)
	{
		if (m_LogLevel >= LogLevelInfo)
		std::cout << "[INFO]:" << message << std::endl;
	}

};

int main()
{
	Log Log;
	//Log.SetLevel(Log.LogLevelError);
	Log.Warn("Hello!");
	Log.Error("Hello!");
	Log.Info("Hello!");
	std::cin.get();
}
```

# protected成员的访问

在C++中，protected成员可以被该类的派生类访问，也可以被该类的成员函数访问。如果在派生类中访问protected成员，可以直接使用该成员的名称，而无需通过任何访问控制符。如果在该类的成员函数中访问protected成员，则必须使用this指针来访问。

例如，假设我们有一个基类`Base`和一个派生类`Derived`，其中`Base`中有一个protected成员变量`prot_member`和一个protected成员函数`prot_func()`。在派生类中，可以这样访问protected成员：

```C++
class Base {
protected:
    int prot_member;
    void prot_func();
};

class Derived : public Base {
public:
    void access_prot() {
        prot_member = 10;  // 直接访问protected成员
        prot_func();       // 直接调用protected成员函数
    }
};
```

在基类中的成员函数中访问protected成员时，必须使用`this`指针，如下所示：

```C++
class Base {
protected:
    int prot_member;
    void prot_func();
public:
    void access_prot() {
        this->prot_member = 10;  // 通过this指针访问protected成员
        this->prot_func();       // 通过this指针调用protected成员函数
    }
};

```



# static静态

在类或结构体的外部使用static修饰变量和函数，会让他们拥有不被外部翻译单元链接的能力，即非全局变量。少用全局变量，这可能造成代码混乱。

静态成员函数只能调用静态成员变量和静态成员函数。

static静态变量这里，因为所有实例都指向相同的内存地址，所以改一个实例就相当于改所有实例，这样多个实例其实相当于是同一个，故多个实例没有意义。

***

静态成员变量在编译时存储在静态存储区，即定义过程应该在编译时完成，因此一定要**在类中声明、类外进行定义**，但可以不初始化。

> **静态成员变量是所有实例共享的，但是其只是在类中进行了声明，并未定义或初始化（分配内存），类或者类实例就无法访问静态成员变量，这显然是不对的，所以必须先在类外部定义，也就是分配内存**

### 静态局部 local static ###

限定作用域中声明的静态变量，是该函数的局部变量。

```c++
#include<iostream>
void Function()
{
	static int i = 0;
	i++;
	std::cout << i << std::endl;
}
int main()
{
	Function();
	Function(); 
	Function();
	std::cin.get();
}
```

![image-20230218165241234](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230218165241234.png)

**等于**

```c++
#include<iostream>
int i = 0
void Function()
{
	i++;
	std::cout << i << std::endl;
}
int main()
{
	Function();
	Function(); 
	Function();
	std::cin.get();
}
```

**区别于**

```c++
#include<iostream>
void Function()
{
    int i = 0;
	i++;
	std::cout << i << std::endl;
}
int main()
{
	Function();
	Function(); 
	Function();
	std::cin.get();
}
```

![image-20230218165446332](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230218165446332.png)

#  枚举

一个特殊的命名方式 ，用来处理各种**整数**，枚举中的数据是一个不可修改的**常量**。其数值默认从0开始。

```c++
enum  名称 : 类型  
{   a,b,....     };
```

```c++
#include<iostream>
class Log
{
public:
	enum level 
	{
		LevelError = 0 , LevelWarning , LevelInfo
	};
private:
	level m_LogLevel = LevelInfo;
public:
	void SetLevel(level level)
	{
		 m_LogLevel = level;
	}
	void Warn(const char* message)
	{
		if(m_LogLevel >= LevelWarning)
		std::cout << "[WARNING]:" << message << std::endl;
	}
	void Error(const char* message)
	{
		if (m_LogLevel >= LevelError)
		std::cout << "[ERROR]:" << message << std::endl;
	}
	void Info(const char* message)
	{
		if (m_LogLevel >= LevelInfo)
		std::cout << "[INFO]:" << message << std::endl;
	}

};
int main()
{
	Log Log;
	Log.SetLevel(Log.LevelError);
	Log.Warn("Hello!");
	Log.Error("Hello!");
	Log.Info("Hello!");
	std::cin.get();
}
```

# 构造函数与析构函数

## 构造函数

 函数名与类名相同（无返回值类型），为私有数据成员赋值的函数，在创造一个新的实例对象运行时使用，不能单独调用。

格式1：类名  对象名（实参表）   （或  类名  对象名=构造函数名（实参表）
格式2：类名*指针变量名=new 类名【{实参表}】

![image-20230219000653160](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230219000653160.png)

注意：

- **构造函数必须与类同名**

- **构造函数无返回值；且前不能加void类型符**

  ------

  ```c++
  class Entity
  {
  public:
  	Entity()
  	{
  	
  	}
  }
  int main()
  {
      Entity A;
  }
  ```

## 析构函数

在销毁对象时运行，用于释放内存

名字与构造函数相同，但其前有“~”，**不能重载，只能有一个**

```c++
class Entity
{
public:
	Entity()
	{
		x = 0.0f;y = 0.0f;
        std::cout << "Created Entity!" << std::endl;
	}
    ~Entity()
    {
        std::cout << "Destroyed Entity!" << std::endl;
    }
    void Print()
    {
        std::cout << x << "," << y << std::endl;
    }

}
void Function()
{
    Entity e;
    e.Print();
}
int main()
{
    Function();
    std::cin.get();
}
```

![image-20230219004227815](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230219004227815.png)

# 继承

为避免不断重复，将相同部分放在一个类里面（父类），起公共基础模板作用，子类时父类的子集。

Class 派生类名：继承方式 基类名  //如何吸收

 {  

成员声明；  //如何调整、新增

}

```c++
class Entity
{
public:
	float x,y;
    void Move(float xa , float ya)
    {
        X += xa;
        Y += ya;
    }
};
class Player : public Entity
{
    float char* name;
    void Pointname()
    {
        std::cout << name << std::endl;
    }
    
}
```

# <a id="虚函数1"></a> 虚函数

**虚函数的作用是允许在派生类中重新定义与基类同名的函数，并且可以通过基类指针或引用来访问基类和派生类中的同名函数。解决继承里赋值兼容所导致的问题**，多态的设计方法可以保证在赋值兼容的前提下，基类、派生类分别以不同的方式来响应相同的消息。

在函数声明前加上`virtual`关键字即

```C++
virtual  返回值类型 函数名（参数表）
```

![image-20230221002358223](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230221002358223.png)

![image-20230221002420141](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230221002420141.png)

------

理解：子类覆写了父类的一个方法，但是我们在调用的时候，本来是想要调用子类的方法，但是程序却调用父类的了，因此我们对父类中的方法加上一个标记（virtual），此时虚表中的方法分为两类，一类是继承自父类的带有virtual标记的，居于表头部，接下来是子类自己的函数。从设计层面来说，**普通函数是子类可以调用，不能修改共能，而虚函数是子类既可以直接用，也可以修改起功能**

#  overload 、override、overwrite 之间的区别

> > **overload** 重载
> >
> > 在C++程序中，可以将语义、功能相似的几个函数用同一个名字表示，但参数不同（包括类型、顺序不同），即函数重载。
> > （1）相同的范围（在同一个类中）；
> > （2）函数名字相同；
> > （3）参数不同；
>
> 请注意，重载解析中不考虑返回类型，而且在不同的作用域里声明的函数也不算是重载。重载可以理解为一个类内部的函数重载
>
> >  override  覆盖
> >
> > 是指派生类函数覆盖基类函数，特征是：
> > （*1*）不同的范围（分别位于派生类与基类）；
> > （*2*）函数名字相同；
> > （*3*）参数相同；
> > （*4*）基类函数必须有*virtual* 关键字。
>
> > overwrite：重定义
> > 是指派生类的函数屏蔽了与其同名的基类函数，规则如下：
> > （1）如果派生类的函数与基类的函数同名，但是参数不同。此时，不论有无virtual关键字，基类的函数将被隐藏。
> > （2）如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual关键字。此时，基类的函数被隐藏（注意别与覆盖混淆)

# 纯虚函数（接口）

virtual  函数类型    函数名（参数列表） =  0 ；

```c++
class Base
{
public:
	virtual void point()        //虚函数
	{
		std::cout << "point" << std::endl;
	}
	virtual void show() = 0;  //纯虚函数
};
class son : public Base
{
public:
	void show()
	{}
};
```

- 纯虚函数只有函数的名字而不具备函数的功能，不能被调用。
- 纯虚函数的作用是在基类中为其派生类保留一个函数的名字，以便派生类根据需要对他进行定义。如果在基类中没有保留函数名字，则无法实现多态性。
- 如果在一个类中声明了纯虚函数，在其派生类中没有对其函数进行定义，则该虚函数在派生类中仍然为纯虚函数。

- 只有声明，没有实现/定义
- 含有纯虚函数的类称为抽象类，抽象类不能被实例化
- 抽象类的派生类如果想成为具体的类（能够被实例化），则必须重写纯虚函数。

# 数组

```C++
int main()
{
	int example [5];
	for (int i = 0; i < 5; i++)
	{
		example[i] = 2;
		std::cout << example[i] << std::endl;
	}
	
	int* another = new int[5];
	for (int i = 0; i < 5; i++)
	{
		example[i] = 2;
		std::cout << example[i] << std::endl;
	}
	
	delete[] another;
	std::cin.get();

}
```

> **如何获得数组大小**
>
> 声明数组时，必须时一个在编译时就确定的常量，因额系统不知道size是否会优先与数组被释放（作用域与声明周期），就需要static定义为静态或者定义为全局变量。如：
>
> ```C++
> static const int Exampleize = 5;
> int example [ExampleSize];
> ```
>
> 或者
>
> ```C++
> #include<array>
> int main()
> {
>     std::array<int,5> another;
>     another.size();
> }
> ```
>
> 

# 字符串

```c++
#include <string>
int main()
{
	const char* name = "bfids";
	std::string name2 = "biukjbf";  // 双引号中为const char*  指针数组
    naem += "hello"; 
		int x, y;
		std::cout << name << std::endl;
		std::cout << name2 << std::endl;
	std::cin.get();
}
```

------

# 字面量

具有**静态存储连续性**的字符数组，即字符串常量

==const char [N]-------const char* [N]==

const char name[8] = "che\0rno"

strlen（name）仅计算终止符（\0或0）之前的.

==char *p = "hello"; // p是一个指针，直接指向常量区，修改p【0】就是修改常量区的内容，这是不允许的。
char p【】 = "hello"; // 编译器在栈上创建一个字符串p，把"hello"从常量区复制到p，修改p【0】就相当于修改数组元素一样，是可以的。==

#  常量（const先作用与左边，再作用于右边）

==指针常量==：一个用指针修饰的常量.指针指向的地址不可以被修改，但是可以修改地址里存储的内容。**const作用与指针本身**

int* const p = &a;

*p = 9;

==常量指针==：在定义指针变量的时候，数据类型前用const修饰，被定义的指针变量就是指向常量的指针变量，可以修改指针指向的地址，不可通过该指针修改指针指向的变量的值。**const作用于指针指向的地址**

const int* p = &a;

p = &b

**指向常量的指针常量 **     const int * const b = &a       //返回一个不能被修改的指针，该指针地址的内容也不可以被修改。

------

==作用于类==:`成员函数   const；`类的对象只能调用该成员函数，不可以修改成员函数中的成员变量，该成员函数称为长成员函数。

```c++
class Entity{

private:
	int  m_x, m_y;
public:
    int GetX() const
    {
        m_x = X;
    }        
}
```

# mutable

1，修饰class const方法中class成员变量，使其可以修改。
2，修饰lambda表达式，值捕获时可以直接操作传入参数。(并非引用捕获，依旧值捕获，不修改原值)

```c++
class Entity
{
    private:
    	std::string m_mane;
    	mutable int m_DebugCount = 0;
    public:
    const std::string& Getname() const
    {
        m_DebugCount++;
        return m_name;
    }
}
int main()
{
    const Entity e;
    e.Getname();
    std::cin.get();
}
```

------

> **lambde**       [ capture ] ( params ) opt -> ret { body; };
>
> ​					其中 capture 是捕获列表，params 是参数表，opt 是函数选项，ret 是返回值类型，body是函数体。
>
> ```
> auto f = [](int a) -> int { **return** a + 1; };
> std::cout << f(1) << std::endl;  // 输出: 2
> ```
>
> [] 不捕获任何变量。
>
> [&] 捕获外部作用域中所有变量，并作为引用在函数体中使用（按引用捕获）。
>
> [=] 捕获外部作用域中所有变量，并作为副本在函数体中使用（按值捕获）。
>
> [=，&foo] 按值捕获外部作用域中所有变量，并按引用捕获 foo 变量。
>
> [bar] 按值捕获 bar 变量，同时不捕获其他变量。
>
> [this] 捕获当前类中的 this 指针，让 lambda 表达式拥有和当前类成员函数同样的访问权限。如果已经使用了 & 或者 =，就默认添加此选项。捕获 this 的目的是可以在 lamda 中使用当前类的成员函数和成员变量。
>
> ​		**auto** x1 = []{ **return** i_; };                    // error，没有捕获外部变量_
>
> ​		**auto** x2 = [=]{ **return** i_ + x + y; };           // OK，捕获所有外部变量
>
> ​        **auto** x3 = [&]{ **return** i_ + x + y; };           // OK，捕获所有外部变量
>
> ​        **auto** x4 = [**this**]{ **return** i_; };                // OK，捕获this指针
>
> ​        **auto** x5 = [**this**]{ **return** i_ + x + y; };        // error，没有捕获x、y
>
> ​        **auto** x6 = [**this**, x, y]{ **return** i_ + x + y; };  // OK，捕获this指针、x、y
>
> ​        **auto** x7 = [**this**]{ **return** i_++; };              // OK，捕获this指针，并修改成员的值

# 成员初始化列表

成员变量初始化的顺序为：先进行声明时初始化，然后进行初始化列表初始化，最后进行构造函数初始化。**初始化列表初始化的变量值会覆盖掉声明时初始化的值，而构造函数中初始化的值又会覆盖掉初始化列表的。**

构造函数的执行可以分成两个阶段，初始化阶段和计算阶段，初始化阶段先于计算阶段。

初始化阶段：所有类类型（class type）的成员都会在初始化阶段初始化，即使该成员没有出现在构造函数的初始化列表中。

计算阶段：一般用于执行构造函数体内的赋值操作

**使用初始化列表少了一次调用默认构造函数的过程****

------

类的初始化表：

在构造函数小括号与大括号之间，添加一个符号 `:`，然后在其后依次写出要初始化的成员变量名称，其名称后面有一个`()`，该小括号中的值就是将要赋值给成员变量的值。

```c++
class Test {
	int x;
	int y;
	Test(int _x) 
		:x(_x),y(0)
	{
	}
};
```

其等价于

```
Test (int _X)
{
	x = _x;
	y = 0;
}
```

**成员变量的初始化顺序与初始化列表中列出的变量的顺序无关，它只与成员变量在类中声明的顺序有关**

==（建议按照成员定义的顺序进行初始化）==

==必须放在初始化成员列表中的：==

- 常量成员，因为常量只能初始化不能赋值，所以必须放在初始化列表里面
- 引用类型，引用必须在定义的时候初始化，并且不能重新赋值，所以也要写在初始化列表里面
- 没有默认构造函数的类类型，因为使用初始化列表可以不必调用默认构造函数来初始化，而是直接调用拷贝构造函数初始化。

没有默认构造函数的类

```C++
struct Test1
{
    Test1(int a):i(a){}
    int i ;
};

struct Test2
{
    Test1 test1 ;
    Test2(Test1 &t1)
    {
        test1 = t1 ;
    }
};
```

以上代码无法通过编译，因为Test2的构造函数中test1 = t1这一行实际上分成两步执行。

1. 调用Test1的默认构造函数来初始化test1

2. 调用Test1的赋值运算符给test1赋值

但是由于Test1没有默认的构造函数，所谓第一步无法执行，故而编译错误。、

```C++
struct Test2
{
    Test1 test1 ;
    Test2(Test1 &t1):test1(t1){}
}
```

# 三元操作符

```c++
static int s_level = 1;
static int s_speed = 2;

int main()
{
    if(s_level > 5)
      s_speed = 10;
    else
        s_speed = 5;
等价于：
   s_speed = s_level > 5 ? 10 : 5; 
或者：
   s_speed = s_level > 5 ? s_level > 10 ? 15 : 10 : 5;//正确，但是最好不要嵌套
   std::string rank = s_level > 5 ? "master" : "beginner";//不用构造并销毁临时字符串
}
```

# C++对象创建

```
Entity e; //在栈上分配
Entity* e = new Entity();    //当对象太多或想控制对象生存期，在堆上分配
```

# new

在堆上分配内存（找到一个足够大的内存块，返回指向该内存块的指针）

```
int* b = new int[50];
Entity* e = new Entity[50];//调用了构造函数
Entity* e = (Entity*)malloc(sizeof(Entity));//C 没有调用构造函数，仅分配内存
delete[] e;
delete[] b;
free(e);
```

# 隐式转换

![image-20230228105706268](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230228105706268.png)

隐式转换只有一次

**explicit:禁用隐式转换**

# ==运算符及其重载==

在C++中，标准库本身已经对左移运算符`<<`和右移运算符`>>`分别进行了重载，使其能够用于不同数据的输入输出，但是输入输出的对象只能是 C++ 内置的数据类型（例如 bool、int、double 等）和标准库所包含的类类型（例如 string、complex、ofstream、ifstream 等）。

`cout`是`iostream`中定义的`ostream`类的对象，`<<`能用在`cout`上是因为，在`iostream`中对`<<`进行了重载。

```
cout << 5`相当于执行函数`cout.operator<<(5)
std::cout << 1 <<"hello";语句，等价于( cout.operator<<(1) ).operator<<("hello");
```

如果我们自己定义了一种新的数据类型，需要用输入输出运算符去处理，那么就必须对它们进行重载。cout 是 ostream 类的对象，cin 是 istream 类的对象，要想达到这个目标，**就必须以全局函数（友元函数）的形式重载 “<<” 和 “>>”**（由于`ostream`类型已经在`iostream`中实现，所以不能作为`ostream`类的成员函数重载），否则就要修改标准库中的类，这显然不是我们所期望的。**若重载市用到了类中的私有成员变量，则需要在类中声明该函数为==友元函数==。**

**函数名：**operator 接【需要重载的运算符符号】

**函数原型：**【返回值类型】【operator】【操作符】【参数列表】

运算符重载时，第一个参数是左操作数，第二个操作数是右操作数（注意，第一个参数是隐藏的`this`指针）

```c++
std::ostream& operator<<(std::ostream& stream, const String& string)//操作符重载
{
	stream << string.m_Buffer;
	return stream; 
}
```

[(5条消息) 【C++】类与对象（三） 运算符重载 赋值重载 取地址及const取地址操作符重载_看到我请叫我滚去学习Orz的博客-CSDN博客](https://blog.csdn.net/qq_65207641/article/details/129001702)

> ==**不支持重载的运算符**==
>
> 1. .  (成员访问运算符)
> 2. .*  (成员指针访问运算符)
> 3. ::  (域运算符)
> 4. sizeof(长度运算符)
> 5. ?:  (条件运算符）

# this关键字

指向当前对象实例的指针，**属于**该对象实例，必须由一个有效的对象来调用。

```c++
class Entity;
void PrintEntity(Entity* e);
class Entity
{
public:
    int x,y;
    Entity(int x,int y)
    {
        this-> x = x; // Entity* e = this;实际是Entity* const e = this const     
        				//e->x = x;  or   (*this).x = x;
        this-> y = y;
        PrintEntity(this);
    }
    PrintEntity(this);
};
void PrintEntity(Entity* e)
{
   // 
}
```

# 栈作用域生存期

在栈上分配内存，超出栈作用域，内存会被释放，栈上的变量会被自动销毁，可以在堆上分配内存（new），或是将数据复制给一个在栈作用域之外的变量。

# 智能指针

用来实现不调用delete，自动化释放内存的功能

### ==unique_ptr==：

作用域指针，超出作用域时，会被销毁。（不能复制该指针）

![image-20230301104728145](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230301104728145.png)

==**#include<memory>**==

```C++
int main()
{
    {
        std::unique_ptr<Entity> entity = std::make_unique<Entity>();
        //std::unique_ptr<Entity> entity(new Entity());   可能有异常安全
        //因为unique_ptr实际上是explitic，故不可用隐式转换，即不能 std::unique_ptr<Entity> entity = new Entity
        entity->print();
    };
}
```

### ==shared_ptr==：

共享指针，通过引用计数，跟踪指针有多少个引用，当引用计数到达0时，删除指针。（解决作用域指针不能复制的问题）

> 需要分配额外的内存（控制块）用来存储引用计数，即两次内存分配

```C++
std::shared_ptr<Entity> Sharedentity = std::make_shared<Entity>();
```

### ==weak_pty==：

weak_ptr<T>（ T 为指针所指数据的类型）,只能和 shared_ptr 类型指针搭配使用。当 weak_ptr 类型指针的指向和某一 shared_ptr 指针相同时，weak_ptr 指针并不会使所指堆内存的引用计数加 1；同样，当 weak_ptr 指针被释放时，之前所指堆内存的引用计数也不会因此而减 1。也就是说，**weak_ptr 类型指针并不会影响所指堆内存空间的引用计数**。weak_ptr<T> 模板类中没有重载 * 和 -> 运算符，这也就意味着，weak_ptr 类型指针**只能访问**所指的堆内存，而无法修改它。

#### weak_ptr指针的创建

创建一个 weak_ptr 指针，有以下 3 种方式：
1) 可以创建一个空 weak_ptr 指针，例如：

```
std::weak_ptr<int> wp1;
```

2) 凭借已有的 weak_ptr 指针，可以创建一个新的 weak_ptr 指针，例如：

```
std::weak_ptr<int> wp2 (wp1);
```

若 wp1 为空指针，则 wp2 也为空指针；反之，如果 wp1 指向某一 shared_ptr 指针拥有的堆内存，则 wp2 也指向该块存储空间（可以访问，但无所有权）。

3) weak_ptr 指针更常用于指向某一 shared_ptr 指针拥有的堆内存，因为在构建 weak_ptr 指针对象时，可以利用已有的 shared_ptr 指针为其初始化。例如：

```
std::shared_ptr<int> sp (new int);std::weak_ptr<int> wp3 (sp);
```

由此，wp3 指针和 sp 指针有相同的指针。再次强调，weak_ptr 类型指针不会导致堆内存空间的引用计数增加或减少。

# 复制与拷贝构造函数

浅拷贝：直接进行复制，若含有指针类型的数据时，拷贝前后的两个指针会指向同一块内存缓冲区，造成崩溃。

解决方法：拷贝构造函数

拷贝构造函数：作用是在建立一个新对象时，使用一个已存在的对象去初始化这个新对象。函数名与类名相同，并且该函数也没有返回值。赋值时采用const 引用赋值

```c++
class String
{
private:
	char* m_Buffer;
	unsigned int m_size;
public:
	String(const char* string)
	{
		m_size = strlen(string);
		m_Buffer = new char[m_size + 1];
		memcpy(m_Buffer, string, m_size + 1);//(目的，来源，大小）
		//memcpy(m_Buffer, string, m_size);
		//m_Buffer[m_size+1];手动添加空终止符
	}

	String(const String& other)
		:m_Buffer(other.m_Buffer),m_size(other.m_size)
	{
		std::cout << "copid string" << std::endl;
		m_Buffer = new char[m_size + 1];
		memcpy(m_Buffer, other.m_Buffer, m_size + 1);
	}

	~String()
	{
		delete[] m_Buffer;
	}
	/*void GetBuffer()
	{
		std::cout << m_Buffer << std::endl;
	}*/
	friend std::ostream& operator<<(std::ostream& stream, const String& string);
};

std::ostream& operator<<(std::ostream& stream, const String& string)//操作符重载
{
	stream << string.m_Buffer;
	return stream; 
}

void PrintString(const String& string)
{
	std::cout << string << std::endl;
}
int main()
{
	String string = "cherno";
	//string.GetBuffer();
	String second = string;
	PrintString(string);
	PrintString(second);
	 
	/*std::cout << string << std::endl;
	std::cout << second << std::endl;*/    
    //浅拷贝，直接复制，内存中的两个string对象会有两个相同的char*指针指向同一块内存程序，同时程序结束是会调用两次析构函数，试图释放同一块内存，造成程序崩溃
	std::cin.get();

}
```

## const_cast 操作符

`const_cast`操作符是专门用于去除`const`修饰符的操作符。它可以将`const`类型转换为非`const`类型，也可以将`volatile`类型转换为非`volatile`类型。其语法如下：

```C++
const_cast<type>(expression)
```

其中，`type`表示需要转换的类型，`expression`表示需要进行转换的表达式。

例如，假设有一个`const`整型变量`x`，我们需要将其转换为非`const`类型，可以使用如下的方式：

```C++
const int x = 10;
int& y = const_cast<int&>(x);
y = 20;
```

在这个例子中，我们使用了`const_cast`操作符将`const int`类型转换为`int&`类型，然后将其赋值给一个非`const`的整型引用`y`。通过修改`y`的值，也就相当于修改了原本的`x`变量。需要注意的是，`const_cast`操作符只能用于去除`const`修饰符和`volatile`修饰符，不能用于去除`const volatile`修饰符。此外，需要确保转换后的值确实是可以被修改的，否则可能会导致未定义行为。

# 箭头操作符

```c++
#include<string>
class Entity
{
public:
	void Print() const
	{
		std::cout << "hello!" << std::endl;
	}
};
int main()
{
	Entity e;
	e.Print();
	Entity* prt = &e;
	prt->Print();
	//Entity& entity = *prt; entity.Print(); 或(*prt).Print();因为此时prt是一个指针，是一个数值，不是一个实例，因此不能用“.”来调用函数，必须对其进行逆引用，箭头则执行自动逆引用的功能。
}
```

# 动态数组

```c++
#include<vector>
std::vector < 元素类型 >
```

传递时要用引用传递

```
void Function(const std::vector<Vertex>& vertices)
{

}
```

```c++
#include<vector>
struct Vertex
{
	float x, y, z;
	Vertex(float x, float y, float z)
		:x(x),y(y),z(z)
	{
	}

	Vertex(const Vertex& vertex)
		:x(vertex.x),y(vertex.y),z(vertex.z)
	{
		std::cout << "copid!" << std::endl;
	}

};
std::ostream& operator<<(std::ostream& stream, const Vertex& vertex)
{
	stream << vertex.x << "," << vertex.y << "," << vertex.z;
	return stream;
}

int main()
{
	std::vector <Vertex> vertices;
	vertices.reserve(3);   //vertices.reserve(int x)  x是vertices的大小,reserve提前申请内存，避免动态申请开销
	vertices.push_back(Vertex(1, 2, 3));
	vertices.push_back(Vertex(4, 5, 6));
	vertices.emplace_back(7, 8, 9); //emplace_back直接在容器尾部创建元素，省略拷贝或移动过程
	for (int i = 0; i < vertices.size(); i++)
		std::cout << vertices[i] << std::endl; 
	std::cin.get();
}
```

> 我（打印一个copy），来个亲戚我要新找个房子（亲戚+打印一个copy），又来一个亲戚再找（2亲戚+打印一个copy），又来个亲戚再找（3亲戚+打印一个copy），又来个亲戚（4亲戚+打印一个copy），又来个亲戚可是这次房子住得下了不用新找（打印一个copy）。再来亲戚的话太挤了我又找新房子并且我后面继续来亲戚所以找的房子大一点（6+copy），来亲戚（打印一个copy），来亲戚（打印一个copy）。再来就需要换房子了，换完我新房子可以多住3个人。打印出来就是（9亲戚+打印一个copy），然后打印一个copy，打印一个copy，打印一个copy。(手动狗头)

# 使用库（静态链接)

静态链接：使用的库会被放到exe文件中，或其他操作系统的可执行文件中；

动态链接：库在运行时被链接

------

在项目目录下创建一个库文件目录（dependencies），将下载好的库文件放入其中

> dll：动态链接库
>
> lib：静态链接库
>
> 解决方案内部的库用`" "`，解决方案外部的库用`<>`

文件目录改为￥（SolutionDir），从解决方案开始得到相对路径

hellow word里选择属性——Linker——input——附加依赖项——添加库名称——general——附加库目录——库文件的根目录

------

***C++创建一个动态链接库，编译后会生成两个可用的文件一个是lib文件一个是dll文件，那么这个lib文件是干嘛的呢？**
**在使用动态库的时候，往往提供两个文件：一个引入库和一个DLL。引入库包含被DLL导出的函数和变量的符号名，DLL包含实际的函数和数据。在编译链接可执行文件时，只需要链接引入库，DLL中的函数代码和数据并不复制到可执行文件中，在运行的时候，再去加载DLL，访问DLL中导出的函数。**
**1． Load-time Dynamic Linking 载入时动态链接**
**这种用法的前提是在编译之前已经明确知道要调用DLL中的哪几个函数，编译时在目标文件中只保留必要的链接信息，而不含DLL函数的代码；当程序执行时，利用链接信息加载DLL函数代码并在内存中将其链接入调用程序的执行空间中，其主要目的是便于代码共享。**
**2． Run-time Dynamic Linking 运行时动态链接**
**这种方式是指在编译之前并不知道将会调用哪些DLL函数，完全是在运行过程中根据需要决定应调用哪个函数，并用LoadLibrary和GetProcAddress动态获得DLL函数的入口地址。**

> 如果只是简单的将项目生成为dll，则不会输出lib文件，也就无法在其他项目中静态链接项目dll。
> 解决的办法是在dll项目中，在声明和定义的函数前均加上__declspec(dllexport)，即可同时生成dll与lib。

# 如何处理多返回值

```C++
1、引用，没有动态分配
2、指针，能够nullptr
3、数组，return new std::string【】 {vs,vv},不知道大小
return array <std::string,2>(vs,vv)（栈） 快
vector(堆) 慢
4、tuple （元组）//元组形式与链表类似，但是一旦被创建，则内容无法改变，是不可变序列，通常用于保存无需修改的内容
    //(element1, element2, ... , elementn)
#include <tuple>//tuple
#include <utility> (make_tuple)//pair
static std::tuple或者pair<返回类型1，返回类型2 > func( , )//pair是将2个数据组合成一组数据，当一个函数需要返回2个数据的时候，可以选择pair。  pair的实现是一个结构体，主要的两个成员变量是first second 因为是使用struct不是class，所以可以直接使用pair的成员变量。
return make_pair(返回值1，返回值2)
从tuple 中获得值
source=返回的元组(tuple or like pair)
std::get<0/1/2/3>(tuple名source);
pair 的类型，可以用 source.first
```

------

其标准库类型--pair类型定义在#include <utility>头文件中，定义如下：

类模板：template<class T1,class T2> struct pair

参数：T1是第一个值的数据类型，T2是第二个值的数据类型。

功能：pair将一对值(T1和T2)组合成一个值，这一对值可以具有不同的数据类型（T1和T2），两个值可以分别用pair的两个公有函数first和second访问。

其构造函数定义如下:

```C++
pair<T1, T2> p1;            //创建一个空的pair对象（使用默认构造），它的两个元素分别是T1和T2类型，采用值初始化。
pair<T1, T2> p1(v1, v2);    //创建一个pair对象，它的两个元素分别是T1和T2类型，其中first成员初始化为v1，second成员初始化为v2。
make_pair(v1, v2);          // 以v1和v2的值创建一个新的pair对象，其元素类型分别是v1和v2的类型。
p1 < p2;                    // 两个pair对象间的小于运算，其定义遵循字典次序：如 p1.first < p2.first 或者 !(p2.first < p1.first) && (p1.second < p2.second) 则返回true。
p1 == p2；                  // 如果两个对象的first和second依次相等，则这两个对象相等；该运算使用元素的==操作符。
p1.first;                   // 返回对象p1中名为first的公有数据成员
p1.second;                 // 返回对象p1中名为second的公有数据成员
```

```C++
pair<int ,double> p1;
p1.first = 1;
p1.second = 2.5;
cout<<p1.first<<' '<<p1.second<<endl;
//输出结果：1 2.5
string firstBook;
if(author.first=="James" && author.second=="Joy")
    firstBook="Stephen Hero";
```

# 元组

元组（tuple）是计算机科学中的一个概念，它是一个不可变（无法修改）的序列，由多个元素按照顺序组成。元素可以是不同的数据类型。元组通常用于将相关的数据组合在一起，可以作为字典中的键、集合中的元素以及函数的返回值等。C++11 引入了 `std::tuple` 类型，它是一个**不可变的、类型安全的、固定大小的**元组。

1、`std::tuple` 作为多个值的返回类型：函数可以返回多个值，使用 `std::tuple` 可以方便地返回多个值。

```C++
#include <tuple>
#include <iostream>
using namespace std;

tuple<string, int> get_name_and_age() {
    string name = "Alice";
    int age = 30;
    return make_tuple(name, age);
}

int main() {
    auto person = get_name_and_age();
    cout << get<0>(person) << endl; // 输出：Alice
    cout << get<1>(person) << endl; // 输出：30
    return 0;
}

```

2、元组作为字典的键：可以使用 `std::map<std::tuple<...>, ...>` 定义一个元组作为键的字典

```c++
#include <tuple>
#include <map>
#include <iostream>
using namespace std;

int main() {
    map<tuple<string, int>, string> people;
    people[make_tuple("Alice", 30)] = "Alice's info";
    people[make_tuple("Bob", 25)] = "Bob's info";
    cout << people[make_tuple("Alice", 30)] << endl;  // 输出：Alice's info
    return 0;
}

```

3、元组用于函数参数传递：函数可以将元组作为参数传递，这样可以方便地传递多个参数

```C++
#include <tuple>
#include <iostream>
using namespace std;

void print_person_info(tuple<string, int> person) {
    cout << "Name: " << get<0>(person) << endl;
    cout << "Age: " << get<1>(person) << endl;
}

int main() {
    tuple<string, int> person = make_tuple("Alice", 30);
    print_person_info(person);  // 输出：Name: Alice, Age: 30
    return 0;
}

```

4、元组用于同时更新多个变量

```C++
#include <tuple>
#include <iostream>
using namespace std;

int main() {
    int a = 1;
    int b = 2;
    tie(a, b) = make_tuple(b, a);
    cout << a << endl;  // 输出：2
    cout << b << endl;  // 输出：1
    return 0;
}
```



# 模板

有两个或多个函数或类，功能是相同的，仅仅是数据类型不同，

```
template<typename T>

函数声明或类声明
```

```C++
template<typename T> 
void Print (T value)
{
	std::cout << value << std::endl;
}

int main()
{
	Print(5);
	Print("hello");
	Print(5.6f);
}
```

# auto

简化变量初始化，根据后面的值，自动推测前面的类型。

- auto 变量必须在定义时初始化，这类似于const关键字。
- 定义在一个auto序列的变量必须始终推导成同一类型。

```C++
auto a4 = 10, a5 = 20, a6 = 30;//正确

auto b4 = 10, b5 = 20.0, b6 = 'a';//错误,没有推导为同一
```

- 类型因为auto是一个占位符，并不是一个他自己的类型，因此不能用于类型转换或其他一些操作，如sizeof和typeid

# 静态数组

#include<array>

array<数据类型，元素个数>

| at    | 访问指定的元素，同时进行越界检查   |
| ----- | ---------------------------------- |
| []    | 访问指定的元素                     |
| front | 访问第一个元素                     |
| back  | 访问最后一个元素                   |
| data  | 返回指向内存中数组第一个元素的指针 |

```C++
template<typename T>
void PrintArray(const T& data)
{
	for ( int i = 0; i < data.size(); i++)
	{
		std::cout << data[i] << std::endl;
	}
}

int main()
{
	int a = 5;
	auto b = a;
	std::array<int, 5> data = { 0,1,2,3,4 };
	std::cout << data[0] << std::endl;
	PrintArray(data);

	std::cin.get();
 }
```

# 函数指针

指向函数入口地址的指针，其一般有两个作用，调用函数和做函数的参数。

==函数名即函数地址的首地址==

```C++
typedef void (*pfun)(int data);
/*typedef的功能是定义新的类型。第一句就是定义了一种 pfun 的类型，并定义这种类型为指向某种函数的指针
```

```c++
void Helloword()
{
	std::cout << "hello word!" << std::endl;
}
int main()
{
	typedef void(*Hellowordfunction)();
	Hellowordfunction function = Helloword;
	Helloword();
	auto function = Helloword;//函数没有括号相当于 &helloword 有一次隐式转换，void(*function)();
	std::cin.get();

}
```

```C++
void Print(int value)
{
	std::cout << "Values:" << value << std::endl;
}
void Foreach(const std::vector<int>& values,  void(*func)(int))
{
	for (int value : values)
		func(value);
}
int main()
{
	std::vector<int> values = { 1,2,3,4,5 };
	Foreach(values, Print);
	std::cin.get();
}
```

# lambda

一种定义匿名函数的方式，视为一个变量。

![img](https://img-blog.csdnimg.cn/img_convert/f86a9e30f7474aff10be27e4b51c6f64.png#pic_center)

- 捕获列表。在C ++规范中也称为Lambda导入器， 捕获列表总是出现在Lambda函数的开始处。实际上，【】是Lambda引出符。编译器根据该引出符判断接下来的代码是否是Lambda函数，捕获列表能够捕捉上下文中的变量以供Lambda函数使用。

  > **描述了上下文中哪些数据可以被Lambda使用，以及使用方式（以值传递的方式或引用传递的方式）。**

- 参数列表。与普通函数的参数列表一致。如果不需要参数传递，则可以连同括号“()”一起省略。

- 可变规格。mutable修饰符， 默认情况下Lambda函数总是一个const函数，mutable可以取消其常量性。在使用该修饰符时，参数列表不可省略（即使参数为空）。

- 异常说明。用于Lamdba表达式内部函数抛出异常。

- 返回类型。 追踪返回类型形式声明函数的返回类型。我们可以在不需要返回值的时候也可以连同符号”->”一起省略。此外，在返回类型明确的情况下，也可以省略该部分，让编译器对返回类型进行推导。

- lambda函数体。内容与普通函数一样，不过除了可以使用参数之外，还可以使用所有捕获的变量。



```C++
Foreach(values, [](int value) {std::cout << "value!" << value << std::endl; });
//auto lambda = [](int value) {std::cout << "value!" << value << std::endl; };
```

# 多线程

```C++
#include<thread>
std::thread worker(Dowork);//worker是线程对象
worker.join();
```

```C++
#include<thread>
static bool s_Finished = false;

void Dowork()
{
	using namespace std::literals::chrono_literals;
	std::cout << "start id = " << std::this_thread::get_id() << std::endl;
	while (!s_Finished)
	{
		std::cout << "working...." << std::endl;
		std::this_thread::sleep_for(1s);
	}
}

int main()
{
	std::thread worker(Dowork);
	std::cin.get();
	s_Finished = true;
	worker.join();
	std::cout << "Finished" << std::endl;
	std::cout << "start id = " << std::this_thread::get_id() << std::endl;

	std::cin.get();

}
```

# 计时

chorno计时库

```C++
#include<chrono>
#include<thread>

int main()
{
	using namespace std::literals::chrono_literals;
	auto start = std::chrono::high_resolution_clock::now();
	std::this_thread::sleep_for(1s);
	auto end = std::chrono::high_resolution_clock::now();
	std::chrono::duration<float> duration = end - start;
	std::cout << duration.count() << "s" << std::endl;


	std::cin.get();

}
```

计时类

```C++
#include<chrono>
#include<thread>

struct Timer
{
	std::chrono::time_point<std::chrono::steady_clock> start, end;
	std::chrono::duration<float> duration;

	Timer()
	{
		start = std::chrono::high_resolution_clock::now();
	}
	~Timer()
	{
		end = std::chrono::high_resolution_clock::now();
		duration = end - start;
		float ms = duration.count() * 1000.0f;
		std::cout << "Timer took" << ms << "ms" << std::endl;
	}
};
void Function()
{
	Timer timer;
	for (int i = 0; i < 100; i++)
		std::cout << "hello\n";
}
int main()
{
	Function();
	std::cin.get();
}

```

# 多维数组

```c++
int main()
{
	int** a2d = new int* [50];//数组，保存实际数组指针的数组	
	for (int i = 0; i < 50; i++)
		a2d[i] = new int[50];//遍历每个地址都创立一个容量50的数组
	for (int i = 0; i < 50; i++)
		delete[] a2d[i];
	delete[] a2d;
}


```

# 排序

```C++
 #include<vector>
#include<algorithm>
int main()
{
	std::vector<int> value = { 3,6,2,1,9 };
	std::sort(value.begin(), value.end(), [](int a, int b) 
		{
			if (a == 1)
				return false;
			if (b == 1)
				return true;
			return a < b;
		});
	for (int values : value)
		std::cout << values << std::endl;
	std::cin.get();
}
```

# 联合体

在C++中，联合体（Union）是一种特殊的数据类型，它允许在同一块内存区域中存储不同类型的数据。这些数据共享同一块内存，只能同时存储一个数据成员。

联合体和结构体类似，都可以包含多个成员变量。但不同之处在于，联合体的所有成员共用同一个内存空间，因此联合体的大小等于其最大成员变量的大小。

由于联合体的所有成员变量共享同一块内存，所以修改其中一个成员变量的值，可能会影响到其他成员变量的值。因此在使用联合体时需要特别小心，确保只操作当前需要的成员变量。

```C++
struct Vector2
{
	float x, y;
};
struct Vector4
{
	union 
	{
		struct 
		{
			float x,y,z,w;
		};
		struct 
		{
			Vector2 a, b;
		};
	};
};

void Printvector2(const Vector2& vector)
{
	std::cout << vector.x << "," << vector.y << std::endl;
}

int main()
{
	Vector4 vector = { 1.0f,2.0f,3.0f,4.0f };
	Printvector2(vector.a);
	vector.z = 500.0f;
	Printvector2(vector.b);

	std::cin.get();
}
```

![image-20230318000911827](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230318000911827.png)

使用联合体的一个典型应用场景是在不同的数据类型之间进行类型转换。通过将不同类型的数据存储在同一块内存区域中，我们可以轻松地将一个数据类型转换为另一个数据类型。但是，需要注意的是，在进行类型转换时需要确保数据的存储方式和字节顺序等是一致的，否则可能会导致不可预测的结果。

------

### 用两个栈实现一个队列

假设有两个栈 stack1 和 stack2，stack1 用于入队操作，stack2 用于出队操作。在入队时，元素直接压入 stack1 中；在出队时，首先判断 stack2 是否为空，如果不为空，直接从 stack2 的栈顶弹出元素；如果为空，需要将 stack1 中的所有元素出栈并压入 stack2，再从 stack2 的栈顶弹出元素。这样做的原因是因为栈的特性是先进后出，而队列的特性是先进先出，将 stack1 中的元素全部倒入 stack2 中之后，元素的顺序就变为了先进先出。

```C++
#include <iostream>
#include <stack>

using namespace std;

class Queue {
private:
    stack<int> stack1;
    stack<int> stack2;
public:
    void enqueue(int x) {
        stack1.push(x);
    }
    int dequeue() {
        if (stack2.empty()) {
            while (!stack1.empty()) {
                stack2.push(stack1.top());
                stack1.pop();
            }
        }
        int x = stack2.top();
        stack2.pop();
        return x;
    }
};

int main() {
    Queue q;
    q.enqueue(1);
    q.enqueue(2);
    q.enqueue(3);
    cout << q.dequeue() << endl;
    cout << q.dequeue() << endl;
    cout << q.dequeue() << endl;
    return 0;
}

```

# 虚析构函数

```C++
class Base
{
public:
	Base() { std::cout << "Base constructor!\n"; };
	~Base() { std::cout << "Base destructor!\n"; };
};

class Derived : public Base
{
public:
	Derived() { std::cout << "Derived constructor!\n"; };
	~Derived() { std::cout << "Derived destructor!\n"; };
};

int main()
{
	Base* base = new Base();
	delete base;
	std::cout << "-----------\n";
	Derived* derived = new Derived();
	delete derived;
	std::cout << "-----------\n";
	Base* poly = new Derived();
	delete poly;
	std::cin.get();
}
```

![image-20230318162054247](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230318162054247.png)

此处未将基类中的析构函数标记为<a href="#虚函数1">虚函数</a>，则在删除一个派生类对象时，只会调用基类的析构函数而不会调用派生类的析构函数，从而导致派生类中的动态分配内存没有被释放，造成内存泄漏。

> 基类中只要定义了虚析构（且只能在基类中定义虚析构，子类析构才是虚析构，如果在二级子类中定义虚析构，编译器不认，且virtual失效），在编译器角度来讲，那么由此基类派生出的所有子类地析构均为对基类的虚析构的重写，当多态发生时，用父类引用，引用子类实例时，此时的虚指针保存的子类虚表的地址，该函数指针数组中的第一元素永远留给虚析构函数指针。所以当delete 父类引用时，即第一个调用子类虚表中的子类重写的虚析构函数此为第一阶段。然后进入第二阶段：（二阶段纯为内存释放而触发的逐级析构与虚析构就没有半毛钱关系了）而当子类发生析构时，子类内存开始释放，因内存包涵关系，触发父类析构执行，层层向上递进，至到子类所包涵的所有内存释放完成。

将上面代码中的基类作以修改

```C++
class Base
{
public:
	Base() { std::cout << "Base constructor!\n"; };
	virtual ~Base() { std::cout << "Base destructor!\n"; };
};
```

![image-20230318170305301](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230318170305301.png)

内存释放正常

# 类型转换

**static_cast:**  static_cast用于静态类型转换，即转换在**编译时**就可以确定。它可以将一个指针或引用转换为另一种类型，或者将一个算术类型转换为另一种算术类型。

**const_cast:** const_cast用于移除变量的const属性。它可以将一个指向const类型的指针或引用转换为指向非const类型的指针或引用。

**dynamic_cast:** dynamic_cast用于动态类型转换，即转换在**运行时**才能确定。它主要用于类之间的类型转换，可以在继承层次结构中安全地将基类指针或引用转换为派生类指针或引用。

**reinterpret_cast:** reinterpret_cast用于进行任意类型之间的转换。它可以将一个指针或引用转换为另一种指针或引用，甚至可以将指针类型转换为整数类型。

# 树结构

```C++
 #include <iostream>
#include <vector>

using namespace std;

class TreeNode {
private:
    int val;
    vector<TreeNode*> children;
public:
    TreeNode(int x) : val(x) {}

    void addChild(TreeNode* node) {
        children.push_back(node);
    }

    int getVal() const {
        return val;
    }

    vector<TreeNode*> getChildren() const {
        return children;
    }
};

int main() {
    TreeNode* root = new TreeNode(1);
    TreeNode* node1 = new TreeNode(2);
    TreeNode* node2 = new TreeNode(3);
    TreeNode* node3 = new TreeNode(4);
    TreeNode* node4 = new TreeNode(5);

    root->addChild(node1);
    root->addChild(node2);
    node1->addChild(node3);
    node2->addChild(node4);

    cout << "Root: " << root->getVal() << endl;
    vector<TreeNode*> children = root->getChildren();
    for (TreeNode* child : children) {
        cout << "Child: " << child->getVal() << endl;
        vector<TreeNode*> grandChildren = child->getChildren();
        for (TreeNode* grandChild : grandChildren) {
            cout << "GrandChild: " << grandChild->getVal() << endl;
        }
    }

    return 0;
}

```

![image-20230319102847139](C:\Users\JACKIE\AppData\Roaming\Typora\typora-user-images\image-20230319102847139.png)

# 预编译头文件

预编译头文件（Precompiled Header File）是一种在编译过程中用来加速编译速度的技术。它的原理是将经常使用的头文件预先编译成二进制形式，然后在编译过程中直接使用这个二进制文件，避免了重复编译，提高了编译效率。

通常情况下，一个项目中会有一些经常被引用的头文件，如系统头文件、常用库的头文件等。如果每次编译都要重新编译这些头文件，会耗费大量的时间，特别是对于较大的项目来说，编译时间会非常长。因此，使用预编译头文件技术可以显著提高编译效率。

在使用预编译头文件时，需要将需要预编译的头文件包含在一个名为“预编译头文件”的源文件中，并在该文件中加入相应的预处理指令，告诉编译器要将这个源文件预编译为头文件。在编译其他源文件时，只需要包含这个预编译头文件即可。

需要注意的是，不是所有的头文件都适合作为预编译头文件，因为预编译头文件需要具有稳定性和高重用性。如果某个头文件经常发生变化，或者被引用的频率很低，那么将其作为预编译头文件反而可能会降低编译效率。因此，选择哪些头文件作为预编译头文件需要仔细考虑。预编译头文件（Precompiled Header File）是一种在编译过程中用来加速编译速度的技术。它的原理是将经常使用的头文件预先编译成二进制形式，然后在编译过程中直接使用这个二进制文件，避免了重复编译，提高了编译效率。

通常情况下，一个项目中会有一些经常被引用的头文件，如系统头文件、常用库的头文件等。如果每次编译都要重新编译这些头文件，会耗费大量的时间，特别是对于较大的项目来说，编译时间会非常长。因此，使用预编译头文件技术可以显著提高编译效率。

在使用预编译头文件时，需要将需要预编译的头文件包含在一个名为“预编译头文件”的源文件中，并在该文件中加入相应的预处理指令，告诉编译器要将这个源文件预编译为头文件。在编译其他源文件时，只需要包含这个预编译头文件即可。

需要注意的是，不是所有的头文件都适合作为预编译头文件，因为预编译头文件需要具有稳定性和高重用性。如果某个头文件经常发生变化，或者被引用的频率很低，那么将其作为预编译头文件反而可能会降低编译效率。因此，选择哪些头文件作为预编译头文件需要仔细考虑。

# dynamic_cast

```C++
class Entity
{
	virtual void Print()
	{};
};
class  Player	: public Entity
{
};
class Enemy	:public Entity
{
};

int main()
{
	Player* player = new Player();
	Entity* actuallyplayer = player;
	Entity* actuallyEnemy = new Enemy();
	Player* p0 = dynamic_cast<Player*>(actuallyEnemy);//失败
	Player* p1 = dynamic_cast<Player*>(actuallyplayer);
}

```

# 基准测试

```C++
class Timer
{
public:
    Timer() : m_Starttimepoint(std::chrono::high_resolution_clock::now())
    {
    }

    ~Timer()
    {
        stop();
    }

    void stop()
    {
        auto endtimepoint = std::chrono::high_resolution_clock::now();

        auto duration = std::chrono::duration_cast<std::chrono::microseconds>(endtimepoint - m_Starttimepoint);
        std::cout << duration.count() << "us (" << duration.count() / 1000.0 << "ms)\n";
    }

private:
    std::chrono::time_point<std::chrono::high_resolution_clock> m_Starttimepoint;
};
int main()
{
    int value = 0;
    {
        Timer time;
        for (int i = 0; i < 1000000; i++)
            value += 2;
    }
    std::cout << value << std::endl;

    return 0;
}

```

# 结构化绑定

结构化绑定是C++17引入的新特性，它允许我们在一个语句中将多个值解构（destructure）并绑定到多个变量上。结构化绑定可以用于任何支持std::tuple-like的类型，例如std::pair和std::array等，它可以提高代码的可读性和可维护性。

具体来说，结构化绑定允许我们将一个复合类型的元素解构为多个独立的变量，而不需要使用多个语句或手动解构元素。例如，如果我们有一个std::pair<int, std::string>，我们可以使用结构化绑定将其解构为两个变量，一个int和一个std::string，代码如下：

```C++
std::pair<int, std::string> p = {42, "hello"};
auto [i, s] = p;
```

这里的auto关键字会自动推导出i和s的类型，并将42和"hello"分别绑定到这两个变量上。这样，我们就可以直接使用i和s来访问std::pair中的两个元素，而不需要再使用p.first和p.second。

除了std::pair和std::tuple之外，结构化绑定还可以用于自定义类型，只需要在该类型中定义一个名为get()的函数，返回一个std::tuple，其中包含要解构的成员变量即可。总之，结构化绑定可以帮助我们简化代码，提高代码可读性和可维护性，使我们可以更方便地操作复杂的数据结构和算法。

# optional

通常使用 "optional" 来表示一个值可能存在或不存在的情况。这种情况经常出现在函数的参数或返回值中，因为有些参数或返回值可能无法确定或者不是必须的。

例如，假设有一个函数可以根据给定的字符串查找数据库中的用户信息。在这种情况下，如果没有找到匹配的用户，则返回值可能为空。因此，函数的返回类型可以定义为 "Optional<User>"，以表示返回值可能是一个 User 对象或者是空值。

> `std::optional` 存储对象时，我们可以使用 `has_value()` 和 `value()` 成员函数来检查对象是否有值，以及访问存储的值。它们之间的区别如下：
>
> - `has_value()` 函数用于检查 `std::optional` 是否存储了一个值。如果 `std::optional` 存储了一个值，则该函数返回 `true`，否则返回 `false`。
> - `value()` 函数用于访问 `std::optional` 存储的值。如果 `std::optional` 存储了一个值，则该函数返回存储的值，否则会抛出 `std::bad_optional_access` 异常。
>
> 需要注意的是，在调用 `value()` 函数之前，应该使用 `has_value()` 函数进行检查，以避免出现异常。

```C++
std::string ReadFileAsString(const std::string& filepath)
{
	std::ifstream stream(filepath);
	if (stream)
	{
		std::string result;
		std::getline(stream, result, '\0');
		stream.close();
		return result;
	}
	return std::string();
}
int main()
{
	std::optional<std::string> data = ReadFileAsString("data,txt");
	std::string value = data.value_or("Not present");
	std::cout << value << std::endl;
	if (data.has_value()) 
	{
		std::string result = data.value();
		std::cout << result << std::endl;
		std::cout << "file read successfully!\n";
	}
	else
	{
		std::cout << "file coule not be opened!\n";
	}
	std::cin.get();

} 
```

>  `std::optional::value_or()` 是一个非常有用的函数，它可以在可选对象为空时提供默认值。如果可选对象有值，则返回该值，否则返回提供的默认值。
>
> ```
> T value_or(const T& default_value) const;
> ```
>
> 其中，`T` 是可选对象的类型。`default_value` 是一个常量引用，表示可选对象为空时要返回的默认值。该函数返回一个类型为 `T` 的值。
>
> ```
> #include <iostream>
> #include <optional>
> 
> int main() {
>     std::optional<int> num = 10;
> 
>     // 获取 num 的值
>     int value1 = num.value_or(0);
>     std::cout << "value1: " << value1 << std::endl; // 输出 10
>     // 清空 num
>     num.reset();
>     // 获取 num 的值，由于 num 为空，返回默认值 20
>     int value2 = num.value_or(20);
>     std::cout << "value2: " << value2 << std::endl; // 输出 20
>     return 0;
> }
> ```
>
> 在上面的示例中，我们首先创建了一个 `std::optional<int>` 对象 `num`，并将其初始化为 `10`。然后，我们使用 `num.value_or(0)` 获取 `num` 的值，由于 `num` 不为空，所以返回 `10`。接着，我们使用 `num.reset()` 清空 `num` 对象，然后再次使用 `num.value_or(20)` 获取 `num` 的值，由于 `num` 为空，所以返回默认值 `20`。

# variant

`std::variant` 是 C++17 中引入的一个类模板，它是一个联合类型（Union Type），可以存储多个不同的数据类型，但每次只能存储其中的一个值。 `std::variant` 与 C++ 中的其他容器（如 `std::vector` 和 `std::map`）不同，因为它只存储一个值，而不是一组值。`std::variant` 的实现类似于 `std::tuple`，但是它可以在运行时选择存储哪种类型的值。这使得 `std::variant` 很适合需要在多种类型之间进行选择的场景。

在使用 `std::variant` 时，您需要指定可能的所有数据类型。当您创建一个 `std::variant` 对象时，它将初始化为其中一个数据类型的默认值。要更改存储在 `std::variant` 对象中的值，您可以使用 `std::get` 或 `std::visit` 函数。如果您尝试访问 `std::variant` 中不存在的类型，则会引发 `std::bad_variant_access` 异常。

------

`std::varint`与`union`的区别：

1. `std::variant` 可以存储多个不同的类型，并且可以在运行时选择存储哪种类型的值，而 `union` 只能存储其中一种类型。

2. `std::variant` 确保在使用存储的值时始终知道其类型，因为它是类型安全的。这是通过提供 `std::get` 和 `std::visit` 等函数来实现的，这些函数允许您在编译时或运行时确定存储的值的类型，并执行相应的操作。相比之下，`union` 可能存在类型不匹配的问题，因为它只是简单的存储一些字节，并且没有类型安全的检查。

3. `std::variant` 可以存储任何可复制的类型，包括内置类型、自定义类型和 STL 容器类型等。相比之下，`union` 只能存储 POD（Plain Old Data）类型，这些类型通常是内置类型或 C 语言结构体。

4. `std::variant` 可以自动处理拷贝构造函数和赋值运算符等特殊成员函数，而 `union` 需要手动实现这些函数，以确保正确地处理存储的值。

   ------

   ```C++
   int main()
   {
   	std::variant<std::string, int>	data;
   	data = "cherno";
   	std::cout << std::get<std::string>(data) << "/n";
   	data = 20;
   	std::cout << std::get<int>(data) << "/n";
   	std::cin.get();
   }
   ```
   
   

将所有可能的数据类型储存为单独的变量，作为单独的成员，

# any

`std::any` 是 C++17 标准库中的一个类，它提供了一种通用的方式来存储任何类型的值，而不需要提前指定该值的具体类型。

具体来说，`std::any` 允许你在运行时存储和访问任意类型的值，而不需要提前指定类型或进行类型转换。通过使用 `std::any`，你可以将值存储为一个 `std::any` 对象，而不需要知道该值的确切类型，然后在需要时使用 `std::any_cast` 函数将其转换为正确的类型。

```c++
#include <iostream>
#include <any>

int main() {
    std::any a = 5; // 存储一个整数
    std::cout << std::any_cast<int>(a) << '\n'; // 输出 5

    a = 3.14; // 存储一个浮点数
    std::cout << std::any_cast<double>(a) << '\n'; // 输出 3.14

    a = "Hello, World!"; // 存储一个字符串
    std::cout << std::any_cast<const char*>(a) << '\n'; // 输出 "Hello, World!"

    return 0;
}
```

注意，`std::any` 对象必须始终存储某种类型的值，因此在创建 `std::any` 对象时必须始终指定初始值。如果在访问 `std::any` 对象时使用了错误的类型，将会抛出 `std::bad_any_cast` 异常。

# 多线程化


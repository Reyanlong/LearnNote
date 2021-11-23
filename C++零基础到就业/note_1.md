# **四种类型转换**
## const_cast
```c++
const_cast 只能对指针，引用，this指针
int* vn = const_cast<int*>(&n);

去除const属性
 const CTest obj;
 常成员对象的this指针是 const this* const类型。
 常成员函数 的this指针是，const this* const类型。
 const_cast<CTest* const>(this)->m_nTest = nTest;
```

## static_cast
```c++
低风险的转换，整型与浮点型，字符与整形

// void指针的转换
void* p = nullptr;
int* pN = static_cast<int*>(p);

operator int(){}      // 定义了 转换运算符的重载
// 转化运算符的方式
CInt nObj;
int k = static_cast<int>(nObj);
等价于 int kk = nObj;      // 隐式转换

// 有继承关系 之间的转换
父类转子类（不安全）
子类转父类（安全）
```

## dynamic_cast
```c++
用于具有虚函数基类与派生类之间的指针或引用的转换
1.基类必须具备虚函数
    原因：dynamic_cast是运行时类型检查，需要运行时类型信息（RTTI），而这个信息是存储与类的虚函数表关系紧密，只有一类定义了虚函数，才会有虚函数表。

2.运行时检查，转型不成功则返回一个空指针
3.非必要不要使用dynamic_cast，有额外的函数开销

常见转换方式：
  (1)基类指针或引用转派生类指针（必须使用dynamic_cast），不安全
  (1)派生类指针或引用转基类指针（可以使用dynamic_cast，但是更推荐使用static_cast），安全
```

## reinterpret_cast
```c++
用于进行各种不同类型的转换
  (1)不同类型指针之间
  (2)不同类型引用之间
  (3)指针和能容纳指针的整数类型之间的转换

编译期处理，执行的是逐字复制的操作。
类似于显式强转，后果自负

//显式强转
//用于各种高危险的转换方式
//整形转指针
//各种类型的指针转换
//父类，子类指针的转换，不存在运行时检查
```

# **异常**
```c++
函数的返回值可以忽略，异常不可以忽略
整形返回值没有任何语义信息，异常包含语义信息，异常返回一个类，类带有信息。

throw 抛出
try   // 捕获异常

//异常是根据类型进行匹配的
catch   // 处理异常

如果异常抛到顶层了，还没有处理异常，这个时候异常会挂掉，调用terminate()函数，程序终止执行


1.c++中异常是跨函数的
2.异常必须处理
a->b->c->d a函数调用b,b调用c,c调用d,若d发生异常，会逐级往上抛，先抛给c,若c不处理则抛给b,同样，b不处理，抛给a,
都不处理，则宕机，有一个处理，问题就能解决。
throw 抛出异常
try 捕获异常
catch 匹配异常
```

## 栈解旋
```c++
异常被抛出后，从进入try块起，到异常被抛掷前，这期间在栈上构造的所有对象，都会被自动析构，析构的顺序与构造的顺序相反，这一过程称为栈的解旋(unwinding)。

//1栈解旋

class person
{
public:
    person()
    {
        cout << "开始构造类" << endl;
    }
    ~person()
    {
        cout << "开始析构函数" << endl;
    }
};

int divide(int a,int b)
{
    person p1,p2;//当divide函数发生异常的时候，函数内部的变量或者对象内存都会被析构掉，这样的现象叫做栈解旋
    if(b==0)
    {
        throw 0;
    }
    return a/b;
}
void test01()
{
    try {
        divide(10,0);
    }
    catch (int e)
    {
        cout<<"异常捕获"<<endl;
    }
}

int main() {
    test01();
    //debug:
//    开始构造类
//    开始构造类
//    开始析构函数
//    开始析构函数
//    异常捕获
    return 0;
}

2.异常类型和异常对象生命周期
3.继承在异常中的应用
```

## 异常处理规范
```c++
/** 抛出指定类型的异常，不能抛出其它类型 */
void fun() throw(int, char, float)

/** 不能抛出任何异常 */
void fun() throw()

/** 可以抛出任何类型 */
void fun()


try{
  //抛出异常的语句

}catch(int){          // 捕获int类型异常

}catch(类类型){        // 捕获某个类，类型的异常

}catch(...){          // 捕获所有异常

}

```

### 重要提示1
```c++
在定义变量的时候，一定要赋初值。否则c++内部有时候会变量复用，给出意想不到的值。

void fun(double a, double b)
{
  /** 这里不给初值：
         第一次调用res假设为5。
         第二次发生异常res却也是5。
      给了初值就不会发生这种情况。
   */
  double res;
  try {
	res = division(a, b);
  }catch(double){
	std::cerr << "除0异常" << endl;
  }
  cout << res << endl;
}
```
### 重要提示2
```c++
  1.绝不能忽略编译器的警告。

  2.按照规范抛出异常，不合法的抛出异常，编译器可能通过但程序就可能宕机。
```

### 异常2
```c++
  1.异常当前就要处理，所以直接就放在栈里，不必存放在堆里，省去了寻址压栈的过程。
  2.异常以引用传递，最好继承标准库的异常类
  3.异常对象catch处理完后就析构
// 特殊提示
    传递指针只能传递堆上的地址，不要传递栈上的地址
    new出来的异常对象用完一定要delete

4.当继承标准异常类时，应该重载父类的what()函数，和虚析构函数
5.在栈展开的过程中，需要拷贝异常类，最好写上拷贝构造函数。

6.用父类异常类去接子类异常类，打印输出信息。

7.出现异常的时候，抛对象，不要跑基本类型。
```


# **标准输入输出流**
```c++
/** 全局输出流，默认输出到显示器 */
std::cout

/** 标准错误，cerr没有缓冲区，默认输出到显示器 */
std::cerr

/** 标准日志，clog有缓冲区，默认输出到显示器 */
std::clog   // 

1.输入缓冲区(一块内存 buf[256])
2.std::endl 输入一个'\n',并刷新缓冲区。
3.EOF,文字流的结尾,文件，或者标准输入流。
  control + z
```

## <font size=3 color=red face="微软雅黑">cin</font>
<font color=yellow>cin.get()</font>
```c++
char ch;

/** 读取一个字符 */
cin.get(ch);
ch = cin.get();
```

<font color=yellow>输入字符串</font>
```c++
char buf[256] = {0};
/** 从缓冲区读取256个字节 */
cin.get(buf, 256);
/** 从缓冲区读一行数据，不读'\n' */
cin.getline(buf, 256);
```

<font color=yellow>cin.ignore()</font>
```c++
char ch2;
/** 从缓冲区要数据，阻塞 */
cin.get(ch2);

/** 忽略缓冲区当前字符,将当前字符从缓冲区删除*/
cin.ignore();
/** 忽略10个换行符，缓冲区不够10个，将能忽略的忽略 */
cin.ignore(10, '\n')
```


<font color=yellow>cin.peek()</font>
```c++
/** cin.peek()从缓冲区复制一份，当前字符，不删除 */

cout << "请输入数字或者字符串" << endl;
char ch;
ch = cin.peek();  //偷窥一下缓冲区，返回第一个字符

if(ch >= '0' && ch <= '9')
{
  int num;
  cin >> num;
  cout << "你输入的是数字: " << num << endl;

}else{
  char str[256] = {0};
  cin >> str;
  cout << "你输入的是字符串: " << str << endl;

}
```

<font color=yellow>cin.putback()</font>
```c++
cout << "请输入数字或者字符串" << endl;
char ch;
cin.get(ch);  // 从缓冲区取走一个字符
if(ch >= '0' && ch <= '9')
{
  /** 将字符放回缓冲区 */
  cin.putback(ch);
  int num;
  cin >> num;
  cout << "你输入的是一个数字: " << num << endl;

}else
{
  /** 将字符放回缓冲区 */
  cin.putback(ch);
  char buf[256];
  cin >> buf;
  cout << "你输入的是一个字符串" << buf << endl;

}
```

# **标准输出流**
<font color=yellow>cout.flush()</font>
```c++
 刷新缓冲区，cout.flush()
 endl,默认执行了数显缓冲区的操作
```
<font color=yellow>cout.put()</font>
```c++
//链式编程，输出一个字符
cout.put('h').put('e').put('l');
```
<font color=yellow>cout.write()</font>
```c++
// 输出字符串
cout.write("HelloWorld", strlen("HelloWorld"));
```

<font color=yellow>格式化输出</font>
```c++
// 成员方法的方式
int num = 10;
cout << num << endl;
/** 卸载当前默认的10进制输出方式 */
cout.unsetf(ios::dec);
cong.setf(ios::oct);  //设置为8进制输出
cout.setf(ios::showbase); //设置显示格式
cout << num << endl;

cout.unsetf(ios::oct);  // 卸载8进制输出   
cout.unsetf(ios::showbase)  // 卸载显示格式
cout.setf(ios::hex);  //设置为16进制输出
cout << num << endl;   
```

```c++
int num = 10;
cout.width(10);   // 设置为宽度为10
cout.fill('*');    // 用*填充
cout.setf(ios::left);   // 左对齐
cout << num << endl; 
```

```c++
#include <iomainip>
int num = 10;
cout << hex
     << setiosflag(ios::showbase)
     << setw(10)
     << setfill('*')
     << setiosflag(ios::left)
     << num
     << endl;
```


# **文件IO**
```c++
1.使用完整的路径。如果要使用相对路径，需要用变量把外部路径传进来，与相对路径拼接起来使用。

  /** 外部路径 */
  std::string path = R"(/Users/mac/CLionProjects/Demo11_15/)";

  /** 源文件相对路径 */
  std::string sourceFilePath = path + R"(./src/demo28/Realization/source.txt)";
  /** 目标文件相对路径 */
  std::string targetFilePath = path + R"(./src/demo28/Realization/target.txt)";

  /** 用只读方式打开源文件 */
  std::ifstream ifsm(sourceFilePath, std::ios::in);

  /** 用写的方式打开源文件 
      不写app默认是覆盖写入，app表示追加    */
  std::ofstream ofsm(targetFilePath, std::ios::out | std::ios::app);

  /** 文件打开失败抛出异常 */
	if (!ifsm){
	  throw std::ios_base::failure("source.txt 打开失败");
	}
	if (!ofsm){
	  throw std::ios_base::failure("target.txt 打开失败");
	}

  /** 读目标文件 */
  char ch = '\0';
  while(ifsm.get(ch))
  {
  
  /** 将字符写入目标文件 */
	ofsm.put(ch);
	std::cout << ch;
  }

  /** 关闭文件流 */
  ifsm.close();
  ofsm.close();
```


```c++
1.目标文件如果没有，就会自己创建一个
2.文件都是以二进制方式存储的
3.序列化指的是将一个内存对象转化成一串字节数据（存储在一个字节数组中）,可用于保存到本地文件或网络传输。反序列化就是将字节数据还原成内存对象。
4.强大的序列化工具protobuf。学会使用。json会使用。
```
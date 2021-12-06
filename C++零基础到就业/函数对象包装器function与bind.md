# 函数对象包装器

```c++
1. std::function是一个可调用对象包装器，是一个类模板，可以容纳除了类成员函数指针之外的所有可调用对象，
它可以用统一的方式处理函数、函数对象、函数指针，
并允许保存和延迟它们的执行。

2. std::function可以取代函数指针的作用，因为它可以延迟函数的执行，
特别适合作为回调函数使用。

#include <functional>

double testFun(int pr, float pa){
  return ++para * pa;
};

std::function<double(int, float)> fun = testFun;
fun(3, 1.3);
```
```c++
# 支持4种函数的封装
1. 普通函数

2. 匿名函数
std::function<double(int, float)> 
    fun = [](int pr, float pa)->double{
  //函数体
};
fun(4, 5.5);

3. 成员函数

4. 仿函数, 重载了()运算符的函数
```
```c++
// 测试类
class TestA{
  public:
  double sum(int pr, float pa){
	return pr + pa;
  }
};

// 封装成员函数, 传入隐藏的this指针
std::function<double(TestA*, int, float)> 
    fun =  &TestA::sum;

// 调用
TestA ta;
fun(&ta, 9, 11.7);
```

```c++
// 仿函数封装

class TestB{
  public:
  double operator()(int pr, float pa){
	return pr + pa;
  }
};

std::function<double(TestB*, int, float)>
    fun = &TestB::operator();

TestB tb;
fun(&tb, 6, 11.5);
```

## **bind封装**
```c++
可将std::bind函数看作一个通用的函数适配器，它接受一个可调用对象，生成一个新的可调用对象来“适应”原对象的参数列表。
std::bind将可调用对象与其参数一起进行绑定，绑定后的结果可以使用std::function保存。std::bind主要有以下两个作用：

1. 将可调用对象和其参数绑定成一个防函数；

2. 只绑定部分参数，减少可调用对象传入的参数。

```
### std::bind绑定普通函数
```c++
/** bind机制 */
double sum(int pr, float pa){
  return pr + pa;
}

auto fun1 = std::bind(sum, 3, 9);
fun();


// 参数的顺序重新设定了, 按封装序号给参数.
auto fun2 = std::bind(sum, std::placeholders::_2, std::palaceholders::_1);
fun2(9.33, 5);  //fun2(float, int)
```
### std::bind 绑定成员函数
```c++
// 绑定成员函数
1. bind绑定类成员函数时，第一个参数表示对象的成员函数的指针，第二个参数表示对象的地址。
2. 必须显示的指定&Foo::print_sum，因为编译器不会将对象的成员函数隐式转换成函数指针，所以必须在Foo::print_sum前添加&；
3. 使用对象成员函数的指针时，必须要知道该指针属于哪个对象，因此第二个参数为对象的地址 &foo；

Foo foo;
/** 传入 函数地址 this指针 函数的参数 */
auto f = std::bind(&Foo::print_sum, &foo, 95, std::placeholders::_1);
f(5); // 100
```

### std::bind 传入引用
```c++
1. 绑定一个引用参数
默认情况下，bind的那些不是占位符的参数被拷贝到bind返回的可调用对象中。
但是，与lambda类似，有时对有些绑定的参数希望以引用的方式传递，
或是要绑定参数的类型无法拷贝。

#include <iostream>
#include <functional>
#include <vector>
#include <algorithm>
#include <sstream>
using namespace std::placeholders;
using namespace std;

ostream & print(ostream &os, const string& s, char c)
{
os << s << c;
return os;
}

int main()
{
// 准备参数  
vector<string> words{"helo", "world", "this", "is", "C++11"};
// 这个流, 可以将常用类型的数据转为string存储, 然后简便的转换
ostringstream os;     
char c = ' ';


// 将容器里的数据放入流中
for_each(words.begin(), words.end(),
[&os, c](const string & s){os << s << c;} );
// 转为string输出
cout << os.str() << endl;

    ostringstream os1;
    // ostream不能拷贝，若希望传递给bind一个对象，
    // 而不拷贝它，就必须使用标准库提供的ref函数
    for_each(words.begin(), words.end(),
                   bind(print, ref(os1), _1, c));
    cout << os1.str() << endl;
}

```
### std::ref
```c++
1. std::ref在std::bind中使用, 可以传入引用

2. 使用std::ref可以在模板传参的时候传入引用，thread的方法传递引用的时候，否则无法传递。
例 在 thread 中使用引用，未使用ref()

#include <iostream>
#include <thread>
using namespace std;

void f(int &i) {
	for (int j = 0; j < i; j++) {
		cout << i << endl;
	}
}




/** 修改后 */

#include <iostream>
#include <thread>
using namespace std;

void f(int &i) {
	for (int j = 0; j < i; j++) {
		cout << i << endl;
	}
}
int main() {
	int i = 5;
	thread t(f, ref(i));
	t.join();
	
}

// ref()方法的返回值是reference_wrapper类型,这个类的源码大概的意思就是维持一个指针，并且重载操作符
```

### 指向成员函数的指针
```c++
1. 成员函数指针的定义：void (Foo::*fun)()，调用是传递的实参: &Foo::f

2. fun为类成员函数指针，所以调用是要通过解引用的方式获取成员函数*fun,即(foo1->*fun)()

#include <iostream>
struct Foo {
    int value;
    void f() { std::cout << "f(" << this->value << ")\n"; }
    void g() { std::cout << "g(" << this->value << ")\n"; }
};


// 对象指针, 分别调用其对象的成员函数
void apply(Foo* foo1, Foo* foo2, void (Foo::*fun)()) {
    (foo1->*fun)();  // call fun on the object foo1
    (foo2->(*fun))();  // call fun on the object foo2
}



int main() {
    Foo foo1{1};
    Foo foo2{2};
	
    apply(&foo1, &foo2, &Foo::f);
    apply(&foo1, &foo2, &Foo::g);
}
```
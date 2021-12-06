# lambda / 匿名函数

```c++
[捕获列表](参数)->类型{函数体};

// 直接调用, c接收返回值
int c = [](int a, int b)->int{
            return a + b;
          }(3, 6);     //传参

// 捕获列表, 用于捕获外界的变量
```
```c++
lambda函数的嵌套使用

int sum = [](int n){
              return [n](int t){
	                     return n + t;
                       }(5);
            }(9);

/** 保存调用 */
auto fun_sum = [](int n){
              return [n](int t){
	                     return n + t;
                       };
            };

sum = fun_sum(9)(5);
```

```c++
1. 捕获列表是按值捕获的, 所以无法修改外部变量

int t = 10;
/** mutable关键字, 可以让捕获的值拷贝, 能够修改 */
/** mutable使得捕获的值拷贝, 会被保存 */
auto f = [t]()mutable{
  return ++t;
};

2. 全局对象可以直接使用, 不用捕获, 类似cout 
3. 捕获列表的值, 是在声明的时候捕获, 不是调用的时候捕获,
   如果在声明后,调用前改变了外部变量的值, 捕获的值不会改变.
```
```c++
1. 按引用捕获改变的是同一个变量
int i = 10;
auto fun = [&i]()mutable{
  return ++i;
};
```
```c++
1. 按值捕获外界所有的变量
[=](){
  //函数体
};

2. 按引用方式捕获所有变量
[&](){
  //函数体
}

3. 值捕获a,c  引用捕获b,d
[a, &b, c, &d](){
  //函数体
}
```
```c++
[this](){};    //捕获this指针

[a, &](){};     //a以值捕获, 其余变量以引用捕获

[=, &a](){};      //a以引用捕获, 其余以值捕获

[this]：通过引用捕获当前对象（其实是复制指针）；

[*this]：通过传值方式捕获当前对象；
```

```c++
1. lambda函数是一个依赖于实现的函数对象类型,这个类型的名字只有编译器知道.
如果用户想把lambda函数做为一个参数来传递, 
那么形参的类型必须是模板类型或者必须能创建一个std::function类似的对象去捕获lambda函数.
使用 auto关键字可以帮助存储lambda函数

auto my_lambda_func = [&](int x) { /*...*/ };
auto my_onheap_lambda_func = new auto([=](int x) { /*...*/ });
```

```c++
1. 把匿名函数存储在变量,数组或vector中,并把它们当做命名参数来传递

#include<functional>
#include<vector>
#include<iostream>
double eval(std::function<double(double)> f, double x = 2.0){return f(x);}
int main()
{
     std::function<double(double)> f0    = [](double x){return 1;};
     auto                          f1    = [](double x){return x;};
     decltype(f0)                  fa[3] = {f0,f1,[](double x){return x*x;}};
     std::vector<decltype(f0)>     fv    = {f0,f1};
     fv.push_back                  ([](double x){return x*x;});
     for(int i=0;i<fv.size();i++)  std::cout << fv[i](2.0) << "\n";
     for(int i=0;i<3;i++)          std::cout << fa[i](2.0) << "\n";
     for(auto &f : fv)             std::cout << f(2.0) << "\n";
     for(auto &f : fa)             std::cout << f(2.0) << "\n";
     std::cout << eval(f0) << "\n";
     std::cout << eval(f1) << "\n";
     return 0;
}
```

```c++
1. 一个没有指定任何捕获的lambda函数,可以显式转换成一个具有相同声明形式函数指针.
   所以,像下面这样做是合法的:

auto a_lambda_func = [](int x) { /*...*/ };
void(*func_ptr)(int) = a_lambda_func;
func_ptr(4); //calls the lambda.
```
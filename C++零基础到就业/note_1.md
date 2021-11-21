# **四种类型转换**
## const_cast
```
const_cast 只能对指针，引用，this指针
int* vn = const_cast<int*>(&n);

去除const属性
 const CTest obj;
 常成员对象的this指针是 const this* const类型。
 常成员函数 的this指针是，const this* const类型。
 const_cast<CTest* const>(this)->m_nTest = nTest;
```

## static_cast
```
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
```
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
```
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


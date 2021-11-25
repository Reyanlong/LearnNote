<font size=9 color=red face="黑体">STL</font>

```c++
1.理解容器的概念
2.理解迭代器的概念
3.理解算法的概念
```
```c++
1.STL是内置库
2.数据结构和算法的分离
3.熟练使用
4.高可重用性，高性能，高移植性，跨平台等优点

5.分为序列式容器，关联式容器
```

```c++
迭代器可以理解为指针，对指针的操作基本都可以对迭代器操作。
实际上迭代器是一个类对象，封装了一个指针。

算法，通过有限步骤解决问题。
STL提供了大约100个实现算法的模版函数。

迭代器 .end返回的迭代器，指向的位置是最后元素的下一个位置。
```

### <font color=blue>for_each</font>
```c++
1.函数对象是一个对象，使用起来像个函数。关键是重载()运算符。
2.for_each(迭代器，迭代器，函数/函数对象)


#include <vector>
#include <algorithm>


namespace AT{

/** 打印容器里的值 回调函数 */
void printValue(int para)
{
  std::cout << para << ' ';
}


/** 函数对象 */
template<typename T>
class AddAverage{
 private:
  T m_value;    //遍历的值
  T m_sum;      //总和
  long m_count;    //计数

 public:
  /** 数据要加的值 */
  explicit AddAverage(T value = 0): m_value(value), m_sum(0), m_count(0) {};

  /** 给参数para加上m_value,并且返回参数para */
  T& operator()(T& para){
	m_sum += para;
	m_count += 1;
	para += m_value;
	return para;
  }

  /** 返回平均值 */
  double getAverage(){
	return static_cast<double>(m_sum) / m_count;
  }
};


/** 执行函数 */
void fun()
{
  std::vector<int> vec_int;
  vec_int.push_back(11);
  vec_int.push_back(2);
  vec_int.push_back(9);
  vec_int.push_back(38);
  vec_int.push_back(90);
  vec_int.push_back(17);

  /** 指向首元素的 指针对象，迭代器 */
  std::vector<int>::iterator p_begin = vec_int.begin();
  /** 指向末尾元素之后地址的 指针对象，迭代器 */
  std::vector<int>::iterator p_end = vec_int.end();
  /** 给容器排序 */
  std::sort(p_begin, p_end);

  /** 传入函数 遍历打印*/
  void(*funp)(int) = std::for_each(p_begin, p_end, printValue);
  std::cout << std::endl;
  funp(-3434);     // for_each 返回的函数指针
  std::cout << std::endl;

  /** 传入函数对象 给value的值加构造参数*/
  std::for_each(p_begin, p_end, AddAverage<int>(1));
  /** for_each 返回的函数对象，可以保存最终数据的对象 */
  AddAverage<int> res = std::for_each(p_begin, p_end, AddAverage<int>(*(p_begin)));

  /** 打印结果 */
  std::for_each(p_begin, p_end, printValue);
  std::cout << "\n平均值是：" << res.getAverage() << std::endl;
}

}
```

```c++
const std::string&   //这个类型可以接受 字符串常量

容器嵌套    //装容器的容器

std::string 和 char* 可以互相转换
```
<font color=orange size=5>std::string</font>
```c++
1.std::string中的[]取字符，越界直接挂了,vs上会挂掉，clang没有挂掉，而是取回了字符。
2.std::string中的at函数取字符，越界抛异常。

string对象的 += 与 + 操作可以作用于 char*, std::string, char 这些类型，常量字符串也可以。

string对象的append，最后追加元素,参数必须是字符串，即双引号的，可以链式调用。

```

#### **查找字符串 find，rfind**
```c++
/** find，从前往后找，返回第一次出现的下标 */
int pos =str.find('c');
/** rfind，从后往前找，返回第一次出现的下标 */
pos = str.rfind('c');
```

#### **替换字符串 replace**
```c++
/** replace，替换指定区间的字符 */
/** 0 <= pos < 5，用"ur"替换区间的内容 */
/** 参数必须是字符串，不能是字符 */
str.replace(0, 5, "ur");
```

#### **比较字符串 compare**
```c++
/** compare，比较字符串大小，区分大小写，字典顺序 */
/** 等于返回0 */
/** 小于返回负的ascii差值 */
/** 大于返回正的ascii差值 */
int res = str1.compare(str2);
```

#### **截取字符串 substr**
```c++
/** 返回由pos开始的n个字符组成的字符串 */
/** npos 默认是 n索引处元素到最后元素的数量 */
string substr(int pos = 0, int n = npos)const;
```

#### **插入与删除 insert erase**
```c++
/** 在pos位置后面插入字符串s */
string& inster(int pos, const char* s);

/** 在pos位置后面插入字符串str */
string& inster(int pos, const string& str);

/** 在pos位置后面插入，n个字符c */
string& inster(int pos, int n, char c);

/** 删除从pos开始，n个字符，默认删完 ，包含索引为pos的元素*/
/** 返回删除后的部分字符串 */
string& erase(int pos, int n = npos);
```

<font color=orange size=5>std::vector</font>
```c++
std::vector 是单口容器
insert方法会移动很多元素，效率不高，不建议使用。
push_back,pop_back,不需要移动其它元素，效率较高。

/** end指向末位元素的后一位，begain指向首位元素 */
/** 正向 */
begain end

/** rend指向首元素的前一位，rbegain指向末位元素 */
/** 反向 */
rend rbegain
```

#### **std::vector的构造函数**
```c++
/** 无参构造函数，模版实现 */
vector<T> v;

/** 将v[begin(), end()]区间中的元素拷贝给本身 */
vector(v.begin(), v.end());

/** n个elem元素拷贝给本身 */
vector(n, elem);

/** 拷贝构造 */
vector(const vector& vec);

最常使用第二个构造函数
int arr[] = {2, 3, 4, 1, 9};
vector(arr, arr + sizeof(arr) / sizeof(int));


/** 示例代码 */
  /** 无参初始化 */
  std::vector<int> vec_1;

  char arr[11] = "HelloWorld";

  /** 使用容器数组初始化，传入首尾地址 */
  std::vector<char> vec_2(arr, arr + sizeof(arr) / sizeof(char));
  std::for_each(vec_2.begin(), vec_2.end(), printValue);

  /** 使用容器向量初始化 传入首尾迭代器 */
  std::vector<char> vec_3(vec_2.begin(), vec_2.end());
  std::for_each(vec_3.begin(), vec_3.end(), printValue);

  /** 拷贝构造 */
  std::vector<char> vec_4(vec_3);
  std::for_each(vec_4.begin(), vec_4.end(), printValue);
```

#### **std::vector 常用赋值操作**
```c++
/** 将[beg, end]区间中的数据拷贝赋值给本身 */
assign(beg, end);

/** 将n个elem元素拷贝赋值给本身 */
assign(n, elem);

/** 重载等号操作符 */
vector& operator=(const vector& vec);

/** 将vec与本身的元素互换, 指针的指向交换，不是拷贝交换*/
swap(vec);


  char arr[11] = "HelloWorld";

  std::vector<char> vec_1(arr, arr + 5);
  std::vector<char> vec_2;
  vec_2.assign(arr + 5, arr + 11);

  /** 打印vec_1，vec_2 */
  std::for_each(vec_1.begin(), vec_1.end(), printValue);
  std::for_each(vec_2.begin(), vec_2.end(), printValue);

  /** vec_3 存放 "HelloWorld" */
  std::vector<char> vec_3(arr, arr + 11);
  vec_1.assign(vec_2.begin(), vec_2.end());
  vec_2.assign(vec_3.begin(), vec_3.begin() + 5);

  /** 打印vec_1，vec_2 */
  std::cout << std::endl;
  std::for_each(vec_1.begin(), vec_1.end(), printValue);
  std::for_each(vec_2.begin(), vec_2.end(), printValue);

  /** 交换vec_1， vec_2 */
  vec_1.swap(vec_2);

  /** 打印vec_1，vec_2 */
  std::cout << std::endl;
  std::for_each(vec_1.begin(), vec_1.end(), printValue);
  std::for_each(vec_2.begin(), vec_2.end(), printValue);
```

#### **从属名称**
```c++
Dependent names  // 从属名称

/** 下面的typename告诉编译器自己使用的T类型，与函数头使用的T类型一样，是依赖关系 */
template<typename T>
void prinVec(const std::vector<T>& vec){
  typename std::vector<T>::const_iterator p_end = vec.end();
  for (typename std::vector<T>::const_iterator itor = vec.begin(); itor != p_end ; ++itor) {
	std::cout << *itor;
  }
  std::cout << std::endl;
}
```

#### **vector的大小操作**
```c++
/** 返回容器中元素的个数 */
size()

/** 判断容器是否为空 bool类型*/
empty()

/** 重新指定容器元素个数，容器变短删除多余元素，容器变长补默认值*/
resize(int num)

/** 用elem填充 */
resize(int num, elem)

/** 容器的容量 */
capacity()

/** T*len 如果大于当前capacity，则将capacity增加到T*len
    小于则不变。
 */
reserve(int len)


reserve增加了vector的capacity，但是它的size没有改变！
reserve只能变大capacity，不能减小capacity。

resize改变了vector的capacity同时也增加了它的size！
```


#### **vector 数据存取操作**
```c++
/** 返回索引处的数据，越界抛出异常 */
at(int idx)

/** 返回索引处的数据引用，越界程序出错 */
operator[]

/** 返回容器中第一个数据元素 */
front()

/** 返回容器中最后一个元素 */
back()
```


#### **vector 插入和删除操作**
```c++
/** 迭代器指向位置pos插入count个元素elem */
insert(const_iterator pos, int count, elem)

/** 在pos位置插入elem */
insert(const_iterator pos, elem)

/** 插入一个范围的元素 使用数组地址也可以 */
template <class 模版迭代器>
void insert (vector的迭代器 pos, 模版迭代器 first, 模版迭代器 last);

/** 尾部插入元素elem */
push_back(elem)



/** ----------------------------------------------- */
/** ----------------------------------------------- */


/** 删除最后一个元素 */
pop_back()

/** 删除迭代器从start到end之间的元素 */
erase(const_iterator start, const_iterator end)

/** 删除迭代器指向的元素 */
erase(const_iterator pos)

/** 删除容器中所有元素 */
clear()
```

#### **巧用swap 收缩vector空间**
```c++
/** 一般支持数组下标的，都支持随机访问 */
vec.insert(vec.bengin() + 2, elem);

1.匿名对象用完就会析构。
2.vector默认的capacity为0。

/** 巧用匿名对象加swap 收缩空间 */
  
  char arr[] = "HelloWorld";
  // size 11, capacity 11
  vector<char> vec_1(arr , arr + sizeof(arr) / sizeof(char));
  

  /** 将空间个数，增加到2000 */
  // size 11, capacity 2000
  vec_1.reserve(2000);
  
  
  /** 用匿名对象的swap方法，回缩空间 */
  // size 11, capacity 11
  vector<char>(vec_1).swap(vec_1);
```

```c++
1.如果知道容器大概需要的空间，可以使用reverse预先开辟大概的空间，可以减少容器分配空间，释放空间，拷贝数据的次数。

  vector<char> vec_1;
  char* ptr = nullptr;
  int count = 0;

  for (int i = 0; i < 100000; ++i) {
	vec_1.push_back('a');

	if (ptr != &(vec_1[0])){
	  ptr = &(vec_1[0]);
	  count++;
	}
  }
  //分配空间的次数是18



  vector<char> vec_2;
  count = 0;
  ptr = nullptr;

  /** 预分配了50000个空间 */
  vec_2.reserve(50000);
  for (int i = 0; i < 100000; ++i) {
	vec_2.push_back('a');
	if (ptr != &(vec_2[0])){
	  count++;
	  ptr = &(vec_2[0]);
	}
  }
  //分配空间的次数是2

```
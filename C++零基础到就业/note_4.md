### **链表 list**
```c++
构造
list<T> listT;      //默认构造

list(beg, end);      //迭代器参数构造
 
list(n, elem);      //n个元素初始化

list(const list& lst);    //拷贝构造
```

```c++
list数据元素的插入和删除操作
push_back(elem);     // 删除元素elem

pop_back();     //删除尾部元素

push_front();     //头部追加元素

pop_front();     //删除头部元素

insert(pos, elem);      //在pos处插入元素,返回新数据的位置

insert(pos, n, elem);    //在pos处插入n个elem,返回新数据的位置

insert(pos, beg, end);    //在pos处插入迭代片段,返回新数据的位置

clear();    //清空

erase(beg, end);   //删除迭代片段,返回下一个数据的位置

erase(pos);   //删除pos位置处元素,返回下一个数据的位置

remove(elem);      //删除容器中所有与elem匹配的元素
```

```c++
list的大小操作
size();      //返回元素个数

empty();     //判断是否为空

resize(num);    //重新设定元素个数
resize(num, elem);     //填充为elem
```
```c++
1.list中的迭代器不能加减,只支持++操作,--操作,代表含义是前后,双向链表的迭代方式.
2.list是双向链表
3.不支持下标随机存取,没有at方法,只能遍历
```


### **匿名函数**
```c++
[](int x, int y) { return x + y; } // 隐式返回类型
[](int& x) { ++x; }   // 没有return语句 -> lambda 函数的返回类型是'void'
[]() { ++global_x; }  // 没有参数,仅访问某个全局变量
[]{ ++global_x; }     // 与上一个相同,省略了()


[](int x, int y) -> int { int z = x + y; return z; }


[]        //未定义变量.试图在Lambda内使用任何外部变量都是错误的.
[x, &y]   //x 按值捕获, y 按引用捕获.
[&]       //用到的任何外部变量都隐式按引用捕获
[=]       //用到的任何外部变量都隐式按值捕获
[&, x]    //x显式地按值捕获. 其它变量按引用捕获
[=, &z]   //z按引用捕获. 其它变量按值捕获
```

```c++
/** 遍历删除符合要求的值 */
remove_if(bool返回值函数);
lst1.remove_if([](int i)->bool{return i > 7;});

```
```c++
/** 参数传递的list,它的全部或部分内容,移动到指定位置,参数列表的被移动元素将丢失 */

void splice(iterator, list&);
void splice(iterator, list&, iterator);
void splice(iterator, list&, iterator, iterator)

// 实例
lstSwap.splice(it, lstI, ++(lstI.begin()), --(lstI.end()) );
```

```c++
/** 在迭代器pos指定位置之前,插入初始化列表(用大括号{}括起来的多个元素,中间有逗号隔开)中所有的元素,并返回第一个新插入元素位置的迭代器 */
iterator insert(pos, initlist);

iterator insert(pos, elem);
iterator insert(pos, n, elem);
iterator insert(pos, first, last);
```
```c++
list容器不支持随机访问,未提供下标操作符[]和at()成员函数,也没有提供data()成员函数.

// 初始化
const std::list<int> mylist{1,2,3,4,5};

// 赋初值
void assign(first, last);
void assign(n, const T&);
```
```c++
//拷贝普通数组，创建list容器
int a[] = { 1,2,3,4,5 };
std::list<int> values(a, a+5);
 
 
//拷贝其它类型的容器，创建 list 容器
std::array<int, 5>arr{ 11,12,13,14,15 };
std::list<int>values(arr.begin()+2, arr.end());//拷贝arr容器
```

```c++
/** 迭代器辅助函数 */

// 迭代器移动n个距离, n的正负控制方向
advance(iterator, n)      // 越界会循环, end的下一个是begin

// 容器的变化,不会自动改变迭代器,有时候迭代器可能会失效,可能会指错位置,所以当容器变了,就要检查迭代器的指向是否正确.



list<int>::iterator it = lstSwap.begin();

lstSwap.push_front(1);
lstSwap.push_front(2);
/** 这里it不会发生改变,迭代器是自己控制的,容器改变,不会发生联动改变 */
advance(it, 2);       // 实际这里指向第6个元素

lstSwap.push_front(3);
```
```c++
advance(it, n)	//it 表示某个迭代器，n 为整数。该函数的功能是将 it 迭代器前进或后退 n 个位置。

distance(first, last)	//first 和 last 都是迭代器，该函数的功能是计算 first 和 last 之间的距离。

begin(cont)	//cont 表示某个容器，该函数可以返回一个指向 cont 容器中第一个元素的迭代器。

end(cont)	//cont 表示某个容器，该函数可以返回一个指向 cont 容器中最后一个元素之后位置的迭代器。

prev(it)	//it 为指定的迭代器，该函数默认可以返回一个指向上一个位置处的迭代器。注意，it 至少为双向迭代器。

next(it)	//it 为指定的迭代器，该函数默认可以返回一个指向下一个位置处的迭代器。注意，it 最少为前向迭代器。
```

```c++
1.erase删除单个匹配值

2.remove删除所有匹配值
```
### list赋值操作
```c++
assign(beg, end);      //将区间内的元素赋值给列表

assign(n, elem);       //将n个元素赋值给列表

list& operator=(const list& lst);   //重载等号操作符

swap(lst);      //将lst与本身互换
```

### list数据存取
```c++
front();      //返回第一个元素

back();         //返回最后一个元素
```

### list反转排列排序
```c++
reverse();     //反转链表,比如lst包含1,3,5元素,用此方法后,lst就包含5,3,1元素.

sort();      //list排序,默认从小到大

sort(函数或者类函数); 

/** 从算法库中的sort函数,只支持可以随机访问的容器 */
lst.sort([](int i, int j){return i > j;});
```


## **set容器 multiset容器**
```c++
1.树,二叉树,平衡二叉树,红黑树

2.插入的元素必须保证能够被排序,自动排序,默认从小到大

3.关联式容器

4.不允许重复元素

5.multiset允许重复

6.只能删了再添,不能修改

```

```c++
/** 构造 */
set<T> st;
muliset<T> nst;
set(const set& st);
```
```c++
/** set赋值操作 */
set& operator=(const set &st);
swap(st);
```
```c++
/** set大小操作 */
size();
empty();
```
```c++
/** set插入和删除操作 */
insert(elem);
clear();
erase(pos);
erase(beg, end);
erase(elem);       //删除容器中值为elem的元素,删除一个
```
```c++
/** 查找键key是否存在,若存在,返回改键的元素的迭代器;若不存在返回map.end(); */
find(key);

/** 返回第一个key >= keyElem元素的迭代器. */
lower_bound(keyElem);

/** 返回第一个key > keyElem元素的迭代器. */
upper_bound(keyElem);

/** 返回容器中key与keyElem相等的上下限的两个迭代器. */
equal_range(keyElem);
```

```c++
1.先序, 后序, 中序, 三种遍历方法
```

### 对组 pair
```c++
1.对组(pair)将一对值组合成一个值,这一对值可以具有不同的数据类型,两个值可以分别用pair的两个共有函数first和second访问.
```
```c++
/** 创建对组 */
pair<T1, T2> pa1(T1, T2);

pair<T1, T2> pa2 = make_pair(T1, T2);

/** pair赋值 first, second方法 */
pair<T1, T2> pa3 = pa2;

pa3.first;

pa3.second;
```


```c++
/** 自定义排序 */

class MyCompare{
 public:
  bool operator()(int a, int b){
	return a > b;
  }
};

  // 初始化
  set<int, MyCompare> seI({4, 66, 2, -24, 98, 87, 77, 1});

  // 遍历
  for_each(seI.begin(), seI.end(), [](int i){cout << i << '\t';});

```
```c++
1.用排序的信息,确定set中的元素标识.
用什么排序,就用什么确定元素.
2.find查找的原理就是用,set比较器的参数,判断.

/** 存储类 */
class TestA{
 public:
  int m_id;
  int m_num;

  TestA(int id, int num): m_id(id), m_num(num) {};

};

/** 比较器 仿函数 */
class comPare{
 public:
  bool operator()(TestA a, TestA b){
	return a.m_num > b.m_num;
  }
};


void fun3(){
  // 初始化
  set<TestA, comPare> stA;
  stA.insert(TestA(1, 2));
  stA.insert(TestA(10, 20));
  stA.insert(TestA(100, 200));
  stA.insert(TestA(1, 6));
  stA.insert(TestA(11, 21));

  //遍历 打印
  for(set<TestA, comPare>::iterator it = stA.begin(); it != stA.end(); ++it){
	cout << (*it).m_id << " : " << (*it).m_num << endl;
  }
  cout << endl;

  //find函数,返回符合元素的迭代器
  /**  这里TestA(55, 200),能找到
       虽然没有一模一样的元素,但是比较规则是依据200的,200可以找到,所以set就认为找到了    */   
  set<TestA, comPare>::iterator iter = stA.find(TestA(55, 200));
  if (iter != stA.end()){
	cout << "找到了" << (*iter).m_id << " : " << (*iter).m_num << endl;
  } else{
	cout << "没找到" << endl;
  }
}
```

```c++
/** 算法库中的一个函数源码 */
template<typename _InputIterator, typename _Predicate>
    inline _InputIterator
    __find_if(_InputIterator __first, _InputIterator __last,
	      _Predicate __pred, input_iterator_tag)
    {
      /** 这里的bool表达式,实现了先判空,再比对的简便写法 */
      while (__first != __last && !__pred(__first))
	++__first;
      return __first;
    }
```

```c++
/** set可以自定义比较器,构造集合,比较器必须是重载了()的类 */
set<TestA, comPare> stA;
```
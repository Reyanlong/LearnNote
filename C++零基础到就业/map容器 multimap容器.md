## **map / multimap 容器**
```c++
1.map相对于set的区别,map具有键值和实值,所有元素根据键值自动排序.pair的第一元素被称为键值,第二元素被称为实值.map也是以红黑树为底层实现机制.

2.map的key不允许重复

3.multimap键值可以重复
4.multset可以元素重复

5.不能通过map的迭代器修改map的键值,任意改变键值会破坏容器的排列规则,但可以改变实值.
```
### map常用API
```c++
/** map构造函数 */
map<T1, T2> mapTT;

map(const map& mp);

/** 测试 */
//比较器
template<typename T>
class mapCompare{
 public:
  bool operator()(T key_one, T key_two){
	return key_one > key_two;
  }

};

void fun1(){
  map<int, char> mp1;
  mp1.insert(pair<int, char>(1, 'a'));

  map<int, char> mp2(mp1);
  /** 使用,重载[] */
  mp2[2] = 'c';

  map<int, char, mapCompare<int>> mp3;
  mp3.insert(map<int, char>::value_type(3, 'f'));

  map<int, char> mp4(mp2);
  /** insert返回pair<iterator, bool> */
  pair<map<int, char>::iterator, bool> ret = mp4.insert(make_pair(6, 'd'));

  for(const pair<int, char>& one : mp4){
	cout << one.first << " : " << one.second << endl;
  }

}

```
```c++
/** map赋值操作 */
map& operator=(const map& mp);

swap(mp);
```
```c++
/** map的大小操作 */
size();

empty();
```
```c++
1. insert返回对组pair<iterator, bool>,分别为插入元素的迭代器,插入是否成功的bool信号.

/** key不存在就创建对组添加, 存在就修改键对应的值 */
map[10] = 'c';
2. 如果直接访问map[40],key不存在,会添加并且给默认值.

3. vector map set等标准容器默认都是使用内存分配器模板类allocator管理内存的分配、释放。allocator调用new/delete操作符进行批量的内存分配。

```


```c++
1. STL容器都是值拷贝

2. 在开发中尽量避免去使用全局变量


struct print{
  int count = 0;    //保存数据
  
  void operator()(val){
	count++;
	cout << val << " ";
  }
  
};


struct print res = for_each(begin, end, print);
cout << res.count << endl;     // 循环的次数
```
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

insert(pos, elem);      //在pos处插入元素

insert(pos, n, elem);    //在pos处插入n个elem

insert(pos, beg, end);    //在pos处插入迭代片段

clear();    //清空

erase(beg, end);   //删除迭代片段

erase(pos);   //删除pos位置处元素

remove(elem);      //删除elem元素
```

```c++
list的大小操作
size();      //返回元素个数

empty();     //判断是否为空

resize(num);    //重新设定元素个数
```
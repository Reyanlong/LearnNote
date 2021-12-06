# sstream
```c++
1. sstream定义了三个类：istringstream、ostringstream和stringstream
   分别用来进行流的输入、输出和输入输出操作
   
2. 由于sstream使用string对象代替字符数组，避免缓冲区溢出的危险;
   其次，因为传入参数和目标对象的类型会被自动推导出来，所以不存在错误的格式化符的问题. 
   相比c库的数据类型转换，sstream更加安全、自动和直接。
```
```c++
istringstream类用于执行C++风格的串流的输入操作。
ostringstream类用于执行C风格的串流的输出操作。
strstream类同时可以支持C风格的串流的输入输出操作。

(1) istringstream类是从istream（输入流类）
和stringstreambase（c++字符串流基类）派生而来，
(2) ostringstream是从ostream（输出流类）
和stringstreambase（c++字符串流基类）派生而来，
(3) stringstream则是从iostream(输入输出流类)
和和stringstreambase（c++字符串流基类）派生而来。
```
```c++
# 数据类型转换

stringstream sstream;
string strResult;
int nValue = 1000;

// 将int类型的值放入输入流中
sstream << nValue;
// 从sstream中抽取前面插入的int类型的值，赋给string类型
sstream >> strResult;

// 这里是变为string对象输出
cout << "[cout]strResult is: " << strResult << endl;
// 这里转为了c风格的, 字符数组模式输出
printf("[printf]strResult is: %s\n", strResult.c_str());

// 程序结果
[cout]strResult is: 1000
[printf]strResult is: 1000
```
```c++
# 字符串的拼接

1. 多次转换, 必须用sstream.str(""), sstream.clear() 清空流
2. 可以使用str方法，将stringstream类型转换为string类型
3. 可以将多个字符串放入stringstream，实现字符串的拼接目的

stringstream sstream;

// 将多个字符串放入 sstream 中
sstream << "first" << " " << "string,";
sstream << " second string";
cout << "strResult is: " << sstream.str() << endl;

// 清空 sstream
sstream.str("");
sstream << "third string";
cout << "After clear, strResult is: " << sstream.str() << endl;

// 输出结果
strResult is: first string, second string
After clear, strResult is: third string
```

```c++
/** 如果构造的时候设置了字符串参数,那么增长操作的时候不会从结尾开始增加,
    而是修改原有数据,超出的部分增长 */
ostr.str("abc"); 


```





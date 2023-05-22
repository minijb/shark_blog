---
date created: 2023-04-26 22:14
date updated: 2023-04-26 22:49
---

# stream

#cpp/stream

an abstraction for  input/output. Streams  convert between data and  the string representation  of data.

```c++
// use a stream to print any primitive type!  
std::cout << 5 << std::endl; // prints 5  
// and most from the STL work!  
std::cout << "Sarah" << std::endl;  
// Mix types!  
std::cout << "Sarah is " << 21 << std::endl;  
// structs?  
Student s = {"Sarah", "CA", 21};  
std::cout << s << std::endl;//err
std::cout << s.name << s.age << std::endl; // work
```

如果我们系统结构体可以直接使用<< 。我们可以通过运算符重载

`std::cout` is an output  stream. It has type  `std::ostrea`

两种分类流的方式
![](https://s2.loli.net/2023/04/26/iExcql6Ja9KVLzm.png)

## output streams

#cpp/stream/output

### Output Streams

- 使用`<<` 连接流
- 将任意类型转化为字符串，送到流内

```c++
std::cout << 5 << std::endl;  
// converts int value 5 to string “5”  
// sends “5” to the console output stream
```

`std::cout` 是我们输出到屏幕的东西

### Output file Streams

- Converts data of any type into a string and sends it  to the file stream
- 需要自己初始化`ofstream`并连接到文件

```c++
std::ofstream out(“out.txt”);  
// out is now an ofstream that outputs to out.txt  
out << 5 << std::endl; // out.txt contains 5
```

`std::cout`  是全局const对象 --- 来自`#include <iostream>`

## input stream

`>>`  是流提取运算符。

- 用来从流中提取数据，并存放到变量中

`std::cin` is an input  stream. It has type  `std::istream`

我们只能从string中得到数据，然后转换为具体变量

- First call to `std::cin >>` creates a command line  prompt that allows the user to type until they hit enter
- 每一个 `>>` 读到 the next whitespace
  - whitespace = tab , space , newline
- 每次cin都会读取 回车之前的所有 saved 内容
  - 存放的地方被称为 buffer
- 如果缓冲区中没有等待的内容，std::cin >>将创建一个新的命令行提示符
- whitespace 会自动删除

**std::getline()**

```c++
// Used to read a line from an input stream  
// Function Signature  
istream& getline(istream& is, string& str, char delim);
```

- is --- read from which stream

- str --- store output

- delim --- stop when read `\n`  by default

- Clears contents in str

- Extracts chars from is and stores them in str until:
  - End of file reached, sets EOF bit (checked using is.eof())
  - Next char in is is delim, extracts but does not store delim
  - str out of space, sets FAIL bit (checked using is.fail())

- If no chars extracted for any reason, FAIL bit se

对比：

- `>>`  一直读--直到遇到 whitespace (so can’t read a  sentence in one go)
- `>>`  可以自动转换类型
- `getline`  可以自己设置停止位置

### Input File Streams

Have type `std::ifstream`

- You can only receive strings using the >> operator
	- Receives strings from a file and converts it to data of  any built-in type  
- Must initialize your own ifstream object linked to your  file

```c++
std::ifstream in(“out.txt”);  
// in is now an ifstream that reads from out.txt  
string str;  
in >> str; // first word in out.txt goes into str
```


## Stringstreams

what: 可以从string类型中读或者写

目的：是我们直接直接总string中进行读取或者写

```c++
std::string input = "123";  
std::stringstream stream(input);  
int number;  
stream >> number;  
std::cout << number << std::endl; // Outputs "123"
```

**进行读写**

- 读: `std::istringstream`

将所有数据变为一个字符串

- 写：`std::ostringstream`

使用字符串走一个ostream

```c++
#include <iostream>
#include <fstream>
#include <sstream>

using namespace std;

int main()

{
    string x = "100 200 300";
    stringstream ss(x);
    int a, b, c;
    ss >> a >> b >> c;
    cout << a << b << c << endl;

    ostringstream  s1;
    s1 << "11" << 22 << 33 << endl;
    cout << s1.str() << endl;

    return 0;

}
```

## 总结

- filestream 都需要绑定
- istringstram需要绑定
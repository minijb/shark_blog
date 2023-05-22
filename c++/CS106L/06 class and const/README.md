---
date created: 2023-05-11 09:42
date updated: 2023-05-14 23:49
---

#cpp/class

**struct issues**

- Public access to all internal state data by default
- the initialize is diffault

**class has public and private**

```c++
//student.h
class Student {
public:
	std::string getName();
	void setName(string name);
	int getAge();
	void setAge(int age);
private:
	std::string name;
	std::string state;
	int age;
};
```

**public 部分**

- 类的使用者可以直接使用public中东西
- 定义了private部分的对外接口

**private 部分**

- 包含大部分成员变量
- 用户不能使用或改变

```c++
//student.cpp#include student.h
std::string 
Student::getName(){
//implementation here!
}
void Student::setName(){
}
int Student::getAge(){
}
void Student::setAge(int age){
}
```

- 每一个类都是一个namespace
- 调用命名空间的方法：`namespace_name::name`

**namespace 中的方法定义**

- `namespace_name::name`
- 在{}内，私有变量会在作用域中

### 关键词this

```c++
svoid Student::setName(string name){
	name = name; //huh?
	this-> name = name;
}
```

this被用来解决命名冲突

### 构造器

```c++
//student.cpp#include student.h
Student::Student(){
	age = 0;
	name = “”;
	state = “”;
} 

//overload
Student::Student(string name, int age, string state){
	this->name = name;
	this->age = age;
	this->state = state;
}
```

使用 initializer list 加速构造

```c++
#include student.h
Student::Student() : name{“”}, age{0}, state{“”} {}
Student::Student(string name, int age, string state) : name{name}, age{name},state{state} {}


```

### memory management

当我们分配了空间，当我们使用完之后就需要 delete

```c++
//int * is the type of an array variable
int *my_int_array;
//this is how you initialize an array
my_int_array = new int[10];
//this is how you index into an array
int one_element = my_int_array[0];
delete [] my_int_array;
```

同理在类中也需要解构，定义如下 `Class_name::~Class_name()`
没有人会主动调用他，知道超出作用域

```c++
//student.cpp#include student.h
Student::~Student(){
	delete [] my_array; // For illustrative purposes
}
```

## 模板

**语法**

```c++
//Example: 
Structstemplate<typename First, typename Second> struct MyPair {
	First first;
	Second second;
};

//Exactly Functionally the same!
template<typename One, typename Two> struct MyPair {
	One first;
	Two second;
};

template<typename First, typename Second> class MyPair {
public:
	First getFirst();
	Second getSecond();
	void setFirst(First f);
	void setSecond(Second f);
private:
	First first;
	Second second;
};

//mypair.cpp#include “mypair.h”
template<typename First, typename Second>//需要加上否则直接失败
First MyPair::getFirst(){
	return first;
}

template<typename Second, typename First>
Second MyPair::getSecond(){
	return second;
}

```

注意模板知道实例化才会插入代码。

![](https://s2.loli.net/2023/05/11/RX1jSdWVkMxcEPy.png)

![](https://s2.loli.net/2023/05/11/eivSnQR9ZkNu3Xh.png)

![](https://s2.loli.net/2023/05/11/GpzgCjXJPHLmZMA.png)

![](https://s2.loli.net/2023/05/11/8enN6rMJphyEdFA.png)

解决方案：让.h文件知道构造方法是什么。否则.h中没有构造

## template in classes

#cpp/class/template

### template function

```c++
//myPair.h
template<typename First, typename Second> class MyPair {
public:
	First getFirst();
	Second getSecond();
	void setFirst(First f);
	void setSecond(Second f);
private:
	First first;
	Second second;
};


//myPair.cpp
#include “mypair.h”
First MyPair::getFirst(){
	return first;
}
//Compile error! Must announce every member function is templated :/

template<class First, typename Second>
First MyPair::getFirst(){
return first;
}
//Compile error! The namespace of the class isn’t just MyPair

//this is a template function
template<class First, typename Second>
First MyPair<First, Second>::getFirst(){
return first;
}
// Fixed!

```

- class typename 是可以更换的

**member type**

```c++
std::vector<int> a = {1, 2};
std::vector<int>::iterator it = a.begin();
```

**syntax**

```c++
//vector.h
template<typename T> class vector {
public:
	using iterator = T*  // something internal like T*
	iterator begin();
}

//vector.cpp
//vector.cpp
template <typename T>
iterator vector<T>::begin() {...}
//compile error! Why?

//vector.cpp
template <typename T>
typename vector<T>::iterator vector<T>::begin() {...}
//iterator is a nested type in namespace vector<T>::
```

- 当我们在class中，使用using ，就像定义了一个嵌套类型 vector::iterator
- 在cpp文件中使用using，相当于别名

### 总结

- `vector<T>::iterator` 确保他在namespace中

- Before class specifier, use typename.

- Add template<class T1, T2..> before class definition in .h

- Add template<class T1, T2..>before all function

signatures in .cpp

- When returning nested types (like iterator types), put

typename ClassName<T1, T2..>::member_type as return
type, not just member_type

- Templates don’t emit code until instantiated, so #include

the .cpp file in the .h file, not the other way around!
44

## const

#cpp/const/var

## const -- var

```c++
std::vector<int> vec{1, 2, 3};
const std::vector<int> c_vec{7, 8};  // a const variable
std::vector<int>& ref = vec;         // a regular reference
const std::vector<int>& c_ref = vec;  // a const reference

vec.push_back(3);    // OKAY
c_vec.push_back(3);  // BAD - const
ref.push_back(3);   // OKAY
c_ref.push_back(3); // BAD - const
```

#cpp/const/class
## const -- class

```c++
//main.cpp
std::string stringify(const Student& s){
	return s.getName() + " is " + std::to_string(s.getAge()) + " years old." ;
}
//error compiler dont kone getxxx dont modify
```

- The compiler doesn’t know getName and getAge don’t modify s!
- We need to promise that it doesn’t by defining them as const functions
- Add const to the end of function signatures!

![](https://s2.loli.net/2023/05/14/rG7leZwvF2snJ9N.png)

- const object 只能使用const 函数

**例子**

```c++
class StrVector {
public:
    using iterator = std::string*;
    const size_t kInitialSize = 2;
    /*...*/
    size_t size() const;
    bool empty() const;
	void push_back(const std::string& elem);
	
	//should not be const !!!!!!!!!!!!!!!!
	std::string& at(size_t indx); // like vec[] but with error checking

    iterator begin();
	iterator end();

//example
// StrVector my_vec = {"sarah", "haven"};
std::string& elem_ref = my_vec.at(1); 
elem_ref = "Now I'm Different" ;
// my_vec = {"sarah", "Now I'm Different"}
//at returns a reference to an element in the vector. That element reference could be modified (thereby modifying the vector).
```

因此我们还需要添加一个方法 `const std::string& at(size_t indx) const;` 确保使用const object的时候可以使用at

注意在实现的时候不要直接复制粘贴，使用 `static_cast` 实现

```c++
std::string& StrVector::at(size_t index) {
   if (index >= size()) {
       throw std::out_of_range("Index out of range in at." );
   }
   return operator[](index); // operator[] =  return *(begin() + index)
}
const std::string& StrVector::at(size_t index) const {
   return static_cast<const std::string&>(const_cast<StrVector*>(this)->at(index));
}
```

- 这个奇妙的static_cast/cons_cast技巧允许我们重用非const版本来实现const版本

步骤：

- 将this 从const 转到 非const中
- 在非const方法中调用 at
- 将返回值从非const转化到const

### static_cast and const_cast

#cpp/static_cast  #cpp/const_cast

- `static_cast<new-type>( expression )`;
  - Used to convert from one type to another
  - Example: `int my_int = static_cast<int>(3.1);`
  - CANNOT BE USED WHEN conversion would cast away constness

- `const_cast<new-type>( expression );`
  - const_cast can be used to cast away (remove) constness
  - Allows you to make non-const pointer or reference to const-object

```c++
const int const_int = 3;
int& my_int = const_cast<int&>(const_int);
```

### beigin() end() 应该是const的吗？

#cpp/const/pointer #cpp/const/using

```c++
void printVec(const StrVector& vec){
	cout << "{ ";
	for(auto it = vec.begin(); it != vec.end(); ++it){
		*it = "dont mind me modifying a const vector :D";
	}
	cout << " }" << endl;
}
```

没有const，我们还是可以修改

```c++
class StrVector {
public:
    using iterator = std::string*;
    using const_ iterator = const std::string*;
    /*...*/
    size_t size() const;
    bool empty() const;
    /*...*/
    void push_back(const std::string& elem);
	const std::string& at(size_t indx) const;
	iterator begin();
	iterator end();
	const_iterator begin()const;
	const_iterator end()const;
/*...*/

//此时
for(auto it = vec.begin(); it != vec.end(); ++it){
	*it = “HELLO”; //compile error!
}
```

![](https://s2.loli.net/2023/05/14/ys2CaqMK4zHGjQ9.png)

<http://c.biancheng.net/view/218.html>

```c++
using iterator = std::string*;
using const_iterator =  const std::string*;

int*const p=&a; // const指针  指针指向不能变
const iterator it_c = vec.begin();  //string * const, const ptr to non-const obj
*it_c = "hi"; //OK! it_c is a const pointer to non-const object
it_c++; //not ok! can’t change where a const pointer points! 


const int* p=&a;//指针 指向 const值
const_iterator c_it = vec.begin();  //const string*, a non-const ptr to const obj
c_it++; // totally ok! The pointer itself is non-const
*c_it = "hi" // not ok! Can’t change underlying const object
cout << *c_it << endl; //allowed! Can always read a const object, just can't change

//const string * const, const ptr to const obj
const const_iterator c_it_c = vec.begin(); 
cout << c_it_c << " points to " << *c_it_c << endl; //only reads are allowed!
c_it_c++; //not ok! can’t change where a const pointer points!
*c_it_c = "hi" // not ok! Can’t change underlying const object
```

## 总结

![](https://s2.loli.net/2023/05/14/hS6ykwXGLIFvRcl.png)

![](https://s2.loli.net/2023/05/14/3ObGJ1tZSqrIcwY.png)

![](https://s2.loli.net/2023/05/14/BHVtM6RNcyDrQgP.png)

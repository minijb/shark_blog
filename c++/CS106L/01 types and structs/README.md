---
date created: 2023-04-20 22:29
date updated: 2023-04-20 22:35
---

c++ fundamental types

```c++
int val = 5; //32 bits  
char ch = 'F'; //8 bits (usually)  
float decimalVal1 = 5.0; //32 bits (usually)  
double decimalVal2 = 5.0; //64 bits (usually)  
bool bVal = true; //1 bit  
std::string str = "Sarah";
```

## struct

#cpp/变量/struct  [[5_基础类型和类#class and struct | struct in web]]

struct --- a group of variables

```c++
struct Student {  
	string name; // these are called fields  
	string state; // separate these by semicolons  
	int age;  
};  
Student s;  
s.name = "Sarah";  
s.state = "CA";  
s.age = 21; // use . to access fields

//简写
Student s = {"Sarah", "CA", 21};

```

^a636ce

## std::pair

#cpp/std/pair

- std::pair is a template
- The fields in std::pairs are named first and  second

```c++
std::pair<int, string> numSuffix = {1,"st"};  
cout << numSuffix.first << numSuffix.second;

struct Pair {  
	fill_in_type first;  
	fill_in_type second;  
};
```

使用pair 我们可以返回  success + result

```c++
std::pair<bool, Student> lookupStudent(string name) {   
	Student blank;  
	if (notFound(name)) return std::make_pair(false, blank);  
	Student result = getStudentWithName(name);  
	return std::make_pair(true, result);  
}  

std::pair<bool, Student> output = lookupStudent(“Julie”);
```

可以使用`make_pair`函数来避免指定类型
#cpp/std/make_pair

```c++
std::pair<bool, Student> lookupStudent(string name) {  
	Student blank;  
	
	if (notFound(name)) return std::make_pair(false, blank);//!!!!!!!!!!   
	Student result = getStudentWithName(name);  

	return std::make_pair(true, result); //!!!!!!!!!! 
}  
std::pair<bool, Student> output = lookupStudent(“Julie”);
```

## auto

#cpp/word/auto

自动推演

```c++
// What types are these?  
auto a = 3;  
auto b = 4.3;  
auto c = ‘X’;  
auto d = “Hello”;  
auto e = std::make_pair(3, “Hello”);
// std::pair<int, char*>
```

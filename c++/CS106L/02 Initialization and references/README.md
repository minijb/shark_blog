---
date created: 2023-04-25 14:54
date updated: 2023-04-25 21:14
---

#cpp/initialization

## initialization

在之前的struct代码中我们就已经进行了初始化
![[c++/CS106L/01 types and structs/README#^a636ce|code]]

其中进行了两次初始化

```c++
Student s; // initialization after we declare  
s.name = "Sarah";  
s.state = "CA";  
s.age = 21;  
//is the same as ...  
Student s = {"Sarah", "CA", 21};  
// initialization while we declare  
```

我们有很多初始化的方法如

```c++
std::pair<int, string> numSuffix1 = {1,"st"};  
std::pair<int, string> numSuffix2;  
numSuffix2.first = 2;  
numSuffix2.second = "nd";  
std::pair<int, string> numSuffix2 =  std::make_pair(3, "rd")
```

**uniform initialization**: 花括号初始化。 花括号初始化适用于所有初始化！！！

```c++
std::vector<int> vec{1,3,5};  
std::pair<int, string> numSuffix1{1,"st"};  
Student s{"Sarah", "CA", 21};  
// less common/nice for primitive types, but possible!  
int x{5};  
string f{"Sarah"};
```

对vector进行初始化的时候需要小心

```c++
std::vector<int> vec1(3,5);  
// makes {5, 5, 5}, not {3, 5}!  
// uses a std::initializer_list (more later)  
std::vector<int> vec2{3,5};  
// makes {3, 5}
```

## using auto

#cpp/word/auto

在编译的时候自动推断类型

```c++
auto a = 3; // int  
auto b = 4.3; // double  
auto c = ‘X’; // char  
auto d = “Hello”; // char* (a C string)  !!!!!
auto e = std::make_pair(3, “Hello”);  
// std::pair<int, char*>
```

需要特别注意的是，"xxx"会被推断为`char*`

### 什么时候用到auto

#### 过长的类型

```c++
std::pair<bool, std::pair<double, double>> result = quadratic(a, b, c);

auto result = quadratic(a, b, c);
```

#### Structured Binding结构化的绑定

结构化绑定使用可以直接从结构体里进行定义

**before**

```c++
auto p = std::make_pair(“s”, 5);  
string a = p.first;  
int b = p.second;
```

**after**

```c++
auto p = std::make_pair(“s”, 5);  
auto [a, b] = p;  
// a is string, b is int  
// auto [a, b] =  
std::make_pair(...);
```

## 引用

#cpp/引用

```c++
vector<int> original{1, 2};  
vector<int> copy = original;  
vector<int>& ref = original;  

original.push_back(3);  
copy.push_back(4);  
ref.push_back(5);  

cout << original << endl; // {1, 2, 3, 5}  
cout << copy << endl; // {1, 2, 4}  
cout << ref << endl; // {1, 2, 3, 5}
```

注意在使用= 时候就是做了一次copy！！！
我们可以使用`&`来避免

**典型bug**

```c++
void shift(vector<std::pair<int, int>>& nums) {  
	for (size_t i = 0; i < nums.size(); ++i) {  
		auto [num1, num2] = nums[i];  
		num1++;  
		num2++;  
	}  
}  
```

- `size_t`常被用于索引 --- unsigned，dynamically sized

问题：此处使用了copy，我们仅仅是修改了副本

```c++
void shift(vector<std::pair<int, int>>& nums) {  
	for (size_t i = 0; i < nums.size(); ++i) {  
		auto& [num1, num2] = nums[i];  
		num1++;  
		num2++;  
	}  
}  

//此处同理
void shift(vector<std::pair<int, int>>& nums) {  
	for (auto& [num1, num2]: nums) {  
		num1++;  
		num2++;  
	}  
}
```

## 左值右值

#cpp/左右值

- l-values 可以出现在左右两侧
  - 非临时值 --- not temporary
  - 有名称 --- have names
- r-values 只能出现在右边‘
  - 没有名称
  - 是临时的

```c++
int x = 3;  
int y = x;
// x --- l-value
// y --- r-value
```

**classic r-value error**

```c++
void shift(vector<std::pair<int, int>>& nums) {
  for (auto& [num1, num2]: nums) {
    num1++;
    num2++;
  }
}
shift({{1, 1}}); //error {{1,1}} is a right value 

auto my_nums = {{1,1}}
shift(my_nums)// it is ok
```

**我们只能给变量常见引用**

`int& item = 5 // error`

## const and const references

#cpp/const

**const : indicates a variable can not be chaneged**

const variable can be references or not

```c++
std::vector<int> vec{1, 2, 3};
const std::vector<int> c_vec{7, 8};  // a const variable
std::vector<int>& ref = vec;         // a regular reference
const std::vector<int>& c_ref = vec;  // a const reference
vec.push_back(3);    // OKAY
c_vec.push_back(3);  // BAD - const
ref.push_back(3);   // OKAY
c_ref.push_back(3); // BAD - const

const std::vector<int> c_vec{7, 8};  // a const variable
// BAD - can't declare non-const ref to const vector
std::vector<int>& bad_ref = c_vec;

// BAD - Can't declare a non-const reference as equal
// to a const reference!
std::vector<int>& ref = c_ref;
```

简单来说就是

- 普通值，可以使用const或者non-const引用
- const值，只能使用const引用
- 普通的引用不能赋值使用const引用

结合一下auto

```c++
std::vector<int> vec{1, 2, 3};
const std::vector<int> c_vec{7, 8};

std::vector<int>& ref = vec;
const std::vector<int>& c_ref = vec;

auto copy = c_ref;         // a non-const copy
const auto copy = c_ref;   // a const copy
auto& a_ref = ref;         // a non-const reference
const auto& c_aref = ref;  // a const reference
```

就是const不会自动添加到auto

### 使用引用的时机

- 我们需要更少的内存
- 需要别名
- 我们不需要修改一个很大的值，我们可以使用const 引用

我们可以返回一个引用, 也可以使用一个const引用

#cpp/parameter/const

```c++
// Note that the parameter must be a non-const reference to return  
// a non-const reference to one of its elements!  
int& front(std::vector<int>& vec) {  
	// assuming vec.size() > 0  
	return vec[0];  
}  

const int& front(std::vector<int>& vec) {  
	// assuming vec.size() > 0  
	return vec[0];  
}  
82

int main() {  
	std::vector<int> numbers{1, 2, 3};  
	front(numbers) = 4; // vec = {4, 2, 3}  
	return 0;  
}  

```
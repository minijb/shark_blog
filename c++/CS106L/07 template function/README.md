---
date created: 2023-05-18 10:04
---

#cpp/template/function

template function 使用模板，快速搭建多个方法。同时也是一种生成方法，就和模板类一样。

**可以设置默认类型**

```c++
template<typename type = int>
type myMin(type a, type b){
	return a < b ? a : b;
}
```

**使用模板方法**

```c++
myMin<int>(3,4);
```

我们也可以让编译器来推断类型

```c++
template<typename t , typename u>
auto myMin(t a, u b){
	return a < b ? a : b;
}

myMin(3.2, 4);
```


和模板类一样，模板方法只有在运行的时候才构建函数


### 约束

在c++20中我们可以显示模板中的类型

- 模板类
- 模板方法
- 模板类内的非模板方法

这些限制和需求被称为constraints --- 约束

一组约束被称为 concept --- 概念

```c++
template<typename T>
concept Addable = requires (T a , T b)
{
	 a+b; // 表达式a+b 在编译的时候是可以用的
};

template<typename T> requires Addable<T>//require可以出现在尾部
T add(T a , T b) {return a+b;}
```

## template metaprogramming

使用元模板，代码只在编译的时候运行一次。

**模板特定优化**

```c++
template<>
struct Factorial<0> {
	enum { value = 1};
}

template<unsigned n>
struct Factorial {
	enum {value = n*Factorial<n-1>::value}
}
```

此时在代码编译的时候就生成了函数，大大大加快了递归的速度。

### constexpr

- Constant expressions must be immediately initialized and will 
run at compile time!
- Passed arguments to constant expressions should be const/constant expressions as well. 


我们也可以使用constexpr来代替模板元编程

```c++
constexpr double fib(int n) { // function declared as constexpn
  if (n == 1) return 1;
  return fib(n-l) * n;
}
int main() {
  const long long bigval = fib(20);
  std: : cout << bigval << std::endl;
}

```
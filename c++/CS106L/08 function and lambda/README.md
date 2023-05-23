---
date created: 2023-05-22 14:04
---

```cpp
#include <iostream>
#include <string>

template <typename InputIt, typename DataType>
int count_occurrence(InputIt begin, InputIt end, DataType val) {
  int count = 0;
  for (auto iter = begin; iter != end; ++iter) {
    if (*iter == val)
      count++;
  }
  return count;
}

int main() {
  std::string str = "Xadia";

  std::cout << 
  count_occurrence(str.begin(), str.end(), 'a')
  << std::endl;
}
```

但是此时我们希望可以不仅仅找到单个字母，而是一个字符串 `Xadia` 。

那此时我们可以构建一个方法 `isVolew` 方法来适配不同的匹配方法

我们来预测以下这个方法

- 本方法返回一个布尔值
- 可以匹配不同的字符串

![](https://s2.loli.net/2023/05/22/CsAV8xkIm9fKoR2.png)

同时参数的数量可能不同。此时我们就想到了 template  但是有一个问题，当前这个名称应该是什么。

![](https://s2.loli.net/2023/05/22/CzJ2jrK9nXgDOpe.png)

这就是函数指针！！！

### funciton pointer

- 和其他指针一样
- 他们可以想 **参数** 一样传递给函数
- 他们也可以向函数一样被调用

此时单个参数可能有一些限制，我们可以让这个函数包含两个参数，我们将函数指针作为参数进行传递

![](https://s2.loli.net/2023/05/22/ujEh6emHytDUacf.png)

有的时候我们并不知道我们需要传递的函数是什么样的。

我们如何在不需要其他参数的情况下传递信息？

## lambda

Lambdas are **inline**, anonymous functions that can know about functions declared in their same scope!

```cpp
auto var = [capture-clause] (auto param) -> bool{
	....
}
```


![](https://s2.loli.net/2023/05/22/B9yoSbtgMlfeUkF.png)

```cpp
int limit = 5;
auto isMoraThan = [limit](int n) { return n > limit; };
```

我们可以**通过引用或者值传递**获得任何外部变量

![](https://s2.loli.net/2023/05/22/YKc5XJzHDb2awVm.png)

```cpp
#include <iostream>
#include <string>
#include <vector>

template <typename InputIt, typename UniPred>
int count_occurrence(InputIt begin, InputIt end, UniPred pred) {
  int count = 0;
  for (auto iter = begin; iter != end; ++iter) {
    if (pred(*iter))
      count++;
  }
  return count;
}

int main() {
  int limit = 5;
  auto isMoreThan = [limit](int n) { return n > limit; };
  std::vector<int> nums = {3, 5, 6, 7, 8, 9};

  std::cout << count_occurrence(nums.begin(), nums.end(), isMoreThan)
            << std::endl;
}

```

### 使用lambda的时机

- 需要一个简短的方法 或者 需要将 **本地变量传递到方法中**
- 如果您需要更多的逻辑或重载，请使用函数指针

### functor

A functor is any class that provides an implementation of operator().

![](https://s2.loli.net/2023/05/22/HDtbhx4JSL7zlTw.png)

- 它们可以创建自定义函数的闭包
- 只是函子的外皮

### 组合

STL有一个包罗万象的标准函数对象
`std::function<return_type(param_types)> func;`

任何lambda，函子，函数指针，都是转化为一个标准函数对象

> Much bigger and more expensive than a function pointer or lambda!

### 虚函数

在类中使用函数指针时要小心，特别是当您有另一个类的子类时

如果你有一个可以接受指向超类的指针的函数，它将不知道使用子类的函数

如果创建一个指向现有子类对象的超类指针，也会出现同样的问题。

为了解决这个问题，我们可以在头文件中将被覆盖的函数标记为virtual

虚函数是超类中的函数，我们希望稍后被重写。

![](https://s2.loli.net/2023/05/22/Pv3sKtOgGpyzLBd.png)

## 不要重复造轮子

使用 `#include <algorithm>` 里面有很多生成，以及模板类

![](https://s2.loli.net/2023/05/22/dpr45flzVBEPiWF.png)
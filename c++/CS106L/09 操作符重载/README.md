```cpp
//.h
bool operator < (const Student& rhs) const;

//.cpp
bool operator < (const Student& rhs) const{
	return age < rhs.age;
}
```

### 两种重载的方式

**成员函数**
- 在类中重载操作符
- 可以使用成员函数

**非成员函数** --- 首选
- 在类外定义
- 将左手和右手参数都定义在参数中

**原因**
- 可以使用非类对象
- 可以重载非我们自己定义的类，如vector

虽然选择了非类成员函数的操作符重载，但此时不能访问this !!!
也就是说我们不能访问私有成员变量

### 解决方案

friend关键字允许非成员函数或类访问另一个类中的私有信息

- 在类名/函数名前加入 `friend`

```cpp
friend class [name];


//student.hclass Student {
public:
/* ... */
	friend bool operator < (const Student& lhs, const Student& rhs) const;
private:
/* ... */
};
	bool operator < (const Student& lhs, const Student& rhs) {
		return lhs.age < rhs.age;
	}
```

```cpp
std::ostream& operator << (std::ostream& out, const Time& time) { 
	out << time.hours << ":" << time.minutes << ":" << time.seconds; 
	return out; 
}

```

### 小心

向new，delete 这些操作符并不需要特定的类型。

- 此时类外重载就是全局重载会影响所有东西

### 别过火

操作符重载有的时候很不明显，别到处用


### 规则

- 语义明确
- 逻辑要和当前操作符一样
- 当语义不明的时候，给一个其他名字的函数
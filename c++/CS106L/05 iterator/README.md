---
date created: 2023-05-10 14:42
---

- Iterators let you access all data in containers

programmatically!

- An iterator has a certain order; it “knows” what element will

come next
- Not necessarily the same each time you iterate!

## 迭代器种类

![](https://s2.loli.net/2023/05/10/cRXaEHGTNL4QfC7.png)

![](https://s2.loli.net/2023/05/10/1GQI64WMju8cHXl.png)

![](https://s2.loli.net/2023/05/10/1Z2nPzEkXCr73D4.png)

![](https://s2.loli.net/2023/05/10/3cYVAmD8b7ztMZK.png)

![](https://s2.loli.net/2023/05/10/uTiEYvHlwfCQ56s.png)

### 为什么使用++iter

++iter return after incremented

```c++
for( auto iter = set.begin(); iter != set.end() ; ++iter){
	const auto& elem = *iter;
}
```

如果是map可以使用 structed binding 

```c++
for( auto iter = map.begin(); iter != set.end() ; ++map){
	const auto& [key, value] = *iter;
}
```
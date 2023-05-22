---
date created: 2023-05-09 23:28
---

#cpp/vector

![](https://s2.loli.net/2023/05/09/4iBJCqolVZadrcI.png)

#cpp/set

![](https://s2.loli.net/2023/05/09/8GBmZ5HrlwSbxug.png)

#cpp/map

![](https://s2.loli.net/2023/05/09/RJmbqScZUfynXiz.png)

container 的两种类型

**Sequence**
- 可以顺序查找
- 顺序存储

**Asscoiative**

- 无需顺序存储
- 查找方便
- map , set

### 顺序存储的选择

![](https://s2.loli.net/2023/05/09/1Vchi8wsmZ4jvpf.png)


**总结**

- 需要顺序的时候
- 最常用的：vector
- 希望在头部快速插入 --- deque  ---- 双端队列
- 组合/使用多个列表 ---- list  ---- 链表 --- 罕见

### 组合存储

maps 和 sets 在stl中都是unorder

- 顺序map/set 需要可比较的运算符
- 无序map/set 需要hash方法

> 无序map/set 通常比 顺序map/set 快

- Unordered containers are faster, but can be difficult 
to get to work with nested containers/collections
- If using complicated data types/unfamiliar with hash 
functions, use an ordered container

## 容器适配器

现有容器的包装器

- 包装器修改接口以对容器进行排序，并更改客户端可以做什么/如何与容器交互。

![](https://s2.loli.net/2023/05/09/fONDPd6xGtQIrCA.png)
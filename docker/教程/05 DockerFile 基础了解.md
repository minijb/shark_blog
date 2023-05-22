---
date created: 2023-05-02 15:59
date updated: 2023-05-02 16:32
---

#docker/dockerfile

用来构建docker镜像的构建文件

```sh

#dockerfile
FROM centos

VOLUME [ "volume01", "volume02" ]

CMD echo "----  end ----"
CMD /bin/bash


#命令
docker build  -f dockerfile -t zhouhao/centos:1.0 .
```

注意：最后有一个点

- 此处我们创建了两个卷，这里没有添加名称，因此就是匿名卷。

## 数据卷容器

多个mysql同步数据
![](https://s2.loli.net/2023/05/02/SDnIh5H39xoNcks.png)

```sh
docker run -dit --name centos01 zhouhao/centos:1.0
docker run -dit --name centos02 --volumes-from centos01 zhouhao/centos:1.0
```

此时两个卷内的内容都是共享的

 >容器删除，卷内容不会删除
 
 
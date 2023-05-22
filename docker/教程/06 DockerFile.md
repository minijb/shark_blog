---
date created: 2023-05-02 19:47
date updated: 2023-05-02 21:33
---

#docker/dockerfile

构建步骤

- 编写dockerfile
- `docker build`
- `docker run`
- `docker push` 发布镜像

**基础**

- `#`注释
- 命令一般大写

## 指令

```sh
FROM #指定基础镜像

#MAINTAINER #维护人信息：姓名+邮
LABEL #代替MAINTAINER

RUN    #运行时的命令
ADD     #步骤+添加基本的环境如tomcat压缩包
WORKDIR     #当前工作目录
VOLUME    #挂载的容器卷
EXPOSE    #指定暴露端口

CMD  #指定容器启动的时候运行的命令 ---- 最后一个生效，可被替代
ENTERYPOINT   #指定容器启动的时候运行的命令 ---- 可以追加

# 如 run xxx -a 
# CMD  -a会直接替代CMD
# ENTERYPOINT 会在 -a后面追加

ONBUILD   #当构建一个被继承的 DOCKERFILE 此时就会运行这个命令， 触发命令
COPY      #类似ADD --- 将文件拷贝到镜像中
ENV       #设置环境变量
```

## 实战

编写文件

```sh
FROM ubuntu

ENV MYPATH /user/local
WORKDIR $MYPATH

RUN apt update
RUN apt upgrade -y
RUN apt install -y neovim 
RUN apt install -y net-tools 

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```

**构建镜像**

```sh
docker build -f dockerfile -t myubuntu .
```

- 启动镜像 --- 通过卷快速部署项目
- 访问测试
- 发布项目

## 实战 --- tomcat

```dockerfile
FROM centos:centos7
MAINTAINER wwater<2773147697@qq.com>

COPY readme.txt /opt/tomcat/readme.txt

ADD jdk-8u221-linux-x64(1).rpm /opt/tomcat/
ADD apache-tomcat-9.0.64.tar.gz /opt/tomcat/

RUN yum install -y vim

ENV MYPATH /usr/local

WORKDIR $MYPATH

ENV JAVA_HOME /opt/tomcat/jdk1.8.0_11
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /opt/tomcat/apache-tomcat-9.0.64
ENV CATALINE_BASH /opt/tomcat/apache-tomcat-9.0.64
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$

EXPOSE 8080

CMD  /opt/tomcat/apache-tomcat-9.0.64/bin/startup.sh && tail -F  /opt/tomcat/apache-tomcat-9.0.64/bin/logs/catalina.out
```

**制造image**

```sh
docker build -t diytomcat .
```

**启动镜像**

此时我们可以直接通过映射本地文件的方式，将项目发布到容器东

```sh
docker run -d -p 3344:8080 --name xiaofantomcat1 \
-v /home/xiaofan/build/tomcat/test:/usr/local/apache-tomcat-9.0.37/webapps/test \
-v /home/xiaofan/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.37/logs \
diytomcat
```

此处将webapp 以及log文件镜像映射

- 我们在本地修改test文件，并映射到容器中的test
- 同时我们可以将容器中的log文件映射到本地

## 发布镜像

#docker/push

网站：
<https://hub.docker.com/>

- 登录

```sh
docker login -u {name} -p {passwd}
```

- push

```sh
docker push {name}:{tag}
```

## 小结

![](https://s2.loli.net/2023/05/02/f6QORgdtqBMZl5N.png)

![](https://s2.loli.net/2023/05/01/cJu94LAeBYqHsDE.png)

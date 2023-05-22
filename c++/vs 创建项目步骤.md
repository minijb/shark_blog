
**1. 创建空项目**

**2. vs项目结构设置**

设置项目的 源文件目录、输出目录、中间目录

- 新建src文件夹
- 设置项目属性
"配置属性"--->常规

输出目录为 `$(SolutionDir)\bin\$(Platform)\$(Configuration)\`
中间目录 `$(SolutionDir)\bin\intermediates\$(Platform)\$(Configuration)\`

**3. 设置include目录**

include 目录所有位置为项目根目录

- 新建include目录
- 在项目属性中添加头文件目录
- 将需要的头文件添加到include目录中

在c/c++中的常规，添加附加包含目录 --- include 

**4. 链接库文件**

- 新建文件夹lib
- 在常规---附加库目录中添加我们之前的链接文件夹---在input中添加需要的库文件(不需要完整目录)
---
date created: 2023-05-28 10:48
date updated: 2023-05-28 21:45
---

### 环境变量

启动shell设置的东西

包含：

- home directory
- username
- path variable

```sh
echo $PATH

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib:/mnt/c/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.7/bin:/mnt/c/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.7/libnvvp:/mnt/c/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.1/bin:/mnt/c/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v12.1/libnvvp:/mnt/c/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.3/bin:/mnt/c/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v11.3/libnvvp:/mnt/c/Python311/Scripts/:/mnt/c/Python311/:/mnt/c/windows/system32:/mnt/c/windows:/mnt/c/windows/System32/Wbem:/mnt/c/windows/System32/WindowsPowerShell/v1.0/:/mnt/c/windows/System32/OpenSSH/:/mnt/c/Program Files (x86)/NVIDIA Corporation/PhysX/Common:/mnt/c/Program Files/NVIDIA Corporation/NVIDIA NvDLISR:/mnt/c/WINDOWS/system32:/mnt/c/WINDOWS:/mnt/c/WINDOWS/System32/Wbem:/mnt/c/WINDOWS/System32/WindowsPowerShell/v1.0/:/mnt/c/WINDOWS/System32/OpenSSH/:/mnt/c/Program Files/Bandizip/:/mnt/c/Program Files/Git/cmd:/mnt/c/Program Files/nodejs/:/mnt/c/ProgramData/chocolatey/bin:/mnt/c/Program Files/nodejs:/mnt/d/environment/maven/apache-maven-3.9.0/bin:/mnt/c/MinGW/bin:/mnt/c/Program Files/NVIDIA Corporation/Nsight Compute 2022.2.1/:/mnt/c/Program Files/CMake/bin:/mnt/c/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.35.32215/bin/Hostx64/x64:/mnt/e/software/nvim-win64/bin:/mnt/c/Program Files/Docker/Docker/resources/bin:/mnt/c/Program Files/dotnet/:/mnt/c/Users/14143/anaconda3:/mnt/c/Users/14143/anaconda3/Library/mingw-w64/bin:/mnt/c/Users/14143/anaconda3/Library/usr/bin:/mnt/c/Users/14143/anaconda3/Library/bin:/mnt/c/Users/14143/anaconda3/Scripts:/mnt/c/Users/14143/AppData/Local/Microsoft/WindowsApps:/mnt/c/Users/14143/AppData/Local/Programs/Microsoft VS Code/bin:/mnt/c/Users/14143/AppData/Roaming/npm:/mnt/c/Users/14143/.dotnet/tools
```

当我们使用命令的时候我们会查找这些目录

如果我们想要知道这个命令具体在哪个文件夹, 可以使用 `which`

```sh
which {xxx}
```

常用的命令

- `pwd` 查看当前错处的目录
- `ls` 产看当前目录下的文件
- `cd -` 返回之前的目录
- `ctrl - l` 清空屏幕
- `#` 使用root权限
- `tee` T形管道  输出的屏幕 并 输出到文件/程序

```sh
# echo 1060 | sudo tee brightness
```

- `find` 查找文件
- `xdg-open` 以适合的程序打开文件

**权限**

对于文件 - 可读，可写，可运行
对于文件夹

- 是否被允许看到文件夹内的文件
- 是否允许在目录中重命名，创建或者修改文件
- 是否可以访问/进入目录

### 重定向

`<` 将文件重定向到程序的输入
`>` 将输出重定向到文件

```sh
echo hello > hello.txt
```

`>>` 追加而不是覆盖

### 管道

`|` 将一个程序的输出 作为 另一个程序的输入

## shell 语法

创建变量

```sh
foo=bar
#注意不能有空格
```

其中一旦有空格就不被当作 **命令**

单引号和双引号的区别

- `" "` 中可以使用变量进行替换
- `' '` 不能进行变量替换

```sh
➜  sh echo "Value is $foo"
Value is var
➜  sh echo 'Value is $foo'
Value is $foo
```

### 函数

```sh
mcd() {
  mkdir -p "$1"
  cd "$1"
}
```

其中 `$1` 就是相当于命令行输入的参数，1代表第一个 , 2-9同理
`$?` 获得上一个命令的错误信息，`$_`上一个命令的最后一个参数

此时我们运行

```sh
source xxx.sh
mcd test
```

就会自动创建并进入test

在命令上套上括号 和 <

```sh
cat < (ls) < (ls ..)

test
test.sh
c++
cs106L
cs106L-assignment1
sh
```

这样可以拼接输出

### 例子

```sh
#!/bin/bash

echo "Starting program at $(date)"

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
	grep foobar "$file" >/dev/null 2>/dev/null

	if [[ "$?" -ne 0 ]]; then
		echo "File $file does not have any foobar , add one"
		echo "# foobar" >>"$file"
	fi
done

```

- `$0` 运行的脚本的名字

- `$#` 参数数量

- `$$` 当前PID

- `$@` 所有参数的列表

- `2>` 重定向错误的流

- 普通的数值比较 `-ne -eq`

- 文件是否存在 `test -b/-f ....`

### 通配符

常见的 ` * ?  `

`{}` 扩展字符串，按下tab后会自动扩展字符串  `image.{jpg,png} ===> image.jpg image.png`

- `,` 添加单独选项
- `..` 添加连续选项

```sh
ls {foo,bar}/{a..f}

ls foo/a foo/b foo/c foo/d foo/e foo/f bar/a bar/b bar/c bar/d bar/e bar/f
```

有的时候全部使用shell脚本可以能不是最好的选择，我们可以配合其他语言脚本来补足

我们有如下的py脚本

```python
#!/usr/local/bin/python
import sys

for arg in reversed(sys.argv[1:]):
    print(arg)

```

其中第一行是让shell知道如何运行这个脚本,有了这一行我们就可以直接`./xxxx`进行运行

```sh
➜  sh ./script.py a b c
c
b
a
```

有的我们可能不知道具体的运行环境，我们可以使用env命令来所有python

```py
#!/usr/bin/env python
```

## 静态检查 shellcheck

```sh
apt-get install shellcheck

shellcheck xxxx
```

### 查找命令用法

tldr

```sh
apt install tldr
```

### 查找文件

```sh
find . -name xxx -type d
```

find有很多好用的参数

- `-path` 查找特定路径的文件
- `-mtime 1`  1天内修改的文件
- `-exec rm {} \;` 执行命令查找到的文件会填充到 `{}` 中

**其他好用的命令**

- `fd`
- `locate` 使用数据库来提高查早的速度，
  - `updatedb` 来更新数据库
- `grep` 在文件内查找指定的内容
  - `-R` 遍历整个目录
- `rg` grep的增强版

### 历史

```sh
history
```

### fzf

查找工具

```sh
history 1 | fzf
```

### tree broot 查看目录结构

```sh
sudo apt install tree
```

### 文件管理器 nnn

```sh
apt install nnn
```
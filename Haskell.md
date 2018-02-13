Haskell是一个惰性的（lazy）、纯（pure）函数式程序设计语言。

# 开发环境

## Stack（推荐）

Stack是一个跨平台的Haskell开发工具。它有如下特性：

- 自动安装GHC，并且允许安装多个不同版本的GHC；
- 自动安装项目依赖；
- 构建项目；
- 测试项目；
- 等等。

Linux/Unix下的脚本安装：

```bash
curl -sSL https://get.haskellstack.org/ | sh
或者
wget -qO- https://get.haskellstack.org/ | sh
```

Linux/Unix下的手动安装：

1. 下载最新的软件包；

2. 将下载的软件包解压到`~/.local/bin/`下；

3. 设置环境变量：

   ```bash
   echo 'export PATH=~/.local/bin:$PATH' >> ~/.bashrc
   ```

4. 安装依赖：

   ```bash
   sudo apt install g++ gcc libc6-dev libffi-dev libgmp-dev zlib1g-dev make xz-utils git gnupg
   ```

由于国内网络问题，在使用Stack时，可以使用TUNA镜像站来加速开发。

在 Stack 的全局配置文件（Linux/OSX 下默认为`~/.stack/config.yaml`，Windows下默认为`%APPDATA%\stack\config.yaml`）里这样写即可：

```yaml
package-indices:
- name: Tsinghua
  download-prefix: https://mirrors.tuna.tsinghua.edu.cn/hackage/package/
  http: https://mirrors.tuna.tsinghua.edu.cn/hackage/00-index.tar.gz
setup-info: "http://mirrors.tuna.tsinghua.edu.cn/stackage/stack-setup.yaml"
urls:
  latest-snapshot: http://mirrors.tuna.tsinghua.edu.cn/stackage/snapshots.json
  lts-build-plans: http://mirrors.tuna.tsinghua.edu.cn/stackage/lts-haskell/
  nightly-build-plans: http://mirrors.tuna.tsinghua.edu.cn/stackage/stackage-nightly/
```

这样，stack 工具下载 Hackage 包、Stackage snapshot 配置文件、ghc 安装包的位置都在 TUNA 镜像站，不需要像之前一样走 github/s3。

### Haskell IDE——VSCode + Haskero + Stack

Haskero是VSCode的一个Haskell插件。

在VSCode中安装完成Haskero后，还要在项目根目录上执行下列命令，安装intero：

```bash
$ stack build intero
```

intero是Haskero的一个后端，它用于抽取Haskell的类型信息和位置等。

## Haskell Platform

### ghc.exe

`ghc.exe`是一个Haskell的优化编译器。

#### 编译并链接

命令格式：`ghc 脚本 依赖`

`依赖`可以是脚本（.hs），也可以是目标文件（.o）。
执行结果将产生三个文件：

- `main.exe`或`main`
- `脚本名.o`
- `脚本名.hi`

`.hi`是一个接口文件，它存储着模块的导出信息，二进制格式。
`.o`是目标文件，它包含编译产生的机器码。



命令格式：`ghc --make 脚本 依赖`

执行结果将产生三个文件：

- `脚本名.exe`或`脚本名`
- `脚本名.o`
- `脚本名.hi`

#### 自己指定可执行文件名

命令格式：`ghc -o 可执行文件 脚本 依赖` 

执行结果将产生三个文件：

- `可执行文件`
- `脚本名.o`
- `脚本名.hi`

#### 编译但不链接

命令格式：`ghc -c 脚本 依赖` 

执行结果将产生两个文件：

- `脚本名.o`
- `脚本名.hi`

前面三种方式的脚本中**必须包含`main`函数**，这样才能产生可执行文件，而这种方式适用任何模块源文件的编译，源文件中可以不包含`main`函数。

#### 使用扩展库

命令格式：`ghc -package 扩展库名 …`

例如：当前Haskell已经将Parsec包安装到lib/extralibs目录下，要使用Parsec包可以使用下面命令：

```bash
$ ghc -package parsec simple.hs
```

#### 使用第三方库

例如：假设已经手工将最新版本的Parsec编译到“c:\parsec”目录下（通常要编译Haskell库，只需要在命令行下在库所在的目录下执行make命令即可，因为Haskell库通常都带makefile），则可以使用下面命令进行编译：

```bash
$ ghc -c myfile -ic:\parsec
```

`-i`参数来告诉ghc导入路径，即告诉ghc在哪里能找到所需的接口文件（.hi）。
如果要链接，则需要使用`-L`告诉GHC在哪里找到库，使用`-l`告诉GHC链接什么库：

```bash
$ ghc -o myprogram myfile1.o myfile2.o -Lc:\parsec -lparsec
```
#### 自动计算依赖

假如A.hs中导入了B.hs中的某些变量，则要编译A.hs，只要：

`ghc-c A.hs`

就可以了，不需要先编译B.hs，然后再：`ghc -c A.hs B.o`。因为GHC会自动计算依赖。

### ghci.exe、WinGHCi.exe

交互式的解释器和调试器。

### runghc.exe

`runghc.exe`可以直接运行Haskell源文件，而不需要事先编译。

# 入门

## 使用REPL

REPL是一个交互式编程环境。要启动REPL可以运行下列命令：

```bash
$ ghci
或者
$ stack ghci
```

### 全局变量`it`

GHC的命令行中有一个全局变量`it`，用于保存上一次计算的结果：

```bash
ghci> it
"foobar"
it :: [Char]

ghci> it ++ 3
<interactive>:1:6:
…

ghci> it
"foobar"
it :: [Char]

ghci> it ++ "baz"
"foobarbaz"
it :: [Char]
```

> 当求值失败时，不改变当前it的值。

### 定义变量

在交互模式下（ghci），定义一个变量与在源文件（.hs）中不一样，要在变量之前加一个`let`：

```bash
ghci> let lostNumbers = [4,8,15,16,23,48]  
ghci> lostNumbers  
[4,8,15,16,23,48]
```

这里的`let`与Haskell中`let…in`中的`let`没关系。也可以理解为，这里的`let`结构的`in`部分被省略，则默认为整个交互环境。

### 常用命令

#### 加载Haskell脚本——`:load`或`:l`

命令格式：

```bash
:load 脚本名
:load "脚本名"
:load  ——表示消除已经加载的脚本（“prelude.hs”除外）。
```

`脚本名`可以不包括扩展名`.hs`，但可以包括路径。路径中的分隔符最好用`/`。

例子：

```bash
ghci> :l add.hs
```

#### 重载Haskell脚本——`:reload`或`:r`

#### 编辑文件——`:edit`或`:e`

命令格式：

```bash
:edit 文件名
:edit  ——打开上次使用edit命令打开的文件。
```

用默认编辑器打开指定任意类型文件，`文件名`必须包含扩展名。并且，如果编辑的是Haskell脚本，则在编辑器被关闭后，将自动重载该脚本。

#### 获取表达式的类型信息——`:type`或`:t`

例子：

```bash
ghci> 3 + 2
5
ghci> :t it
it :: Integer
ghci> :t 3 + 2
3 + 2 :: (Num t) => t
```

> 注意：`:type`并不计算表达式的值，只是输出它的类型信息。

#### 获取详细信息——`:info`或`:i`

例子：

```bash
ghci> :i (+)
```

#### 查找——`:find`或`:f`

#### 改变工作路径——`:cd`

#### 导入或移除模块——`:module`或`:m`

例子：

```bash
ghci> :m + Data.Ratio  ——导入“Data.Ratio”模块
ghci> :m - Data.Ratio  ——移除“Data.Ratio”模块
```

#### 设置系统选项——`:set`或`:s`、`:unset`

例如：GHCi提示符将显示当前顶层模块名，可以使用“:set”命令改变默认的显示：

```bash
:s prompt "ghci>"
```

该命令将默认的命令提示符改成“ghci>”。

如果要在GHCi输出计算结果的后面跟上它的类型信息，则需要：

```bash
ghci> :s +t
ghci> 'c'
'c'
it :: Char
```



#### 获取帮助——`:?`

#### 退出交互模式——`:quit`或`:q`

### 开启扩展功能

在交互模式下，默认没有开启扩展功能，要开启它们，要加“-fglasgow-exts”命令行参数。

## 使用Stack

### 创建项目

使用模板新建一个项目：

```bash
$ stack new my-project -p "category:office"  ——使用默认模板创建项目
$ stack new my-project new-template -p "category:office"  ——使用“new-template”模板创建项目
```

可以通过命令：`stack templates`来查看所有可用的模板。

安装编译工具：

```bash
$ cd my-project
$ stack setup
```

它会根据需要安装编译工具，默认安装在`~/.stack`目录下。

可以通过命令：`stack path`来查看Stack的各种路径设置。

### 编写代码

`hello.hs`：

```haskell
main = putStrLn "Hello, World!"
```

#### Haskell脚本

Haskell的源文件也称脚本有两种扩展名：

- 以`.hs`为后缀的脚本：它是传统的脚本风格。注释用`--`或`{- … -}`表示，其它的部分表示程序文本。
- 以`.lhs`为后缀的脚本。
  - Bird-scripts风格：每一以`>`开始的行表示程序文本，其它部分表示注释内容。注意：代码与注释之间必须有至少一个空行分隔。
  - LaTEX风格：所有被`\begin{code}`与 `\end{code}`包围的部分为代码部分，除此以外为注释部分。注意：代码与注释可以不需要有空行分隔。它特别适合使用LaTeX的文本处理系统。

#### 编码风格

Haskell的代码既支持*自由编码风格*，也支持利用缩进来表示代码结构的*缩进编码风格*。

```haskell
--自由编码风格
foo = let {a = 1; b = 2;
       c = 3} in a + b + c
       
--缩进编码风格
foo = let a = 1
          b = 2
          c = 3
      in a + b + c
```

> 缩进规则：
>
> 在源文件的开始，第一个顶层声明或定义可以从任意一列开始，Haskell编译器或解释器将记住此缩进位置。后续的所有顶层声明必须缩进到同样位置。
> 后续如果是空白行或者缩进更加靠右，则被当作当前项的延续。如果缩进与前一个项开始的位置相同，会被当作在同一个块中开始一个新的项。 
> 缩进时，尽量使用空格，而不要使用`Tab`键。

#### `Main`模块

Haskell的代码总是定义在一个模块中。上面的代码没有显式定义在一个模块中，则默认为`Main`模块。

`Main`模块的`main`是程序的入口。如果模块不需要独立运行（例如作为库），则`main`不是必须的。

### 构建项目

```bash
$ stack build
```

这将生成：

- 库文件（位于`my-project/.stack-work/install/i386-linux/lts-10.5/8.2.2/lib/i386-linux-ghc-8.2.2/`）
- 一个可执行文件（位于`my-project/.stack-work/install/i386-linux/lts-10.5/8.2.2/bin/hello-exe`）。

### 运行程序

```bash
$ stack exec hello-exe
```

### 测试

```bash
$ stack test
```



# 类型

# 声明

## 命名空间

Haskell有六种命名空间：

- 变量（variable）：表示值。
- （值）构造器（constructor）：表示值。
- 类型变量（type variable）：表示类型。
- 类型构造器（type constructor）：表示类型。
- 类型类（type class）：类似于接口。
- 模块名（module name）：表示模块。

变量和类型变量必须以小写字母或下划线开头，其他四种名称必须以大写字母开头。
在同一个作用域中，类型构造器与类型类不能同名。

# 表达式

# 类型类

# 模式

# 模块


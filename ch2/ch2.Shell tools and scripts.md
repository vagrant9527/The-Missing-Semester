## 一、内容概览
## 二、
### 1.shell脚本
上一节课介绍了一些shell常用的命令以及利用pipe和重定向将命令组合使用，但是在很多情况下我们需要执行一系列的操作，因此shell也提供**变量赋值、控制流（条件、循环）、定义函数**等功能。
接下来以bash为例进行介绍。
#### 1）变量赋值
```bash
foo=bar  #变量赋值
echo "$foo" #用$变量名的方式来访问变量中的数值
>> bar
```
注：赋值这边注意不要多打空格，会报语法错误，因为在shell中空格默认用来分隔变量
```bash
echo "this world is $foo"
>>this world is bar

echo 'this world is $foo'
>>this world is $foo          #双引号会展开变量，而单引号则不会展开.
```
注：shell提供一种叫引用的机制来控制展开
双引号：如果你把文本放在双引号中， shell 使用的特殊字符，都失去它们的特殊含义，被当作普通字符来看待。 有几个例外： $，\ (反斜杠），和 `（倒引号）。这意味着单词分割、路径名展开、 波浪线展开和花括号展开都将失效，然而参数展开、算术展开和命令替换 仍然执行。
单引号：禁止所有展开
#### 2）控制流
和其他大多数的编程语言一样，bash也支持if, case, while 和 for 这些控制流关键字
#### 3）函数
bash支持定义函数，并传递参数
```bash
mcd () {
    mkdir -p "$1"
    cd "$1"
}
# $1表示脚本的第一个参数

soucre mcd.sh
mcd test  #执行结果，创建test文件夹并进入
```
注：bash中有许多关$的保留字，来表示错误参数、错误代码和相关变量

| 符号 | 含义 |
| --- | --- |
| $0 | 运行的脚本文件名 |
| $1~$9 | 脚本的参数，$1是第一个 |
| $? | 前一个命令的返回值 |
| $_ | 上一个命令的最后一个参数，如果你正在使用的是交互式 shell，你可以通过按下 Esc 之后键入 . 来获取这个值。 |
| $# | 给定的参数个数 |
| $$ | 当前脚本的进程码PID |
| $@ | 展开所有参数 |
| !! | 完整的上一条命令包括参数。常见用法：权限不足执行失败的时候，使用sudo !! 再执行一次 |

```bash
示例1：
mkdir test
cd $_
示例2：
grep foobar mcd.sh
echo $?
>> 1   #grep命令执行完虽然屏幕上没有任何输出，但是返回码不是0.
       #也说明存在两个流，标准输出流和标准错误流
```
#### 4)标准错误流
命令通常使用 STDOUT来返回输出值，使用STDERR 来返回错误及错误码，便于脚本以更加友好的方式报告错误。返回码或退出状态是脚本/命令之间交流执行状态的方式。返回值0表示正常执行，其他所有非0的返回值都表示有错误发生。
注：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/33626411/1674721297131-7660ddfe-c765-4a76-966c-8be60b4c75eb.png#averageHue=%23f5f3f1&clientId=u2a58e1f2-1fef-4&from=paste&height=180&id=u2b3a2977&originHeight=270&originWidth=1060&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64574&status=done&style=none&taskId=u86ab1f7a-53d7-4d7c-b1de-f73c321f999&title=&width=706.6666666666666)
#### 5）短路运算符
退出码可以搭配 &&（与操作符）和 ||（或操作符）使用，用来进行条件判断，决定是否执行其他程序。

- &&：逻辑与，当用此连接符连接多个命令时，前面的命令执行成功，才会执行后面的命令，前面的命令执行失败，后面的命令不会执行
- ||：逻辑或，当用此连接符连接多个命令时，前面的命令执行成功，则后面的命令不会执行。前面的命令执行失败，后面的命令才会执行
- 用；连接：没有任何逻辑关系的连接符。当多个命令用分号连接时，各命令之间的执行成功与否彼此没有任何影响，都会一条一条执行下去
- true返回码永远为0，false返回码永远为1
#### 6）命令替换
命令的输出也可以保存在变量中。
命令替换允许我们把一个命令的输出作为另一个命令的一部分来使用：
```bash
示例1：
foo=$(pwd)
echo $foo
>>/home/test
示例2：
echo "we are in $(pwd)"
>>we are in /home/test     #注意这里要用双引号，不然变量无法展开

```
#### 7）进程替换
<( CMD ) 会执行 CMD 并将结果输出到一个临时文件中，并将 <( CMD ) 替换成临时文件名。这在我们希望返回值通过文件而不是STDIN传递时很有用。
```bash
diff <(ls foo) <(ls bar) #显示foo和bar文件夹中文件的区别
cat <(ls) <(ls..)        #连接该目录文件和父目录文件
```
#### 8）标准错误重定向
```bash
#!/bin/bash

echo "Starting program at $(date)" # date会被替换成日期和时间

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # 如果模式没有找到，则grep退出状态为 1
    # 我们将标准输出流和标准错误流重定向到Null，因为我们并不关心这些信息
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done

```

- bash将`2>`用作标准错误重定向（**标准错误输出重定向没有专用的重定向操作符。为了重定向标准错误输出，我们必须用到其文件描述符。** 一个程序的输出会流入到几个带编号的文件中。这些文件的前三个称作标准输入、标准输出和标准错误输出，shell 内部分别将其称为文件描述符0、1和2。shell 使用文件描述符提供 了一种表示法来重定向文件。因为标准错误输出和文件描述符2一样，我们用这种表示法来重定向标准错误输出）
- /dev/null  ：处理不需要的输出（我们不想要一个命令的输出结果，只想把它们扔掉。这种情况 尤其适用于错误和状态信息。具体做法是重定向输出结果到一个叫做”/dev/null”的特殊文件。这个文件是系统设备，叫做数字存储桶，它可以 接受输入，并且对输入不做任何处理。为了丢掉命令错误信息）
- **重定向标准输出和错误到同一个文件：**
```bash
二次定向：
ls -l /bin/usr > ls-output.txt 2>&1 
#首先重定向标准输出到文件 ls-output.txt，
#然后重定向文件描述符2（标准错误输出）到文件描述符1（标准输出）使用表示法2>&1。
#注意这种方法的顺序不能反。不然会重定向至屏幕

&>
ls -l /bin/usr &> ls-output.txt
```
#### 9)比较运算符
上面的例子中有个`ne`用来表示不等于（not equal），这是比较运算符。其他更多的可以查看`man test`。
在bash中进行比较时，尽量使用双方括号 [[ ]] 而不是单方括号 [ ]，这样会降低犯错的几率，尽管这样并不能兼容 sh。更详细的说明参考课件。
#### 10）通配符

- 使用 ? 和 * 来匹配一个或任意个字符。
- 花括号{} - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令
```bash
示例1：当前目录下有foo，foo1,foo2,foo10,bar
rm foo? #删除foo1和foo2
rm foo* #删除除了bar之外的文件

示例2：
echo Number_{1..5}
Number_1  Number_2  Number_3  Number_4  Number_5

做嵌套
echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b

示例3：结合通配符
mv *{.py,.sh} folder #会移动所有 *.py 和 *.sh 文件
```
#### 11）首行解释器--shebang
shebang来源于#！(# sharp ! bang)，shell能够通过shebang了解如何运行这个程序
如：脚本并不一定只有用 bash 写才能在终端里调用，比如下面这个python脚本，内核知道去用 python 解释器而不是 shell 命令来运行这段脚本，是因为脚本的开头第一行的shebang。
```bash
#!/usr/local/bin/python
import sys                        # python默认是不与bash交互的，需要导入一些库，如sys
for arg in reversed(sys.argv[1:]):
    print(arg)

#!/usr/bin/env python   #shebang也可以这样写，env会利用PATH这个变量进行定位，这样能够提高程序的可移植性
```
#### 12）shell函数和脚本的不同点

- 函数只能与shell使用相同的语言，脚本可以使用任意语言。因此在脚本中包含 shebang 是很重要的。
- 函数仅在定义时被加载，脚本会在每次被执行时加载。这让函数的加载比脚本略快一些，但每次修改函数定义，都要重新加载一次。
- 函数会在当前的shell环境中执行，脚本会在单独的进程中执行。因此，函数可以对环境变量进行更改，比如改变当前工作目录，脚本则不行。脚本需要使用 `export`将环境变量导出，并将值传递给环境变量。
- 与其他程序语言一样，函数可以提高代码模块性、代码复用性并创建清晰性的结构。shell脚本中往往也会包含它们自己的函数定义。
### 2.shell工具
#### 1）shellcheck -->调试shell脚本
#### 2）查看命令如何使用

- 对应命令行添加`-h,--help`
- man命令：提供了命令的用户手册
- tldr：替代man提供了一些示例关于如何调用命令
#### 3）查找文件

- find命令：递归地搜索符合条件的文件
```bash
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '*/test/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'
# 查找所有的临时文件并删除
find . -name "*.tmp" -exec rm {} \;
# -iname选项是不区分大小写
```
注：shell 最好的特性就是您只是在调用程序，因此除了find之外，还有一个更简单，更快速的程序可以用来替代find

- locate：locate 使用一个由 updatedb负责更新的数据库，在大多数系统中 updatedb 都会通过 cron 每日更新
#### 4)查找代码

- grep:`-C ：获取查找结果的上下文（Context）；-v 将对结果进行反选（Invert），也就是输出不匹配的结果,当需要搜索大量文件的时候，使用 -R 会递归地进入子目录并搜索所有的文本文件。`
- ack
- ag
- ripgrep(rg)
#### 5)查找输入过的shell命令

- history
- CTRL+R
- zsh
#### 6)文件夹导航--查找最常用或最近使用的文件和目录

- fasd
- autojump
#### 7)概览目录结构

- tree
- broot
- nnn
- ranger

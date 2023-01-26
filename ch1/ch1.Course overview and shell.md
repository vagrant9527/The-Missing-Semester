## 一、内容概览
- [前言](#jump1)
- [2.shell](#jump2)
+ [3.文件目录](#jump3)
+ [4.改变当前工作目录-cd](#jump4)
+ [5.列出当前目录下的文件-ls](#jump5)
+ [6.移动/重命名文件和目录-mv](#jump6)
+ [7.复制文件或者目录-cp](#jump7)
+ [8.删除文件或者目录-rm](#jump8)
+ [9.创建目录-mkdir](#jump9)
+ [10.查看命令的帮助信息-man](#jump10)
+ [11.文件流](#jump11)
+ [12.连接文件并打印到标准输出设备上-cat](#jump12)
+ [13.管道符(pipe)- |](#jump13)
+ [14.以另一个用户的身份来执行命令-sudo](#jump14)
+ [15.从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件-tee](#jump15)

## 二、
### 1.前言-为什么会有这门课？
<span id="jump1"></span>
计算机擅长做重复性工作，把事情自动化。合理的利用工具能够提升开发效率。这个课程主要就是会教授一些工具的使用。课程共分为11个小节，每个小节有一个相应的主题。
### 2.shell
<span id="jump2"></span>
#### 1）什么是shell？
我们常常使用图像化界面（GUI）来和电脑交互，而shell是另一种和电脑交互的主要方式之一。使用命令行工具能够完成许多GUI无法完成的事情，shell，命令行界面（CLI,command line interface）则是做这些事情的地方。
#### 2）常见的shell
windows的powershell,linux的bash
#### 3)命令行界面
当打开一个shell时通常会看到以下界面，这一行文字的具体含义是：启动shell的用户名@机器名：所在路径#（或者$）
其中#和$代表命令提示符。超级用户为#，其他用户为$.
在这里你可以输入一些**命令**，这些命令也可以带有一些参数。参数能够修改程序的行为。
例如：
```bash
date  #显示当前的时间
echo  #用来打印你传递的参数，参数是跟在它后面用空格分隔的东西。
```
注：echo这边如果想打印带空格的单词怎么办？
a.用“”将这个单词括起来
b.使用转义字符/ 
![image.png](https://cdn.nlark.com/yuque/0/2023/png/33626411/1674551219437-fbe48f04-0179-4110-a51d-5d43391bd609.png#averageHue=%232d2e28&clientId=u41d78b28-b4ab-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=151&id=u51bea016&margin=%5Bobject%20Object%5D&name=image.png&originHeight=226&originWidth=816&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15464&status=done&style=none&taskId=u9390120a-4dac-4855-8348-31aa295fc7c&title=&width=544)
#### 4)shell如何找到命令并执行的
通过环境变量PATH。
shell会在PATH包含的这些目录中寻找与你输入指令相同的程序或者文件。
#### 5）查看程序的位置-which
```bash
which commond  #查看commond所在的位置
```
### 3.文件目录
<span id="jump3"></span>
#### 1）文件系统
系统以分层目录结构来组织所有文件。 这就意味着所有文件组成了一棵树型目录，这个目录树可能包含文件和其它的目录。linux以/来分隔，只有一个根目录，用/表示。windows则不同，每个驱动器下都有一个独自的文件系统树。
两种特殊的目录：.(当前目录)，..(上一层目录)
#### 2）绝对路径和相对路径
绝对路径：文件的完整路径
相对路径：文件相对于当前所在位置的路径
#### 3）显示当前所在路径
```bash
pwd  #print working directory
```
### 4.改变当前工作目录-cd
<span id="jump4"></span>
```bash
cd path  #切换位置到path.这里的path可以是相对路径，也可以是绝对路径
cd /usr/bin #绝对路径
cd usr/bin  #假设当前在根目录下，使用绝对路径
cd的一些快捷操作：
cd -   #更改工作目录到先前的工作目录。该命令可以在当前目录和上一个目录之前来回切换
cd ~   #切换到家目录
```
### 5.列出当前目录下的文件-ls
<span id="jump5"></span>
```bash
ls  #列出当前目录下的文件
ls path #列出给定路径下的文件
ls --help #显示ls的帮助信息。补充：大部分程序都包含--help，以ls为例通常会这样显示，
          #Usage:ls [OPTION]... [FILE]...。其中[]表示这部分内容可填可不填，...代表可以填写多个option或者file
          #通常把"- 字母 不加值"叫做flag，"- 字母 加值"叫做option
ls -l #以长格式的形式输出。如下图所示
```
## ![image.png](https://cdn.nlark.com/yuque/0/2023/png/33626411/1674556131469-be21ff5a-0d4e-4825-8e5d-be9b8fe29d0e.png#averageHue=%232d2e27&clientId=u41d78b28-b4ab-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=87&id=u0f0d6498&margin=%5Bobject%20Object%5D&name=image.png&originHeight=130&originWidth=630&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21696&status=done&style=none&taskId=u08a4f2cc-3201-4e9b-9fb0-43941174e27&title=&width=420)
以test.txt来说明：

| 字段 | 说明 |
| --- | --- |
| -rw-r--r-- | 文件的访问权限，三个为一组，分别代表owner，group，other。r,w,x分别代表可读、可写、可执行。（d开头代表目录） |
| 1 | 文件的硬链接数目 |
| root | 文件所有者的用户名 |
| root | 文件所属组的用户名 |
| 0 | 以字节数表示文件的大小 |
| Jan 24 10:28 | ·上次修改文件的时间和日期 |
| test.txt | 文件名字 |

注：如果你有文件的r权限，但是没有目录的r权限，你可以修改文件的内容，但是你无法删除这个文件。
注：目录的执行权限，它意味着你能不能进入这个目录，访问里面的文件（把目录成一个文件，里面放着目录下文件的地址）
#### 6.移动/重命名文件和目录-mv
<span id="jump6"></span>
```bash
mv item1 item2 #重命名文件。
mv item... directory  #把一个或多个条目从一个目录移动到另一个目录中
```
#### 7.复制文件或者目录-cp
<span id="jump7"></span>
```bash
#cp接受两个参数，复制源路径和目标路径
cp item1 item2    #复制单个文件或目录”item1”到文件或目录”item2”（如果是文件的话就复制了会把文件的内容复制过去）
cp item... directory    #复制多个项目（文件或目录）到一个目录下。
```
#### 8.删除文件或者目录-rm
<span id="jump8"></span>
```bash
rm item...   #用来删除文件和目录：“item”代表一个或多个文件或目录。
             #linux上默认删除是非递归的。也就是不能够移除目录
rm -r path   #移除目录
rmdir        #可以移除目录，但必须是空目录
```
#### 9.创建目录-mkdir
<span id="jump9"></span>
```bash
mkdir directory1 directory2 ... #创建多个目录
```
#### 10.查看命令的帮助信息-man
<span id="jump10"></span>
接受其他命令的名字作为参数，用于显示帮助信息
#### 11.文件流
<span id="jump11"></span>
（1）上面单独讲述了一些命令的用法，除此之外，shell还能够将不同的命令组合起来使用。这需要借助流（stream）的概念.
（2）shell的文件有两个主要的流，输入流（input stream）和输出流（output stream）。默认输入流来自于键盘，输出流是程输出的内容，在终端上显示。
（3）shell可以重定向这两个流，将它们更改至指定的地方。
（4）<:重定向程序的输入流，>重定向文件的输出流
```bash
ls -l /usr/bin > ls-output.txt  #将ls -l的输出重定向至ls-output.txt中
> ls-output.txt                 #清空文件内容或者创建一个新的空文件
>>                              #追加，不是覆写
```

#### 12.**连接文件并打印到标准输出设备上-cat**
<span id="jump12"></span>
```bash
cat file1 file2 ... #将文件连接起来展示
cat <hello.txt     #结合输入重定向符使用，与cat hello.txt效果相同，
                   #这里实现了标准输入的重定向，把从键盘的输入源改到了文本hello.txt
cat <hello.txt >hello1.txt  #将hello.txt的内容复制到hello1.txt中去
```
#### 13.管道符(pipe)- |
<span id="jump13"></span>
管道符的意思是左侧程序的输出作为右侧程序的输入
```bash
ls -l | tail -n1   #ls -l的输出作为了tail -n1的输入，结果是显示了ls -l的最后一行
curl --head --silent google.com | grep -i content-length #输出：Content-Length"219。
												 #这是一个比较复杂的例子，展示了shell如何利用pipe进行多文本操作
```
#### 14.以另一个用户的身份来执行命令-sudo(do as super user)
<span id="jump14"></span>
```bash
sudo command   #以root用户运行这个命令
```
注：linux中/下有个sys目录，它们不是真实存在的文件，而是一堆内核参数。通过文件系统能够访问这些内核参数，这意味着我们可以用之前的命令去操作它们。（但一般最好不要这样，因为这是内核参数，2333）
这里提到一个例子，如果我们使用 `sudo echo 500 > brightness `去修改亮度，这里会报一个权限错误。原**因是不论是重定向还是pipe都是shell定义的，通过他们连接的命令是不知道彼此的，前后互不影响，只关心输入输出。**这里是用root权限执行了echo这个命令，然后发送输出到这个brightness文件。如果想使命令成功运行，需要用到`sudo su`。注意下面使用了`sudo su`之后命令提示符由$变成#
![image.png](https://cdn.nlark.com/yuque/0/2023/png/33626411/1674567674469-7061e81f-1be3-4072-b625-c9e3bcbc75d1.png#averageHue=%23242826&clientId=u17a3a2cc-8900-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=321&id=ueee53c18&margin=%5Bobject%20Object%5D&name=image.png&originHeight=481&originWidth=1381&originalType=binary&ratio=1&rotation=0&showTitle=false&size=649444&status=done&style=none&taskId=ue6e965a3-fcd4-4126-96e5-c973e782443&title=&width=920.6666666666666)

#### 15.从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件-tee
<span id="jump15"></span>
因此14中的例子还可以这样解决权限问题，`echo 1060 |sudo tee brightness`

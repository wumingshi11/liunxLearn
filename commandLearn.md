# linux 中常用命令

## 文件系统

```shell
pwd  -  打印当前工作目录名
cd   -  更改目录（注意相对目录和绝对目录）
ls   -  列出目录内容（不带参数默认为当前文件夹）
cd   -  更改目录到家目录(不带任何参数)
cd-  -  更改目录到先前的工作目录
cd ~user_name    - 更改目录到用户家目录，需要权限
```



## 操作系统

```shell
command -options arguments   -  命令行完整模式,命令，选项，参数
ls -a   等价于 ls --all         列出所有文件 - 短选项和长选项
ls -l  以长格式显示结果
ls - r 反转
ls -t(S) 按照大小（时间）来排序

file filename   -查看文件信息
less filename   - 浏览文本文件内容
```

## 操作文件和目录

- cp - 复制文件和目录
- mv - 移动/重命名文件和目录（重命名即在当前文件夹下移动）
- mkdir - 创建目录
- rm - 删除文件和目录
- ln - 创建硬链接和符号链接

### 通配符



| 通配符         | 意义                               |
| -------------- | ---------------------------------- |
| *              | 匹配任意多个字符（包括零个和一个） |
| ？             | 匹配任意一个字符                   |
| [characters]   | 匹配任意一个属于字符集中的字符     |
| [！characters] | 匹配任意一个不属于字符集中的字符   |
| [[:class:]]    | 匹配任意一个属于指定字符类中的字符 |

shell中可以使用通配符来加速操作

### cp

```shell
mkdir  dir1,dir2   -- create dir
cp  item1 item2   --- item2 will be change
cp item...  directory   --复制多个文件到指定目录
```

| 选项             | 意义                                                         |
| ---------------- | ------------------------------------------------------------ |
| -a, --archive    | 复制目录和文件                                               |
| -i,--interactive | 覆盖文件时征求用户许可                                       |
| -r, --recursive  | 递归地复制目录 （复制文件需要-r or -a）<br />目标文件夹不存在时创建，两个文件夹完全相同<br />存在时将目录拷贝到文件夹下 |
| -u, --update     | 仅复制当前文件夹下不存在的文件，或者文件内容新于旧文件       |
| -v , --verbose   | 现实命令操作信息                                             |

### mv

mv 没有选项  -a,-r

### rm

| 选项              | 意义                             |
| ----------------- | -------------------------------- |
| -i，--interactive | 提示用户确认信息                 |
| -r,               | 递归删除，删除目录必用           |
| -f, --force       | 忽视不存在的文件(即忽视无效参数) |
| -v                |                                  |

### ln 创建链接

linux中文件和名字是分开的，一个硬链接就是名字，指向内存，当所有硬链接删除后，内存中内容删除

硬链接有2个缺点

- 不能关联关联它所在文件系统之外的文件。即不能关联不在一个**磁盘分区**的文件
- 一个硬链接不能关联一个目录

为了克服这2个缺点，软链接  出现 软链接 相当于快捷方式吧

```shell
ln file file-hard        //在当前文件下 为 file 创建了一个 名为  file-hard 的硬链接
ln file dir1/file-hard   //在子目录dir1下为 file 创建了 硬链接
ln -s fun fun-sym        //在当前文件下 为 file 创建了一个 名为  file-sym 的软链接(符号链接)
```

删除文件会使符号链接失效



## 重定向

文件的从定向，读取文件内容，输出导文件

- cat    -  连接文件
- sort  - 排序文本行
- uniq - 报道或省略重复行
- grep - 打印匹配行
- wc - 打印文件中的换行符，子喝字节个数
- head - 输出文件的第一部分
- tail - 输出文件的最后一部分
- tee  - tee程序从标准输入读入数据，并且同时复制数据到标准输出喝一个或多个文件

### 标准输入，输出和错误

- stdin    ---  标准输入，默认来自键盘
- stdout  ---  标准输出，默认输出到屏幕
- stderr  ---  标准错误

### 重定向标准输出

```shell
ls -l /usr/bin > ls-output.txt  //覆盖或创建文件，并将结果输入该文件中
ls -l /usr/bin >> ls-output.txt  //创建或append文件
> filename    //小技巧，创建文件
```

## 重定向标准输出

```shell
ls -l /bin/usr 2> ls-error.txt
ls -1 /bin/usr > ls-outout.txt 2>&1  //将标准输出和标准错误输出到同一个文件
ls -l /bin/usr &> ls-output.txt      //效果同上
```

### 处理不需要的输出

```shell
ls -l /bin/usr 2> /dev/null         //unix中的dev文化，接受输入，但不做任何处理
```

### cat

cat 命令读取一个或多个文件，然后复制他们到标准输出（可以种定向）

```shell
cat file
cat file  > ls-output.txt
cat    //重标准输入种读取数据，然后标准输出
cat < ls-output.txt   //从标准输入种读取，并重定向到输出
```



### 管道线

命令可以从标准输入读取数据，然后再把数据输送到标准输出，命令的这种能力被一个shell特性所利用，这个特性叫做管道线。使用管道操作符“|”,一个命令的标准输出可以到管道的另一个命令的标准输入

```shell
ls -l /usr/bin | less
```



### 在管道线种常用的命令

- sort  对输出数据排序
- uniq   删除重复行  uniq -d  可以看到重复行
- wc    统计
- grep 打印匹配行
- head(tail) -n number filename 
- tee  从Stdin读取数据，并同时输出到Stdout 和文件

```shell
cat filename | sort | uniq |less
cat filename |  sort | uniq | grep xiao   //打印含小的行
cat filename |  sort | uniq |head -n 5 | less //打印前5

```



## 文本格式控制

***echo 命令在不加引号的情况下，将由空格，换行间隔开的字符串当参数，输出时去掉这些格式***

### 字符展开

```shell
echo *   //输出当前路径下的文件名
echo *txt  // 输出以txt结尾的文件名
echo [[:upper:]]*   //输出以大写字母开头的文件名
echo ~              // 指定用户的家目录  cd ~
echo $((2+2))
echo Front-{A..C}-Back
echo pre-{aa,as,dd}-aft
echo {Z..A}
echo $USER        //输出环境变量
printenv | less   //查看环境变量
echo $noEnv       //输出不存在的环境变量，啥也不输出
mkdir  {2007..2009}-0{1..9}   {2007..2009}-{10..12}   //创建36个目录
rm -r  {2007..2009}-0{1..9}   {2007..2009}-{10..12}   //删除36个目录
```

### 命令替换

命令替换允许我们把一个命令的输出作为一个展开模式来使用

```shell
file $(echo *)   // 即 file * ，可将echo 换为更复杂的程序
```

### 引用

- 双引号 " " : 将文中放入双引号种，shell中的特殊字符，除$,\,` 都失效。"  "内的内容被当成一个参数
- 单引号'  '  : 所有都失效
- 如果不想再双引号或者无引号时被安排，使用转义字符  \

## linux权限

- id - 显示用户身份号

- chmod - 更改文件模式

- umask - 设置默认的文件权限

- su - 以另一个用户的身份来运行shell

- sudo - 以另一个用户的身份来执行命令

- chown - 更改文件所有者

- chgrp - 更改文件组所有权

- passwd - 更改用户密码

  

### 文件权限

```shell
[21:00 localhost liunxLearn] $ ls -l foo.txt
-rw-rw-r--. 1 zhaoxiaoming zhaoxiaoming 97 Aug 19 17:11 foo.txt

```

#### 首字母表示含义

| 属性 | 文件类型                                       |
| ---- | ---------------------------------------------- |
| -    | 普通文件                                       |
| d    | 一个目录                                       |
| \|   | 一个符号链接                                   |
| c    | 一个字符设备文件。按照字节流，来处理数据的设备 |
| b    | 一个快折本文件，按照数据块，来处理数据的设备   |



### 文件模式

剩下的九个字符，叫做文件模式，代表着文件所有者，文件组所有者，和其他人的读，写，执行权限。

八进制表示文件权限法  rwx  有权限为1，没有为0，三个二进制数 组成一个一个八进制数（0-7），3个八进制数控制3个身份的权限



```shell
chmod 600 foo.txt   #将文件所有者的权限设为读写，不能执行，其他人不能读写，执行
```

chmod命令还支持符号表示法，来指定文件模式，模式为  身份（ugoa) + 操作符（+-=）+ 权限（rwx)

| 符号      | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| u         | user                                                         |
| g         | 用户组                                                       |
| o         | other                                                        |
| a         | 以上三者                                                     |
| u+x  u-x  | 为文件所有者添加可执行权限,  为文件所有者去除可执行权限      |
| +x        | 等价于 a+x,为所有接触文件者添加执行权限                      |
| go=rw     | 给群组的主人和其他文件所有者设置读写指定权限为rw,如果有x权限，将被去除 |
| u+x,go=rw | 给文件所有者执行权限，并给用户组和其他人rw权限。多种设定可以用","分开 |

```shell
chmod u+x,go=rw foo.txt   给文件所有者执行权限，并给用户组和其他人rw权限
```

此外，还可以用可视化界面（文件的 peimissions)改变文件的模式

### umask - 设置默认权限

文件的初始模式是  ugo=rw

umask  初始值为  0002

umask 是用来去掉权限的，后3为数对应 3种的用户

0002 去掉了 other  用户的  w 权限

```shell
[21:01 localhost liunxLearn] $ umask 0022 
[23:04 localhost liunxLearn] $ >foo2.txt
[23:04 localhost liunxLearn] $ ll foo*
-rw-r--r--. 1 zhaoxiaoming zhaoxiaoming  0 Aug 19 23:04 foo2.txt
-rw-rw-r--. 1 zhaoxiaoming zhaoxiaoming 97 Aug 19 17:11 foo.txt

```

umask的第四位（首位） 用来设置一些特殊权限



### 更改身份

su [-[l]] [user]   # l可省略

```shell
su -   # 切换导超级用户
exit   # 退出超级用户
su -c 'command'  # 再超级用户级别下执行command
sudo 以普通用户的身份执行一些不普通的命令
```



### chown - 更改文件所有者和用户组

chown [owner][:[group]] file



### passwd

输入passwd就能修改密码

passwd [user]  # 超级用户来修改密码

还有 创建 和维护用户，用户组



## 进程

- ps -- 报告当前进程的快照
- top - 显示任务
- jobs  - 列出活跃的任务
- bg -把一个任务放到后台执行
- fg - 把一个任务放到前台执行
- kill - 给一个进程发送型号
- killall
- shutdown

### ps

默认情况下，ps不会显示很多进程信息，只是列出与当前终端会话相关的进程。 

ps  x   展示所有进程



### top  相当于windows的进程管理器

top命令持续显示进程变化状态



### 中断一个终端中启动的程序

Ctrl-c

### 把一个进程放置到后台（执行）

我们在程序命令之后 ，加上 “&”字符。

一个后台运行的进程对一切来自键盘的输入都免疫。

### jobs  命令可以查看此终端启动的程序 及其编号

jobs命令可以查看此终端启动的程序 及其编号

```shell
[zhaoxiaoming@localhost ~]$ jobs
[1]+  Running                 gedit &
[zhaoxiaoming@localhost ~]$ fg %1
gedit
```

gedit 为一个后台启动的进程，编号为1

###  fg 切换后台程序到前台



### Ctrl-z  停止一个进程

### 将停止进程移到前台或者后台

```
[zhaoxiaoming@localhost ~]$ gedit
^Z
[1]+  Stopped                 gedit
[zhaoxiaoming@localhost ~]$ jobs
[1]+  Stopped                 gedit
[zhaoxiaoming@localhost ~]$ bg %1
[1]+ gedit &

```

### Signals  通过kill发送信号

```shell
#杀死一个进程
[zhaoxiaoming@localhost ~]$ ps
   PID TTY          TIME CMD
  2784 pts/0    00:00:00 bash
  3536 pts/0    00:00:01 gedit
  3627 pts/0    00:00:00 ps
[zhaoxiaoming@localhost ~]$ kill 3536   # 可以中jobspec(如%1)来代替PID
[1]+  Terminated              gedit

```

```shell
[zhaoxiaoming@localhost ~]$ gedit
^Z
[1]+  Stopped                 gedit
[zhaoxiaoming@localhost ~]$ bg %1
[1]+ gedit &
[zhaoxiaoming@localhost ~]$ kill -1 %1
[1]+  Hangup                  gedit
[zhaoxiaoming@localhost ~]$ ps
   PID TTY          TIME CMD
  2784 pts/0    00:00:00 bash
  3914 pts/0    00:00:00 ps
[zhaoxiaoming@localhost ~]$ gedit &
[1] 3929
[zhaoxiaoming@localhost ~]$ kill -2 %1
[1]+  Interrupt               gedit
[zhaoxiaoming@localhost ~]$ gedit &
[1] 3962
[zhaoxiaoming@localhost ~]$ kill -19

[zhaoxiaoming@localhost ~]$ kill -19 %1

[1]+  Stopped                 gedit
[zhaoxiaoming@localhost ~]$ bg %1
[1]+ gedit &

```





| 编号 | 名字            | 含义                                 |
| ---- | --------------- | ------------------------------------ |
| 1    | HUP             | 通常情况下会终止一个进程             |
| 2    | INT             | 中断,其作用相当于Ctrl-c              |
| 9    | KILL（不是kill) | 使用内核终止进程                     |
| 15   | TERM            | 终止，kill的默认信号                 |
| 18   | CONT            | 继续，在停止一段时间后，进程恢复运行 |
| 19   | STOP            | 停止，其作用相当于 Ctrl-z            |



## shell 入门

### 打印语句

```shell
echo "helloworld!"
```

### 变量

1. 自定义变量
2. Linux已定义的环境变量
3. Shell 变量 ：由Shell程序设置的特殊变量

```shell
#！/bin/bash
#使用自定义变量
hello="hello world"
echo $hello
echo ${#hello}   #获取字符串长度，相当去于 
expr length $hello
echo ${hello:0:5} #输出前5个字符
#根据正则表达式截取

```



### 基本运算符

针对整数 ： 加减乘除取余，赋值，相等

```shell
[zhaoxiaoming@localhost ~]$ int1=5
[zhaoxiaoming@localhost ~]$ int2=3
[zhaoxiaoming@localhost ~]$ echo int1+int2
int1+int2
[zhaoxiaoming@localhost ~]$ echo $int1+$int2
5+3
[zhaoxiaoming@localhost ~]$ echo `$int1+$int2`
bash: 5+3: command not found...
Failed to search for file: Cannot update read-only repo

[zhaoxiaoming@localhost ~]$ echo `expr $int1+$int2`
5+3
[zhaoxiaoming@localhost ~]$ echo `expr $int1 + $int2`
8
[zhaoxiaoming@localhost ~]$ int3=`expr $int1 + $int2`
[zhaoxiaoming@localhost ~]$ echo $int3
8

```

关系运算符



![image-20200825180137911](E:\会议\image-20200825180137911.png)

### 控制流程

```shell
#!/bin/bash
a=3;
b=9;
if [ $a -eq $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
else
   echo "a 小于 b"
fi
```



 for while 语句

```shell
for i in {1..9}
    do
          echo $i
    done
   
# 按下<CTRL-D>退出   
while read input
do 
  echo "input : $input"
done
```



### 函数

无返回，有返回，有参数

```shell
hello(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行hello-----"
hello
echo "-----函数执行完毕hello-----"

#there a return value
hello2(){
    echo "这是我的第2个 shell 函数!"
    return 5
}
echo "-----函数开始执行hello-----"
hello2
ss=$?
echo $ss
echo "-----函数执行完毕hello-----"

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}

echo "-----函数开始执行funWithParam-----"
funWithParam 1 2 3 4 5 6 7 8 9 34 73
echo "-----函数执行完毕funWithParam-----"
```




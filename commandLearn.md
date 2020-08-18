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











 


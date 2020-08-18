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









 

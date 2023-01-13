# Linux Shell 命令



## APT 命令

​	apt（Advanced Packaging Tool）是一个在 Debian 和 Ubuntu 中的 Shell 前端软件包管理器。apt 命令提供了查找、安装、升级、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。apt 命令执行需要超级管理员权限(root)。

###  1，命令语法

```
apt [options] [command] [package ...]
```

- options：可选，选项包括 -h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
- command：要进行的操作。
- package：安装的包名

### 2，常用命令

| 命令                                     |                                                     |
| ---------------------------------------- | --------------------------------------------------- |
| sudo apt update                          | 列出所有可更新的软件清单命令                        |
| sudo apt install <package_name>          | 安装指定的软件命令                                  |
| sudo apt install <package_1> <package_2> | 安装多个软件包                                      |
| sudo apt update <package_name>           | 更新指定的软件命令                                  |
| sudo apt show <package_name>             | 显示软件包具体信息,例如：版本号，安装大小，依赖关系 |
| sudo apt remove <package_name>           | 删除软件包命令                                      |
| sudo apt autoremove                      | 清理不再使用的依赖和库文件                          |
| sudo apt purge <package_name>            | 移除软件包及配置文件                                |
| sudo apt search <keyword>                | 查找软件包命令                                      |
| apt list --installed                     | 列出所有已安装的包                                  |
| apt list --all-versions                  | 列出所有已安装的包的版本信息                        |

### 3，安装问题

​	（1）， 安装软件时，提示如下，安装软件时， 提示相关软件依赖包rsyslog有问题。

```
Errors were encountered while processing:
	rsyslog
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

​			解决方法，使用apt-get -f install 补全软件依赖包问题。当发现可能是安装的其他软件包不兼容导致了安装包出错时，可以根据提示需要执行“sudo apt-get  -f install ”来卸载之前的冲突包。

```bash
mv /var/lib/dpkg/info /var/lib/dpkg/info.bk
mkdir /var/lib/dpkg/info
apt-get update
apt-get -f install
mv /var/lib/dpkg/info/* /var/lib/dpkg/info.bk/
rm -rf /var/lib/dpkg/info
mv /var/lib/dpkg/info.bk /var/lib/dpkg/info
```

## 配置终端环境变量

- 变量PS1是用于控制bash终端的输出 样式的变量，一般值可以通过命令echo $PS1获取 ，而终端显示颜色和样式使用方法， 可以使用 man console_codes查看。PS1中的非打印字符都必须用`\[\]\`将其包围起来,否则在计算提示符长度时,会将其计算在内，导致其无法正确地换行,出现长命令回到行首的情况.

```bash
$ echo $PS1

#imx6ull开发板
${debian_chroot:+($debian_chroot)}\u@\h:\w\$ 	

#ubuntu虚拟机
${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$		  
```

- 控制终端颜色，采用了(ESC)控制符来配置颜色，`ESC` 对就的数值为 `\033`(八进制)，`\027`(十进制)(十进制只能在window下有效)，`\x1b`(十六进制)， 下面用printf()进行样式测试.  

  注 `\e` 转义符，等效于 `\033 , \x1B`   都表示ESCl转义符。

​    语法格式 

```
\033 [ 参数1 ; 参数2 ; 参数3m  字符内容 \033[0m   #以字母m结尾
```

```
printf("\033[0;30;41m color!!! \033[0m Hello \n"); #配置了"color!!!"为红色背景,黑色字体的效果.
```



| 控制码            | 效果             |
| :---------------- | ---------------- |
| \033[0m           | 关闭所有属性     |
| \033[1m           | 设置字体高亮度   |
| \033[2m           | 低亮（减弱）显示 |
| \033[4m           | 下划线           |
| \033[5m           | 闪烁             |
| \033[7m           | 反显             |
| \033[8m           | 消隐             |
| \033[30m~\033[39m | 字体颜色         |
| \033[40m~\033[49m | 背景颜色         |

| 字体码   | 颜色   |
| -------- | ------ |
| \033[30m | 黑色   |
| \033[31m | 红色   |
| \033[32m | 绿色   |
| \033[33m | 黄色   |
| \033[34m | 蓝色   |
| \033[35m | 紫色   |
| \033[36m | 浅蓝色 |
| \033[37m | 白色   |
| \033[38m | 无     |
| \033[39m | 无     |

| 背景码   | 颜色   |
| -------- | ------ |
| \033[40m | 黑色   |
| \033[41m | 红色   |
| \033[42m | 绿色   |
| \033[43m | 黄色   |
| \033[44m | 蓝色   |
| \033[45m | 紫色   |
| \033[46m | 浅蓝色 |
| \033[47m | 白色   |
| \033[48m | 无     |
| \033[49m | 无     |


光标控制码, 只能单独使用,不可以与其他控制码联用.

| 控制码    | 效果                    |
| --------- | ----------------------- |
| \033[nA   | 光标上移n行             |
| \033[nB   | 光标下移n行             |
| \033[nC   | 光标右移n列             |
| \033[nD   | 光标左移n列             |
| \033[y;xH | 设置光标位置（y行,x列） |
| \033[2J   | 清屏                    |
| \033[K    | 清除从光标到行尾的内容  |
| \033[s    | 保存光标位置            |
| \033[u    | 恢复光标位置            |
| \033[?25l | 隐藏光标                |
| \033[?25h | 显示光标                |

输出格式控制。

| 控制码  | 效果                                                         |
| ------- | ------------------------------------------------------------ |
| \033[0m | 清除所有格式（常用在格式控制末尾，以免对后序字符串造成影响） |
| \033[1m | 加粗，与格式`2`冲突                                          |
| \033[2m | 字体变暗，与格式`1`冲突                                      |
| \033[3m | 斜体                                                         |
| \033[4m | 下划线                                                       |
| \033[5m | 呼吸闪烁（但有的机器上没效果)）                              |
| \033[6m | 同上                                                         |
| \033[7m | 反显（背景色当前景色，前景色当背景色）                       |
| \033[8m | 隐形（字符仍然存在，可以选中，只是看不到）                   |
| \033[9m | 删除线                                                       |

- 命令行字符含义

| 转义符 |                                                       |
| ------ | ----------------------------------------------------- |
| \d     | 代表日期                                              |
| \H     | 完整的主机名称                                        |
| \h     | 仅取主机的第一个名字                                  |
| \t     | 显示时间为24小时格式，如：HH：MM：SS                  |
| \T     | 显示时间为12小时格式                                  |
| \A     | 显示时间为24小时格式：HH：MM                          |
| \u     | 用户名                                                |
| \v     | BASH的版本信息                                        |
| \w     | 完整的工作目录名称                                    |
| \W     | 列出最后一个目录                                      |
| \\$    | 提示字符，如果是root时，提示符为：# ，普通用户则为：$ |

​     结论：根据上面的转义字符，则'`${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ `, 可以拆分为	A=`\[\033[01;32m\]\u@\h\[\033[00m\]` B=`：` C=`\[\033[01;34m\]\w\[\033[00m\]` D=`\$` 。

​	A表示  username@hostname, 字体颜色为绿色加粗， C表示完整工作目录，字体颜色为蓝色加粗，D表示 普通 用户提示符为$。

​	注：

- 配置终端tty窗口大小， stty size 命令，可以查看当前tty的大小。

```
# stty size
24 80	
```

​	默认的窗口大小行列为24*80 ， 其中列为80， 会导致当终端输入字符超过80时，光标会重新会到首行，继续输入字符时，会覆盖之前字符，为了解决这个问题，可以重新重置行列数。

```
# stty rows 48 cols 320
```

- 终端颜色配置， 窗口大小配置，为了能开机应用，可以将命令添加到用户环境.bashrc, 下面我们配置root用户下的 .bashrc 文件，修改代码如下。

```
# vim .bashrc 
stty rows 48 cols 320
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\\]\$ '
# source .bashrc 
```

​		如果配置了stty rows 48 cols 320 时，vim 里面也会的 row,colunms也会被配置了， 需要手动配置vim窗口大小，不然会出现显示错位，命令如下。

```
touch .vimrc
输入set lines=48 columns=100
source .vimrc
```

## TAR 命令

​	tar命令用于linux下打包工具命令。

### 1，语法格式

​	tar [options]  [pathname ...]

- options ： 操作模式
- pathname : 操作文件

### 2，options选项

​	常用选项如下表格。

| 选项 | 功能                                                     |
| ---- | -------------------------------------------------------- |
| -z   | --gizp, --gunzip,--ungizp ， .tar.gz                     |
| -j   | --bzip2, .bz2                                            |
| -J   | --xz, .xz                                                |
| -f   | --file , use archive file or device ARCHIVE              |
| -c   | --create                                                 |
| -C   | --directory=DIR , 解压到指到目录, 缺省，解压到当前目录。 |
| -x   | --extract，--get                                         |
| -t   | --list , 查看文件                                        |
| -v   | --verbose                                                |
| -p   | --preserve-permissions , 保留数据原本根限与属性。        |
| -P   | --absolute-names , 使用绝对路径                          |

### 3， 使用命令

```
# 解压文件, xxx.tar.gz 解压对象， pathname 解压路径
tar -zxvf  xxx.tar.gz -C pathname
# 压缩文件  xxx,tar.gz 压缩对象， sourcefile 压。
缩源文件
tar -zcvf  xxx.tar.gz sourcefile 
```

## FIND命令

​	find 查找命令，可以通过文件名，时间，权限，inode, 大小，等属性查找

### 1，命令格式

​	find [-H] [-L] [-P] [-D debugopts] [-Olevel] [path...] [expression]， [-H] [-L] [-P] [-D debugopts] [-Olevel] 选项，在实际中，比较少用，常用项为 find [path] [expression]。

- path : 表示要查找路径。
- experession : e xpression表示为 -options [-print -exec -ok -ls  ...] ， 更多命令，可以查看man手册。
- -options : 属性选项



### 2, 属性选项

​	常用属性

| 属性     | 功能                                                         |
| -------- | ------------------------------------------------------------ |
| -name    | 按照文件名搜索;                                              |
| -iname   | 按照文件名搜索，不区分文件名大小;                            |
| -inum    | 按照 inode 号搜索；                                          |
| -print   | find命令将匹配的文件输出到标准输出                           |
| -ls      | 格式以 ls -disl 形式输出 。                                  |
| -exec    | command {} +，  find命令对匹配的文件执行该参数所给出的shell命令，例： rm -f {} \;  `{}` 与` \` 空格符格开。 |
| -ok      | 作用与-exec相同，更加安全                                    |
| -size    | --size n[cwbkMG], c(byte), b (block(512字节))，w ( 2 byte)， k( kb ,1024), M ( Mb) , G (Gb) |
| -type    | 查找类型，b (block), c ( character), d (dir), p (pip), f (file) , l (link) , s (socket), D (Door) |
| -atime n | 在过去 n 天内被读取过的文件                                  |

### 3, 使用命令

```bash
# 常用字符文件
find / -name strings
# 查找字符文件夹
find / -type d -name busybox
# 查找文件以a.开头的文件， 并执行删除命令
find ./test -name a.* -exec rm -f {} \;
#查找系统中所有文件长度为 0 的普通文件，并列出它们的完整路径：
find / -type f -size 0 -exec ls -l {} \;
```

## GREP 命令

​	**grep** （global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。用于过滤/搜索的特定字符。可使用正则表达式能配合多种命令使用，使用上十分灵活

### 1，语法格式

       grep [OPTIONS] PATTERN [FILE...]
       grep [OPTIONS] [-e PATTERN]...  [-f FILE]...  [FILE...]

- optinos ： 属性选项
- -e : 使用正则表达式
- pattern :  配对参数
- file ： 文件

### 2，属性选项

​	常用属性选项

| 属性 | 功能                                                         |
| ---- | ------------------------------------------------------------ |
| -i   | --ignore-case   ， 忽略字符大小写的差别。                    |
| -l   | --file-with-matches，  列出文件内容符合指定的范本样式的文件名称 |
| -L   | --files-without-match ，列出文件内容不符合指定的范本样式的文件名称。 |
| -n   | --line-number ，标示出该列的编号。                           |
| -a   | --text  不要忽略二进制数据。                                 |
| -o   | 只输出文件中匹配到的部分。                                   |
| -v   | --revert-match ， 查找不与pattern 不匹配的字符               |
| -e   | --regexp=<范本样式>  ，指定字符串作为查找文件内容的范本样式。 |
| -E   | --extended-regexp  ，使用能使用扩展正则表达式。              |
| -P   | --perl-regexp ， pattern 是一个 Perl 正则表达式              |
| -c   | --count  ,计算符合范本样式的列数。                           |

### 3， 规则表达式

| 表达式     | 备注                                                         |
| ---------- | ------------------------------------------------------------ |
| ^          | 锚定行的开始 如：'^grep'匹配所有以grep开头的行。             |
| $          | 锚定行的结束 如：'grep$' 匹配所有以grep结尾的行。            |
| .          | 匹配一个非换行符的字符 如：'gr.p'匹配gr后接一个任意字符，然后是p。 |
| .*         | 一起用代表任意字符。                                         |
| []         | 匹配一个指定范围内的字符，如'[Gg]rep'匹配Grep和grep。        |
| [^]        | 匹配一个不在指定范围内的字符，如：'[^A-Z]rep' 匹配不包含 A-Z 中的字母开头，紧跟 rep 的行 |
| `\(..\)`   | 标记匹配字符，如`\(love\)`，love被标记为1。                  |
| `\<`       | 锚定单词的开始，如:'\<grep'匹配包含以grep开头的单词的行。    |
| `\>`       | 锚定单词的结束，如'grep\>'匹配包含以grep结尾的单词的行       |
| `x\{m\}`   | 重复字符x，m次，如：`0\{5\}`匹配包含5个o的行                 |
| `x\{m,\}`  | 重复字符x,至少m次，如：`o\{5,\}`匹配至少有5个o的行           |
| `x\{m,n\}` | 重复字符x，至少m次，不多于n次，如：`o\{5,10\}`匹配5--10个o的行 |
| \w         | 匹配文字和数字字符，也就是[A-Za-z0-9]，如：'G\w*p'匹配以G后跟零个或多个文字或数字字符，然后是p。 |
| \W         | \w的反置形式，匹配一个或多个非单词字符，如点号句号等         |
| \b         | 单词锁定符，如: '\bgrep\b'只匹配grep。                       |

### 4， 使用命令

```bash
# 从文件a.c a.h输出配对“test”字符的行。
grep test a.c a.h
grep "test" a.*
#统计显示匹配的行数
grep -c "text" file_name
#显示包含tesssssssst行。
grep  -n -E tes\{8\}t a.*
```

## DD 命令

​	dd是 device driver 的缩写，它用来读取设备、文件中的内容，并原封不动地复制到指定位置。dd 命令读取 /dev/null 文件时，可以创造出空洞文件，而如果你的磁盘足够大.！

### 1， 语法格式

​	dd [opiton]

- 属性选项

### 2，属性选项

| 属性       | 功能                                                   |
| ---------- | ------------------------------------------------------ |
| bs=BYTES   | read and write up to BYTES bytes at a time             |
| cbs=BYTES  | convert BYTES bytes at a time                          |
| conv=CONVS | 指定文件转换的方式                                     |
| count=N    | 仅读取指定的区块数                                     |
| ibs=BYTES  | read up to BYTES bytes at a time (default: 512),       |
| if=FILE    | 从文件读取替代标准输入                                 |
| obs=BYTES  | write BYTES bytes at a time (default: 512)             |
| of=FILE    | 输出 到文件替代标准输出                                |
| seek=N     | skip N obs-sized blocks at start of output（标准输出） |
| skip=N     | skip N ibs-sized blocks at start of input （标准输入） |

​	注， 单位， c (byte), w(world), b (block), k, M, G

### 3，使用命令

```bash
# 将字符设备输出sun.txt文件，总共内容=bs*connt, /dev/zero 是一个字符设备，会不断返回0值字节（\0）
dd if=/dev/zero of=sun.txt bs=1M count=1

```

## FDISK命令

​		fdisk命令用于查看系统硬盘及sd卡的分区情况，同是也可以利用fdisk对设备进行分区管理。

### 1，语法格式

​	fdisk [options] device

​	fdisk -l device

- options：属性选项
- -l ： 列出当前设备

### 2，属性选项

| 属性 | 备注                                                         |
| ---- | ------------------------------------------------------------ |
| -b   | --sector-size <size>      physical and logical sector size   |
| -c   | --compatibility[=<mode>]  mode is 'dos' or 'nondos' (default |
| -l   | --list                    display partitions end exit        |
| -C   | --cylinders <number>      specify the number of cylinders    |
| -H   | --heads <number>          specify the number of heads        |
| -S   | --sectors <number>        specify the number of sectors per track |

```bash
sudo fdisk /dev/sdb
Welcome to fdisk (util-linux 2.27.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Command (m for help):m							#m显示帮助菜单
  DOS (MBR)
   a   toggle a bootable flag
   b   edit nested BSD disklabel
   c   toggle the dos compatibility flag

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   u   change display/entry units
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty DOS partition table
   s   create a new empty Sun partition table

```



### 3，使用命令

-  查看当前磁盘设备列表

```bash
umount /media/fridy/B5C1-EEAE 		# 先
sudo fdisk -l 

Disk /dev/sdb: 29.7 GiB, 31914983424 bytes, 62333952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x37ef716d

Device     Boot Start      End  Sectors  Size Id Type
/dev/sdb1        8192 62333951 62325760 29.7G  7 HPFS/NTFS/exFAT
```

-  删除设备分区

```
sudo fdisk /dev/sdb
Command (m for help):p 				#可以查看磁盘分区情况
Command (m for help):d				#删除分区，如果只有一个分区，删除当前分区
Selected partition 1
Partition 1 has been deleted.
```

- 创建分区

```
#输入n建立新的磁盘分区， 分别创那200M和29.5G的主分区
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-62333951, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-62333951, default 62333951): +200M           

Created a new partition 1 of type 'Linux' and of size 200 MiB.

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2): 
First sector (411648-62333951, default 411648): 
Last sector, +sectors or +size{K,M,G,T,P} (411648-62333951, default 62333951): 

Created a new partition 2 of type 'Linux' and of size 29.5 GiB.
```

- 确认 分区情况

```
Select (default p): w
Command (m for help): p
Disk /dev/sdb: 29.7 GiB, 31914983424 bytes, 62333952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x37ef716d

Device     Boot  Start      End  Sectors  Size Id Type
/dev/sdb1         2048   411647   409600  200M 83 Linux
/dev/sdb2       411648 62333951 61922304 29.5G 83 Linux
```

- 格式化分区

```
sudo mkfs.vfat /dev/sdb1
sudo mkfs.ext4 /dev/sdb2
```

- 更改分区信息

```bash
#注意，fatlabel对应fat，toune2fs 对应ext4，e2label对应 ext3,ext2.
sudo umont /dev/sdb1 
sudo fatlabel /dev/sdb1 boot
sodo umont /dev/sdb2
sudo tune2fs /dev/sdb2 -L rootfs
#可以查看分区信息
df -Th
```

## SUDO命令

如果 sudo命令提示：`sudo: /usr/bin/sudo must be owned by uid 0 and have the setuid bit set`无法进入root权限。

```
chown root:root /usr/bin/sudo
chmod 4755 /usr/bin/sudo
chmod 644 /usr/lib/sudo/sudoers.so
chown -R root /usr/lib/sudo
```


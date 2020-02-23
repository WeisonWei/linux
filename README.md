# Linux 常识

Linux 内核最初只是由芬兰人林纳斯·托瓦兹（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。
Linux 是一套免费使用和自由传播的类Unix操作系统，是一个基于 POSIX 和 UNIX 的多用户、多任务、支持多线程和多 CPU 的操作系统。
Linux 能运行主要的 UNIX 工具软件、应用程序和网络协议。它支持 32 位和 64 位硬件。
Linux 继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。

>Linux 的发行版说简单点就是将 Linux 内核与应用软件做一个打包。

Linux系统的启动过程可以分为5个阶段：

    内核的引导 --> 操作系统接管硬件以后，首先读入 /boot 目录下的内核文件
    运行init --> init进程是系统所有进程的起点，没有这个进程，系统中任何进程都不会启动，通过读取配置文件/etc/inittab启动不同的运行级别
    系统初始化 --> 激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行任务
    建立终端 --> init下来会打开6个终端，以便用户登录系统,在inittab中的以下6行就是定义了6个终端
    用户登录系统 --> 用户的登录方式有三种：（1）命令行登录 （2）ssh登录 （3）图形界面登录
    
## 1.系统目录
执行` ls / `后会看到系统目录；
这些目录的解释：

    /bin：
    bin是Binary的缩写, 这个目录存放着最经常使用的命令。

    /boot：
    这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。

    /dev ：
    dev是Device(设备)的缩写, 该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。

    /etc：
    这个目录用来存放所有的系统管理所需要的配置文件和子目录。

    /home：
    用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

    /lib：
    这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。

    /lost+found：
    这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

    /media：
    linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

    /mnt：
    系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

    /opt：
     这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

    /proc：
    这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
    这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：

    echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all

    /root：
    该目录为系统管理员，也称作超级权限者的用户主目录。

    /sbin：
    s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。

    /selinux：
     这个目录是Redhat/CentOS所特有的目录，Selinux是一个安全机制，类似于windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

    /srv：
     该目录存放一些服务启动之后需要提取的数据。

    /sys：
    这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。
    sysfs文件系统集成了下面3种文件系统的信息：针对进程信息的proc文件系统、针对设备的devfs文件系统以及针对伪终端的devpts文件系统。
    该文件系统是内核设备树的一个直观反映。
    当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

    /tmp：
    这个目录是用来存放一些临时文件的。

    /usr：
     这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。

    /usr/bin：
    系统用户使用的应用程序。

    /usr/sbin：
    超级用户使用的比较高级的管理程序和系统守护程序。

    /usr/src：
    内核源代码默认的放置目录。

    /var：
    这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

    /run：
    是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。

### 1.1 重要目录
在 Linux 系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。

/etc： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。
/bin, /sbin, /usr/bin, /usr/sbin: 这是系统预设的执行文件的放置目录，比如 ls 就是在/bin/ls 目录下的。
值得提出的是，/bin, /usr/bin 是给系统用户使用的指令（除root外的通用户），而/sbin, /usr/sbin 则是给root使用的指令。
/var： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，
具体在/var/log 目录下，另外mail的预设放置也是在这里。

### 1.2 程序安装目录
Linux 的软件安装目录是也是有讲究的，理解这一点，在对系统管理是有益的

/usr：系统级的目录，可以理解为C:/Windows/，/usr/lib理解为C:/Windows/System32。
/usr/local：用户级的程序目录，可以理解为C:/Progrem Files/。用户自己编译的软件默认会安装到这个目录下。
/opt：用户级的程序目录，可以理解为D:/Software，opt有可选的意思，这里可以用于放置第三方大型软件（或游戏），
当你不需要时，直接rm -rf掉即可。在硬盘容量不够时，也可将/opt单独挂载到其他磁盘上使用。

源码放哪里？
/usr/src：系统级的源码目录。
/usr/local/src：用户级的源码目录。

## 2.环境变量

### 2.1 Linux读取环境变量

    export  显示当前系统定义的所有环境变量
    echo $PATH  输出当前的PATH环境变量的值
其中PATH变量定义了运行命令的查找路径，以冒号:分割不同的路径，使用export定义的时候可加双引号也可不加。


### 2.2 Linux设置环境变量
#### 2.2.1 export PATH
使用export命令直接修改PATH的值
```bash
export PATH=/home/uusama/mysql/bin:$PATH
# 或者把PATH放在前面
export PATH=$PATH:/home/uusama/mysql/bin
```
注意事项：

    生效时间：立即生效
    生效期限：当前终端有效，窗口关闭后无效
    生效范围：仅对当前用户有效
    配置的环境变量中不要忘了加上原来的配置，即$PATH部分，避免覆盖原来配置

#### 2.2.2 vim ~/.bashrc
通过修改用户目录下的~/.bashrc文件进行配置：

```bash
vim ~/.bashrc
# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin

```
注意事项：

    生效时间：使用相同的用户打开新的终端时生效，或者手动source ~/.bashrc生效
    生效期限：永久有效
    生效范围：仅对当前用户有效
    如果有后续的环境变量加载文件覆盖了PATH定义，则可能不生效

#### 2.2.3 vim ~/.bash_profile
和修改~/.bashrc文件类似，也是要在文件最后加上新的路径即可：

```bash
vim ~/.bash_profile
# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

    生效时间：使用相同的用户打开新的终端时生效，或者手动source ~/.bash_profile生效
    生效期限：永久有效
    生效范围：仅对当前用户有效
    如果没有~/.bash_profile文件，则可以编辑~/.profile文件或者新建一个


#### 2.2.4 vim /etc/bashrc
该方法是修改系统配置，需要管理员权限（如root）或者对该文件的写入权限：

```bash
# 如果/etc/bashrc文件不可编辑，需要修改为可编辑
chmod -v u+w /etc/bashrc
vim /etc/bashrc
# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin

```
注意事项：

    生效时间：新开终端生效，或者手动source /etc/bashrc生效
    生效期限：永久有效
    生效范围：对所有用户有效

#### 2.2.5 vim /etc/profile
该方法修改系统配置，需要管理员权限或者对该文件的写入权限，和vim /etc/bashrc类似：

```bash
# 如果/etc/profile文件不可编辑，需要修改为可编辑
chmod -v u+w /etc/profile
vim /etc/profile
# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

    生效时间：新开终端生效，或者手动source /etc/profile生效
    生效期限：永久有效
    生效范围：对所有用户有效

#### 2.2.6 vim /etc/environment
该方法是修改系统环境配置文件，需要管理员权限或者对该文件的写入权限：

```bash
# 如果/etc/bashrc文件不可编辑，需要修改为可编辑
chmod -v u+w /etc/environment
vim /etc/profile
# 在最后一行加上
export PATH=$PATH:/home/uusama/mysql/bin
```

注意事项：

    生效时间：新开终端生效，或者手动source /etc/environment生效
    生效期限：永久有效
    生效范围：对所有用户有效


#### 2.2.7 总结
- 1.Linux环境变量加载原理

环境变量可以简单的分成用户自定义的环境变量以及系统级别的环境变量。

    用户级别环境变量定义文件：~/.bashrc、~/.profile（部分系统为：~/.bash_profile）
    系统级别环境变量定义文件：/etc/bashrc、/etc/profile(部分系统为：/etc/bash_profile）、/etc/environment

另外在用户环境变量中，系统会首先读取~/.bash_profile文件，
如果没有该文件则读取~/.bash_login，
如果也没有该文件，则读取~/.profile，
根据这些文件中内容再去读取~/.bashrc。


- 2.Linux环境变量加载顺序

可以推测出Linux加载环境变量的顺序如下：

    /etc/environment
    /etc/profile
    /etc/bash.bashrc
    /etc/profile.d/test.sh
    ~/.profile
    ~/.bashrc
系统环境变量 -> 用户自定义环境变量
/etc/environment -> /etc/profile -> ~/.profile
/.profile文件只在用户登录的时候读取一次，而/.bashrc会在每次运行Shell脚本的时候读取一次。

## 3.文件属性 -- 文件管理
```bash
[root@www /]# ls -l
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot
```
在Linux中第一个字符代表这个文件是目录、文件或链接文件等等。

    当为[ d ]则是目录
    当为[ - ]则是文件；
    若是[ l ]则表示为链接文档(link file)；
    若是[ b ]则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；
    若是[ c ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。

- 更改文件属性
 1、chgrp：更改文件属组 -->  chgrp [-R] 属组名 文件名   -R：递归更改文件属组
 2、chown：更改文件属主，也可以同时更改文件属组 --> chown [–R] 属主名 文件名 或 chown [-R] 属主名：属组名 文件名
 3、chmod：更改文件9个属性 -->   chmod [-R] xyz 文件或目录 或 chmod u=rwx,g=rx,o=r 文件名
 
chmod有两种设置方法，一种是数字，一种是符号
Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。
文件的权限字符为：『-rwxrwxrwx』
各权限的分数对照表如下：
 
     r:4
     w:2
     x:1
每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： [-rwxrwx---] 分数则是：

    owner = rwx = 4+2+1 = 7
    group = rwx = 4+2+1 = 7
    others= --- = 0+0+0 = 0     


## 4.用户和用户组
实现用户账号的管理，要完成的工作主要有如下几个方面：

    用户账号的添加、删除与修改。
    用户口令的管理。
    用户组的管理。 


添加用户：useradd 选项 用户名
选项:

    -c comment 指定一段注释性描述。
    -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
    -g 用户组 指定用户所属的用户组。
    -G 用户组，用户组 指定用户所属的附加组。
    -s Shell文件 指定用户的登录Shell。
    -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

删除帐号：userdel 选项 用户名    
-r，它的作用是把用户的主目录一起删除

修改帐号：usermod 选项 用户名 
-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样



用户口令的管理：passwd 选项 用户名
 可使用的选项：

    -l 锁定口令，即禁用账号。
    -u 口令解锁。
    -d 使账号无口令。
    -f 强迫用户下次登录时修改口令。

> 更多见：https://www.runoob.com/linux/linux-user-manage.html
        
## 5.磁盘管理
Linux磁盘管理常用三个命令为df、du和fdisk。

    df：列出文件系统的整体磁盘使用量
    du：检查磁盘空间使用量
    fdisk：用于磁盘分区

df [-ahikHTm] [目录或文件名] --> 检查文件系统的磁盘空间占用情况，可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息
选项与参数：

    -a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
    -k ：以 KBytes 的容量显示各文件系统；
    -m ：以 MBytes 的容量显示各文件系统；
    -h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
    -H ：以 M=1000K 取代 M=1024K 的进位方式；
    -T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
    -i ：不用硬盘容量，而以 inode 的数量来显示
    
du [-ahskm] 文件或目录名称 --> 查看使用空间，与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看
选项与参数：

    -a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
    -h ：以人们较易读的容量格式 (G/M) 显示；
    -s ：列出总量而已，而不列出每个各别的目录占用容量；
    -S ：不包括子目录下的总计，与 -s 有点差别。
    -k ：以 KBytes 列出容量显示；
    -m ：以 MBytes 列出容量显示；    

fdisk [-l] 装置名称   --> Linux 的磁盘分区表操作工具
选项与参数：

    -l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。

    
## 6.常用命令
## 7.vi/vim
## 8.shell


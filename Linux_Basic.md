登录Linux
---

### 命令提示符

- #: root用户
- $: 普通用户

### X window与文字模式的切换

[ctrl] + [alt] + [f(1..7)]

### 退出登录

$ exit

### 指令下达

command [-option]... [arg]... + [enter]

基础命令
---

#### date: 显示或设置系统日期和时间

**显示日期时间**
```
date [OPTION]... [+FORMAT]
```

**设置日期时间**
```
date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
```

#### sync: 数据同步写入硬盘

#### shutdown: 关闭||重启计算机
```
shutdown [OPTIONS...] [TIME] [WALL...]
```

##### 选项
```
-h: 暂停服务后关机
-r: 重启
-c: 取消关机||重启
```

##### TIME(执行时间)
```
now: 立刻
+m: 相对时间表示法，从命令提交开始多久之后
hh:mm: 绝对时间表示，指明具体时间
```

获取帮助
---

- command --help
- man [option]... command
- info [option]... command

#### man: 帮助手册(分章节号)

**章节号及意义**
```
1: 用户命令
2: 系统调用
3: C库调用
4: 设备文件及特殊文件
5: 配置文件格式
6: 游戏
7: 杂项
8: 管理类的命令
```
**按章节号查看帮助**
```
man (1..8) command
```

##### man下的操作(指令)
**翻页**
```
Space, ^V, ^f, ^F: 向文件尾翻屏
b, ^B: 向文件首部翻屏
d, ^D: 向文件尾部翻半屏
u, ^U: 向文件首部翻半屏
RETURN, ^N, e, ^E or j or ^J: 向文件尾部翻一行
y or ^Y or ^P or k or ^K：向文件首部翻一行
q: 退出
```
**文本搜索**
```
/keyword: 以KEYWORD指定的字符串为关键字, 从当前位置向文件尾部搜索; 不区分字符大小写
    n: 下一个
    N: 上一个
?keyword: 以KEYWORD指定的字符串为关键字, 从当前位置向文件首部搜索; 不区分字符大小写
    n: 下一个
    N: 上一个
```

##### whatis: 显示一行man手册也描述(同man -f command)
```
whatis command
```
**特征**
```
- 非实时
- 基于wahtis数据库
```
***更新whatis数据库***
```
# mandb
```

**文件权限与目录配置**
---

### 文件属性

```
-rw-------.     1         root  root    1689        Mar  3 08:54        anaconda-ks.cfg
 [访问权限]   [硬链接数]    [属主] [属组]   [文件大小]   [文件最近被修改的时间]  [文件名]
```

### 权限

```
    -       rw-              rw-                  rw
[文件类型]   [属主具备的权限]   [属组具备的权限]       [其他人具备的权限]
```

#### 文件类型

```
- (f): 普通文件
d: 目录文件
b: 块设备
c: 字符设备
l: 符号链接文件
p: 管道文件
s: 套接字文件；socket
```

#### 权限对于文件||目录的意义
**文件**
```
r (read): 可读取此一文件的实际内容, 如读取文本文件的文字内容等
w (write): 可以编辑&新增或者是修改该文件的内容(但不含删除该文件)
x (eXecute): 该文件具有可以被系统执行的权限
```

**目录**
```
r (read): 可查询该目录下的文件名数据
w (write): 可改变目录中的结构
x (eXecute): 可进入目录
```

### 权限变更(命令)

#### chown: 改变所属主[属组]
**仅改变属主**
```
chown [option]... OWNER FILE...
```

**仅改变属组**
```
chown [option]... :GROUP FILE...
```

**改变属主属组**
```
chown [option]... OWNER:GROUP FILE...
```

##### 选项
```
-R: 递归
```

#### chgrp: 改变所属组
```
chgrp [option]... GROUP FILE...
```

##### 选项
```
-R: 递归
```

#### chmod: 改变访问权限
##### 一般权限及相关符号描述
```
[符号]  [意义]  [数字描述]
r       可读       4
w       可写       2
x       可执行     1
a       所有人
u       属主
g       属组
o       其他人
+       添加
-       去除
=       赋予
```

**根据符号改变权限**
```
chmod [option]... (a|u|g|o)(+-=)(r|w|x) FILE...
```
**根据八进制数改变权限**
```
chmod [option]... OCTAL FILE...
```

##### 选项
```
-R: 递归
```

### SUID, SGID, STICKY
#### SUID
    (1) 任何一个可执行程序文件能不能启动为进程：取决发起者对程序文件是否拥有执行权限；
    (2) 启动为进程之后，其进程的属主为原程序文件的属主；

    权限设定：
        chmod u+s FILE...
        chmod u-s FILE...

#### SGID
    默认情况下，用户创建文件时，其属组为此用户所属的基本组；
    一旦某目录被设定了SGID，则对此目录有写权限的用户在此目录中创建的文件所属的组为此目录的属组；

    权限设定：
        chmod g+s FILE...
        chmod g-s FILE...

#### Sticky
    对于一个多人可写的目录，如果设置了sticky，则每个用户仅能删除自己的文件；

    权限设定：
        chmod o+t FILE...
        chmod o-t FILE...

### Linux的文件系统：
#### 根文件系统(rootfs)：
    root filesystem

#### LSB, FHS: (FileSystem Heirache Standard)
    /etc, /usr, /var, /root, /home, /dev

    /boot：引导文件存放目录，内核文件(vmlinuz)、引导加载器(bootloader, grub)都存放于此目录；
    /bin：供所有用户使用的基本命令；不能关联至独立分区，OS启动即会用到的程序；
    /sbin：管理类的基本命令；不能关联至独立分区，OS启动即会用到的程序；
    /lib：基本共享库文件，以及内核模块文件(/lib/modules)；
    /lib64：专用于x86_64系统上的辅助共享库文件存放位置；
    /etc：配置文件目录(纯文本文件)；
    /home/USERNAME：普通用户家目录；
    /root：管理员的家目录；
    /media：便携式移动设备挂载点；
    	cdrom
    	usb
    /mnt：临时文件系统挂载点；
    /dev：设备文件及特殊文件存储位置；
    	b: block device，随机访问
    	c: character device，线性访问
    /opt：第三方应用程序的安装位置；
    /srv：系统上运行的服务用到的数据；
    /tmp：临时文件存储位置；
    /usr: universal shared, read-only data；
    	bin: 保证系统拥有完整功能而提供的应用程序；
    	sbin:
    	lib：
    	lib64：
    	include: C程序的头文件(header files)；
    	share：结构化独立的数据，例如doc, man等；
    	local：第三方应用程序的安装位置；
    		bin, sbin, lib, lib64, etc, share

    /var: variable data files
    	cache: 应用程序缓存数据目录；
    	lib: 应用程序状态信息数据；
    	local：专用于为/usr/local下的应用程序存储可变数据；
    	lock: 锁文件
    	log: 日志目录及文件；
    	opt: 专用于为/opt下的应用程序存储可变数据；
    	run: 运行中的进程相关的数据；通常用于存储进程的pid文件；
    	spool: 应用程序数据池；
    	tmp: 保存系统两次重启之间产生的临时数据；

    /proc: 用于输出内核与进程信息相关的虚拟文件系统；
    /sys：用于输出当前系统上硬件设备相关信息的虚拟文件系统；
    /selinux: security enhanced Linux，selinux相关的安全策略等信息的存储位置；

#### Linux上的应用程序的组成部分：
    二进制程序：/bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin
    库文件：/lib, /lib64, /usr/lib, /usr/lib64, /usr/local/lib, /usr/local/lib64
    配置文件：/etc, /etc/DIRECTORY, /usr/local/etc
    帮助文件：/usr/share/man, /usr/share/doc, /usr/local/share/man, /usr/local/share/doc


























快捷键
---

- [tab]: 命令||参数补全
- #[tab] + [tab]: 显示以#开头的所有命令||参数
- [ctrl] + c: 终止当前程序运行

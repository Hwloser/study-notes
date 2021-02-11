### cat /etc/passwd

huanwei:x:1000:1000:Ubuntu,,,:/home/huanwei:/bin/bash

以`:`为分隔符

1. 用户名
2. 密码(以x为占位符)
3. UID 用户编号，用户编号用以计算权限
4. 组id
5. 对用户的说明
6. 当前用户的家目录
7. 当前使用shell的是谁(bash:Bourne-again shell)
	1. /usr/sbin/nologin
	2. /bin/false
	3. 这些表示当前用户不允许登录，安装某些服务的时候需要的用户身份

### sudo vim /etc/shadow(su查看)

huanwei:。。。/:18141:0:99999:7:::

1. 用户名
2. 密码(加密之后的形式)
3. 密码设置时间戳(day)
4. 下一次密码的上修改时间

### sudo cat /etc/group

adm:x:4:syslog,huanwei

1. 组的名字
2. 密码
3. 组的编号
4. 组员

### ls

ls -R 递归显示指定目录的所有子文件及子目录

ls -t 按照时间对文件排序

ls -l 
```
-rw-rw-r--  1 huanwei huanwei   51 Sep  3 18:45 a.txt

			链接号(拥有几个别名)

```

### file 文件名

查看文件类型

### ls -F

显示文件类型

@链接文件 

*可执行文件

none普通文件

### touch

创建文件和修改文件的最后一次修改时间

### mkdir -p a/b/c/d

默认情况下mkdir只能创建单级目录，-p

### ln [-s] 原始链接的文件 目标文件

加入-s时软连接，不加就是硬链接

### man command

man -k ..

### 

`>`重定向覆盖

`>>`重定向追加

### 

awk 语言解释器，针对单行字符串(文本)处理

`-F`标准分割

### 

sort 排序

`-u`去掉重复的

###

head

tail

### diff [-U] filename1 filename2	比较两个文件是否相等

### vi屏幕分屏

:split	横向切割

:vsplit	纵向切割

切换光标到其他屏**crtl+w+w**

:open fliepath	引入新的文件的

：wall	保存**所有**窗口的且不关闭

:close	关闭光标所在的窗口

### file	查看文件类型

### echo [-e(使得输出内容中的转义字符生效)][-n(剔除输出内容中的回车换行符)]	输出内容到(自定义字符串或系统变量)屏幕

### script [-a] filename 记录用户对filename的操作

`-a`追加效果

### vi

kjhi上下左右

i	在光标前插入

a	在光标后插入

o	

command mode：

x	删除光标后的一个字符

dw	删除光标后的一个单词

3dw	[3是任意的]	删除3(任意的单词)

dd	删除当前行

3dd	删除3行

:3,5d	从第3行删除到第5行闭区间

### 

r	替换光标后的一个单词

cw	替换一个单词

cc	替换一行

###

yw	拷贝词

yy	拷贝行

p	当前行向下粘贴

:1,20co3	将第1~20行拷贝到第3行后

:1,3m9		将

###

~	大小写转换

J	把两行连成一行

u	撤销

:set nu	设置行代码

:21	将光标直接跳到21行

21G	将光标跳到21行

/String	查询字符串	n是next

?String	查询字符串	n是next

：r	file	在光标的下一行位置插入另一个文件

:1,$s/StringOld/StringNew/g		$表示行末:g表示全文搜索

   %s/StringOld/StringNew/g		%表示行末:g表示全文搜索

###

id	显示用户信息

id	[用户]

###

who	查看当前服务器有哪些终端连入

w	详细查看


###

finger	远程查看其他计算机中有哪些终端连入

前提：服务器由支持finger服务

###

find  [-H]  [-L]  [-P]  [-D  debugopts]  [-Olevel]  [starting-point...] [expression]

find 目录 表达式 [action]

对指定的目录进行递归查找

-mtime 10	修改时间
 -pring	-打印到屏幕

-user	用户名		-user后可跟用户的名字和UID

-size	文件的大小：+表示大于指定的大小

-perm	基于权限查找

-type	基于文件类型查找	跟d：目录	f普通文本文件

-atime	基于访问时间查找	+表示大于n day

-exec	将前面的结果追加到命令后命令后面的{}占位中

xargs	把前面命令执行的结果(作为参数)交给后面执行的命令

perl	针对一个文件或者多个文件做模式处理	

-p	针对字符串进行查找替换	

-i.bak	模式处理之前对原始文件备份	

-e表示执行""中的代码

### grep

基于关键字查找一行或多行

-i	检索的关键字忽略(不区分)大小写

-v	检索出关键字不匹配的行

### wc

统计文件的行，单词或字符数

### ps

-e

-f	全部

### kill	[-信号]	process-id

send a signal to a process

### sleep time	&

&会使进程进入后台状态

### pkill	sleep

将sleep的进程都kill

### jobs

作业控制,控制当前shell提交的进程

fg	[%n]	把后台进程编程前台进程，不加%n会默认显示jobs最后一个进程

ctrl+z	将前台进程转换为后台进程

ctrl+c	终止进程

bg	%n	让最后一个stopped进程进入Running状态

kill	%n	终止n进程

kill	-9	PID

##

/etc/init.d/	操作目录启动时的初始化目录，只能放shell脚本(*.sh)

##

ping host[ip/domin]

ifconfig	查看IP

netstat	-rn	联网到某个目的地，经过的网关

traceroute	IP	跟踪到目的地之间经过的所有网关

telnet

ssh	

rlogin

##

ftp	文件的上传和下载

在ftp中执行linux命令		！+Linux命令

切换目录的时候在本机执行使用lcd

###

退出ftp服务：bye,quit

get,put,mget,mput

!ls dir	查看远程计算机中dir中的内容

hash	显示进度条

prompt	提示

##	mail服务

write	用户名

mesg -y|-n	是否禁止其他用户发消息

wall	广播，一般管理员使用，wall的内容来源于管道或者文件

##	shell脚本

crontab	按照计划执行任务

crontab -l	列出所有的计划

crontab	-e	设置执行时间

m	h	dom	mon	dow	command
分钟	小时	第几天	第几个月	
1，2	8-17	*		*		
1. 命令必须是绝对路径
2. 

crontab	-r	删除计划任务

## /etc/profile

设置环境变量的几种方式：

1. 变量=变量值
2. export 变量=变量值		（全局）

可以将命令执行的结果赋给环境变量：
``（英文中的tab键上的感叹号旁的）

## which命令

which从PATH中找命令，只要找寻到符合条件的，直接结束。

而whereis则是找出所有的命令。

## history命令

显示先前输入的命令

!n 执行第n条命令

!!上一条

## 给命令(单一、组合)起别名	alias命令

alias 别名=命令

命令是组合的情况下加单引号

例如：
1. alias list='ls -l'

2. alias home='cd;ls'

### 删除别名：

unalias 别名

## EDITOR=vi

在环境变量中加入此命令会默认使用vi编辑器

##scp

linux计算机之间文件拷贝

sudo 用户名@ip：文件(路径）             拷贝文件的路径

##linux用户管理

### groupadd [-g 组号] 组的名字

groupadd -g 30000 huanwei

相当于在/etc/group中增加

`huanwei:x:30000:`

### 删除组

groupdel 组名

### groupmod

修改组的名字

groupmod -n 新名字 旧名字

groupmod -g 新的组的编号 组的名字

### gpasswd [] 组名

gpasswd -a user 组名

添加user到group

gpasswd -d user 组名

删除组名中的user

## 用户管理

useradd [-option] 用户名

-u UID

-g 组名

-d 路径

-m 建立用户目录

sudo -m -d /home/hw -u 8888 -g root  -s /bin/bash

### 删除用户

userdel user

### 修改用户信息

usermod -l 新名字 旧名字

修改家目录的位置

usermod -d dirPath user

## linux文件系统的管理

### chown [-R]

-R，针对目录，递归修改目录下的所有文件及子目录；修改文件夹的时候同时修改目录下的文件的拥有者

chown user file	修改file的拥有者

### chgrp [-R] 所属组名 文件/目录

chgrp huanwei s.sh 

## linux中的NFS服务管理(文件共享)

1. 安装
2. vim /etc/exports

/srv/nfs4（共享目录）        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
```
* 共享给哪一个主机
* 表示所有计算机
若果是具体计算机写客户端
ro	只读
rw	可读写
root_
()表示权限
rw读写操作
sync不缓存
no_subtree_check 操作共享目录中的子目录，不检查父目录的权限

root_squash	->	将root用户或其所属组映射为匿名用户	-》	nobody
no_root_squash	->	Root
all_squash	->	nobody
no_all_squash	->	服务器用户UID和客户端用户UID相等的用户身份

```
3. 发布

exportfs -a	全部挂在或者卸载 

[-v] [-u卸载]
### 查看服务器端发布的共享文件

showmount -e 服务器端的ip

showmount -e 192.168.32.130

showmount -d ipaddress

### 将共享目录挂在在本机的。。目录中

mount -t 挂在的类型 共享文件服务器ip：共享目录(最好绝对路径) 本地挂在目录

umount dir解除挂载

### 启动时挂载

/etc/fstab

<file system> <mount point>   <type>  <options>       <dump>  <pass>

共享服务器ip:共享目录	本地挂载点	挂载方式(nfs)	选项	<dump>	<pass>

选项：
1. hard	不停的挂载
2. intr	请求没有响应，允许失败一次

dump:表示软件，数据是否备份	直接写0（即不备份）

pass:默认直接写0	如果共享目录是/，写1
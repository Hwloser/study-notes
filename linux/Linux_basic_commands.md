## 修改权限

chmod (change mode):符号法和八进制数字法。

### 符号法

`chmod [u\g\o\a] [+\-\=] [r\w\x] fileName`
```
u (user)   表示用户本人。 
g (group)  表示同组用户。 
o (oher)   表示其他用户。 
a (all)    表示所有用户。 
+          用于给予指定用户的许可权限。 
-          用于取消指定用户的许可权限。 
=          将所许可的权限赋给文件。 
r (read)   读许可，表示可以拷贝该文件或目录的内容。 
w (write)  写许可，表示可以修改该文件或目录的内容。 
x (execute)执行许可，表示可以执行该文件或进入目录。
```

### 八进制数字法

`chmod [abc] fileName`
`chmod [741] fileName` 

```
其中a,b,c各为一个八进制数字，分别表示User、Group、及Other的权限。 
4 (100)    表示可读。 
2 (010)    表示可写。 
1 (001)    表示可执行。 
若要rwx属性则4+2+1=7； 
若要rw-属性则4+2=6； 
若要r-x属性则4+1=5。 
```
```
例如：
# chmod a+rx filename 
	让所有用户可以读和执行文件filename。 
# chmod go-rx filename 
	取消同组和其他用户的读和执行文件filename的权限。 
# chmod 741 filename 
	让本人可读写执行、同组用户可读、其他用户可执行文件filename。 
# chmod -R 755 /home/oracle 
	递归更改目录权限，本人可读写执行、同组用户可读可执行、其他用户可读可执行 
```

## 查看CPU

cat /proc/cpuinfo

## 查看内存

cat /proc/meminfo

## 配置环境变量

1. 修改/etc/profile文件，将影响全局，所有用户。/etc/profile在系统启动后第一个用户登录时运行。在/etc/profile文件中添加
```
export PATH=/someapplication/bin:$PATH
```
要使修改生效，可以重启系统，或者执行
```
source /etc/profile
echo $PATH
```
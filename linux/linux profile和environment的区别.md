# linux profile和environment的区别

## profile

只有Login shell 启动时才会运行 /etc/profile 这个脚本，而Non-login shell 不会调用这个脚本。

说明：当一个用户登录Linux系统或使用su -命令切换到另一个用户时，也就是Login shell 启动时，首先要确保执行的启动脚本就是 /etc/profile 。

在/etc/profile 文件中设置的变量是全局变量。

在/etc/profile.d 目录中存放的是一些应用程序所需的启动脚本，而这些脚本文件是用来设置一些变量和运行一些初始化过程的。其中包括了颜色、语言、less、vim及which等命令的一些附加设置。

/etc/profile.d 下的脚本之所以能自动执行，是因为在/etc/profile 中有一个for循环语句来调用这些脚本。

## 在登录Linux时要执行文件的过程如下

在刚登录Linux时，首先启动 /etc/profile 文件，然后再启动用户目录下的 ~/.bash_profile、 ~/.bash_login或 ~/.profile文件中的其中一个(根据不同的linux操作系统的不同，命名不一样)

执行的顺序为：~/.bash_profile、 ~/.bash_login、 ~/.profile

如果 ~/.bash_profile文件存在的话，一般还会执行 ~/.bashrc文件因为在 ~/.bash_profile文件中一般会有下面的代码： if [ -f ~/.bashrc ] ; then . ./bashrc fi ~/.bashrc中，一般还会有以下代码： if [ -f /etc/bashrc ] ; then . /bashrc fi 所以，~/.bashrc会调用 /etc/bashrc文件。最后，在退出shell时，还会执行 ~/.bash_logout文件。

执行顺序为：/etc/profile -> (~/.bash_profile | ~/.bash_login | ~/.profile) -> ~/.bashrc -> /etc/bashrc -> ~/.bash_logout

## /etc/profile和/etc/environment等各种环境变量设置文件的用处

/etc/environment是设置整个系统的环境，而/etc/profile是设置所有用户的环境，前者与登录用户无关，后者与登录用户有关。

系统应用程序的执行与用户环境可无关但与系统环境是相关的，对于用户的SHELL初始化而言是先执行/etc/profile，再读取文件/etc/environment。对整个系统而言是先执行/etc/environment

/etc/enviroment --> /etc/profile --> \$HOME/.profile -->​ \$HOME/.env (如果存在)

/etc/profile 是所有用户的环境变量

/etc/enviroment是系统的环境变量

登陆系统时shell读取的顺序应该是/etc/profile ->/etc/enviroment -->\$HOME/.profile -->\$HOME/.env

如果同一个变量在用户环境(/etc/profile)和系统环境(/etc/environment)有不同的值那应该是以用户环境为准了。
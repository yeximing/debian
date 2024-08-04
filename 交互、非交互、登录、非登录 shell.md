
一、交互shell 和 非交互shell

1、交互shell

交互式：顾名思义就是 shell 与用户存在交互，
用户登录后，在终端上输入命令，shell 立即执行用户提交的命令。

当用户退出后，shell 也终止了。

2、非交互shell

非交互式：即 shell 与用户不存在交互，而是以 shell script 的方式执行的。

shell 读取存放在文件中的命令, 并且执行它们。
当它读到文件的结尾 EOF，shell 也就终止了。

3、区分方法

可以通过打印 $- 变量的值，并查看其中的 i - interactive 选项来区分交互式与非交互式shell。
比如

`
```
yeximing@yeximing-laptop:~$ echo $-
himBHs
```

有 i - interactive ，所以是 交互式 shell；反之则为非交互式。


二、登录shell 和 非登录shell

1、登录shell

登录 shell 是指需要用户名、密码登录后进入的 shell，或者通过 --login 选项生成的 shell 。

2、非登录shell

非登录 shell 是指不需要输入用户名和密码即可打开的 shell，
比如输入命令 bash或者sh 就能进入一个全新的非登录 shell，
在 Gnome 或 KDE 中打开一个 “terminal” 窗口，也是一个非登录 shell。

3、区分方法

如何区分登录 shell 和非登录 shell 呢，可以通过查看 $0 的值，登录 shell 返回 -bash，而非登录 shell 返回的是 bash 。

需要注意的是：
执行 exit 命令， 退出的 shell 可以是登录 或者 非登录 shell ；
执行 logout 命令，则只能退出登录 shell，不能退出非登录 shell 。

```
yeximing@yeximing-laptop:~$ echo $0
bash
yeximing@yeximing-laptop:~$ logout
bash: logout: not login shell: use `exit'
```


三、四种 shell 在调用上的区别

交互/非交互/登录/非登录 shell 这些组合情况， 在脚本的调用方面有区别；而且对于 bash 与 sh 也存在差异；

以下分情况说明具体的调用情况（假如所执行脚本名为 test.sh ）

**bash**


1、交互式的登录shell （bash –il test.sh）

载入的信息：
/etc/profile
~/.bash_profile（ -> ~/.bashrc -> /etc/bashrc）
~/.bash_login
~/.profile

2、非交互式的登录shell （bash –l test.sh）

载入的信息：
/etc/profile
~/.bash_profile （ -> ~/.bashrc -> /etc/bashrc）
~/.bash_login
~/.profile
$BASH_ENV

3、交互式的非登录shell （bash –i test.sh）

载入的信息：
~/.bashrc （ -> /etc/bashrc）

4、非交互式的非登录shell （bash test.sh）

载入的信息：
$BASH_ENV

**sh**


1、交互式的登录shell （sh –il test.sh）

载入的信息：
/etc/profile
~/.profile

2、非交互式的登录shell （sh –l test.sh）

载入的信息：
/etc/profile
~/.profile

3、交互式的非登录shell （sh –i test.sh）

载入的信息：
$ENV

4、非交互式的非登录shell （sh test.sh）

载入的信息：
nothing

综上可知，
交互/非交互/登录/非登录，这四种 shell 主要区别在于：是否载入相关配置文件！

这些配置的载入与否，导致了 Linux 很多默认选项的差异。
————————————————
版权声明：本文为CSDN博主「一得同学」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44648216/article/details/104056712
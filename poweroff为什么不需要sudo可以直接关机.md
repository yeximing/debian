**https://yaqia.github.io/blogs/poweroff_permission.html**


# 在Arch Linux中，普通用户也可以直接使用poweroff，是如何实现的？

## 个人探索过程

首先通过

```
which poweroff | xargs file
```

发现了poweroff的文件类型是`systemctl`的软链接：

```
/usr/bin/poweroff: symbolic link to systemctl
```

而`systemctl`则是一个在`/usr/bin`目录下、普通用户拥有可执行权限的指令

在此时突然想到了尝试用`strace`了解其详细调用过程，但执行后发现其中没有特殊的syscall，大多是常见的系统函数

由于`systemctl`是管理系统服务的，故而又想到了尝试搜索poweroff.service

```
~ > locate poweroff.service
/usr/lib/systemd/system/systemd-poweroff.service
/usr/share/man/man8/systemd-poweroff.service.8.gz
```

> 按照以往使用systemctl的常识，对服务的开启和关闭应当是root用户的权限，但在此处普通用户不使用sudo提权就能poweroff，这是为什么？

## 查找资料

通过查阅资料发现[一篇解释](https://unix.stackexchange.com/questions/209766/what-are-the-default-polkit-privileges-on-arch-linux-for-shutdown-halt-etc-a)

资料中提及了如下内容：

> Commands `poweroff` and `reboot` work in two steps:
> 
> if invoked under a non-root-user and logind is available, logind is asked to perform the action, using polkit actions org.freedesktop.login1.*;

它会利用logind守护进程利用`polkit`来执行些什么

[那`polkit`是什么？](https://wiki.archlinux.org/title/Polkit)

> polkit提供了非特权进程向特权进程通信的方法，它以action的方式进行通信
> 
> 常见的action组中就包括了systemd-logind守护进程的actions
> 
> 每一个action都以一个.policy文件来定义

查找资料至此，已经能够确定如何查看action内部信息了

```
~ > locate org.freedesktop.login1.policy | xargs file
/usr/share/polkit-1/actions/org.freedesktop.login1.policy: XML 1.0 document, ASCII text
```

可以发现它是XML文档，其中的action定义了相关的内容

```
<action id="org.freedesktop.login1.inhibit-block-shutdown">
        <description gettext-domain="systemd">Allow applications to inhibit system shutdown</description>
        <message gettext-domain="systemd">Authentication is required for an application to inhibit system shutdown.</message>
        <defaults>
                <allow_any>no</allow_any>
                <allow_inactive>yes</allow_inactive>
                <allow_active>yes</allow_active>
        </defaults>
        <annotate key="org.freedesktop.policykit.imply">org.freedesktop.login1.inhibit-delay-shutdown org.freedesktop.login1.inhibit-block-sleep org.freedesktop.login1.inhibit-delay-sleep org.freedesktop.login1.inhibit-block-idle</annotate>
</action>
```

继续在Arch Wiki中查找得到了allow_any、allow_inactive、allow_active设置选项的解释：

> Both `inactive` and `active` here refer to local sessions on local consoles or displays, whereas the `allow_any` setting is used for all others, including remote sessions (SSH, VNC, etc.).

如此一来，就能完整地解释一个桌面普通用户为什么能本地直接poweroff，而服务器的远程ssh普通用户则没有这个权限

解决方案：
# 对于fcitx5

在`/etc/environment`里添加相关的环境变量：

```bash

   # 解决firefox无法输入中文的问题

   XIM=fcitx5

   XIM_PROGRAM=fcitx5
   # 下面三项后面不带5即fcitx
   # 可以解决原生Linux微信无法输入中文的问题
   GTK_IM_MODULE=fcitx5

   QT_IM_MODULE=fcitx5

   XMODIFIERS=@im=fcitx5

   SDL_IM_MODULE=fcitx5

   # 解决kitty无法输入中文的问题

   GLFW_IM_MODULE=ibus

```

最后一项如果也是写 fcitx5 的话，解决不了 kitty 无法输入中文的问题。根据[这个 issue](https://github.com/kovidgoyal/kitty/issues/469)，kitty 作者只给 ibus 添加里 GLFW 支持，没有给 fcitx 添加，所以最后一项写 ibus.fcitx 会模拟 Ibus.

# 对于ibus

在`/etc/environment`里添加相关的环境变量：
```bash
   # 解决kitty无法输入中文的问题
   GLFW_IM_MODULE=ibus
```

在`/usr/share/applications/kitty.desktop`里的`exec`参数：
```bash
ec=GLFW_IM_MODULE=ibus kitty 
```


之后再`source /etc/environment`即可。（建议先修改权限，可以无脑`sudo chmod 777 /etc/environment`，之后记得改回权限。再进入 root 用户后执行）。

参考链接：

[解决 Linux 使用 fcitx5 输入法无法输入问题](https://www.bilibili.com/read/cv21085970/)

[这个 issue](https://github.com/kovidgoyal/kitty/issues/469)

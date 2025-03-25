# 系统中文

如果安装系统时是英文，那么需要给系统添加中文显示。

1. 安装中文字体：

```bash
sudo apt install fonts-wqy-zenhei
```

2. 安装 locales：

```bash
sudo apt install locales
```

3. 设置中文：

```bash
vim /etc/locale.gen

vim /etc/locale.conf
# 取消掉LANG=zh_CN.UTF-8前的注释“#”号
```

4. 使其生效：

```bash
sudo dpkg-reconfigure locales
```

# 安装中文输入法

## 安装中文引擎 rime

```bash
sudo apt install ibus-rime
```

安装之后重启，否则系统识别不到。

## 配置安装

在 Settings > Keyboard 中的 InputSources 中添加输入源, 选择 Chinese > Chinese(Rime) 即可。

## 配置输入法

打开 Tweaks > Keyboard & Mouse > Show Extended Input Sources

使用[rime-auto-deploy](https://github.com/Mark24Code/rime-auto-deploy)安装[雾凇拼音](https://github.com/iDvel/rime-ice)。

---

参考链接 🔗：

1. [设置中文界面](https://www.cnblogs.com/shanhubei/p/17517381.html)
2. [设置中文输入法](https://juejin.cn/post/7260795888338583610)

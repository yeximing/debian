为每个应用添加以下命令即可

```bash
flatpak override --user --env=GTK_IM_MODULE=fcitx --env=QT_IM_MODULE=fcitx --env=XMODIFIERS=@im=fcitx --filesystem=xdg-config/fcitx com.tencent.WeChat
```

最后的应用名需要是完整的。

英文字体自带的中文字体往往一言难尽，所以我决定再不改变英文字体的情况下，修改中文字体。

以下是修改中文字体为**微软雅黑**的方法：

---

- 首先是 VSCODE：
  在它的`Setting`中搜索`Font`，找到`Editor:Font Family`，修改为`FiraMono,'YaHei',monospace`。中间是你要修改的中文字体，前提是你的电脑上有这个字体。
  重启 VSCODE。

---

- 其次是终端：
  我使用的是 kitty,`vim ~/.config/kitty/kitty.conf`后，修改里面的内容
  ```bash
  # 设置中文字体
   font_family "YaHei"
  ```
  重启 kitty 即可。

---

- 最后是 Gnome:
  ```bash
  # 没有此路径或文件可以新建
  vim ~/.config/fontconfig/fonts.conf
  ```
  添加以下内容：
  ```xml
  <?xml version="1.0"?>
   <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
   <fontconfig>
       <match target="pattern">
           <test qual="any" name="family">
               <string>sans-serif</string>
           </test>
           <edit name="family" mode="prepend" binding="strong">
               <string>FiraMono Nerd Font</string> <!-- 英文字体 -->
           </edit>
       </match>
       <match target="pattern">
           <test qual="any" name="lang">
               <string>zh</string>
           </test>
           <edit name="family" mode="prepend" binding="strong">
               <string>YaHei</string> <!-- 中文字体 -->
           </edit>
       </match>
   </fontconfig>
  ```
  注销，重新登录即可。


ls发现有些文件夹有绿色的背景，非常突兀，所以尝试给他取消掉。
`echo $LS_COLORS`后发现：`ow=34;42`，也就是蓝字绿底。
直接改成`ow=34`（后面的42是绿底的意思），发现成功绿。

那就在`zshrc`里`export LS_COLORS='ow=34'`，再`source ~/.zshrc`就好了。
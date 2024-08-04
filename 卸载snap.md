
1、首先我们通过 snap list 命令列出所有已经安装的snap软件包.

snap list

2、你可以看到例如如下内容：

$ snap list
Name                       Version          Rev    Tracking         Publisher   Notes
bare                       1.0              5      latest/stable    canonical✓  base
core18                     20231027         2812   latest/stable    canonical✓  base
core20                     20240227         2264   latest/stable    canonical✓  base
core22                     20240111         1122   latest/stable    canonical✓  base
firefox                    124.0-1          3972   latest/stable/…  mozilla✓    -
gnome-3-38-2004            0+git.efb213a    143    latest/stable/…  canonical✓  -
gnome-42-2204              0+git.510a601    176    latest/stable/…  canonical✓  -
gtk-common-themes          0.1-81-g442e511  1535   latest/stable/…  canonical✓  -
snap-store                 0+git.3a85550    1110   latest/stable/…  canonical✓  -
snapd                      2.61.2           21184  latest/stable    canonical✓  snapd
snapd-desktop-integration  0.9              157    latest/stable/…  canonical✓  -

3、然后你就通过 snap remove 命令逐个删除它们，如果包很多，也可以写个脚本批量删除。

sudo snap remove firefox

或者使用脚本

for p in $(snap list | awk '{print $1}'); do
  sudo snap remove $p
done;

4、删除Snap守护进程

键入运行以下命令以停止 snap 服务。

sudo systemctl stop snapd

然后运行以下命令来禁用它。

sudo systemctl disable snapd

最后，运行以下命令来掩盖快照。这可以防止在您的系统上启动或启用 snapd 服务。

sudo systemctl mask snapd

然后键入以下命令，使用 apt 清除快照

sudo apt purge snapd -y

键入以下命令将 snapd 软件包标记为已保留，这会阻止它被apt软件包管理器自动升级。

sudo apt-mark hold snapd

5、删除Snap软件包目录

sudo rm -rf ~/snap
sudo rm -rf /snap
sudo rm -rf /var/snap
sudo rm -rf /var/lib/snapd

6、防止Snap重新安装

sudo nano /etc/apt/preferences.d/nosnap.pref

然后添加这些行，就像你在这里看到的一样。

Package: snapd
Pin: release a=*
Pin-Priority: -10

然后键入以下命令来更新apt源列表。

sudo apt update

就是这样，我们已经成功从 Ubuntu 中删除了所有 snap 软件包。
*使用 apt 安装 deb 包的 Firefox *

用命令来创建一个 apt keyrings 。

sudo install -d -m 0755 /etc/apt/keyrings

运行此命令导入 Mozilla apt 存储库签名密钥。

wget -q https://packages.mozilla.org/apt/repo-signing-key.gpg -O- | sudo tee /etc/apt/keyrings/packages.mozilla.org.asc > /dev/null

运行此命令将 Mozilla 签名密钥添加到 apt 源列表中。

echo "deb [signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main" | sudo tee -a /etc/apt/sources.list.d/mozilla.list > /dev/null

运行此命令，配置 apt 来优先处理来自 Mozilla 存储库的软件包。

echo '
Package: *
Pin: origin packages.mozilla.org
Pin-Priority: 1000
' | sudo tee /etc/apt/preferences.d/mozilla

最后，键入命令以更新 apt 源列表并安装Firefox deb包。

sudo apt update && sudo apt install firefox

至此，你就可以使用 apt 安装需要的deb软件包了。
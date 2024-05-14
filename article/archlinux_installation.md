---
title: 安装archlinux系统
date: 2020-11-22 20:22:22
categories:
  - linux
tags:
  - system
---

## 制作 u 盘

- rufus
- 进入 bios,选择 u 盘启动

## 连接 wifi

- 查看 wifi 设备：`ip link`

- 启用设备：`ip link set 设备 up`
- 扫描查看 wifi 名称：`iwlist 设备 scan | grep ESSID`

或者使用

`iw dev scan ....`

或者使用 usb 网络，安装系统后记得安装驱动`pacman -S usbmuxd`,live 系统中自带有所以不用装，但新安装的系统中没有

### 连接 wpa 加密 wifi

`wpa_passphrase wifi名称 密码 > internet.conf`

`wpa_supplicant -i 网卡 -c internet.conf &`

`dhcpcd 网卡 &`

## 检查是否能上网

(ctrl+c 停止)

`ping www.baidu.com`

## 更新系统时钟

`timedatectl set-ntp true`

## 分区

> 双系统不用分 boot 启动分区，只需要根分区、家分区、交换分区 swap。linux 的 boot 分区直接挂载在 windows 的 efi 分区上

- 查看当前的可用磁盘：`fdisk -l`

### 使用 cgdisk 分区

- 选择硬盘：`cgdisk /dev/nvme0n1`
- 选择`NEW`进行分区：开始位置（默认），大小(40G) ,编号（默认）
- swap 分区编号为 8200
- 最后选择 write

### 使用 fdisk 分区

- 创建新分区：`fdisk /dev/sda    //注意磁盘名称`

```bash
--\>n -->回车 -->回车 -->回车 -->+500m  （boot分区）

--\>n -->回车 -->回车 -->回车 -->+6G      （根分区）

--\>n -->回车 -->回车 -->回车 -->回车  （将剩余空间全部分配给该分区）

--\>w    （写入）
```

## 根目录格式化为 ext4

请注意自己的对应的目录是那块区域

```bash
 mkfs.fat -F32 /dev/sda1     /// boot分区
 mkswap /dev/sda2   //swap 分区
 mkfs.ext4 /dev/sda3
```

## 打开 swap 分区

请注意自己的 swap 分区是哪块

`swapon /dev/sda2`

## 更改 pacman 配置文件

`vim /etc/pacman.conf`

去掉 Color 的注释,用于安装时提示信息

## 挂载分区

- 双系统

> 先挂载根分区，再启动分区，在其他分区

```bash
#根分区
mount /dev/nvme0n8 /mnt
#启动分区，挂载在win系统efi分区上
mkdir /mnt/boot
mount /dev/nvme0n1p1 /mnt/boot
#swap分区
swapon /dev/nvme0n1p7
#home分区...
mkdir /mnt/home
mount /dev/nvme0n1p6 /mnt/home
```

- 单系统

```bash
#根分区
mount  /dev/sda3 /mnt
ls /mnt  /////查看是否挂载成功，若出现lost+found 则代表挂载成功
#启动分区
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot      /// boot分区
```

## 配置源

将国内源放到第一位
`vim /etc/pacman.d/mirrorlist`

- 自动查找源：`reflector --country China --age 72 --sort rate --protocol https --save /etc/pacman.d/mirrorlist`
- 手动换源：

```text
## Country : China
Server = https://mirrors.sjtug.sjtu.edu.cn/manjaro/stable/$repo/$arch

## Country : China
Server = https://mirrors.bfsu.edu.cn/manjaro/stable/$repo/$arch

## Country : China
Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch

## Country : China
Server = https://mirrors.ustc.edu.cn/manjaro/stable/$repo/$arch

## Country : South_Korea
Server = https://mirror.funami.tech/manjaro/stable/$repo/$arch

## Country : Russia
Server = https://mirror.kamtv.ru/manjaro/stable/$repo/$arch
```

- 刷新源:`pacman -Syy`

## 安装基本包

`pacstrap /mnt linux linux-firmware base base-devel`

- 普通内核：`linux`,`linux-headers`
- lts 稳定内核：`linux-lts`,`linux-lts-headers`
- 高性能内核：`linux-zen`,`linux-zen-headers`（不支持 nvidia 显卡）

## 生成 fstab 文件

- 开机时，系统会安装 fstab 文件的分区信息进行挂载：`genfstab -U /mnt >> /mnt/etc/fstab`

- 后续可以手动修改,用于挂载 win 盘：`UUID=xxxxxxxxxxx /home/xxx/xxx ntfs-3g defaults 0 0`

## 切换环境

`arch-chroot /mnt`

## 安装 vim

`pacman -S vim`

## 设置语言

- 将/etc/locale.gen 中 en_US.UTF-8 的注释去掉,避免中文乱码,推荐先使用英文：`vim /etc/locale.gen`
- 执行：`locale-gen`

- 配置 /etc/locale.conf 文件：`vim /mnt/etc/locale.conf`

- 写入：`LANG=en_US.UTF-8`

## 配置时区

`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

## 硬件时间

`hwclock --systohc`

## 设置主机名

`vim /etc/hostname`

## 重置 root 密码

`passwd`

## 安装网络相关的包

- 动态 Ip：`pacman -S dhcpcd`
- USB 网络：`pacman -S usbmuxd`
- 基本网络连接工具：`iw wpa_supplicant`
- 网络管理工具(多个管理工具之间相互冲突，安装一个即可)：`pacman -S  networkmanager dhcpcd（可不装）`

  - 启动服务：`systemctl enable NetworkManager.service`
  - 图形化界面：`nm-connection-editor`
  - 系统托盘：`network-manager-applet`
  - 使用：
    NetworkManager 附带 nmcli 和 nmtui。

  ```bash
  # 显示附近的 Wi-Fi 网络：
  $ nmcli device wifi list

  # 连接到 Wi-Fi 网络：
  $ nmcli device wifi connect SSID_或_BSSID password 密码

  # 连接到隐藏的 Wi-Fi 网络：
  $ nmcli device wifi connect SSID_或_BSSID password 密码 hidden yes

  # 连接到 wlan1 网络接口上的 Wi-Fi：
  $ nmcli device wifi connect SSID_或_BSSID password 密码 ifname wlan1 profile_name

  # 断开网络接口上的连接：
  $ nmcli device disconnect ifname eth0

  # 显示连接列表及其名称、UUID、类型和支持设备：
  $ nmcli connection show

  # 激活连接（即使用现有配置文件连接到网络）：
  $ nmcli connection up name_或_uuid

  # 删除连接：
  $ nmcli connection delete name_或_uuid

  # 显示所有网络设备及其状态：
  $ nmcli device

  # 关闭 Wi-Fi：
  $ nmcli radio wifi off

  #编辑连接
  ### 获取连接列表：
  $ nmcli connection
  ###编辑
  nmcli connection edit 'wifi名称'
  ###或
  nmcli connection modify 'wifi名称' 配置项
  ###或在 /etc/NetworkManager/system-connections/ 中修改对应的 wifi名.nmconnection 文件。别忘了使用nmcli connection reload 重新加载配置文件。

  ```

  - 通过 Wi-Fi 共享网络连接：安装`dnsmasq`包 以实现连接共享。NetworkManager 会启动自己的 dnsmasq 实例，与 dnsmasq.service 独立。点击小程序，选择"Create new wireless network"。按照指南 (如果使用 WEP, 确保使用 5 到 13 字符长度的密码,其他的长度可能会失败)。
  - 移动网络支持:安装 `modemmanager`包 和 `usb_modeswitch`包。然后启用并启动 `ModemManager.service`。可能需要重新启动 `NetworkManager.service` 才能使其检测 `ModemManager`。重新启动后，重新插入调制解调器应该就可以识别了。
  - VPN 支持:只需要 `wireguard` 内核模块
  - 通过 Ethernet 共享连接：安装 dnsmasq 包以能够真正地共享连接。你的连有 internet 的设备和其他的设备通过合适的 ethernet 线缆连接（通常这意味着一个交叉线(cross over cable)或者之间有一个交换机（switch））。从终端运行 nm-connection-editor,添加一个新的 ethernet 连接，给它起一个合适的名字。比如"共享连接"，转到"IPv4 设置"（IPv4 settings），对于"方法:"（method） 选择 "与其他电脑共享"（Shared to other computers）,保存。现在，你在 NetworkManager 的有线连接下应该有了一个新"共享连接"的选项。
  - 更换内置的 DHCP 客户端：请在 `/etc/NetworkManager/conf.d/` 中的配置文件中设置选项 `[main]\n dhcp=客户端名称`。注意！不要启用 dhclient 包 和 dhcpcd 包 软件包提供的 systemd 单元。它们会与 NetworkManager 冲突

- 网络管理工具：`dialog net-tools netctl wpa_supplicant dhcpcd`

## 安装 CPU 编码

`pacman -S intel-ucode（amd-ucode）`

- intel CPU 安装 ：`intel-ucode`
- amd CPU 安装 ：`amd-ucode`

## 安装显卡驱动

- intel 核显：`xf86-video-intel`
- nvidia:`mesa`,`nvidia(linux-lts内核，navidia-lts)`,`navida-settings`,`nvidia-dkms`,`nvidia-utils`,`nvidia-prime`
- amd：`xf86-video-amdgpu`

## 安装引导

- 安装工具

  ```bash
   pacman -S grub efibootmgr（efi启动）os-prober(寻找其他系统的工具）
   mkdir /boot/grub
  ```

- Grub2.06 后不再启用 os-prober，需要手动启用：`vim /etc/default/grub`

  ```conf
  ...
  #grub2.06默认禁用os-prober
  #启用os-prober来识别win引导
  GRUB_DISABLE_OS_PROBER=false
  ...
  ```

- 安装 grub

  ```bash
  #gpt引导32位
   grub-install --target=i386-pc /dev/sda (整个磁盘，即根目录`/`）
  #efi引导
  grub-install --target=x86_64-efi --efi-directory=/boot
  ```

- 生成 grub.cfg：`grub-mkconfig -o /boot/grub/grub.cfg`

## 退出当前环境

`exit`

## 关闭网络

`killall wpa_supplicant dhcpcd`

## 重启, 拔 u 盘

`reboot`

---

## 添加用户

- 安装 sudo：`pacman -S sudo`
- 添加用户：`useradd -m 用户名`
- 添加用户：`useradd -m -G 组名 用户名`
- 设置密码：`passwd 用户名`
- 编辑权限：`vim /etc/sudoers`

> 第一列用户名：test、root
> 第二列，等号左边：允许从任何主机登陆当前用户
> 第二列，等号右边：第一列用户可以切换到系统中的其他用户
> 第三列，NOPASSWD 表示 sudo 不用打密码

```txt
root    ALL=(ALL:ALL) ALL
test    ALL=(ALL:ALL) NOPASSWD:ALL
```

## 添加 archlinuxcn 源

- `vim /etc/pacman.conf`

  ```conf
  [archlinuxcn]
  #archlinuxcn-keyring不报错的话可以不加下行
  #SigLevel=Optional TrustAll
  #Server=https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
  Server=https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
  ```

- 取消`[multilib]`中两行的注释，不然 32 位包无法安装
- 同步：`pacman -Syu`
- 安装 GPGkey：`pacman -S archlinuxcn-keyring`

## 图形界面

> 用户登陆成功后，会读取用户目录下的`.bashrc`文件(zsh 终端则是`.zshrc`文件)，相当于手动执行`source ~/.bashrc`。此时登陆到 tty1。接着执行 startx 命令会执行`~/.xinitrc`中的内容
> .Xresources 文件，用来调节 dpi(默认 96),`Xft.dpi:192`(数字为：放大倍数\*96)

- 安装 xorg：`xorg` `xorg-server` `xorg-xinit`

- 登陆管理器：
  - slim:
    - 安装 `pacman -S slim`
    - 开机自启动：`systemctl enable slim.service`
  - ssdm
    - 安装：`pacman -S ssdm`
    - 启动：`systemctl enable ssdm`
  - lightdm
    - 安装：`pacman -S lightdm lightdm-gtk-greeter`
    - 启动：`systemctl enable lightdm`
  - gdm
    - 安装：`pacman -S gdm`
    - 启动：`systemctl enable gdm`
- 桌面环境
  - xfce4(简洁)：
    - 安装:`pacman -S xfce4 xfce4-goodies(工具和插件，可不装)`
    - 配置: `vim .xinitrc`，添加`exec startxfce4`
  - kde(绚丽)：
    - 安装:`pacman -S plasma kde-applications`
  - gnome(易用)：
    - 安装:`pacman -S gnome gnome-extra`
    - 配置: `vim .xinitrc`，添加`exec gnome-session`
  - lxde(轻量)：
    - 安装:`pacman -S lxde`或最少需要安装 `lxde-common`包, `lxsession`包, `openbox`包(或者其他窗口管理器）。
    - 配置: `vim .xinitrc`，添加`exec startlxde`

## 蓝牙服务

- 安装： `bluez bluez-utils`
- 启动服务：`systemctl enable bluetooth.service`，`systemctl start bluetooth.service`
- 使用

  ```bash
  $ bluetoothctl --help          查看帮助命令
  $ bluetoothctl -v              查看蓝牙版本
  $ bluetoothctl            进入蓝牙管理工具环境
  [bluetooth]# power on                打开蓝牙
  [bluetooth]# agent on                开启代理
  [bluetooth]# scan on                 扫描蓝牙设备
  [bluetooth]# pair xx:xx:xx:...       配对该设备
  [bluetooth]# trust xx:xx:xx:...      信任该设备
  [bluetooth]# connect xx:xx:...       连接该设备
  ```

- 图形化界面：`sudo pacman -S blueman`
- 如果要用蓝牙耳机，或者蓝牙音响，还需要安装 pulseaudio-bluetooth
  `# pacman -S pulseaudio-bluetooth`
  这个程序默认开机启动。但经常发生开机后能连接音响，却不是用音响输出声音的情况。
  重新启动 pulseaudio 执行`pulseaudio -k`

再打开音频或者视频就能从蓝牙音响输出了。

## 声音

- 安装工具包:`pacman -S alsa-utils`
- 可以使用 `amixer` 解除声卡的主音量静音：

```bash
amixer sset Master unmute
amixer sset Speaker unmute
amixer sset Headphone unmute
```

- 使用 `alsamixer` 可以解除声卡的静音：`alsamixer`

声道下方标有`MM`表示其已经静音，而标有`00`表示已经启用。

使用`←`和`→`键滚动到 Master 和 PCM 声道，按下 m 键解除静音。

使用 `↑` 键增加音量，获得 0dB 的增益。增益值可在左上方 Item: 字段旁边看到。
注意： 若增益高于 0 dB，可能会听到失真。

- 若没有声音，需配置声卡

  - 获取声卡的声卡 ID 和设备 ID:`aplay -l`
  - 获取 amixer 控制器配置:`amixer scontrols`
  - 获取 amixer 声卡配置:`amixer -c 1 scontrols`
  - 在刚才 aplay -l 里面选择声卡 1,设备 ID 为 0 的声卡。把下列配置添加到系统级别的 /etc/asound.conf 或用户级别的 ~/.asoundrc 文件。如果文件不存在，可以手动创建。其中的各个 ID，请根据实际情况调整：

  ```conf
  defaults.pcm.card 1
  defaults.pcm.device 0
  defaults.ctl.card 1
  ```

  pcm 选项决定用来播放音频的设备，而 ctl 选项决定那个声卡能够由控制工具（如 alsamixer）使用。上述配置在重启音频程序（如 mplayer）后立即生效。

- 启用麦克风
  要启用麦克风，按 F4 切换至 Capture （捕获）选项卡，然后按 空格 启用一个声道即可

## 安装字体

- 开源：`ttf-dejavu`

- 中文：`wqy-zenhei`
- 中文：`noto-fonts-cjk`

## 设置中文

- 删除`zh_CN.UTF-8`注释：`vim /etc/locale.gen`
- 执行：`locale-gen`
- 全局配置中文（不建议，tty 中不支持中文，会乱码）：`echo LANG=zh_CN.UTF-8 >> /etc/locale.conf`
- 局部配置，在`~/.xprofile`或`~/.xinitrc`中配置`LANG=zh_CN.UTF-8`

## 中文输入法 fcitx5

- 安装：`pacman -S fcitx5 fcitx5-chinese-addons（简体中文输入） fcitx5-gtk（gtk程序支持） fcitx5-qt fcitx5-configtool fcitx5-config-qt（qt5程序支持） fcitx5-git fcitx5-pinyin-zhwiki(维基百科词库，无版权风险) kcm-fcitx5（kde桌面环境支持）`
- 配置：编辑 /etc/environment 并添加以下几行，然后重新登录

  ```conf
  GTK_IM_MODULE=fcitx
  QT_IM_MODULE=fcitx
  XMODIFIERS=@im=fcitx
  #SDL_IM_MODULE 是为了让一些使用特定版本 SDL2 库的游戏能正常使用输入法。
  SDL_IM_MODULE=fcitx
  #GLFW_IM_MODULE 是为了让 kitty 启用输入法支持。此环境变量的值只能为 ibus。
  GLFW_IM_MODULE=ibus
  ```

- 开机自启，执行：`cp /usr/share/applications/org.fcitx.Fcitx5.desktop ~/.config/autostart/`

- 主题和外观

  - 安装：`yay -S fcitx5-material-color`
  - 配置：

  ```conf
  # 垂直候选列表
  Vertical Candidate List=False
  # 按屏幕DPI使用
  PerScreenDPI=True
  # Font(设置成你喜欢的字体)
  Font="思源黑体 CN Medium 13"
  # 主题
  Theme=Material-Color-DeepPurple
  ```

- Emoji
  1. 首先确保电脑上已经安装了带有 Emoji 的字体（例如 noto-fonts-emoji 包）。
  2. 将字体设置为 Noto Sans CJK SC
- wps 无法输入中文：该问题在国内版 wps-office-cn 11.1.0.9604-1 版本更新后部分用户出现，于 wps-office-cn 11.1.0.9615-1 版本修复
- 自定义词库

> 注意： 手动下载的词典文件直接放到 ~/.local/share/fcitx5/pinyin/dictionaries 路径下即可。词典文件的后缀名应当为 .dict
> 一般而言,由于 fcitx5 包 支持 导入搜狗词库，因此很大程度上不需要自定义词库，但是 fcitx5 包 依然提供了相关工具。

- 安装 `libime`包:
  原始词库文件是一个文本文件，其格式为： 汉字 拼音 频率,在得到原始词库文件后，调用 `libime_pinyindict "词库文件.txt" "词库文件.dict"` 即可。

自定义词库文件放置在 `~/.local/share/fcitx5/pinyin/dictionaries`

## 安装 yay

> 需要配置 etc/pacman.conf 中，加入 archlinuxcn Server 配置

- pacman 命令

| 命令                            | 说明                                          |
| ------------------------------- | --------------------------------------------- |
| pacman -Syu                     | 系统升级，更新软件,对整个系统进行更新         |
| pacman -Su                      | 将本地的包数据库与远程的仓库进行了同步        |
| pacman -S 包名                  | 安装或升级单个软件                            |
| pacman -Sy 包名                 | 同步数据后,再安装或升级单个软件               |
| pacman -U 包名                  | 安装本地包，扩展名为 pkg.tar.gz 或 pkg.tar.xz |
| pacman -U url 名                | 安装远程包                                    |
| pacman -R 包名                  | 删除软件，保留依赖                            |
| pacman -Rs 包名                 | 删除软件，和其他没有被引用的依赖              |
| pacman -Rdd 包名                | 删除软件，及依赖它的软件                      |
| pacman -Qs 名称                 | 查看已安装的软件                              |
| pacman -Ss 名称                 | 搜索仓库中的软件                              |
| pacman -Sc                      | 清空软件包缓存                                |
| pacman -Scc                     | 清空所有缓存                                  |
| pacman -R $(pacman -Qdtq)       | 清楚系统中的无用包                            |
| pacman -R $(pacman -Qsq 关键词) | 清楚系统中的包                                |

- 软件降级
  利用 Arch Linux 存档库（Arch Linux Archive，简称 ALA）来实现旧版本软件的安装。ALA 保存了官方仓库快照、iso 镜像 和 引导程序包 的历史版本。
  在 ALA 上查找想要的软件包
  ALA 的网址为：https://archive.archlinux.org/
  ALA 的存档库分为下列三个主目录：iso、packages、repos。repos 目录包含官方仓库镜像的每日快照；packages 目录包含每个包的所有版本及其相应的数字签名；iso 目录按发布日期，保存官方 ISO 镜像和启动压缩包。
  找到想要的软件后，可以直接从浏览器下载，然后用 pacman 来安装,例如：

```bash
sudo pacman -U guile2.2-2.2.7-1-x86_64.pkg.tar.zst
```

也可以直接用 pacman 通过链接来安装，例如：

```bash
sudo pacman -U https://archive.archlinux.org/packages/g/guile2.0/guile2.2-2.2.7-1-x86_64.pkg.tar.zst
```

装完之后为了防止下次`pacman -Syu`时又自动升级到最新版，可以在/etc/pacman.conf 里的“IgnorePkg” 加上刚刚安装的包。多个用空格分隔

```bash
IgnorePkg   = linux linux-headers linux-lts llinux-lts-headers
```

- yay 命令,不要加 sudo

| 命令                     | 说明                                      |
| ------------------------ | ----------------------------------------- |
| yay                      | 同步并更新所有来自仓库和 AUR 的软件包     |
| yay `软件包` 或 `搜索词` | 从仓库和 AUR 中交互式搜索和安装软件包     |
| yay -S `软件包`          | 从仓库和 AUR 中安装一个新的软件包。       |
| yay -Ss `软件包`         | 从仓库和 AUR 中搜索软件包数据库中的关键词 |
| yay -Syu                 | 更新软件                                  |

## 其他软件

### 官网

- [listen1](http://listen1.github.io/listen1/)

### pacman

- 男人：`man`
- 屏幕上显示键盘：`screenkey`
- 快速启动：`albert`
- 文件管理器：`ranger`
- 电脑参数：`neofetch`
- 挂载 ntfs：`ntfs-3g`
- 办公：`libreoffice-fresh`
- 火狐：`firefox`
- 截图：`flameshot`
- 看图：`feh`
- pdf：`okular`
- 播放器：`mpv`,`vlc`
- 录屏：`obs-studio`( 确保安装了`pipewire`,`libpipewire02`，`xdg-desktop-portal-wlr`)
- 图形化解压工具：`xarchiver`,`file-roller`
- 虚拟摄像头：`v4l2loopback`（可用在直播软件中实现共享屏幕）

  - 移除虚拟相机命令：`sudo modprobe --remove v4l2loopback`
  - 添加虚拟相机命令：`sudo modprobe v4l2loopback devices=2`

- 编辑器：`nvim`
  - 安装 LazyVim 配置:
    - `git clone https://github.com/LazyVim/starter ~/.config/nvim`
    - `rm -rf ~/.config/nvim/.git`
    - `nvim`
  - 安装字体：`Nerd Fonts`

### [aur](https://aur.archlinux.org/)

- 谷歌:`google-chrome`（root 用户无法启动）
- markdown:`typora-free-cn`,`remarkable`
- vscode:`visual-studio-code-bin`（root 用户无法启动）
- 快照保存：`timeshift`

  - 打开 Timeshift 后，从下一个窗口中选择快照类型。您有两个选项可供选择：
    `RSync`：使用 rsync 和硬链接拍摄快照。快照之间共享公共文件，从而节省磁盘空间。每个快照都是完整的系统备份，可以使用文件管理器进行浏览。
    `BTRFS`：使用 BTRFS 文件系统的内置功能拍摄快照。 BTRFS 快照仅在具有 Ubuntu 类型子卷布局（带有 @ 和 @home 子卷）的 BTRFS 系统上受支持。
  - 第三步：选择存储设备
    在此阶段，您需要指定存储备份的位置。您可以使用计算机的硬盘或外部 USB 磁盘进行备份存储。如果您使用计算机的硬盘，则可以按计划自动创建 Timehsift 备份。如果您使用外部 USB 磁盘，则必须在插入时手动创建备份或始终保持插入状态。建议使用外部驱动器作为备份位置，以防硬盘发生严重问题。您要使用的 USB 或外部驱动器应格式化为 Linux 文件系统。否则，将不会被检测到。
  - 配置计划快照
    下一个屏幕将指导您设置计划备份。有多种计划选项，例如每月、每周、每天、每小时等
  - 选择文件/文件夹
    在此步骤中，您需要选择要备份系统的哪些部分。默认情况下，不备份用户文件。仅备份系统启动和运行所需的文件。这有几个优点。如果您备份用户文件，则在恢复时，这些文件将被覆盖，并且创建备份后所做的任何更改都将消失。因此，请明智地选择这些文件夹。
  - 创建备份
    初始设置完成后，您将到达 Timeshift 主页，您可以在其中单击创建按钮创建第一个备份。

- wps：`wps-office`

  - 无法输入中文错误来源
    好像是因为 wps 在较新的版本之后就不在读取用户的默认的配置文件~/.pam_environment，所以你在这个文件中再怎么改也没有用。

  - 解决办法
    正确的做法是直接在启动脚本中添加 export 变量导出。
    启动脚本位于/usr/bin 目录下，`/usr/bin/wps`, `/usr/bin/et` ,`/usr/bin/wpp`。
    在 gOpt 一行下添加

    ```bash
    export GTK_IM_MODULE=fcitx
    export QT_IM_MODULE=fcitx5
    export XMODIFIERS=@im=fcitx
    ```

    然后保存退出。这样就可以成功在 WPS 中使用 fcitx5 了。

- qq:`linuxqq`
- 腾讯会议：`wemeet-bin`
- 微信：`wechat-uos`
- 飞书：`feishu-bin`
- 迅雷：`xunlei-bin`

## 触摸板

- 安装驱动：`libinput`，`xf86-input-libinput`
- 配置软连接：`ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf/40-libinput.conf`
- 查看触摸板编号：`xinput list`，找到 touchpad 的 id
- 编辑配置： `vim 40-libinput.conf`，找到 touchpad，添加两条 Option，重启.

  ```conf
  ...
  Section "inputClass"
         Identifier "libinput touchpad catchall"
         MatchIsTouchpad "on"
         MatchDevicePath "/dev/input/event*"
         Driver "libinput"
         Option "Tapping" "on"
         Option "clickMethod" "clickfinger"
  EndSection
  ...
  ```

## 屏幕亮度

> 亮度由 ACPI，图形或者平台的驱动来控制

- 查看 ACPI 内核模块：`ls /sys/class/backlight/`
- 查看最大亮度：`ls /sys/class/backlight/acpi_video0/max_brightness`
- 测试,向 brightness 写入代表亮度等级的数值：`echo 5 > /sys/class/backlight/acpi_video0/brightness`
- 若不生效或是 amd 显卡，可能需要修改内核：
  编辑 `/etc/default/grub`并将您的内核选项添加至 `GRUB_CMDLINE_LINUX_DEFAULT='...'` 行

```conf
acpi_backlight=video
acpi_backlight=vendor
acpi_backlight=native
acpi_backlight=none
```

然后重新生成 `grub.cfg` 文件：`grub-mkconfig -o /boot/grub/grub.cfg`

- 关闭背光：`xset dpms force off`
- 开启背光：`xset s`

- 启用 fn 快捷键
  - 安装：`acpid2`,`openbsd-netcat`
  - 启动服务：`systemctl start/enable acpid.service.`
  - 查看按钮：`netcat -U /var/run/acpid.socket`
- 创建屏幕控制脚本(重启 acpid 服务)：

  ```sh
  ### vim /etc/acpi/handlers/bl
  #!/bin/sh
  bl_dev=/sys/class/backlight/acpi_video0
  step=1
  case $1 in
    -) echo $(($(< $bl_dev/brightness) - $step)) >$bl_dev/brightness;;
    +) echo $(($(< $bl_dev/brightness) + $step)) >$bl_dev/brightness;;
  esac

  #vim /etc/acpi/events/bl_d
  event=video/brightnessdown.*
  action=/etc/acpi/handlers/bl -

  #vim /etc/acpi/events/bl_u
  event=video/brightnessup.*
  action=/etc/acpi/handlers/bl +
  ```

- 创建声音控制脚本：

  ```sh
  #vim /etc/acpi/events/vol-d
  event=button/volumedown
  action=amixer set Master 5-

  #vim /etc/acpi/events/vol-m
  event=button/mute
  action=amixer set Master toggle

  #vim /etc/acpi/events/vol-u
  event=button/volumeup
  action=amixer set Master 5+
  ```

## 安装 Deb 包

- 安装 debtap：`yay -S debtap`
- 更新数据库：`sudo debtap -u`
- 转化 deb 包：`debtap xxx.deb`
- 安装：`sudo pacman -U xxxx.pkg`

### 文件管理工具

#### dolphin

安装： `sudo pacman -S dolphin breeze`
文件预览：

- kdegraphics-thumbnailers: Image files, PDFs and Blender application files.
- kimageformats: Gimp .xcf files
- libheif: HEIF files
- qt5-imageformats : .webp, .tiff, .tga, .jp2 files
- resvgAUR: Fast and accurate SVG image thumbnails
- kdesdk-thumbnailers: Plugins for the thumbnailing system
- ffmpegthumbs: Video files (based on ffmpeg)
- raw-thumbnailerAUR: .raw files
- taglib : Audio files
- kde-thumbnailer-apkAUR: Android package files
  Enable preview showing of required file type in`Settings > Configure Dolphin... > General > Previews.`

To enable `heif/heic image thumbnails`, edit `/usr/share/kservices5/imagethumbnail.desktop` by adding `image/heif;` to the line which starts with `MimeType=`.

Note that heif/heic thumbnails will be enabled by default on versions of `kio-extras` after 21.12

#### pcmanfm

- 自动挂载 u 盘、垃圾桶:`gvfs`,`udisks`
- 解压：`xarchiver`

### 远程控制 windowns

- `rdesktop`
  在此之前需要让 Windows 支持远程协助，可以在系统属性-远程中开启：【允许远程协助连接这台计算机】+【允许远程连接到此计算机】，如果勾选了【仅运行运行使用网络级别身份验证的远程桌面单位计算机连接】，那么 rdesktop 无法连接，报错信息：

```bash
Core(warning): Certificate received from server is NOT trusted by this system, an exception has been added by the user to trust this specific certificate.
Failed to initialize NLA, do you have correct Kerberos TGT initialized ?
Failed to connect, CredSSP required by server (check if server has disabled old TLS versions, if yes use -V option).
```

解决方法有两种：1）不勾选该选项；2）使用 xfreerdp
rdesktop 192.168.xxx.xxx 默认选项就可以远程桌面连接 Windows（需要输入用户口令）。
rdesktop 一些常用选项：
-u : Windows 用户
-p : Windows 口令（非 PIN）
-g : 窗口大小，如 1366x768
-f :全屏
-a : 色彩深度 ：8, 15, 16, 24, 32
-r sound ：支持声音
-r clipboard：支持剪切板
-r disk： 远程连接时挂载本地文件目录

详细信息 man rdesktop

- `freerdp`

```bash
/v:<server>[:port] 默认端口 3389
/w、/h 窗口大小
/size:<width>x<height> 窗口大小，如 1024x768
/f 全屏
/workarea Use available work area
/bpp:<depth> 色彩深度
/u:<user>[@<domain>]
/p:<password>
/d:<domain> 域，可选
+fonts 平滑字体
```

- 共享盘或者传输文件

  - 对于 xfreerdp 指定 /drive:
    xfreerdp /drive:SharedDir,/home/user/SharedDir /u:user /p:password /v:ip

  - 对于 rdesktop 指定 -r disk:
    rdesktop -r disk:SharedDir=/home/user/SharedDir ip
    其中 SharedDir 是随便输入的名字，接着是共享文件夹的本地绝对路径

### 设置

xrandr
xrandr --output screen --mode 1280x800
feh --randomize --bg-fill /地址
picom

## 常见错误

- `...invalid or corrupted package (PGP sinature)`

同步时间：`timedatectl set-ntp true`
运行：`pacman -S archlinu-keyring`
回到安装，重新 pacstrap 安装

- 普通用户无法 startx

  - `tmp`目录是否有权限
  - 拷贝 root 用户下的`.Xauthority`，`xinitrc`到普通用户的 home 目录下

- unzip 解压大文件错误
  下载 p7zip

- npm nrm 安装后报错
  原因：应该使用 open 的 CommonJs 规范的包 ，现在 open v9.0.0 是 ES Module 版本的包
  解决方法：`npm install -g nrm open@8.4.2 -save`

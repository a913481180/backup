---
title: dwm
date: 2020-10-11 22:11:33
categories:
- linux
---

## 补丁

```bash
cd dwm
patch < xxxx.diff
patch -R < xxx.diff
```

- dwm-alpha-xxx.diff：窗口透明，需配合picom
- dwm-awesomebar-xxx.diff：tabar隐藏窗口
- dwm-gaps-6.0.diff：窗口之间的缝隙
- dwm-focusonclick.xxx.diff：鼠标点击才窗口聚焦
- dwm-moveresize-xxx.diff：浮动模式，调整窗口位置大小
- dwm-autostart-xxxx.diff：自动运行脚本，这个补丁将在dwm进入主循环运行之前运行~/.dwm/autostart_blocking.sh和~/.dwm/autostart.sh &，然后才启动dwm。可以省略其中一个或两个文件。 上面列出的文件分别在目录$XDG_DATA_HOME/DWM、$HOME/.local/Share/DWM和$HOME/.dwm中查找。将会在最先找到的目录中运行脚本，即使该目录下没有脚本。直接使用，将脚本文件放入~/.dwm下，同时确保没有上面提到的另两个目录。同时注意若~/.dwm/autostart_blocking.sh无法执行完毕，则将卡死。

## 安装

```bash
cd dwm
sudo make clean install
```

## 修改配置

### config.h

- 字体

```c
static const char *fonts[]          = { "DejaVu Sans Mono:size=18" };
//static char *font = "Droid Sans Mono:pixelsize=18:antialias=true:autohint=true";
//static char *font = "Monaco:pixelsize=18:antialias=true:autohint=true";
//static char *font = "Inconsolata:pixelsize=18:antialias=true:autohint=true";

```

- 颜色

```c
static const char col_gray1[]       = "#222222";
static const char col_gray2[]       = "#444444";
static const char col_gray3[]       = "#bbbbbb";
static const char col_gray4[]       = "#eeeeee";
//static const char col_cyan[]        = "#005577";
static const char col_cyan[]        = "#00d997";

```

- 自定义快捷键

我们以flamshot为例，为flameshot设置截图快捷键

首先在/*commands*/下添加你要设置快捷键的命令

- flameshot的截图命令是：flameshot gui，后面没有参数所以设为NULL，这条命令的名字我们设为flameshot

`static const char *flameshot[]  = { "flameshot","gui", NULL };`

进行快捷键和命令的绑定

```c
{ 0,           XK_Print,      spawn,          {.v = flameshot } },
```

- 重新编译

```bash
sudo make clean install
```

## 脚本

- 启动脚本

```sh
#~/.dwm/autostart.sh
feh   --bg-fill ~/Downloads/bg.jpg
picom &
albert &
nm-applet &
fcitx5 -d

while true
do
    bash ~/.dwm/dwm_status.sh
    sleep 1
done

```

- 状态脚本

```sh
# ~/.dwm/dwm_status.sh
# This function parses /proc/net/dev file searching for a line containing $interface data.
# Within that line, the first and ninth numbers after ':' are respectively the received and transmited bytes.
function get_bytes {
 # Find active network interface
 interface=$(ip route get 8.8.8.8 2>/dev/null| awk '{print $5}')
 line=$(grep $interface /proc/net/dev | cut -d ':' -f 2 | awk '{print "received_bytes="$1, "transmitted_bytes="$9}')
 eval $line
 now=$(date +%s%N)
}



# Function which calculates the speed using actual and old byte number.
# Speed is shown in KByte per second when greater or equal than 1 KByte per second.
# This function should be called each second.

function get_velocity {
 value=$1
 old_value=$2
 now=$3

 timediff=$(($now - $old_time))
 velKB=$(echo "1000000000*($value-$old_value)/1024/$timediff" | bc)
 if test "$velKB" -gt 1024
 then
  echo $(echo "scale=2; $velKB/1024" | bc)MB/s
 else
  echo ${velKB}KB/s
 fi
}

# Get initial values
get_bytes
old_received_bytes=$received_bytes
old_transmitted_bytes=$transmitted_bytes
old_time=$now

get_bytes

# Calculates speeds
vel_recv=$(get_velocity $received_bytes $old_received_bytes $now)
vel_trans=$(get_velocity $transmitted_bytes $old_transmitted_bytes $now)




print_date(){
 #date '+Y-%m-%d %H:%M:%S'
 date '+%m-%d %H:%M:%S'
}



is_charging(){
 charging=$(acpi -b | grep -Eo  "Charging")
 if [ "$charging" == "Charging" ];
 then
  echo "🔌";
 else
  echo "🔋";
 fi

}
print_bat(){
 remain=$(acpi -b |awk '{print $4}'| grep -Eo "[0-9]+" |paste -sd+ |bc)
 echo -e "$remain%"
}

print_volume(){
 volume="$(amixer get Master | tail -n1 |sed -r 's/.*\[(.*)%\].*/\1/')"
 if test "$volume" -gt 0
 then
  echo -e "V$volume%"
 else
  echo -e "MM"
 fi
}
print_mem(){
 memFree=$(($(grep -m1 'MemAvailable:' /proc/meminfo | awk '{print $2}')/1024))
echo -e "$memFree"
}

LOC=$(readlink -f "$0")
DIR=$(dirname "$LOC")
export IDENTIFIER="unicode"

. "$DIR/dwm_resources.sh"
. "$DIR/dwm_alsa.sh"
#. "$DIR/dwm_weather.sh"
#. "$DIR/dwm_network.sh"




xsetroot -name "|$(dwm_resources)|⬇️ $vel_recv ⬆️ $vel_trans| $(print_date) |$(dwm_alsa) $(is_charging)$(print_bat)"



# Update old values to perform new calculations
old_received_bytes=$received_bytes
old_transmitted_bytes=$transmitted_bytes
old_time=$now

exit 0

```

- 触摸板滑动反转

```sh
#!/bin/bash

# Get id of touchpad and the id of the field corresponding to
# natural scrolling
id=`xinput list | grep "Touchpad" | cut -d'=' -f2 | cut -d'[' -f1`
natural_scrolling_id=`xinput list-props $id | \
                      grep "Natural Scrolling Enabled (" \
                      | cut -d'(' -f2 | cut -d')' -f1`

# Set the property to true
xinput --set-prop $id $natural_scrolling_id 1

```

- 声音

```sh
#!/bin/sh

# A dwm_bar function to show the master volume of ALSA
# Joe Standring <git@joestandring.com>
# GNU GPLv3

# Dependencies: alsa-utils

dwm_alsa () {
    VOL=$(amixer get Master | tail -n1 | sed -r "s/.*\[(.*)%\].*/\1/")
    printf "%s" "$SEP1"
    if [ "$IDENTIFIER" = "unicode" ]; then
        if [ "$VOL" -eq 0 ]; then
            printf "🔇"
        elif [ "$VOL" -gt 0 ] && [ "$VOL" -le 33 ]; then
            printf "🔈 %s%%" "$VOL"
        elif [ "$VOL" -gt 33 ] && [ "$VOL" -le 66 ]; then
            printf "🔉 %s%%" "$VOL"
        else
            printf "🔊 %s%%" "$VOL"
        fi
    else
        if [ "$VOL" -eq 0 ]; then
            printf "MUTE"
        elif [ "$VOL" -gt 0 ] && [ "$VOL" -le 33 ]; then
            printf "VOL %s%%" "$VOL"
        elif [ "$VOL" -gt 33 ] && [ "$VOL" -le 66 ]; then
            printf "VOL %s%%" "$VOL"
        else
            printf "VOL %s%%" "$VOL"
        fi
    fi
    printf "%s\n" "$SEP2"
}

```

- 天气

```sh
#!/bin/sh

# A dwm_bar function to print the weather from wttr.in
# Joe Standring <git@joestandring.com>
# GNU GPLv3

# Dependencies: curl

# Change the value of LOCATION to match your city
dwm_weather() {
    LOCATION="chuangzhou"

    printf "%s" "$SEP1"
    if [ "$IDENTIFIER" = "unicode" ]; then
        printf "%s" "$(curl -s wttr.in/$LOCATION?format=1)"
    else
        printf "WEA %s" "$(curl -s wttr.in/$LOCATION?format=1 | grep -o "[0-9].*")"
    fi
    printf "%s\n" "$SEP2"
}

dwm_weather
```

- 内存

```sh
dwm_resources () {
    # Used and total memory
    MEMUSED=$(free -h | awk '(NR == 2) {print $3}')
    MEMTOT=$(free -h |awk '(NR == 2) {print $2}')
    # CPU temperature
    #CPU=$(sysctl -n hw.sensors.cpu0.temp0 | cut -d. -f1)

    # Used and total storage in /home (rounded to 1024B)
    #STOUSED=$(df -h | grep '/home$' | awk '{print $3}')
    ##STOTOT=$(df -h | grep '/home$' | awk '{print $2}')
    #STOPER=$(df -h | grep '/home$' | awk '{print $5}')

    printf "%s" "$SEP1"
    if [ "$IDENTIFIER" = "unicode" ]; then
        #printf "💻 MEM %s/%s CPU %s STO %s/%s: %s" "$MEMUSED" "$MEMTOT" "$CPU" "$STOUSED" "$STOTOT" "$STOPER"
        printf "%s/%s" "$MEMUSED" "$MEMTOT"
    else
        #printf "STA | MEM %s/%s CPU %s STO %s/%s: %s" "$MEMUSED" "$MEMTOT" "$CPU" "$STOUSED" "$STOTOT" "$STOPER"
        printf "%s/%s " "$MEMUSED" "$MEMTOT" 
    fi
    printf "%s\n" "$SEP2"
}

dwm_resources
```

- 网速

```sh
#!/bin/sh

# A dwm_bar function to show the current network connection/SSID, private IP, and public IP
# Joe Standring <git@joestandring.com>
# GNU GPLv3

# Dependencies: NetworkManager, curl

dwm_network () {
    CONNAME=$(nmcli -a | grep 'Wired connection' | awk 'NR==1{print $1}')
    if [ "$CONNAME" = "" ]; then
        CONNAME=$(nmcli -t -f active,ssid dev wifi | grep '^yes' | cut -c 5-)
    fi

    PRIVATE=$(nmcli -a | grep 'inet4 192' | awk '{print $2}')
    PUBLIC=$(curl -s https://ipinfo.io/ip)

    printf "%s" "$SEP1"
    if [ "$IDENTIFIER" = "unicode" ]; then
        printf "🌐 %s %s | %s" "$CONNAME" "$PRIVATE" "$PUBLIC"
    else
        printf "NET %s %s | %s" "$CONNAME" "$PRIVATE" "$PUBLIC"
    fi
    printf "%s\n" "$SEP2"
}

dwm_network

```

- 系统托盘

安装

```bash
pacman -S trayer
yay -S trayer-srg
```

编写脚本

```sh
# ~/.dwm/showTrayer.sh
#!/bin/bash
result=$(ps ax |grep -v grep|grep trayer)
if [ "$result" == "" ]
then
 eval "trayer --transparent false --edge top --align right --width 21 --tint 0x88888888 --SetDockType true --margin 10 &"
else
 eval "killall trayer"
fi
```

绑定快捷键

```c
static const char *showTrayercmd[]  = { "/home/kk/.dwm/showTrayer.sh", NULL };
 { MODKEY|ShiftMask,             XK_t, spawn,          {.v = showTrayercmd } },
```

## 卸载

```bash
sudo make clean uninstall
```

## 基础快捷键

- 打开新终端
`Alt + shift + Enter`

- 关闭一个窗口
`Alt + shift + C`

- 窗口横向排列
`Alt + D`

- 窗口竖向排列
`Alt + I`

- 窗口位置互换
`Alt + Enter`

- 在窗口间切换
`Alt + J`
`Alt + k`

- 改变窗口的长度/比例
`Alt + H`
`Alt + L`

- 平铺模式（tiling)
`Alt + T`

- 单窗口模式
`Alt + M`

- 浮动模式（float)
`Alt + F`

- 窗口模式切换
`Alt + 空格`
`Alt + shift + 空格`

### 多屏幕问题

在主副屏之间移动焦点

```
# 移动焦点至左边屏幕
Mod + < 
# 移动焦点至右边屏幕
Mod + >
```

在主副屏之间移动窗口

```
# 移动窗口至左边屏幕
Mod + shift + <
```

- 切换标签页

`Mod + num`

- 移动窗口至某标签页

`Mod + shift + num`

---

## st

## 补丁

```bash
cd st
patch < xxxx.diff
patch -R < xxx.diff
```

- st-alpha-x.x.diff：窗口透明
- st-dracula-0.8.5.diff：主题
- st-desktopentry-0.8.2.diff：启动程序
- st-anysize-xxx.diff：满屏
- st-scrollback-xxx.diff：滚动，config.h绑定案件

    ```c
    { MODKEY,            XK_u,     kscrollup,      {.i = 2} },
    { MODKEY,            XK_d,     kscrollup,      {.i = 2} },
    ```

- st-scrollback-mouse-xxx.diff：支持鼠标滚动，需按shift键
- st-scrollback-mouse-altscreen-xxx.diff：不用按shift键滚动

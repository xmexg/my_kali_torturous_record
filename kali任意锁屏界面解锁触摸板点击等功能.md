# kali任意锁屏界面解锁触摸板点击等功能.md
可查看[这里](https://github.com/sddm/sddm/issues/657)

```shell
sudo mousepad /etc/X11/xorg.conf.d/20-touchpad.conf
```
添加如下内容
```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"

        Option "Tapping" "on"
        Option "NaturalScrolling" "on"
        Option "MiddleEmulation" "on"
        Option "DisableWhileTyping" "on"
EndSection
```
关键在于`Option "Tapping" "on"`, 有这一个就行
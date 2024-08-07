# kali双锁屏&锁屏密码间歇消失
安装了`xfce4-screensaver`导致的, 要么卸载`xfce4-screensaver`,要么关闭`light locker`,二选一

# 关闭`light locker`(我的选择)
`设置` -> `电源管理器` ->  `安全性` -> `light locker` -> `自动锁定会话` -> `从不`

# 卸载`xfce4-screensaver`(我喜欢xfce4-screensaver)
```shell
sudo apt remove --purge xfce4-screensaver
```
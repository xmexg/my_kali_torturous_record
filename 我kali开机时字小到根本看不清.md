# 我kali开机时字小到根本看不清
小屏高分辨率导致的, 笔记本屏幕小, 分辨率偏偏又高，字号还是原来的字号，自然显示出来的字小。

# 解决方法
1. 编辑`/etc/default/grub.d/kali-themes.cfg`文件
    ```shell
    sudo mousepad /etc/default/grub.d/kali-themes.cfg
    ```
    修改分辨率,屏幕分辨率小了文字显示自然大
    ```shell
    GRUB_GFXMODE="1280x720,1280x800,auto"
    ```
    同时这里也能改开机主题,比如我正在使用名为[Firefly_cn](https://www.gnome-look.org/p/2076542)的主题
    ```shell
    GRUB_THEME="/boot/grub/themes/Firefly_cn/theme.txt"
    ```
2. 更新grub配置
    ```
    sudo update-grub
    ```

# 屏幕显示的字太小怎么办?
+ 调低分辨率
+ 自定义缩放
+ 使用Kali HiDPI Mode
+ 使用kde-plasma桌面(漂亮又好用)